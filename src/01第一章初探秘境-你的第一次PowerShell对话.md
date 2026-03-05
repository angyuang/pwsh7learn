

**第一篇：启航——初识 PowerShell 的世界 (入门篇)**
-----------------------------------

### **第一章：初探秘境——你的第一次 PowerShell 对话**



欢迎你，亲爱的读者，踏上这段注定不凡的旅程。在你的指尖之下，一个强大而优雅的世界正缓缓拉开序幕。它，就是 PowerShell。或许你曾听闻它的威名，或许你对它还一无所知，但这都无关紧要。因为从这一刻起，你将亲手揭开它的神秘面纱，学会与计算机进行一场前所未有的、深度而高效的对话。

本章将是你探索的起点。我们将像初生的孩童认识世界一样，从最基本的问题“它是什么？”开始，为你搭建一个专属的“数字实验室”，并引导你发出第一声响亮的问候——“你好，世界！”。请放松心情，带着好奇与期待，让我们一同启航。

* * *

#### **1.1. 揭开神秘面纱：什么是 PowerShell？**

在数字世界的版图上，PowerShell 如同一座新发现的大陆，既熟悉又新奇。它究竟是什么？简单来说，PowerShell 是由微软设计的一款任务自动化和配置管理框架。但这个定义过于学术，远不足以描绘其万分之一的魅力。让我们用更生动的方式来理解它。

##### **1.1.1. 不仅仅是命令行：它是一个 Shell，更是一门脚本语言**

想象一下，你想让你的计算机为你做一件事，比如“告诉我今天几号了？”。你如何与它沟通？图形界面（GUI）是一种方式，你点击时钟图标。而另一种更直接、更强大的方式，就是通过一个“命令行界面”（Command\-Line Interface, CLI）。PowerShell，首先就是这样一个界面，一个现代化的 **Shell**。

 **Shell 的职责：与操作系统对话的桥梁**

**命令解释器**：Shell 是你与操作系统内核之间不可或缺的翻译官。当你用它能听懂的语言（即命令）下达指令时，Shell 会精准地将你的意图传达给操作系统，并把执行结果反馈给你。无论是古老的 `cmd.exe` 还是经典的 `bash`，它们都扮演着同样的角色。PowerShell 在此基础上，构建了一个更智能、更友好的交互环境。

**交互式体验**：打开 PowerShell 窗口，你会看到一个闪烁的光标，它在静静地等待。这便是它的交互式模式。你输入一行命令，回车，几乎在瞬间，结果就会呈现在眼前。这种“即问即答”的模式，让你能迅速验证想法，探索系统状态，获得即时满足感。    

**脚本语言的魅力：自动化任务的魔法棒**

*   如果说交互式命令是与计算机的即兴对话，那么脚本就是为它谱写的一首可以反复演奏的乐章。PowerShell 不仅仅满足于一次性的命令执行，它更是一门功能完备的**脚本语言**。
    
    **从命令到脚本**：当你发现每天都在重复执行一系列相同的命令来完成某项任务时（例如，备份文件、生成报表），你就可以将这些命令按照顺序写入一个以 `.ps1` 为扩展名的文本文件中。这个文件，就是一个 PowerShell 脚本。未来，你只需运行这一个脚本，所有步骤便会自动完成。
    
    **逻辑与控制**：作为一门语言，PowerShell 提供了变量（用于存储信息）、循环（用于重复执行）、条件判断（用于根据不同情况做出决策）等核心编程构件。这意味着你的脚本不再是简单的命令堆砌，而是拥有了“思考”和“决策”的能力，能够应对复杂的、动态变化的场景。
    
    **可重用性与一致性**：脚本化最大的恩惠在于，它将人类的智慧和经验固化为可执行的代码。这确保了无论何时、何地、由何人执行，任务都能以完全相同的方式被精确处理，彻底告别了手动操作中可能出现的遗漏和错误。这，就是自动化的核心价值。
    

##### **1.1.2. 对象（Object）的革命：告别纯文本，拥抱结构化数据**

这是 PowerShell 最具革命性的特质，也是它与所有传统 Shell 分道扬镳的根本所在。理解了“对象”，你就掌握了解锁 PowerShell 全部潜能的钥匙。

