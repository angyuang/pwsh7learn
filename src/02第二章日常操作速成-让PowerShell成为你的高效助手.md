### **第二章：日常操作速成——让 PowerShell 成为你的高效助手**



欢迎来到旅程的第二站。在第一章中，我们已经与 PowerShell 初次见面，理解了它的核心思想。现在，是时候将这些理念付诸实践，解决我们日常工作中那些最常见、最基础的任务了。

本章的目标是让你“用起来，快起来”。我们将聚焦于文件系统的管理、系统信息的查询以及应用程序的控制。你将发现，许多过去需要通过鼠标多次点击才能完成的操作，如今只需一行简洁的命令即可实现。更重要的是，你将开始真正领会到 PowerShell “动词-名词”命名法的一致性之美，并初步接触到“管道”和“别名”这两个能让你效率倍增的强大武器。

请将本章视为你的“PowerShell 日常生活指南”。学完本章，你将有足够的能力，将 PowerShell 无缝融入到你的每日工作流中，让它成为你名副其实的数字管家和高效助手。

* * *

#### **2.1. 文件与目录操作：你的数字世界管家**

计算机中最基本、最核心的元素莫过于文件和目录（文件夹）。它们是你所有数字资产的载体。因此，高效地管理它们，是掌握任何一个命令行环境的必修课。PowerShell 提供了一套设计优雅、功能强大且高度一致的 Cmdlet，让你能够像一位经验丰富的管家一样，精准、利落地整理你的数字家园。

##### **2.1.1. 查看与导航：在文件系统的版图上自由穿梭**

在开始任何操作之前，我们首先需要知道“我在哪里？”以及“如何去往别处？”。这就像在城市中漫步，你需要知道当前的街道名，并能够顺利走到下一个目的地。

*   **你在何处？—— `Get-Location`**
    
    想知道当前所在的目录路径吗？`Get-Location` 命令会告诉你答案。
    
    ```powershell
    Get-Location
    ```
    
    执行后，它会返回一个包含当前路径信息的对象。在默认显示下，你将直接看到 `Path` 属性，也就是你熟悉的路径字符串，例如 `C:\Users\YourName` 或 `/home/yourname`。
    
    **小贴士：别名 `pwd`** 对于熟悉 Linux/macOS `pwd` (Print Working Directory) 命令的朋友，PowerShell 提供了一个完全相同的别名。输入 `pwd`，你会得到和 `Get-Location` 一样的结果。别名是命令的“小名”，我们稍后会深入探讨。
    
*   **前往目的地 —— `Set-Location`**
    
    知道了当前位置，下一步就是移动。`Set-Location` 负责改变你当前所在的目录。
    
    *   **绝对路径**：提供一个从根目录开始的完整路径。 
        
        ```powershell
        #Windows
        Set-Location -Path "C:\Windows\System32"
         
        #Linux/macOS
        Set-Location -Path "/var/log"
        
    *   **相对路径**：基于当前位置进行移动。
        
        *   `.` (一个点) 代表当前目录。
        *   `..` (两个点) 代表上一级（父）目录。
        
        ```powershell
        # 进入当前目录下的 "Documents" 文件夹
        Set-Location -Path ".\Documents" 
        # 返回上一级目录Set-Location -Path ".."
        ```
        
    
    **小贴士：别名 `cd`** `Set-Location` 的功能与传统命令行中的 `cd` (Change Directory) 命令完全一致。为了方便老用户，PowerShell 同样内置了 `cd` 这个别名。因此，你可以更快捷地输入：
    
    ```powershell
    cd C:\Users
    cd ..
    ```
    
    这可能是你日常使用频率最高的命令之一。
    

##### **2.1.2. 列出文件和目录：`Get-ChildItem` 的世界**

当你进入一个房间（目录），你自然想看看里面都有什么。`Get-ChildItem` 就是你的“透视眼镜”，用于列出指定位置下的所有子项（文件和目录）。

*   **基本用法**
    
    不带任何参数，它会列出当前目录下的所有内容。
    
    ```powershell
    Get-ChildItem
    ```
    
    你会看到一个类似文件资源管理器的列表，包含了模式（Mode）、最后写入时间（LastWriteTime）、大小（Length）和名称（Name）。
    
    **小贴士：别名 `ls` 和 `dir`** 为了照顾所有背景的用户，PowerShell 非常贴心地为 `Get-ChildItem` 提供了两个广为人知的别名：Linux/macOS 用户熟悉的 `ls` 和 Windows 用户熟悉的 `dir`。它们用起来完全一样：
    
    ```powershell
    ls
    dir C:\
    ```
    
*   **高级用法：精准探索**
    
    `Get-ChildItem` 的强大远不止于此，它丰富的参数让你可以进行精细的控制：
    
    *   **`-Path`**：指定要查看的路径，而非当前路径。 
        
        ```powershell
        Get-ChildItem -Path "C:\Program Files"
        
    *   **`-Recurse`**：进行递归查找，深入所有子目录，列出所有后代项。 
        
        ```powershell
        # 找出 C:\Windows 目录下及所有子目录下所有的 .log 文件
        Get-ChildItem -Path "C:\Windows" -Filter "*.log" -Recurse
        
    *   **`-Filter`**：使用提供程序支持的筛选器（通常更快），用于简单的模式匹配。
    *   **`-Include` / `-Exclude`**：包含或排除符合特定模式的项，通常与 `-Recurse` 配合使用。
    *   **`-File` / `-Directory`**：只显示文件或只显示目录（PowerShell 4.0+）。 
        
        ```powershell
        # 只看当前目录下的文件夹
        Get-ChildItem -Directory

