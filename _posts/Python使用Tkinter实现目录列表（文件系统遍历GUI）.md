---
layout:     post
title:      "Python使用Tkinter实现目录列表（文件系统遍历GUI）"
subtitle:   ""
date:       2018-08-31 19:31:00
author:     "崔天祥"
header-img: "img/post-bg-os-metro.jpg"
catalog:    true
tags:
    - Python
    - 编程
    - 其他
---

> Python使用Tkinter实现目录列表（文件系统遍历GUI） | 若本文不能正常显示 [点击这里](https://blog.csdn.net/qq_34755081/article/details/82261527)

这个应用是一个目录树遍历工具。它会从当前目录（或指定目录，本文代码指定为桌面）开始，提供一个文件列表。
双击列表中任意其他目录，就会使得工具切换到新目录中，用新目录中的文件列表代替旧文件列表。
该应用主界面如下：
![这里写图片描述](https://img-blog.csdn.net/2018083119180259?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM0NzU1MDgx/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
该应用的主要目的是为条用它的界面提供返回值，即在单击确认的时候把下方文本框中的内容（选中的文件路径和文件名）更新到主界面。
#### 文件系统遍历GUI（listdir.py）
这个GUI程序扩展了控件的使用，新增了列表框，文本框和滚动条。此外还增加了鼠标单击、键盘按下、滚动操作等回调函数。
<!-- more -->
```
import os
from time import sleep
from tkinter import *
class Dirlist():
    def __init__(self,initdir=None,path=None,root=None):# 在主界面生成Dirlist对象时，把变量和控件对象传递过来。参数initdir为初始文件路径
        # 主界面中：变量：path = StringVar()   控件：filename = Entry(frame1,textvariable = path) 即在主界面中生成对象Dirlist('路径',path=path,root=filename,)

        self.top2 = Tk()
        self.top2.title('选择文件')
        self.path = path
        self.root = root

        #这里声明了Tk的一个变量cwd，用于保存当前所在的目录名，并用Label控件对象dirl显示出来（在调用函数doLS时显示出来）
        self.cwd = StringVar(self.top2)
        self.dirl = Label(self.top2,fg = 'blue')
        self.dirl.pack()

        # 第一个框架dirfm，应用核心部分，里面放置Listbox控件dirs，和Scrollbar滚动条。并通过使用List的bind()方法，将鼠标双击事件绑定
        self.dirfm = Frame(self.top2)
        self.dirsb = Scrollbar(self.dirfm)
        self.dirsb.pack(side=RIGHT,fill=Y)
        self.dirs = Listbox(self.dirfm,height=15,width=50,yscrollcommand=self.dirsb.set)

        #通过使用List的bind()方法，将鼠标双击事件绑定，并调用setDirAndGo函数
        self.dirs.bind('<Double-1>',self.setDirAndGo)

        # 下面实现单击时，将所选文件路径加名字更新到下方输入框控件中，不能用self.dirs.bind('<Button-1>', self.setDirn)绑定单击事件，会出错
        self.dirs.bind("<<ListboxSelect>>", self.setDirn)
        self.dirsb.config(command=self.dirs.yview)
        self.dirs.pack(side=LEFT,fill=BOTH)
        self.dirfm.pack()
        #界面下方输入框，随着单击更新内容，并在点击打开按钮时，打开输入框的内容目录
        self.dirn = Entry(self.top2,width=50,textvariable=self.cwd)
        #绑定回车事件，即当光标在输入框时，回车调用doLS函数
        self.dirn.bind('<Return>',self.doLS)
        self.dirn.pack()

        #第二个框架bfm，放置按钮
        self.bfm = Frame(self.top2)
        self.open = Button(self.bfm,text='打开',command=self.doLS,activeforeground='white',activebackground='blue')
        self.ls = Button(self.bfm,text='确认',command=self.result,activeforeground='white',activebackground='green')

        #没有用到退出按钮，故注释掉了
        # self.quit = Button(self.bfm,text='退出',command=self.top2.quit,activeforeground='white',activebackground='red')
        self.open.pack(side=LEFT)
        self.ls.pack(side=LEFT)
        # self.quit.pack(side=LEFT)
        self.bfm.pack()

        #若存在初始文件路径，则更新界面
        if initdir:
            self.cwd.set(initdir)
            self.top2.update()
            self.doLS()

    #当点击确认按钮时调用，将当前所在的目录名和文件名，更新到主界面，并关闭当前界面（在被调用的情况下点击确认可正常关闭）
    def result(self):
        tdir = self.cwd.get()
        self.path.set(tdir)#更新主界面变量
        self.root.update()#更新主界面
        self.top2.destroy()#关闭当前界面

    #单击列表中内容时，调用此函数，更新下方输入框内容
    def setDirn(self,ev=None):
        t = self.dirs.get(self.dirs.curselection())
        print(t)
        text=os.getcwd()+'\\'+t  #文件目录和文件名
        self.dirn.delete(0, END)
        self.dirn.insert(INSERT, text)

    #双击时调用，双击时，设置背景色为红色，并调用doLS函数打开所选文件
    def setDirAndGo(self,ev=None):
        self.last = self.cwd.get()
        self.dirs.config(selectbackground='red')
        check = self.dirs.get(self.dirs.curselection())
        if not check:
            check = os.curdir
        self.cwd.set(check)
        self.doLS()

    #实现更新目录的核心函数
    def doLS(self,ev=None):
        error = ''
        tdir = self.cwd.get()
        if not tdir:tdir=os.curdir
        #若路径输入错误，或者打开的是文件，而不是目录，则更新错误提示信息
        if not os.path.exists(tdir):
            error = os.getcwd()+'\\'+tdir + '：未找到文件'
        elif not os.path.isdir(tdir):
            error = os.getcwd()+'\\'+tdir + '：未找到目录'
        if error:
            self.cwd.set(error)
            self.top2.update()
            sleep(1)
            if not (hasattr(self,'last') and self.last):
                self.last = os.curdir
            self.cwd.set(os.curdir)
            self.dirs.config(selectbackground='LightSkyBlue')
            self.dirn.config(text=os.getcwd()+'\\'+tdir)
            self.top2.update()
            return

        self.cwd.set(os.getcwd()+'\\'+tdir)
        self.top2.update()
        dirlist = os.listdir(tdir)#os.listdir() 方法用于返回指定的文件夹包含的文件或文件夹的名字的列表。
        dirlist.sort()
        os.chdir(tdir)#os.chdir() 方法用于改变当前工作目录到指定的路径。

        #更新界面上方标签内容
        self.dirl.config(text=os.getcwd())
        self.top2.update()

        self.dirs.delete(0,END)
        self.dirs.insert(END,os.pardir)#os.chdir(os.pardir)  切换到上级目录 即将上级目录.. 插入到dirs对象中

        #把选定目录的文件或文件夹的名字的列表依次插入到dirs对象中
        for eachFile in dirlist:
            self.dirs.insert(END,eachFile)
            self.cwd.set(os.curdir)
            self.dirs.config(selectbackground='LightSkyBlue')
if __name__ =='__main__':
    #设定初始目录为桌面
    d = Dirlist(r'C:\Users\Administrator\Desktop')
    mainloop()

```
此应用暂时还有一定的缺陷，如点击上一级目录，最多可返回到初始目录的盘符根目录下，即不能方便的切换盘符，只能通过在输入框输入盘符路径，点击打开进行切换。