*   **传统 Shell 的困境：在文本的海洋中挣扎**
    
    在 PowerShell 出现之前，命令行工具的世界是纯文本的。当一个命令（如 `ls` 或 `dir`）执行后，它输出的是一连串的字符。如果你想从这些字符中提取有用的信息——比如文件名、修改日期或文件大小——你必须像一位在沙滩上寻找特定沙砾的探险家，使用 `grep`, `awk`, `sed` 等工具，编写复杂的正则表达式，小心翼翼地进行切割、匹配和筛选。
    
    **脆弱的约定**：这种方式极度依赖于输出文本的格式。开发人员对格式的任何微小改动，比如在日期和文件名之间多加了一个空格，都可能导致下游所有依赖于此格式的脚本瞬间崩溃。这是一种基于脆弱“屏幕抓取”的协作，效率低下且极不可靠。
    
*   **PowerShell 的创举：万物皆为对象**
    
    PowerShell 彻底颠覆了这一模式。它宣告：**在我的世界里，流动的不再是文本，而是结构化的对象（Object）！**
    
    **结构化的数据流**：当你执行一个 PowerShell 命令（在 PowerShell 中称为 **Cmdlet**），例如 `Get-Process`，它返回的不是一堆描述进程的文字，而是一个个活生生的“进程对象”的集合。每一个对象都像一个精心打包的包裹，里面整齐地存放着关于这个进程的所有信息：它的名字（`ProcessName`）、ID（`Id`）、CPU占用（`CPU`）、内存使用（`WorkingSet`）等等。这些信息被称为对象的**属性（Properties）**。此外，对象还可能包含你可以对其执行的操作，称为**方法（Methods）**，比如终止进程的方法。
    
    **精准操作**：有了对象，一切都变得简单而精确。想知道所有进程的名称？只需告诉 PowerShell：“嘿，把所有进程对象拿过来，我只对它们的 `ProcessName` 属性感兴趣。” 你不再需要猜测名称在哪一列、从第几个字符开始。这种直接访问属性的方式，稳健、可靠且优雅。
    
    **管道中的对象传递**：PowerShell 的管道（`|`）也因此变得魔力十足。当对象在管道中从一个命令流向下一个命令时，它的完整结构和所有信息都被原封不动地保留下来。这意味着，管道的下游命令接收到的是一个完整的、富含信息的数据包，而非一堆需要费力解析的乱码。这种基于对象的协作，使得将简单命令组合成复杂工具链成为一种享受。
    

##### **1.1.3. 跨平台的统一体验：Windows, Linux, macOS 的无缝衔接**

PowerShell 的雄心并未止步于 Windows。它早已挣脱平台的束缚，演变成一个真正无处不在的管理工具。

*   **历史的演进：从 Windows PowerShell 到 PowerShell 7+**
    
    **Windows PowerShell (版本 5.1 及以下)**：最初，PowerShell 是作为 Windows 操作系统的一部分诞生的，它深度集成于系统中，基于强大的 .NET Framework，是 Windows 管理员的得力助手。这便是我们通常所说的“Windows PowerShell”。
    
    **PowerShell Core (版本 6) 的诞生**：随着云计算和开源浪潮的兴起，微软做出了一个历史性的决定：将 PowerShell 开源，并基于跨平台的 .NET Core 重写。这催生了 PowerShell Core 6，它首次让 PowerShell 的强大能力登陆到了 Linux 和 macOS 的土地上。
    
    **现代的 PowerShell (版本 7+)**：PowerShell 7 是一个里程碑，它整合了 Windows PowerShell 的兼容性与 PowerShell Core 的跨平台能力，标志着 PowerShell 的正式统一。它既是 Windows PowerShell 的最佳继承者，也是所有平台上的首选版本。本书中，我们将主要聚焦于现代的 PowerShell 7+，因为它代表了 PowerShell 的未来。
    
*   **一致性的力量：一次学习，处处运行**
    
    PowerShell 的跨平台特性，为开发者和系统管理员带来了前所未有的福音。
    
    **核心 Cmdlet 的通用性**：无论你是在 Windows 上管理文件，还是在 Linux 上查看目录，`Get-ChildItem` 这个命令的行为都基本一致。你为管理 Windows 服务而学习的 `Get-Service` 知识，可以平滑地迁移到管理 Linux 上的 `systemd` 服务。这种核心体验的一致性，意味着你的知识投资是高度可复用的。
    
    **管理异构环境**：想象一下，你的公司同时拥有 Windows Server、Red Hat Linux 服务器和几台 Mac 开发设备。在过去，你需要掌握 CMD/Batch、Bash Shell 和 Zsh 等多种不同的 Shell 和脚本语言。而现在，你可以使用 PowerShell 这一套统一的技能和工具集，来编写脚本、部署应用、监控状态，优雅地管理整个异构IT环境。
    
    **社区与生态**：开源带来了繁荣的社区。PowerShell Gallery 是一个官方的模块仓库，你可以在上面找到由社区贡献的、用于管理 AWS、Google Cloud、VMware、网络设备乃至数据库的成千上万个模块。无论你的工作涉及到哪个领域，都很可能已经有现成的 PowerShell 工具在等着你。
    