##### **2.1.3. 创建新天地：`New-Item` 的创造之力**

探索完已有世界，我们来学习创造。`New-Item` 是一个通用的创建命令，可以用来创建文件、目录，甚至是注册表项。

*   **创建新目录**
    
    使用 `-ItemType Directory` 参数来指明你要创建的是一个目录。
    
    ```powershell
    # 在当前位置创建一个名为 "MyScripts" 的新文件夹
    New-Item -Path ".\MyScripts" -ItemType Directory
    ```
    
    **小贴士：别名 `mkdir`** 同样，为了方便，你可以使用大家熟悉的 `mkdir` 别名：
    
    ```powershell
    mkdir MyProject
    ```
    
*   **创建空文件**
    
    使用 `-ItemType File` 参数。
    
    ```powershell
    # 在 MyScripts 文件夹下创建一个名为 "FirstScript.ps1" 的空文件
    New-Item -Path ".\MyScripts\FirstScript.ps1" -ItemType File
    ```
    

##### **2.1.4. 复制与移动：`Copy-Item` 和 `Move-Item` 的灵活运用**

整理文件离不开复制和移动。PowerShell 为此提供了 `Copy-Item` 和 `Move-Item` 两个直观的 Cmdlet。

*   **复制文件或目录：`Copy-Item`**
    
    ```powershell
    # 将 FirstScript.ps1 复制到 "Backup" 目录
    Copy-Item -Path ".\MyScripts\FirstScript.ps1" -Destination ".\Backup"
     
    # 复制整个 MyScripts 目录及其内容到 Backup 目录
    Copy-Item -Path ".\MyScripts" -Destination ".\Backup" -Recurse
    ```
    
    **小贴士：别名 `cp` 或 `copy`** 你可以使用 `cp` (源自 Linux) 或 `copy` (源自 CMD) 作为 `Copy-Item` 的别名。
    
*   **移动或重命名：`Move-Item`**
    
    `Move-Item` 的用法与 `Copy-Item` 几乎完全相同，只是它执行的是“剪切-粘贴”操作。
    
    ```powershell
    # 将 FirstScript.ps1 移动到 Archive 目录
    Move-Item -Path ".\MyScripts\FirstScript.ps1" -Destination ".\Archive"
    ```
    
    一个有趣的用途是**重命名**。当 `-Destination` 是同一个目录下 的一个新名字时，`Move-Item` 的效果就是重命名。
    
    ```powershell
    # 将 FirstScript.ps1 重命名为 MainScript.ps1
    Move-Item -Path ".\MyScripts\FirstScript.ps1" -Destination ".\MyScripts\MainScript.ps1"
    ```
    
    **小贴士：别名 `mv` 或 `move`** `mv` (源自 Linux) 和 `move` (源自 CMD) 是 `Move-Item` 的常用别名。
    

##### **2.1.5. 查看文件内容：`Get-Content` 的目光**

想快速查看一个文本文件的内容，而不想打开重量级的编辑器？`Get-Content` 为你效劳。

