# Unsafe Safari Extensions 不安全的 Safari 扩展

> 这是 [zachdrago/Unsafe-Safari-Extensions](https://github.com/zachdrago/Unsafe-Safari-Extensions) 的中文版本，包括中文翻译以及支持中文系统的 Automator App

> **注意:** 请先勾选 偏好设置 -> 高级 -> 在菜单栏中显示“开发”菜单

如果您已更新到 Safari 12 或 macOS Mojave，您可能会发现某些扩展程序不再受支持。

幸运的是，这些扩展可以通过 Safari Extension Builder 手动重新安装。唯一的缺点是，每次退出并启动 Safari 时都需要重新安装扩展程序，并且不会保存它的首选项。

更多相关信息可以在这里阅读：

https://georgegarside.com/blog/macos/install-any-safari-extension-macos-mojave/


> **注意:** 您可能需要更改脚本中的重复次数（即 repeat times），因为它需要大于Extension Builder中的扩展个数。
> 
![enter image description here](https://github.com/Ji4n1ng/Unsafe-Safari-Extensions/raw/master/img/repeat.png)


## Automator App

由于我无法在 Safari 启动时自动运行脚本，因此我创建了一个 Automator 应用程序来帮助我。下面代码的具体流程是，打开Safari，然后在 Extension Builder 中循环并运行所有扩展（*您仍然需要手动输入每个扩展的系统密码*）。你可以下载[程序](https://github.com/Ji4n1ng/Unsafe-Safari-Extensions/raw/master/Safari%20Extensions.app.zip)，或者在 Automator 中使用下面的代码创建你自己的应用程序：

> **注意:** 下面的代码只支持中文系统，如果需要英文系统下的应用程序，请看 [zachdrago/Unsafe-Safari-Extensions](https://github.com/zachdrago/Unsafe-Safari-Extensions)

```
tell application "Safari" to activate
tell application "System Events"
	tell process "Safari"
		set frontmost to true
		click menu item "显示扩展构建器" of menu "开发" of menu bar 1
		set frontmost to true
		delay 0.5
		
		set myCount to count row of table 1 of scroll area 1 of splitter group 1 of window "扩展构建器"
		
		repeat 6 times
			key code 125
		end repeat
		
		repeat with counter from 1 to myCount
			click row counter of table 1 of scroll area 1 of splitter group 1 of window "扩展构建器"
			click button "运行" of splitter group 1 of window "扩展构建器"
			delay 0.5
			key code 126
		end repeat
		
		click button 1 of window "扩展构建器"
	end tell
end tell
```

![enter image description here](https://github.com/Ji4n1ng/Unsafe-Safari-Extensions/raw/master/img/automator.png)

![enter image description here](https://github.com/Ji4n1ng/Unsafe-Safari-Extensions/raw/master/img/run.png)

## 只需要 AppleScript 就可以

以下是改进的 AppleScript 脚本，并在Extension Builder中运行所有扩展（不启动Safari）。 *您仍然需要为每个扩展手动输入系统密码。*

```
    tell application "System Events"
        tell process "Safari"
        	set frontmost to true
        	click menu item "显示扩展构建器" of menu "开发" of menu bar 1
        	set frontmost to true
        	delay 0.5
		
		set myCount to count row of table 1 of scroll area 1 of splitter group 1 of window "扩展构建器"
    		
		repeat 6 times  -- MUST BE GREATER THAN THE # OF EXTENSIONS IN YOUR EXTENSION BUILDER
			key code 125
		end repeat
		
		repeat with counter from 1 to myCount
			click row counter of table 1 of scroll area 1 of splitter group 1 of window "扩展构建器"
			click button "运行" of splitter group 1 of window "扩展构建器"
			delay 0.5
			key code 126
		end repeat
		
		click button 1 of window "扩展构建器"
	end tell
    end tell
```
