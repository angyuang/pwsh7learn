### **第七章：跨平台与云时代——PowerShell 的新征程**

PowerShell，这个诞生于 Windows 的强大工具，早已不满足于偏安一隅。它脱去了“Windows”的前缀，穿上了跨平台的“外衣”，开启了属于它的“大航海时代”。在第七章，我们将跟随 PowerShell 的脚步，扬帆起航，去探索这些全新的领域。我们将学习如何用 PowerShell 的哲学去管理 Linux 系统，如何与云端的庞大资源进行对话，以及如何通过 API 与万物互联。

这不仅仅是学习一项新技术，更是一次思想的升维。准备好，让我们一起见证 PowerShell 在这个波澜壮阔的跨平台与云时代中，如何开启它的新征程，展现其前所未有的广度与力量。

欢迎来到 PowerShell 的未来。在前面的章节中，我们深入挖掘了 PowerShell 在其“故土”——Windows 平台——上的强大能力。现在，我们将视野拓宽，去拥抱一个更加广阔和多元化的 IT 世界。随着 PowerShell（之前被称为 PowerShell Core）的开源和跨平台，它已经从一个 Windows 专属的管理工具，演变成一个可以在任何地方运行的、真正的通用自动化平台。

本章，我们将探索 PowerShell 在新时代的三个重要战场：在 Linux 和 macOS 上的原生应用，与主流公有云（Azure/AWS）的深度集成，以及通过 REST API 与任何现代服务进行交互的能力。学完本章，你将掌握使用同一种语言、同一种思维模式，去管理和自动化一个异构的、云原生的、服务化环境的核心技能。

* * *

#### **7.1. PowerShell on Linux/macOS**

对于许多人来说，“在 Linux 上运行 PowerShell”听起来可能有些不可思议。但事实是，PowerShell 已经成为 Linux 和 macOS 系统管理员工具箱中一个备受欢迎的新成员。它带来的，不仅仅是另一个 shell，更是一种全新的、以对象为中心的、结构化的管理哲学。对于那些需要同时管理 Windows 和 Linux 环境的“双栖”管理员来说，PowerShell 提供了一座弥合两大生态系统鸿沟的宝贵桥梁。

##### **7.1.1. 管理 Linux 系统：熟悉的配方，不同的风味**

在 Linux/macOS 上安装 PowerShell 后（具体安装步骤请参考 1.2.2 节），你就可以打开终端，输入 `pwsh`，进入一个熟悉的世界。许多核心的 PowerShell Cmdlet 依然有效，但它们操作的对象，已经变成了 Linux/macOS 的原生组件。

*   **查询进程** `Get-Process` 依然是你查询进程的好帮手，它返回的依然是结构化的进程对象。
    
    ```powershell
    # 获取所有进程，并按 CPU 使用率降序排序，取前 10 个
    Get-Process | Sort-Object -Property CPU -Descending | Select-Object -First 10
     
    # 查找所有名为 "sshd" 的进程
    Get-Process -Name "sshd"
    ```
    
*   **管理服务 (systemd)** 在现代 Linux 发行版中，`systemd` 是标准的服务管理器。虽然没有像 Windows 上那样的原生 `Get-Service`，但我们可以轻松地与 `systemctl` 命令交互，并将其输出转换为 PowerShell 对象。
    
    ```powershell
    # 获取所有服务的状态，并将其转换为对象
    # 'systemctl list-units --type=service' 的输出是文本，我们用 PowerShell 来解析它
    function Get-LinuxService {
        systemctl list-units --type=service --all |
            Select-Object -Skip 1 | # 跳过表头
            ForEach-Object {
                $parts = $_.Trim() -split '\s+'
                [PSCustomObject]@{
                    Unit = $parts[0]
                    Load = $parts[1]
                    Active = $parts[2]
                    Sub = $parts[3]
                    Description = $parts[4..($parts.Length - 1)] -join ' '
                }
            }
    }
     
    # 现在我们可以像使用原生 Cmdlet 一样使用它了
    Get-LinuxService | Where-Object { $_.Active -eq 'active' }
    ```
    
    这个例子完美地体现了 PowerShell 的哲学：即便是面对纯文本输出，也能将其结构化，赋予其对象的生命。
    