```powershell
# 查看 FirstScript.ps1 的内容
Get-Content -Path ".\MyScripts\MainScript.ps1"
 
# 查看文件的前 10 行
Get-Content -Path "C:\Windows\System32\drivers\etc\hosts" -TotalCount 10
```

**小贴士：别名 `cat` 或 `type`** `cat` (源自 Linux) 和 `type` (源自 CMD) 都是 `Get-Content` 的便捷别名。

##### **2.1.6. 删除操作：`Remove-Item` 的决断**

整理的最后一步，往往是清理不再需要的东西。`Remove-Item` 负责执行删除操作。

**请务必小心使用此命令！**

```powershell
# 删除一个文件
Remove-Item -Path ".\MyScripts\MainScript.ps1"
 
# 删除一个空目录
Remove-Item -Path ".\MyScripts"
 
# 删除一个非空目录及其所有内容（需要 -Recurse）
Remove-Item -Path ".\Backup" -Recurse
```

*   **安全第一：`-WhatIf` 和 `-Confirm`**
    
    由于删除是危险操作，PowerShell 提供了两个非常有用的“通用参数”来防止意外：
    
    *   **`-WhatIf`**：模拟执行。它不会真的删除任何东西，而是告诉你“如果执行此命令，将会发生什么”。这是执行破坏性操作前极好的预览工具。 
        
        ```powershell
        Remove-Item -Path ".\Backup" -Recurse -WhatIf
        
    *   **`-Confirm`**：执行前提示。在删除每一个项目前，它都会停下来询问你是否确认。 
        
        ```powershell
        Remove-Item -Path ".\Backup" -Recurse -Confirm

**小贴士：别名 `rm` 或 `del`** `rm` (源自 Linux) 和 `del` (源自 CMD) 是 `Remove-Item` 的别名。

* * *

至此，你已经掌握了文件与目录管理的“六脉神剑”：`Get-Location`, `Set-Location`, `Get-ChildItem`, `New-Item`, `Copy-Item/Move-Item`, `Get-Content`, 和 `Remove-Item`。你会发现，它们都严格遵循着 `动词-名词` 的命名法，并且通过别名照顾了不同背景用户的使用习惯。

花点时间在你的终端里练习这些命令吧。熟练地掌握它们，你就能在文件系统的世界里信步闲庭，游刃有余。接下来，我们将把目光从文件转向更宏观的系统层面。

#### **2.2. 系统与应用管理：掌控你的计算机**

如果说文件与目录管理是操作系统的“内政”，那么对系统本身状态的洞察和应用程序的控制，则更像是“运筹帷幄”。PowerShell 赋予了用户超越图形界面的能力，可以直接与系统的核心组件对话，查询硬件状态、启动应用、诊断网络，乃至控制计算机的电源。本节将引导读者掌握这些关键技能，将 PowerShell 的应用范围从文件管理扩展到更宏观的系统控制层面。

##### **2.2.1. 磁盘管理：洞察存储全局**

数据是数字世界的基石，而磁盘是承载数据的物理载体。清晰地了解磁盘配置与使用状况，是系统管理的首要任务之一。

*   **检视物理磁盘：`Get-Disk`**
    
    `Get-Disk` Cmdlet 用于展示连接到本机的物理存储设备信息，无论是传统的机械硬盘（HDD）、固态硬盘（SSD），还是可移动的 USB 驱动器。
    
    ```powershell
    Get-Disk
    ```
    
    执行此命令，将返回一个包含所有物理磁盘的列表。输出信息十分关键，通常包括：
    
    *   `Number`：系统分配的磁盘编号。
    *   `FriendlyName`：易于识别的磁盘型号或名称。
    *   `HealthStatus`：磁盘的健康状况，如 "Healthy"。
    *   `Size`：磁盘的总容量。
    
    此命令对于快速盘点系统硬件、检查新加硬盘是否被识别等场景，极为高效。
    
*   **查看逻辑驱动器：`Get-PSDrive`**
    
    用户在日常操作中更常接触的是逻辑驱动器，即我们熟悉的 C:、D: 等盘符。`Get-PSDrive` 不仅能够列出这些基于文件系统的驱动器，还能显示 PowerShell 内部定义的其他“虚拟驱动器”，如用于访问注册表的 `HKLM:`。
    
    ```powershell
    Get-PSDrive
    ```
    
    为了专注于磁盘存储，可以利用 `-PSProvider` 参数进行过滤，只显示文件系统类型的驱动器。
    
    ```powershell
    Get-PSDrive -PSProvider FileSystem
    ```
    
    其结果会清晰地展示每个驱动器的已用空间（Used）和可用空间（Free），是监控磁盘容量最直接的方式。
    

