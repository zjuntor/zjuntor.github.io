---
title: Update Your Console Program With Qt UI
author: torrge
comments: true
layout: post
permalink: /2010/11/update-your-console-program-with-qt-ui/
categories:
  - Programming
tags:
  - Python
  - Qt
---
正式上班以来，为工作上的事情，没少写程序。

开始的想法是不做UI，通过输入命令行参数与用户交互，毕竟console做得好还是很cool的。可后来同事们纷纷表示不习惯黑漆漆的console。也对，对于习惯用鼠标在SNS上从一个链接点到另一个链接，愣是能点一下午的用户，说console的好处就好比想看韩剧的观众，你愣是给他播新闻联播。后来试着用硬编码的方法解决问题，同事们方便了，可自己的麻烦也多了。于是开始着手做个UI，让大家都能解脱。

考虑到有以前在C++ 环境下Qt 的经验，所以在PyGtk、PyQt 中选择了PyQt。PyQt 说到底毕竟是Qt 的Python 绑定，所以主要的依旧还是C++ 环境下Qt 的几大模块：

*   QtCore： 包含核心的非GUI功能，用于时间、文件和目录、各种数据类型、流、网址、MIME类型、线程或进程；
*   QtGui：包含图形组件和相关的类，如按钮、窗体、状态栏、工具栏、滚动条、位图、颜色、字体等；
*   QtNetwork：包含了网络编程相关的类，这些类允许编写TCP/IP 和UDP 的客户端和服务器端程序；
*   QtXml：包含使用XML文件的类，这个模块提供了SAX 和DOM API 的实现；
*   QtSvg：包含了显示SVG文件内容的类；
*   QtOpenGL：包含了使用OpenGL库渲染3D和2D图形的类；
*   QtSql：包含了提供提供数据库操作的类，同时也提供了一个SQLite 的实现。

考虑安全问题，就不用用来工作的程序示例，下面示例一个“Hello World”程序  
<img src="http://pic.yupoo.com/convallariaa/ADxq52Kf/elAxd.png" alt="Calculate" width="280" height="278" />

<pre class="brush:py">import sys
from math import *
from PyQt4.QtCore import *
from PyQt4.QtGui import *

#calculate form widget
class Form(QDialog):
    def __init__(self,parent=None):
        super(Form,self).__init__(parent)
        self.browser = QTextBrowser()
        self.lineedite = QLineEdit("Type a expression and press Enter")
        self.lineedite.selectAll()
        layout = QVBoxLayout()
        layout.addWidget(self.browser)
        layout.addWidget(self.lineedite)
        self.setLayout(layout)
        self.lineedite.setFocus()
        self.connect(self.lineedite,SIGNAL("returnPressed()"),self.updateUI)
        self.setWindowTitle("Calculate")
    def updateUI(self):
        try:
            text = unicode(self.lineedite.text())
            self.browser.append("%s = %s" % (text, eval(text)))
        except:
            self.browser.append("%s is invalid!" % text)
</pre>