*   **分析日志文件** Linux 的日志通常存放在 `/var/log` 目录下。`Get-Content` 配合 PowerShell 强大的文本处理和对象操作能力，让日志分析变得异常强大。
    
    ```powershell
    # 实时监控 sshd 的认证日志，并高亮显示 "failed" 的行
    # -Tail 10 显示最后10行，-Wait 持续监控
    Get-Content -Path "/var/log/auth.log" -Tail 10 -Wait | ForEach-Object {
        if ($_ -match 'failed') {
            Write-Host $_ -ForegroundColor Red
        } else {
            Write-Host $_
        }
    }
     
    # 读取 Apache 的访问日志，统计每个 IP 地址的访问次数
    Get-Content -Path "/var/log/apache2/access.log" |
        ForEach-Object { $_.Split(' ')[0] } | # 提取每行的第一个字段（IP地址）
        Group-Object | # 按 IP 地址分组并计数
        Sort-Object -Property Count -Descending |
        Select-Object -First 20
    ```
    

##### **7.1.2. 与原生 Shell 工具协同：强强联合**

PowerShell 的设计者深知，它并非要取代 Linux 上那些久经考验的、强大的原生工具（如 `grep`, `awk`, `sed`, `jq`），而是要与它们协同工作。PowerShell 可以轻松地调用这些外部命令，并对其返回的纯文本输出进行处理。

*   **调用外部命令** 直接输入命令即可。PowerShell 会执行它，并逐行返回其标准输出（stdout）流。每一行都是一个字符串。
    
    ```powershell
    # 调用 `df -h` 查看磁盘空间，返回的是一个字符串数组
    $diskUsage = df -h
    $diskUsage.GetType() # System.String[]
    ```
    
*   **处理文本流** 由于返回的是字符串数组，我们可以立即使用 `ForEach-Object`、`Where-Object` 等 PowerShell 工具对其进行解析和结构化。
    
    ```powershell
    # 将 `df -h` 的输出转换为对象
    df -h | Where-Object { $_ -match '^/dev/' } | ForEach-Object {
        $fields = $_ -split '\s+'
        [PSCustomObject]@{
            FileSystem = $fields[0]
            Size = $fields[1]
            Used = $fields[2]
            Avail = $fields[3]
            UsePercent = [int]($fields[4] -replace '%', '')
            MountedOn = $fields[5]
        }
    } | Sort-Object -Property UsePercent -Descending
    ```
    
    在这个例子中，我们调用了 `df`，然后用 `Where-Object` 过滤出我们感兴趣的行，再用 `ForEach-Object` 将每一行文本解析成一个富含信息的 PowerShell 自定义对象。这就是 PowerShell 与原生工具“强强联合”的最佳体现。
    

##### **7.1.3. SSH 远程管理：PowerShell Remoting over SSH**

这是跨平台管理中最激动人心的特性之一。传统的 PowerShell Remoting (WinRM) 主要用于 Windows。现在，PowerShell Remoting 也可以运行在 **SSH (Secure Shell)** 之上！这意味着，你可以从你的 Windows 管理机，建立一个**完整、交互式的 PowerShell 远程会话**到一台 Linux 服务器，反之亦然。

**这与直接 SSH 有何不同？**

*   **直接 SSH**：你连接到的是 Linux 的默认 Shell (如 bash)。你输入的是 bash 命令，返回的是纯文本。
*   **PowerShell Remoting over SSH**：你连接到的是 Linux 上的 PowerShell 引擎 (`pwsh`)。你输入的是 PowerShell 命令，在远程执行后，返回的是**序列化的、完整的 .NET 对象**！

**配置与使用：** 

