# 修改Jupyter的工作空间
## 1、安装Jupyter Notebook
[刚安装完并配置好Sublime Text 3](https://www.cnblogs.com/yangfan-123/p/9927508.html)后被学长推荐使用Jupyter notebook，于是就想着看看试试有没有他说的那么好。

- 安装：命令行直接`pip install jupyter`(前提是先安装好了python和pip)
## 2、安装完后困惑
这么大一堆目录谁知道在哪儿？我新建一个文件总得知道我这文件在哪儿吧，于是乎我便去寻找！
怎么找呢？
## 3、寻找
我知道jupyter肯定是在python的安装目录下的，于是乎我便去查找python的安装目录：
```
C:\Users\dell>python
Python 3.7.0b5 (v3.7.0b5:abb8802389, May 31 2018, 01:54:01) [MSC v.1913 64 bit (AMD64)] on win32
Type "help", "copyright", "credits" or "license" for more information.
>>> import sys
>>> sys.path
['', 'E:\\programs\\python37.zip', 'E:\\programs\\DLLs', 'E:\\programs\\lib', 'E:\\programs', 'E:\\programs\\lib\\site-packages']
```
好了我知道python的安装目录了：`E:\\programs\`,于是找到jupyter的安装目录：`E:\\programs\Scripts`(一般都是在Scripts目录下),于是一顿操作：
```
C:\Users\dell>e:
E:\>cd E:\programs\Scripts
E:\programs\Scripts>jupyter notebook --generate-config
Writing default config to: C:\Users\dell\.jupyter\jupyter_notebook_config.py
```
打开`C:\Users\dell\.jupyter\jupyter_notebook_config.py`这个文件。
直接Ctrl + F 找到 `c.NotebookApp.notebook_dir`
```
## The directory to use for notebooks and kernels.
c.NotebookApp.notebook_dir = 'E:\\python\\JupterSpace'
```
将其修改成为你像存放的目录。保存，重启jupyter后就会发现修改完成！
## 4、将jupyter notebook设置成为可远程访问！
### 4.1 生成jupyter密码
- 自动生成
从 jupyter notebook 5.0 版本开始，提供了一个命令来设置密码：jupyter notebook password，生成的密码存储在 jupyter_notebook_config.json。
```
C:\Users\dell>jupyter notebook password
Enter password:
Verify password:
[NotebookPasswordApp] Wrote hashed password to C:\Users\dell\.jupyter\jupyter_notebook_config.json
```
- 手动生成
除了使用提供的命令，因为`jupyter notebook password` 出来一堆内容，没耐心看。打开 ipython 执行下面内容：
```
In [1]: from notebook.auth import passwd
In [2]: passwd()
Enter password:
Verify password:
Out[2]: 'sha1:67c9e60bb8b6:9ffede0825894254b2e042ea597d771089e11aed'
```
sha1:67c9e60bb8b6:9ffede0825894254b2e042ea597d771089e11aed 这一串就是要在 jupyter_notebook_config.py 添加的密码。
```
{
  "NotebookApp": {
    "password": "sha1:140172243383:5cc22b24839464e6a30203a492b297bcf754971c"
  }
}
```
### 4.2 修改配置文件
在 `jupyter_notebook_config.py `中找到下面的行，取消注释并修改。
```
c.NotebookApp.ip='*'
c.NotebookApp.password = u'sha:ce...刚才复制的那个密文'
c.NotebookApp.open_browser = False
c.NotebookApp.port =8888 #可自行指定一个端口, 访问时使用该端口
```
以上设置完以后就可以在服务器上启动 `jupyter notebook`打开 IP:指定的端口, 输入密码就可以访问了。<br>
需要注意的是不能在隐藏目录 (以 . 开头的目录)下启动 jupyter notebook, 否则无法正常访问文件。

##### Enjoy the sunshine today!     **:)**