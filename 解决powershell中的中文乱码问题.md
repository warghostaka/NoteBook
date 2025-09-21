# 解决powershell中的中文输出乱码问题

使用vscode，git bash时，即使为他们设置了用utf-8编码输出，还是会出现中文乱码

原因可能是powershell的控制台输出使用系统代码页，系统语言是简体中文的情况下会自动用GBK编码进行输出，这种情况会导致git等程序输出utf-8编码时，控制台仍然会用GBK编码解读，最终导致乱码

解决方式是写入powershell配置文件（profile）中，指定powershell控制台输出编码为utf-8

首先查看profile路径，默认在`C:\Users\WarghostAKA\Documents\WindowsPowerShell\profile.ps1`

```powershell
$PROFILE
```

打开并添加内容，在PowerShell中查看当前输出编码的变量

```powershell
[Console]::OutputEncoding = [System.Text.Encoding]::UTF8
```

> - 这是 .NET 的 `System.Console` 类，PowerShell 可以直接调用。
> - 它控制当前控制台的输入输出行为。
>
> **`OutputEncoding`**
>
> - 这是 `System.Console` 的一个静态属性，表示 **控制台输出的编码方式**。
> - 默认情况下，Windows 控制台使用系统代码页（例如简体中文是 **GBK / CP936**）。
> - 这就导致当 Git 输出 UTF-8 编码的中文时，控制台按 GBK 去解读，结果乱码。
>
> **`[System.Text.Encoding]::UTF8`**
>
> - 这是 .NET 框架里表示 UTF-8 编码的对象。
>
> **整体作用**
>
> - 把 PowerShell 的 **控制台输出编码** 改为 **UTF-8**。
> - 这样，当 Git（或其他程序）输出 UTF-8 内容时，PowerShell 会正确解码并显示中文。