1.  **在 Linux 服务器上**：安装 PowerShell 和 OpenSSH Server，并编辑 `sshd_config` 文件，添加一个 PowerShell 子系统。 
    ```powershell
    # /etc/ssh/sshd_config
    Subsystem powershell /usr/bin/pwsh -sshs -NoLogo -NoProfile
    
2.  **在客户端**：
    
    powershell
    
    ```powershell
    # 使用 SSH 建立一个交互式的 PowerShell 会话
    # -HostName 指定目标，-UserName 指定用户
    Enter-PSSession -HostName "my-linux-server" -UserName "admin"
     
    # 或者，使用 Invoke-Command 在远程执行命令并取回对象
    Invoke-Command -HostName "my-linux-server" -UserName "admin" -ScriptBlock {
        Get-Process -Name "cron"
    }
    ```
    

通过 SSH 的 PowerShell Remoting，我们终于有了一种标准化的、面向对象的、跨平台的远程管理协议。它将 PowerShell 的管道和对象模型，无缝地扩展到了整个异构网络，是实现真正统一自动化的关键技术。

* * *

#### **7.2. 拥抱云端：Azure & AWS**

在上一节中，我们驾驶着 PowerShell 这艘坚固的航船，成功地驶入了 Linux 和 macOS 的港湾，领略了跨平台管理的别样风景。现在，我们要将目光投向更远方那片广袤无垠、风起云涌的“云端大陆”。这片大陆由 Azure、AWS、Google Cloud 等科技巨头构建，它正在以不可阻挡之势，重塑着整个 IT 世界的版图。

对于现代的系统管理员、DevOps 工程师和开发者而言，云，不再是一个遥远的概念，而是日常工作中必须打交道的现实。手动在网页控制台中点击、配置成百上千的云资源，不仅效率低下，更无法满足自动化、可重复部署（Infrastructure as Code）的时代要求。幸运的是，PowerShell 早已为我们准备好了通往这片新大陆的“船票”和“航海图”。

云，本质上是一个由 API 驱动的、按需分配的、巨大的资源池。无论是虚拟机、数据库、存储，还是人工智能服务，云中的一切皆为资源，皆可通过代码进行编程化管理。PowerShell，凭借其强大的脚本能力和丰富的模块生态，已经成为管理主流公有云（特别是微软自家的 Azure）的首选工具之一。

本节，我们将学习如何使用 PowerShell 连接到 Azure 和 AWS 这两大云平台，并执行核心的资源管理任务。我们将看到，PowerShell 的 `动词-名词` 哲学，如何被平滑地扩展到云端，让我们能够以一种熟悉而统一的方式，去创建、查询、配置和删除云资源。

##### **7.2.1. 连接到云：获取你的“云端护照”**

要通过 PowerShell 管理云资源，第一步是进行身份验证，证明“你是谁”以及“你被授权做什么”。这通常通过安装相应的云厂商官方提供的 PowerShell 模块来完成。

*   **连接到 Microsoft Azure**
    
    Azure 的 PowerShell 模块被称为 **Az** 模块。它是一个包含了数十个子模块（如 `Az.Compute`, `Az.Storage`, `Az.Network`）的聚合模块，每个子模块对应 Azure 的一类服务。
    
    1.  **安装 Az 模块**: 打开 PowerShell (最好是管理员身份)，从 PowerShell Gallery 安装。
        
        ```powershell
        # 安装 Az 模块（如果从未安装过）
        Install-Module -Name Az -Scope CurrentUser -Repository PSGallery -Force
         
        # 更新 Az 模块（如果已安装）
        Update-Module -Name Az
        ```
        
    2.  **进行身份验证**: `Connect-AzAccount` 是你登录 Azure 的入口。执行它会弹出一个浏览器窗口，让你通过标准的 Microsoft 账户进行登录（支持多因素认证 MFA）。
        
        ```powershell
        # 连接到你的 Azure 账户
        Connect-AzAccount
        ```
        
        登录成功后，PowerShell 会缓存你的凭据。你还可以使用服务主体（Service Principal）或托管身份（Managed Identity）等方式，在自动化脚本中进行非交互式的、更安全的身份验证。
    
*   **连接到 Amazon Web Services (AWS)**
    
    AWS 的 PowerShell 工具同样以模块化的方式提供，称为 **AWS.Tools**。与 Azure 的 Az 模块不同，AWS 提供了更细粒度的模块安装，你可以只安装你需要的服务模块。
    
    1.  **安装 AWS.Tools**:
        
        ```powershell
        # 安装通用的引导模块
        Install-Module -Name AWS.Tools.Installer -Scope CurrentUser
         
        # 使用引导模块来安装所有或特定的服务模块
        # 安装所有可用模块（较大）
        Install-AWSToolsModule -Name AWS.Tools -Force
         
        # 或者，只安装 EC2, S3 和 IAM 服务的模块
        Install-AWSToolsModule -Name "AWS.Tools.EC2", "AWS.Tools.S3", "AWS.Tools.IdentityManagement" -Force
        ```
        
    2.  **配置凭据**: AWS 通常不使用交互式登录。更常见的做法是，在你的计算机上配置一个包含 `Access Key ID` 和 `Secret Access Key` 的凭据文件。你可以通过安装 AWS CLI (命令行界面) 并运行 `aws configure` 来轻松完成此操作。 一旦配置完成，AWS PowerShell 模块会自动读取这些凭据。你可以通过 `Get-AWSCredential` 来验证。
        

##### **7.2.2. 管理 Azure 资源：`Az` 模块实战**

`Az` 模块的 Cmdlet 遵循 `动词-Az名词` 的命名规范，非常直观。

**核心概念**：

*   **资源组 (Resource Group)**：Azure 中所有资源的逻辑容器。任何资源都必须属于一个资源组。
*   **位置 (Location)**：部署资源的地理区域（如 `EastUS`, `WestEurope`）。

**示例：创建一个完整的 Web 应用环境**

```powershell
# 定义通用变量
$resourceGroupName = "MyWebAppResourceGroup"
$location = "EastUS"
$storageAccountName = "mywebappstorage$(Get-Random)" # 存储账户名需全局唯一
$appServicePlanName = "MyWebAppServicePlan"
$webAppName = "MyUniqueWebApp-$(Get-Random)"
 
