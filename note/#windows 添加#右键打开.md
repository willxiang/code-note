以sublime text为例：
```
 Windows Registry Editor Version 5.00

[HKEY_CLASSES_ROOT\*\shell\SublimeText]
@="SublimeText"
"Icon"="D:\\App\\Sublime Text 3\\sublime_text.exe"

[HKEY_CLASSES_ROOT\*\shell\SublimeText\Command]
@="\"D:\\App\\Sublime Text 3\\sublime_text.exe\" \"%1\""
```
保存为reg文件，打开注册即可。