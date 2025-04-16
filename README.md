# Oh My Posh 简单配置

## 1. 安装powershell7.5

打开powershell输入：

```powershell
winget search Microsoft.PowerShell
```

<img src="https://github.com/Syrix555/oh_my_posh_config/blob/main/README.assets/image-20250416232023704.png" alt="image-20250416232023704"  />

根据id安装对应的pwsh版本：

```powershell
winget install --id Microsoft.PowerShell --source winget
```

安装后将会与原先的powershell5.1共存，安装位置如下：

- Windows PowerShell 5.1：`$env:WINDIR\System32\WindowsPowerShell\v1.0`
- PowerShell 7：`$env:ProgramFiles\PowerShell\7`

在 Windows PowerShell 中，PowerShell 可执行文件名为 `powershell.exe`。 在版本 6 及更高版本中，可执行文件名为 `pwsh.exe`。在 win + r 的运行窗口中输入pwsh即可使用powershell 7.5。

为了修改默认终端，在设置页面 -> 启动 -> 默认配置文件中选择`PowerShell`即可。

![image-20250416232858905](https://github.com/Syrix555/oh_my_posh_config/blob/main/README.assets/image-20250416232858905.png)

## 2. 安装oh my posh

有三种方式可以安装：

1. Mircosoft Store里搜索`oh-my-posh`直装即可。

2. 使用winget，输入`winget install JanDeDobbeleer.OhMyPosh -s winget`。

3. 访问[JanDeDobbeleer/oh-my-posh: The most customisable and low-latency cross platform/shell prompt renderer](https://github.com/JanDeDobbeleer/oh-my-posh)在releases中选择对应平台安装。

## 3. 安装Nerd Fonts

为了显示各种各样的小图标，oh my posh采用nerd fonts，在[ryanoasis/nerd-fonts: Iconic font aggregator, collection, & patcher. 3,600+ icons, 50+ patched fonts: Hack, Source Code Pro, more. Glyph collections: Font Awesome, Material Design Icons, Octicons, & more](https://github.com/ryanoasis/nerd-fonts)中下载自己喜欢的字体安装。

我使用的是JetBrainsMonoNerdFontMono-Regular，点击ttf文件，再安装即可。

![image-20250416233706647](https://github.com/Syrix555/oh_my_posh_config/blob/main/README.assets/image-20250416233706647.png)

随后在终端中应用字体，进入设置，左侧栏的配置文件里选择`PowerShell`（当然也可以全局使用），点击外观，在字体中找到安装的Nerd Font保存即可。

![image-20250416233901002](https://github.com/Syrix555/oh_my_posh_config/blob/main/README.assets/image-20250416233901002.png)

完成这些步骤后，在pwsh中输入oh-my-posh init pwsh | Invoke-Expression即可使用oh my posh界面的shell了。

![image-20250416234339994](https://github.com/Syrix555/oh_my_posh_config/blob/main/README.assets/image-20250416234339994.png)

##    4. 自定义主题

oh my posh提供了大量自定义主题供选择，在[Themes | Oh My Posh](https://ohmyposh.dev/docs/themes)中可以预览这些主题并选择自己喜欢的。选择好心仪的主题之后，记住主题对应的名称，在shell中输入：

```powershell
code $PROFILE
```

输入如下的命令：

```powershell
oh-my-posh init pwsh --config 'C:\Users\<User Name>\AppData\Local\Programs\oh-my-posh\themes\<Theme Name>.omp.json' | Invoke-Expression
```

其中`<User Name>`是你的用户名，`<Theme Name>`是你选取好的主题名。

保存后在shell中输入：

```powershell
. $PROFILE
```

即可应用shell配置文件，完成主题自定义。

## 5. 配置自动补全

在shell中输入如下命令安装自动补全包`posh-git`：

```powershell
PowerShellGet\Install-Module posh-git -Scope CurrentUser -Force
```

打开`$PROFILE`文件输入如下的命令：

```powershell
Set-PSReadLineKeyHandler -Key Tab -Function MenuComplete #Tab键会出现自动补全菜单
Set-PSReadlineKeyHandler -Key UpArrow -Function HistorySearchBackward
Set-PSReadlineKeyHandler -Key DownArrow -Function HistorySearchForward

Import-Module posh-git
```

应用配置文件后即可使用自动补全。

## 6. conda虚拟环境显示异常

打开shell后不会显示当前使用的conda虚拟环境名称，这是由主题json文件配置不完全导致的。

打开你所使用的主题json文件，在里面使用搜索找到python对应的配置块，如果缺失`properties`项，在这个块中添加如下的代码：

```json
"properties": {
	"fetch_version": false,
	"fetch_virtual_env": true,
	"home_enabled": true,
	"display_mode": "environment"
},
```

重新应用配置文件`$PROFILE`即可。