##### **2.2.2. 启动应用程序：命令驱动的执行力**

PowerShell 提供了多种启动应用程序的方式，从便捷的快速调用到精确的过程控制，满足不同场景的需求。

*   **直接调用：最高效的日常方式**
    
    对于已经包含在系统 `Path` 环境变量中的可执行程序，最快捷的启动方式就是直接输入其程序名。
    
    ```powershell
    # 启动记事本
    notepad
     
    # 启动计算器
    calc
     
    # 启动 Visual Studio Code (若安装时已配置好路径)
    code
    ```
    
    这种方式同样支持传递命令行参数，例如使用记事本直接打开一个特定文件：
    
    ```powershell
    notepad "C:\Users\Public\Documents\readme.txt"
    ```
    
*   **精细化控制：`Start-Process`**
    
    当需要对程序的启动过程进行更多控制时，应使用 `Start-Process` Cmdlet。这是一个功能更丰富的“启动器”。
    
    ```powershell
    # 等同于直接调用
    Start-Process -FilePath "notepad.exe"
     
    # 以管理员权限运行程序 (系统将弹出 UAC 提权确认框)
    Start-Process -FilePath "cmd.exe" -Verb RunAs
     
    # 使用默认浏览器打开一个网页
    Start-Process -FilePath "https://docs.microsoft.com/powershell/"
    ```
    
    `Start-Process` 的价值在于其丰富的参数集 ，它允许脚本在启动程序时指定窗口样式、传递复杂参数、等待进程结束后再继续执行等，这在自动化工作流中是不可或缺的。
    

##### **2.2.3. 网络诊断：感知系统的外部连接**

在互联互通的时代，网络连接的健康状况至关重要。PowerShell 内置了强大的网络诊断工具，能够帮助用户快速定位网络问题。

*   **基础连通性测试：`Test-Connection`**
    
    `Test-Connection` 是传统 `ping` 工具的 PowerShell 升级版。它不仅能完成 ICMP Echo 请求来测试主机是否可达，更重要的是，它的输出是结构化的对象，而非纯文本。
    
    ```powershell
    # 测试到公共 DNS 服务器 8.8.8.8 的连通性
    Test-Connection -ComputerName "8.8.8.8"
    ```
    
    **别名 `ping`**：为了方便，可以直接使用 `ping` 这个别名来调用 `Test-Connection`。
    
    ```powershell
    ping www.microsoft.com
    ```
    
    由于其输出是对象，因此可以轻易地在脚本中获取如响应时间（ResponseTime）、目标 IP 地址（IPV4Address）等信息用于逻辑判断。
    
*   **高级网络探测：`Test-NetConnection`**
    
    `Test-NetConnection` (PowerShell 4.0+ 中引入) 是一个功能更全面的网络诊断利器。它除了能执行 `ping` 测试外，还能检查特定 TCP 端口的连通性，这对于判断服务是否正常运行、防火墙是否阻断等问题至关重要。
    
    ```powershell
    # 执行一个包含路由追踪和 ping 的综合测试
    Test-NetConnection -ComputerName "github.com"
     
    # 专门测试远程主机 443 (HTTPS) 端口是否开放
    Test-NetConnection -ComputerName "github.com" -Port 443
    ```
    
    当进行端口测试时，返回结果中的 `TcpTestSucceeded` 属性（`True` 或 `False`）直接告知了端口的开放状态，是网络故障排查的得力助手。
    

##### **2.2.4. 系统开关：安全地控制电源状态**

无论是自动化脚本的收尾工作，还是远程服务器的维护，安全地关闭或重启计算机都是一项基本而严肃的操作。

**警告：**  以下命令将直接影响目标计算机的运行状态。请务必在确认无误的环境下执行，或在测试时使用 `-WhatIf` 参数。

*   **关闭计算机：`Stop-Computer`**
    
    ```powershell
    # 关闭本地计算机。命令会立即生效。
    Stop-Computer
    ```
    