# 1. 创建一个资源组
New-AzResourceGroup -Name $resourceGroupName -Location $location
 
# 2. 创建一个存储账户 (例如，用于存放日志或静态内容)
New-AzStorageAccount -ResourceGroupName $resourceGroupName -Name $storageAccountName -Location $location -SkuName Standard_LRS
 
# 3. 创建一个应用服务计划 (定义了 Web 应用的计算资源和价格)
New-AzAppServicePlan -ResourceGroupName $resourceGroupName -Name $appServicePlanName -Location $location -Tier "Free"
 
# 4. 创建一个 Web 应用 (App Service)
New-AzWebApp -ResourceGroupName $resourceGroupName -Name $webAppName -Location $location -AppServicePlan $appServicePlanName
 
# 5. 获取新建的 Web 应用信息，特别是其默认域名
$webApp = Get-AzWebApp -ResourceGroupName $resourceGroupName -Name $webAppName
Write-Host "Web 应用已创建，访问地址: https://$($webApp.DefaultHostName )"
```

这个简单的脚本，就以代码的形式，定义并部署了一个包含多个相互关联资源的云端环境。这就是**基础设施即代码 (Infrastructure as Code, IaC)** 的核心思想。

##### **7.2.3. 管理 AWS 资源：`AWS.Tools` 模块实战**

AWS 的 Cmdlet 同样遵循 `动词-服务缩写-名词` 的规范。

**核心概念**：

*   **EC2 (Elastic Compute Cloud)**：虚拟服务器（虚拟机）。
*   **S3 (Simple Storage Service)**：对象存储服务。
*   **VPC (Virtual Private Cloud)**：虚拟网络。

**示例：在 AWS 上启动一个 EC2 实例并管理 S3 存储桶**

```powershell
# 1. 获取最新的 Amazon Linux 2 AMI (Amazon Machine Image) ID
$ami = (Get-EC2Image -Owner "amazon" -Filter @{ Name "name"; Value "amzn2-ami-hvm-*-x86_64-gp2" }).ImageId | Sort-Object -Descending | Select-Object -First 1
 
# 2. 启动一个新的 EC2 实例
$instance = New-EC2Instance -ImageId $ami -InstanceType "t2.micro" -TagSpecification @{ ResourceType="instance"; Tags=@{Name="MyNewInstance"} }
Write-Host "正在启动 EC2 实例: $($instance.InstanceId)"
 
# 等待实例进入运行状态
Wait-EC2InstanceStatus -InstanceId $instance.InstanceId -InstanceStatus 'ok'
Get-EC2Instance -InstanceId $instance.InstanceId | Select-Object InstanceId, PublicDnsName, State
 
