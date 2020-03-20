Sublime Text


通过`Ctrl+` 快捷键或者 `View > Show Console` 菜单打开控制台，复制粘贴如下代码回车即可。
```python
import urllib.request,os,hashlib; h = '6f4c264a24d933ce70df5dedcf1dcaee' + 'ebe013ee18cced0ef93d5f746d80ef60'; pf = 'Package Control.sublime-package'; ipp = sublime.installed_packages_path(); urllib.request.install_opener( urllib.request.build_opener( urllib.request.ProxyHandler()) ); by = urllib.request.urlopen( 'http://packagecontrol.io/' + pf.replace(' ', '%20')).read(); dh = hashlib.sha256(by).hexdigest(); print('Error validating download (got %s instead of %s), please try manual install' % (dh, h)) if dh != h else open(os.path.join( ipp, pf), 'wb' ).write(by)
```



---

##### windows 右键添加快捷打开

打开注册表（Win键+`regedit`），跳转到以下位置：

```
计算机\HKEY_CLASSES_ROOT\*\shell\
计算机\HKEY_CLASSES_ROOT\Directory\shell\
```

带`Directory`的路径下添加的话，可以直接右键打开整个文件夹，不带的则是针对文件的。

具体添加内容看截图即可：



先在`shell`下新增一个`项`，命名为`Sublime Text 3`，然后新增右侧两个字符数据。

![](https://raw.githubusercontent.com/willxiang/code-note/master/img/Snipaste_2020-03-20_17-48-48.jpg)



再新增`command`子项：

![](https://raw.githubusercontent.com/willxiang/code-note/master/img/Snipaste_2020-03-20_17-51-52.jpg)



文件夹打开方式与上方的操作类似，只是在`command`项的数据中，第二个参数不同：

![](https://raw.githubusercontent.com/willxiang/code-note/master/img/Snipaste_2020-03-20_17-53-13.jpg)