*   **重启计算机：`Restart-Computer`**
    
    ```powershell
    # 重启本地计算机。
    Restart-Computer
    ```
    
*   **远程控制与安全措施**
    
    这两个 Cmdlet 的真正威力体现在远程管理上。通过 `-ComputerName` 参数，可以同时操作一台或多台远程计算机，前提是执行者拥有足够的权限且目标计算机已开启 PowerShell 远程管理。
    
    ```powershell
    # 同时重启名为 WEB-SRV01 和 DB-SRV01 的两台服务器
    Restart-Computer -ComputerName "WEB-SRV01", "DB-SRV01"
    ```
    
    **安全阀：`-WhatIf` 与 `-Confirm`** 对于这类具有破坏性的操作，PowerShell 提供了两个至关重要的通用参数：
    
    *   `-WhatIf`：预演参数。命令不会实际执行，但会详细报告如果执行将会发生什么。
    *   `-Confirm`：确认参数。在执行敏感操作前，系统会暂停并请求用户确认。
    
    在执行任何可能中断服务的操作前，使用 `Restart-Computer -WhatIf` 进行预演，是一种值得推广的最佳实践。
    

#### **2.3. 核心基石：理解 PowerShell 的内在逻辑**

到目前为止，我们已经学习了许多实用的命令，并感受到了它们的便捷。但要真正发挥 PowerShell 的威力，释放其全部潜能，我们必须理解其优雅设计背后的核心思想。本节将深入 PowerShell 的“灵魂”——对象（Object）、管道（Pipeline）和别名（Alias）。这三大基石共同构成了 PowerShell 高效、连贯且极具扩展性的内在逻辑。掌握它们，是从“使用”PowerShell 到“精通”PowerShell 的关键一步。

##### **2.3.1. 万物皆对象：通过 `Get-Member` 初探奥秘**

我们在第一章曾提到，PowerShell 最具革命性的特点是它在命令之间传递的是“对象”，而非纯文本。现在，让我们用一个强大的“探查”工具——`Get-Member`——来亲眼验证并深入理解这一点。

`Get-Member`（别名 `gm`）像一台功能强大的X光机，可以透视任何一个从管道传递给它的对象，并将其内部的“骨架”——即它的类型、属性和方法——清晰地展示出来。

让我们以熟悉的 `Get-Process` 为例。首先，执行 `Get-Process`，我们看到的是一个格式化的进程列表：

```powershell
Get-Process
```

这看起来是文本，但它真的是吗？现在，让我们将 `Get-Process` 的输出通过**管道**（`|`，我们马上会讲到）送给 `Get-Member`：

```powershell
Get-Process | Get-Member
```

执行后，屏幕上出现的不再是进程列表，而是一份关于“进程对象”的详细技术报告。让我们来解读这份报告的关键信息：

*   **TypeName**: `System.Diagnostics.Process` 这行告诉我们，`Get-Process` 输出的对象的“真实身份”或“类型”是 `.NET` 世界里的 `System.Diagnostics.Process` 类。这证明了我们处理的确实是一个标准化的、定义明确的数据结构，而非随意组织的文本。
    
*   **Members (成员)**：接下来是一个长长的列表，列出了这个类型的所有成员。成员主要分为两类：
    
    *   **Property (属性)**：这是对象的静态特征或数据。你会看到许多熟悉的名字，比如 `Name`, `Id`, `CPU`, `WorkingSet64` (内存占用)等。它们就像对象的“名词”或“标签”，存储着关于对象的具体信息。这就是为什么 `Get-Process` 的默认表格能显示这些列的原因——它只是挑选了几个重要的属性来展示。
    *   **Method (方法)**：这是对象能够执行的**动作**。你会看到像 `Kill()` (终止进程)、`Refresh()` (刷新信息)、`Start()` (启动)等。它们就像对象的“动词”，定义了我们可以对这个对象做什么。

**“万物皆对象”的启示：**  这个简单的实验揭示了 PowerShell 的核心秘密：**所见非所得**。屏幕上美观的表格只是 PowerShell 为了方便阅读而对对象进行“格式化”后的样子。在幕后，流动的是包含了丰富数据（属性）和内置功能（方法）的完整对象。