# 3. 创建一个新的 S3 存储桶 (Bucket)
$bucketName = "my-unique-test-bucket-$(Get-Random)"
New-S3Bucket -BucketName $bucketName
 
# 4. 上传一个文件到 S3 存储桶
Write-S3Object -BucketName $bucketName -File ".\local-file.txt" -Key "uploaded-file.txt"
 
# 5. 列出存储桶中的对象
Get-S3Object -BucketName $bucketName
```

##### **7.2.4. 实现云资源的自动化部署与报表**

PowerShell 在云管理中的真正威力，体现在自动化的工作流中。

*   **自动化部署**： 你可以将如 7.2.2 中的脚本，整合到一个部署脚本中，并参数化。每次需要部署一套新的测试环境时，只需运行脚本并提供新的环境名即可。这确保了部署的一致性、速度和可重复性。
    
*   **自动化报表**： 云环境的成本和资源使用情况是管理者非常关心的问题。你可以编写 PowerShell 脚本，定时运行，来收集这些信息并生成报告。
    
    **示例：生成一个 Azure 资源成本报表 (简易版)**
    
    ```powershell
    # 获取所有资源组
    $resourceGroups = Get-AzResourceGroup
     
    # 遍历每个资源组，获取其下的所有资源
    $report = foreach ($rg in $resourceGroups) {
        Get-AzResource -ResourceGroupName $rg.ResourceGroupName | ForEach-Object {
            [PSCustomObject]@{
                ResourceName = $_.Name
                ResourceType = $_.ResourceType
                ResourceGroup = $rg.ResourceGroupName
                Location = $_.Location
            }
        }
    }
     
    # 将报告导出为 CSV 文件
    $report | Export-Csv -Path ".\AzureResourceReport.csv" -NoTypeInformation
    ```
    
    (注意：真实的成本分析需要使用更专业的 `Az.Billing` 或 `Az.CostManagement` 模块)
    

* * *

通过本节的学习，我们已经成功地将 PowerShell 的触角，从本地数据中心和单一的操作系统，延伸到了广阔的公有云。我们看到，无论是面对 Azure 还是 AWS，PowerShell 都提供了一套一致、强大、可编程的接口。这使得我们能够用已有的知识和技能，去驾驭这个由代码和 API 定义的云端新世界，实现真正意义上的、跨越本地与云端的统一自动化。

* * *

#### **7.3. API 交互与数据处理**

在前面的小节中，我们学会了如何用 PowerShell 管理本地的 Windows 和 Linux 系统，以及如何与 Azure 和 AWS 这两大云平台进行交互。这些能力之所以能够实现，是因为微软、亚马逊以及 PowerShell 社区为我们提供了封装好的、现成的模块。

但如果我们要管理的系统，是一个没有官方 PowerShell 模块的内部应用？一个智能家居设备？一个社交媒体平台？或者任何一个提供了 Web 服务的第三方系统？难道我们就束手无策了吗？

答案是否定的。只要这个系统提供了一个 **REST API**，PowerShell 就能与它对话。本节，我们将学习如何使用 PowerShell，直接与构成现代互联网基石的 REST API 进行交互。掌握了这项技能，你的 PowerShell 将突破所有模块的限制，真正实现与万物互联。

**API (Application Programming Interface)**，应用程序编程接口，是一套定义了不同软件组件之间如何相互通信的规则和协议。而在 Web 时代，**REST (Representational State Transfer)** 是一种极其流行的、用于构建 Web API 的架构风格。

一个 RESTful API 通常通过标准的 HTTP 方法（如 `GET`, `POST`, `PUT`, `DELETE`）来对资源进行操作，并使用 **JSON (JavaScript Object Notation)** 作为其首选的数据交换格式。

PowerShell 提供了一组强大的原生 Cmdlet，特别是 `Invoke-RestMethod`，让我们能够轻松地作为客户端，与任何 REST API 进行交互。这为 PowerShell 打开了一扇通往无限可能的大门。

##### **7.3.1. 与 REST API 对话：精通 `Invoke-RestMethod`**

`Invoke-RestMethod` 是你与 REST API 对话的核心工具。它会发送一个 HTTP 请求到指定的 URL，然后智能地将服务器返回的 JSON 或 XML 响应，自动地**解析**成一个 PowerShell 自定义对象 (`[PSCustomObject]`)。这免去了我们手动解析文本的繁琐工作，是其最大的魅力所在。

*   **`GET` 请求：获取数据** `GET` 是最常见的 HTTP 方法，用于从服务器检索信息。这是 `Invoke-RestMethod` 的默认方法。
    
    **示例：查询一个公开的 IP 地址信息 API**
    
    ```powershell
    # ip-api.com 是一个提供免费 IP 地理位置查询的 API
    $myIpInfo = Invoke-RestMethod -Uri "http://ip-api.com/json"
     
    # 由于返回的是对象 ，我们可以直接访问其属性
    Write-Host "你所在的城市是: $($myIpInfo.city)"
    Write-Host "你的 ISP 是: $($myIpInfo.isp)"
    $myIpInfo | Format-List
    ```
    
*   **`POST` 请求：创建新资源** `POST` 用于向服务器提交数据，以创建一个新的资源。数据通常放在请求的**主体 (Body)** 中。
    
    **示例：向一个测试 API (jsonplaceholder.typicode.com) 创建一篇新的博客文章**
    
    ```powershell
    $newPost = @{
        title = 'My Awesome PowerShell Post'
        body = 'This post was created automatically using Invoke-RestMethod.'
        userId = 10
    }
     
    # 使用 -Method 指定请求方法
    # 使用 -Body 将 PowerShell 哈希表作为请求主体
    # -ContentType 告诉服务器我们发送的是 JSON 数据
    # Invoke-RestMethod 会自动将哈希表转换为 JSON 字符串
    $createdPost = Invoke-RestMethod -Uri "https://jsonplaceholder.typicode.com/posts" -Method Post -Body $newPost -ContentType "application/json"
     
    # API 会返回创建成功后的对象 ，通常会包含一个由服务器分配的 ID
    Write-Host "成功创建文章，ID 为: $($createdPost.id)"
    ```
    
*   **`PUT` 和 `DELETE` 请求：更新和删除资源**
    
    *   `PUT` 用于更新一个已存在的资源。
    *   `DELETE` 用于删除一个资源。
    
    ```powershell
    # 更新我们刚刚创建的文章 (PUT)
    $updatedData = @{
        id = $createdPost.id
        title = 'My Updated PowerShell Post'
        body = 'The content has been updated.'
        userId = 10
    }
    $updatedResult = Invoke-RestMethod -Uri "https://jsonplaceholder.typicode.com/posts/$($createdPost.id )" -Method Put -Body $updatedData
     
    # 删除这篇文章 (DELETE)
    # 对于 DELETE 请求，通常只关心是否成功，返回的状态码是关键
    $deleteResponse = Invoke-RestMethod -Uri "https://jsonplaceholder.typicode.com/posts/$($createdPost.id )" -Method Delete
    Write-Host "删除操作完成。"
    ```
    
*   **处理认证和请求头 (Headers)** 大多数私有 API 都需要认证。通常是通过在请求的**头信息 (Headers)** 中包含一个认证令牌（如 API Key 或 Bearer Token）来实现的。
    
    ```powershell
    $apiKey = "YourSecretApiKey"
    $headers = @{
        "X-Api-Key" = $apiKey
    }
    $secureData = Invoke-RestMethod -Uri "https://api.my-secure-service.com/data" -Headers $headers
    ```
    

##### **7.3.2. 处理 JSON 数据：PowerShell 的“母语”**

JSON 是当今 API 世界的通用语言 ，而 PowerShell 对其有着近乎原生的支持。

*   **`ConvertFrom-Json`**：将一个 JSON 格式的**字符串**，转换成一个 PowerShell 自定义对象。`Invoke-RestMethod` 在内部已经为我们自动完成了这一步。但如果我们从文件或其他来源获得了一个 JSON 字符串，这个 Cmdlet 就非常有用。
    
    ```powershell
    $jsonString = '{"name":"John Doe","age":30,"isStudent":false,"courses":[{"name":"History"},{"name":"Math"}]}'
    $personObject = $jsonString | ConvertFrom-Json
    $personObject.name # 输出 John Doe
    $personObject.courses[0].name # 输出 History
    ```
    
*   **`ConvertTo-Json`**：将一个 PowerShell 对象（或对象集合），转换成一个 JSON 格式的**字符串**。这在构建 `POST` 或 `PUT` 请求的 Body 时非常有用，尽管 `Invoke-RestMethod` 通常会自动帮我们转换。
    
    ```powershell
    $myObject = [PSCustomObject]@{
        ComputerName = $env:COMPUTERNAME
        TimeStamp = Get-Date
    }
    $jsonObjectString = $myObject | ConvertTo-Json
    # $jsonObjectString 的内容会是:
    # {
    #   "ComputerName": "YOUR_PC_NAME",
    #   "TimeStamp": "2023-10-27T10:30:00.1234567+08:00"
    # }
    ```
    

##### **7.3.3. 解析 XML 数据：与传统系统对话**

虽然 JSON 是主流，但许多企业内部的、传统的 Web 服务（特别是基于 SOAP 的）仍然使用 **XML (eXtensible Markup Language)** 作为数据格式。PowerShell 同样能优雅地处理 XML。

只需将一个包含 XML 的字符串，用 `[xml]` 类型加速器进行转换，它就会变成一个可导航的 `System.Xml.XmlDocument` 对象。

```powershell
$xmlString = @"
<books>
  <book id="bk101">
    <author>Gambardella, Matthew</author>
    <title>XML Developer's Guide</title>
    <genre>Computer</genre>
    <price>44.95</price>
  </book>
  <book id="bk102">
    <author>Ralls, Kim</author>
    <title>Midnight Rain</title>
    <genre>Fantasy</genre>
    <price>5.95</price>
  </book>
