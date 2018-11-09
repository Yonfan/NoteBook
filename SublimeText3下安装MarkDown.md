# Sublime下安装MarkDown并实现实时预览
## 1、安装前需要注意的一些问题：
- 安装Sublime：[Sublime Text 3](http://www.sublimetext.com/3)
- 安装Package Control：[Package Control](https://packagecontrol.io/installation)

> import urllib.request,os,hashlib; h = '6f4c264a24d933ce70df5dedcf1dcaee' + 'ebe013ee18cced0ef93d5f746d80ef60'; pf = 'Package Control.sublime-package'; ipp = sublime.installed_packages_path(); urllib.request.install_opener( urllib.request.build_opener( urllib.request.ProxyHandler()) ); by = urllib.request.urlopen( 'http://packagecontrol.io/' + pf.replace(' ', '%20')).read(); dh = hashlib.sha256(by).hexdigest(); print('Error validating download (got %s instead of %s), please try manual install' % (dh, h)) if dh != h else open(os.path.join( ipp, pf), 'wb' ).write(by)

键入Ctrl + ~ 打开控制台，输入上述命令回车即可安装。<br/>
如果这种方法失败，可以去官网下载Package Control.sublime-package 并且将它拷贝到 Sublime的安装包目录下解压。<br/>
再重启Sublime查看Preference --> Package Control是否安装成功。
## 2、安装MarkdownEditing和LiveReload
- 按Ctrl + Shift + P，输入install Packages回车。
- 输入MarkDown Preview回车，进行安装。
## 3、安装MarkdownPreView + LiveReload
### 3.1先安装MarkDownPreview并进行配置
安装MarkdownPreview的方法跟上面一样，安装完成后会弹出一个安装成功的文本文件。<br/>
MarkdownPreview没有快捷键，可以为Markdown Preview： Preview in Browser设置快捷键。<br/>
方法是在Preferences -> Key Bindings打开的文件的右侧栏的中括号中添加一行代码：<br/>
```
{ "keys": ["alt+m"], "command": "markdown_preview", "args": {"target": "browser", "parser":"markdown"}  }
```
如前Markdown Preview安装成功后，设置前文所述的快捷键（如需），打开其配置文件 Preferences -> Package Settings -> Markdown Preview -> Settings，检查左侧enable_autoreload条目是否为true，若是，跳过。若不是，右侧栏加一条下面这个后重启Sublime:
```
{
    "enable_autoreload": true
}
```
### 3.2安装配置LiveReload实现自动刷新预览
Ctrl+Shift+p, 输入 Install Package，输入LiveReload, 回车安装 
安装成功后, 再次Ctrl+shift+p, 输入LiveReload: Enable/disable plug-ins, 回车, 选择 Simple Reload with delay (400ms)或者Simple Reload，两者的区别仅仅在于后者没有延迟。
## 4、使用
安装完成后重启Sublime，新建一个后缀为filename.md的文件，键入几个字后保存，按alt + m（刚设置的快捷键）即可弹出一个可以即时浏览的网页！<br/>

>Enjoy the sunshine today!     **:)**