至此，我们对 PowerShell 的初步探索告一段落。它是一个强大的 Shell，一门优雅的脚本语言，一个以对象为核心的革命性框架，更是一个横跨所有主流操作系统的统一管理平台。它不是一个简单的工具，而是一个全新的生态系统。

现在，你已经站在了这个新世界的入口。接下来，就让我们动手搭建自己的实验室，准备发出第一声问候吧！

#### **1.2. 搭建你的专属实验室：安装与配置**

理论的星空固然璀璨，但唯有实践的土地才能让知识生根发芽。在正式与 PowerShell 进行深度对话之前，我们需要确保它已经在你的系统中正确就位，并且你拥有一个舒适高效的工作环境。本节将手把手带你完成从安装到配置的全过程，无论你使用何种操作系统，都能找到清晰的指引。

##### **1.2.1. Windows 平台的内置与升级：区分 Windows PowerShell 与 PowerShell 7+**

对于 Windows 用户来说，你可能早已在不经意间与 PowerShell 打过照面。但此 PowerShell 非彼 PowerShell，了解它们的区别至关重要。

*   **Windows PowerShell：系统内置的“经典版”**
    
    如果你使用的是 Windows 10 或 Windows 11，乃至更早的 Windows 7/8.1，你的系统中已经预装了 **Windows PowerShell**。
    
    *   **如何找到它**：点击“开始”菜单，输入“PowerShell”，你看到的蓝色图标、名称为“Windows PowerShell”的应用，就是它了。
    *   **版本识别**：打开它，输入 `$PSVersionTable.PSVersion` 并回车，你很可能会看到主版本号为 5.1。
    *   **特点**：这个版本基于经典的、完整的 .NET Framework，与 Windows 系统深度集成，功能强大。在过去很长一段时间里，它都是 Windows 自动化的核心。然而，它的生命周期已基本定格在 5.1 版本，不会再有重大的功能更新，且**仅限于 Windows 平台**。
*   **PowerShell 7+：现代、跨平台的“进化版”**
    
    为了拥抱开源和跨平台的世界，微软推出了基于 .NET (Core) 的全新 PowerShell，我们称之为**现代 PowerShell**（版本 6 以后，目前主流是 7+）。这才是我们本书的主角，也是我们强烈推荐你安装和使用的版本。
    
    *   **为何选择它**：它不仅包含了 Windows PowerShell 的绝大部分功能，还带来了海量的性能改进、新的语法特性（如 `ForEach-Object -Parallel`），并且完美兼容 Linux 和 macOS。它是 PowerShell 的未来。
    *   **安装方式**：
        
        1.  **推荐：通过 Winget (Windows 程序包管理器)**：打开你的“终端”或“命令提示符”，输入以下命令即可一键安装：
            
            ```powershell
            # 可能因为网络原因会出现下载报错，请在不同时间多尝试几次或者开启VPN重新尝试
            winget install --id Microsoft.PowerShell --source winget
            
        2.  **通过 Microsoft Store**：在微软商店中搜索“PowerShell”，直接点击安装。这种方式可以获得自动更新，非常便捷。
        3.  **手动下载**：访问 PowerShell 的 [https://github.com/PowerShell/PowerShell/releases](https://github.com/PowerShell/PowerShell/releases "https://github.com/PowerShell/PowerShell/releases")，找到最新的稳定版（Stable），下载对应你的系统架构（通常是 x64）的 `.msi` 安装包进行安装。
    *   **和平共存**：请放心，安装 PowerShell 7+ **不会覆盖**你系统中内置的 Windows PowerShell 5.1。它们拥有不同的安装路径和程序名称（新版的可执行文件是 `pwsh.exe`，而旧版是 `powershell.exe`），可以完美地**并存**。安装后，你的开始菜单会有一个黑色的新图标，名为“PowerShell 7”。

**叮嘱**：从现在开始，除非有特殊说明，本书中所有的“PowerShell”都特指 PowerShell 7+ 版本。请确保你已经安装了它，并习惯于使用那个更酷的黑色图标，它将是你通往新世界的大门。

##### **1.2.2. 在 Linux 和 macOS 上安家：主流系统的安装指南**

PowerShell 的跨平台特性是其魅力的重要组成部分。在 Linux 和 macOS 上安装它，同样轻而易举。

*   **macOS 平台**
    
    对于 Mac 用户，最简单的方式是使用 [Homebrew — The Missing Package Manager for macOS (or Linux)](https://brew.sh/ "Homebrew — The Missing Package Manager for macOS (or Linux)")，一个广受欢迎的包管理器。
    
    1.  **安装 Homebrew**（如果尚未安装）：打开你的“终端”（Terminal）应用，运行其官网提供的一行安装命令。
    2.  **安装 PowerShell**：在终端中执行以下命令： 
        
        
        ```powershell
        brew install --cask powershell
    
    安装完成后，直接在终端里输入 `pwsh`，即可启动 PowerShell。
    