</books>
"@
 
# 将字符串转换为 XML 对象
[xml]$xmlDoc = $xmlString
 
# 现在可以通过点号 (.) 来导航 XML 结构
$xmlDoc.books.book[0].title # 输出 XML Developer's Guide
$xmlDoc.books.book[1].author # 输出 Ralls, Kim
```

##### **7.3.4. Web 抓取入门：`Invoke-WebRequest` 解析 HTML**

有时候，我们需要交互的网站并没有提供一个结构化的 API，而只有一个供人类阅读的 HTML 网页。在这种情况下，我们可以进行**Web 抓取（Web Scraping）**，即通过程序解析 HTML 页面的内容，从中提取我们需要的信息。

`Invoke-WebRequest` 是为此设计的工具。与 `Invoke-RestMethod` 不同，它返回一个包含了页面所有元素的、非常丰富的“网页对象”。

**示例：获取 PowerShell 在 GitHub 上的最新 Release 版本号**

```powershell
# -UseBasicParsing 是一个常用参数，可以避免对 Internet Explorer 引擎的依赖
$response = Invoke-WebRequest -Uri "https://github.com/PowerShell/PowerShell/releases/latest" -UseBasicParsing
 
# 返回的对象有一个 ParsedHtml 属性 ，它是一个可以交互的 HTML DOM 对象
# 我们可以像在浏览器开发者工具中一样，通过 getElementById 等方法来查找元素
# (注意：这种方式很脆弱，一旦网页结构改变，脚本就会失效)
 
# 一个更稳健的方式是直接分析返回的链接
# 最新 Release 页面的 URL 会重定向到具体的版本页面
$latestVersionUri = $response.BaseResponse.ResponseUri.AbsoluteUri
$latestVersion = $latestVersionUri.Split('/')[-1] # 从 URL 中提取版本号
 
Write-Host "PowerShell 的最新 Release 版本是: $latestVersion"
```

Web 抓取是一项强大但需要谨慎使用的技术。它非常依赖于目标网页的 HTML 结构，一旦网站改版，脚本就可能失效。如果目标网站提供了 API，应始终优先使用 API。

至此，我们已经为 PowerShell 插上了飞向任意远方的翅膀。通过掌握与 REST API 的交互，我们不再受限于任何特定的平台或模块。整个互联网，只要它以服务的方式开放其功能，就都可以成为 PowerShell 自动化的疆域。这是一种终极的、面向未来的集成能力，它确保了你的 PowerShell 技能，在快速变化的技术浪潮中，永远不会过时。

* * *