这意味着，我们不再需要用复杂的文本解析去“猜”进程名在哪一列，而是可以直接、精确地访问对象的属性，如 `(Get-Process -Name "notepad").CPU`。这种从处理不确定的“文本”到操作确定的“对象”的转变，是 PowerShell 效率和可靠性的根源。

##### **2.3.2. 管道（Pipeline）的艺术：用 `|` 连接命令**

现在我们来正式介绍刚才已经用过的 `|` 符号。它就是**管道（Pipeline）**，PowerShell 中最强大、最核心的“胶水”。管道的作用，是将一个命令的输出，直接作为下一个命令的输入。

在传统 Shell 中，管道传递的是文本流。而在 PowerShell 中，管道传递的是**对象流**，并且对象在传递过程中**不会丢失其类型和结构**。这使得命令之间的协作达到了前所未有的高度。

**管道的工作流程：** 

想象一条自动化生产线：

1.  `Get-Process` (第一道工序) 生产出许多“进程对象”，并将它们一个个放到传送带（管道）上。
2.  传送带将这些完整的对象，原封不动地送到下一道工序。
3.  `Where-Object` (第二道工序，一个过滤器) 检查每个传送过来的对象，只留下符合条件的（例如，内存占用大于100MB的）。
4.  传送带继续将筛选后的对象送到 `Sort-Object` (第三道工序，一个排序器)。
5.  `Sort-Object` 按照指定的属性（例如，按CPU使用率）对传送过来的对象进行排序。
6.  最后，排序后的对象被送到 `Select-Object` (第四道工序)，它从每个对象中挑选出我们关心的几个属性，打包成一个新的、更简洁的对象，最终展示在屏幕上。

将这个流程写成 PowerShell 命令，就是一行优雅的“管道链”：

```powershell
# 获取所有进程，筛选出工作集大于100MB的，按CPU使用降序排序，最后只显示名称和CPU
Get-Process | Where-Object { $_.WorkingSet64 -gt 100MB } | Sort-Object -Property CPU -Descending | Select-Object -Property ProcessName, CPU
```

这行命令完美地诠释了 PowerShell 的哲学：**每个工具只做一件事，并把它做到最好。然后用管道将它们连接起来，共同完成复杂的任务。**  这种“乐高积木式”的组合能力，是 PowerShell 强大生产力的核心源泉。

##### **2.3.3. 别名（Alias）：发现命令的“小名”，提高效率**

在前面的学习中，我们已经多次提到 `cd`, `ls`, `ping` 等“小名”。这些就是**别名（Alias）**。别名是现有 Cmdlet 的一个替代名称或昵称，它的存在主要是为了：

1.  **提高效率**：输入 `ls` 显然比输入 `Get-ChildItem` 要快得多。
2.  **兼容并包**：为来自不同命令行背景（如 `CMD` 或 `bash`）的用户提供熟悉的环境，降低迁移成本。`dir`, `cd`, `cat`, `rm` 等别名的存在，让老用户倍感亲切。

**探索别名：`Get-Alias`**

想知道一个别名对应的是哪个“大名”（Cmdlet）？或者想看看系统里都有哪些别名？`Get-Alias` 可以帮你。

```powershell
# 查看别名 ls 对应的原始命令
Get-Alias -Name ls
 
# 查看原始命令 Get-ChildItem 有哪些别名
Get-Alias -Definition Get-ChildItem
 
# 列出系统中所有的别名
Get-Alias
```

**创建自己的别名：`New-Alias`**

你甚至可以为你自己常用的长命令创建别名。例如，如果你经常需要查看帮助示例：

```powershell
# 为 Get-Help -Examples 创建一个名为 "ghe" 的别名
New-Alias -Name ghe -Value "Get-Help -Examples"
 
# 现在你可以这样用了：
ghe Get-Process
```

**重要提示**：虽然别名在交互式使用时非常方便，但在编写需要分享、重用或长期维护的**脚本**时，**强烈建议使用 Cmdlet 的全名**。因为全名具有自解释性，更清晰，可读性更高，能让其他（以及未来的你）更容易理解脚本的意图。

* * *

至此，我们已经探明了 PowerShell 的三大核心基石。**对象**是其信息的载体，**管道**是其流动的血脉，而**别名**则是其便捷的交互界面。理解了这三者如何协同工作，你就真正掌握了 PowerShell 的思维方式。带着这份全新的理解，你将发现，后续的学习会变得豁然开朗。

* * *