*   **Linux 平台**
    
    Linux 发行版众多，但主流的系统都提供了便捷的安装方式。
    
    *   **Ubuntu / Debian**： 
        sudo apt-get updatesudo apt-get install -y powershell
        
        ```powershell
        sudo apt-get update
        sudo apt-get install -y powershell
        
    *   **CentOS / RHEL**： 
        sudo dnf install -y powershell
        
        ```powershell
        sudo dnf install -y powershell
        
    *   **通用方式：使用 Snap**（如果你的系统支持）： 
        
        ```powershell
        sudo snap install powershell --classic
    
    同样，安装完成后，在你的终端模拟器中输入 `pwsh` 并回车，熟悉的 PowerShell 提示符就会出现。
    

##### **1.2.3. 初识你的工作台：认识控制台（Console）、终端（Terminal）与 VS Code**

安装好 PowerShell 只是第一步，我们还需要一个舒适且高效的工作环境来挥洒创意。你需要理解以下几个关键概念：

*   **控制台（Console）**
    
    这是最基础的命令行交互界面。在 Windows 上，当你直接运行 `pwsh.exe` 或 `powershell.exe` 时，弹出的那个简单的、通常是黑底白字或蓝底白字的窗口，就是一个控制台程序（`conhost.exe`）。它很朴素，功能有限，比如不支持多标签页。
    
*   **终端（Terminal）**
    
    终端是一个现代化的“控制台宿主”或“终端模拟器”。它本身不提供 Shell，但它为各种 Shell（如 PowerShell, cmd, bash）提供了一个功能丰富的“外壳”。
    
    *   **Windows Terminal**：Windows 11 已内置，Windows 10 用户可从 Microsoft Store 免费安装。它支持**多标签页**、**窗口拆分**、丰富的颜色主题和自定义配置。你可以同时打开一个 PowerShell 7 标签页、一个 Windows PowerShell 标签页和一个 Ubuntu (WSL) 的 bash 标签页，在同一个窗口里无缝切换。**这是我们在 Windows 上进行交互式操作的首选环境。** 
    *   **macOS 的 Terminal.app / iTerm2**：macOS 自带的 `Terminal.app` 功能尚可，但许多专业人士更青睐功能更强大的第三方终端，如 `iTerm2`。
    *   **Linux 的 GNOME Terminal, Konsole 等**：各大桌面环境都自带了优秀的终端模拟器。
*   **Visual Studio Code (VS Code)：终极脚本编辑器**
    
    当你的工作从简单的交互式命令，转向编写几十上百行的复杂脚本时，一个专业的代码编辑器就必不可少了。**Visual Studio Code (VS Code)** 是目前全球最受欢迎的免费、开源、跨平台的代码编辑器，它也是 PowerShell 官方推荐的脚本编写环境。
    
    *   **为何选择 VS Code**：
        1.  **PowerShell 官方扩展**：在 VS Code 的扩展市场中搜索并安装 `PowerShell` 扩展。它将为你的编辑器带来无与伦比的超能力：
            *   **智能感知（IntelliSense）**：在你输入时，自动提示 Cmdlet、参数、变量名，极大地减少拼写错误和查阅文档的频率。
            *   **语法高亮**：让代码色彩分明，结构一目了然。
            *   **代码片段（Snippets）**：快速插入常用的代码块，如 `if-else` 结构、`foreach` 循环等。
            *   **集成调试器**：可以设置断点，单步执行你的脚本，检查任意时刻的变量值，是解决复杂问题的利器。
            *   **集成控制台**：VS Code 内置了一个终端面板，你可以在编写脚本的同时，直接在下方运行和测试，无需切换窗口。
    
    **建议**：请立刻去 [Visual Studio Code - Code Editing. Redefined](https://code.visualstudio.com/ "Visual Studio Code - Code Editing. Redefined") 下载并安装它。从一开始就养成在 VS Code 中编写和调试 `.ps1` 脚本的习惯，这将使你的学习效率和编程体验产生质的飞跃。
    

至此，你的专属实验室已然落成。现代化的 PowerShell 7+ 已经就位，功能强大的 Windows Terminal（或你选择的其他终端）为你提供了清爽的交互界面，而专业的 VS Code 则像一个智能助手，随时准备帮你构建复杂的自动化脚本。

万事俱备，只欠东风。接下来，让我们深吸一口气，怀着一丝激动，准备向这个新世界发出我们的第一声问候吧！

#### **1.3. 你好，世界！——执行你的第一行命令**

在学习任何一门编程或脚本语言时，打印出“Hello, World!”（你好，世界！）都是一个充满仪式感的传统。它代表着我们与一个新系统成功建立了连接。在 PowerShell 中，我们同样可以做到，而且能做得更多、更有趣。本节将引导你执行第一行命令，并理解其背后的核心理念。

要向世界问好，请打开你刚刚配置好的终端（推荐 Windows Terminal），在 `pwsh` 提示符后，输入以下内容，然后按下回车键：

```powershell
"你好，世界！"
```

或者，使用专门用于输出的命令：

```powershell
Write-Output "Hello, World!"
```

看！PowerShell 立刻在下一行回应了你，将你输入的字符串原样输出。这简单的一步，证明了你与 PowerShell 的沟通渠道已经畅通无阻。现在，让我们深入探索这些命令背后蕴含的“魔法”。

##### **1.3.1. Cmdlet：PowerShell 的魔法咒语：理解 动词-名词 的命名艺术**

你刚才使用的 `Write-Output` 就是一个典型的 PowerShell 命令。在 PowerShell 的世界里，我们不叫它“command”或“instruction”，而是有一个更专业、更精确的名字：**Cmdlet**（读作 "command-let"，意为“小命令”）。

Cmdlet 是 PowerShell 原生的、最核心的命令形式。它们并非孤立的程序，而是构建在 PowerShell 运行时之上的 .NET 类，这赋予了它们与生俱来的、能处理对象的能力。而它们最直观、最友好的一个特点，就是其高度规范的命名艺术：

**动词-名词 (Verb-Noun) 结构**

每一个 Cmdlet 都由一个标准的“动词”和一个特定的“名词”通过连字符 `-` 连接而成。

*   **动词 (Verb)**：描述这个命令要执行的**动作**。例如 `Get`（获取）、`Set`（设置）、`New`（新建）、`Remove`（移除）、`Start`（启动）、`Stop`（停止）。动词都是从一个官方批准的、有限的列表中选取的，这保证了动作的一致性。
*   **名词 (Noun)**：指明这个命令要操作的**对象或资源**。例如 `Process`（进程）、`Service`（服务）、`Item`（项，通常指文件或目录）、`Content`（内容）。

让我们来看几个例子，感受一下这种命名法的优雅与直观：

*   `Get-Process`：**获取** **进程**信息。
*   `Start-Service`：**启动**一个**服务**。
*   `New-Item`：**新建**一个**项**（比如文件或文件夹）。
*   `Get-Content`：**获取**文件的**内容**。

**这种命名方式的好处是显而易见的：** 

*   **可预测性**：一旦你熟悉了常用的几个动词，即使遇到一个全新的名词，你也能大致猜出 `Get-Noun`, `Set-Noun`, `New-Noun` 是用来做什么的。想找文件？你可能会尝试 `Get-File` 或 `Get-Item`。想看帮助？`Get-Help` 呼之欲出。这种直觉性大大降低了学习曲线。
*   **清晰性**：命令本身就几乎是一句自解释的英文短语，`Stop-Process` 的意图一目了然，几乎不需要额外的注释。

Cmdlet 是 PowerShell 世界的基石，是构成所有自动化脚本的基本原子。掌握了 `动词-名词` 的艺术，你就掌握了与 PowerShell 高效沟通的语法。

##### **1.3.2. 探索的起点：Get-Help：学会使用帮助，它是你最好的老师**

面对成百上千个 Cmdlet，你可能会感到一丝不知所措。别担心，PowerShell 为你配备了一位全天候、全知全能的私人教师——`Get-Help` 命令。它是你在探索之路上最重要的工具，没有之一。

**初次见面：更新帮助文档**

在你第一次使用帮助系统之前，强烈建议你先更新一下本地的帮助文件库，以确保获取到最新、最全的信息。请以**管理员权限**打开 PowerShell（右键点击 PowerShell 7 图标，选择“以管理员身份运行”），然后执行：

```powershell
Update-Help
```

这个过程需要连接互联网，请耐心等待它完成。

**如何向“老师”请教？**

`Get-Help` 的使用非常简单。它的基本用法就是 `Get-Help [你要求助的Cmdlet名称]`。

例如，我们想了解一下 `Get-Process` 这个命令怎么用：

```powershell
Get-Help Get-Process
```

PowerShell 会立即返回 `Get-Process` 的基本信息，包括：

*   **NAME**：Cmdlet 名称。
*   **SYNOPSIS**：功能简介。
*   **SYNTAX**：语法结构，告诉你它有哪些参数。
*   **DESCRIPTION**：详细描述。
*   **RELATED LINKS**：相关的其他命令。

**解锁更详细的帮助：参数的力量**

`Get-Help` 本身也是一个 Cmdlet，它也有自己的参数，可以让你获取不同维度的帮助信息。

*   **查看完整帮助**：`Get-Help Get-Process -Full` 这会显示关于该 Cmdlet 的所有信息，包括每个参数的详细说明、输入输出类型以及注意事项。
*   **查看实例**：`Get-Help Get-Process -Examples` 这是最实用的参数之一！它会直接给你展示几个立即可用的使用范例，从简单到复杂。很多时候，模仿和修改示例是学习新命令最快的方式。
*   **打开在线帮助**：`Get-Help Get-Process -Online` 这个命令会自动用你的默认浏览器打开该 Cmdlet 在微软官方文档网站上的最新页面。网页版通常排版更友好，信息也可能是最新的。

**智慧**：遇到不认识的命令，不要怕，问 `Get-Help`。想知道一个命令能做什么，问 `Get-Help -Examples`。忘记了参数怎么写，问 `Get-Help -Full`。将 `Get-Help` 刻在你的指尖，你的 PowerShell 之旅将畅通无阻。

##### **1.3.3. 初试牛刀：Get-Date 与 Get-Process：感受 PowerShell 的即时反馈**

现在，你已经掌握了 Cmdlet 的命名规律，并认识了 `Get-Help` 这位良师益友。让我们学以致用，通过两个简单而强大的命令，亲身感受 PowerShell 的魅力。

*   **获取当前日期和时间：`Get-Date`**
    
    在 PowerShell 提示符后，输入：
    
    ```powershell
    Get-Date
    ```
    
    瞬间，当前的日期、时间和时区信息就会清晰地呈现在你眼前。但别忘了，这不仅仅是文本。`Get-Date` 返回的是一个**DateTime 对象**。这意味着你可以轻松地操作它。比如，只想要今天的年份？
    
    ```powershell
    (Get-Date).Year
    ```
    
    我们会在后续章节深入讲解对象的属性和方法，这里你只需感受一下这种直接获取结构化信息的能力。
    
*   **查看当前运行的进程：`Get-Process`**
    
    这是一个更能体现 PowerShell 对象优势的命令。输入：
    
    ```powershell
    Get-Process
    ```
    
    屏幕上会立即滚动显示出你电脑上当前正在运行的所有进程的列表，并以一个美观的表格形式呈现，包含了句柄数（Handles）、内存占用（WS）、CPU 时间（CPU）和进程名称（ProcessName）等信息。
    
    这个表格并非简单的文本拼凑。每一行都是一个独立的“进程对象”。正是因为 PowerShell 知道每一列对应的是哪个**属性**，我们才能在后续的章节中学习如何轻松地对这些进程进行排序、筛选和操作。例如，只找出名为“chrome”的进程，或者将所有进程按内存使用量从高到低排列。
    

通过这第一行命令、第一个 Cmdlet、第一次求助 `Get-Help`，以及两次初试牛刀，你已经成功地在 PowerShell 的世界里发出了自己的声音，并收到了它清晰、结构化的回应。你不再是一个门外的观察者，你已经手握钥匙，推开了通往自动化王国的大门。

从这激动人心的第一步开始，前方的道路将越发宽广和精彩。稳住心神，朋友们，我们真正的冒险，才刚刚开始。

* * *