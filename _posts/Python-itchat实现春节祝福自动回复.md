---
title: Python itchat实现春节祝福自动回复
date: 2019-02-3 23:12:12
tags: Python
---

itchat是一个开源的微信个人号接口，使用python调用微信从未如此简单。
使用不到三十行的代码，就可以完成一个能够处理所有信息的微信机器人。

首先安装itchat
``` 
pip install itchat
```

接下来就可以针对对方发的关键字进行回复了
``` 
import itchat, re
from itchat.content import *

@itchat.msg_register([TEXT])
def text_reply(msg):
    match = re.search('年|春|快乐', msg['Text'])
    # 第一个单引号中的内容是关键词，可以自行编辑
    if match:
      itchat.send(('新年快乐！'), msg['FromUserName'])
      # 第一个单引号中的内容是回复的内容，可以自行编辑

itchat.auto_login(enableCmdQR=True)
itchat.run()
```
<!--more-->

然后扫描文件生成的二维码登录即可。

上述方法的回复较为单一，如果要想实现更丰富的回复措辞，可以使用机器人。
登录这个网址：http://www.tuling123.com/help/h_cent_webapi.jhtml ，可以通过注册使用一个现成的机器人——图灵机器人。
注册后，创建一个机器人，在应用终端中选择“网站”。
![Alt text](./1549811275715.png)
点击创建好机器人，可以获取一个 apikey 。
![Alt text](./1549811311056.png)
代码如下：
``` 
import requests
import itchat

KEY = 'XXXXXXXXXXXXXXXXXXX'
# 单引号中的内容换成之前获取的apikey

def get_response(msg):
    # 这里我们就像在“3. 实现最简单的与图灵机器人的交互”中做的一样
    # 构造了要发送给服务器的数据
    apiUrl = 'http://www.tuling123.com/openapi/api'
    data = {
        'key'    : KEY,
        'info'   : msg,
        'userid' : 'wechat-robot',
    }
    try:
        r = requests.post(apiUrl, data=data).json()
        # 字典的get方法在字典没有'text'值的时候会返回None而不会抛出异常
        return r.get('text')
    # 为了防止服务器没有正常响应导致程序异常退出，这里用try-except捕获了异常
    # 如果服务器没能正常交互（返回非json或无法连接），那么就会进入下面的return
    except:
        # 将会返回一个None
        return

# 这里是我们在“1. 实现微信消息的获取”中已经用到过的同样的注册方法
@itchat.msg_register(itchat.content.TEXT)
def tuling_reply(msg):
    # 为了保证在图灵Key出现问题的时候仍旧可以回复，这里设置一个默认回复
    defaultReply = 'I received: ' + msg['Text']
    # 如果图灵Key出现问题，那么reply将会是None
    reply = get_response(msg['Text'])
    # a or b的意思是，如果a有内容，那么返回a，否则返回b
    # 有内容一般就是指非空或者非None，你可以用`if a: print('True')`来测试
    return reply or defaultReply

# 为了让实验过程更加方便（修改程序不用多次扫码），我们使用热启动
itchat.auto_login(enableCmdQR=True)
itchat.run()
```
现在，当微信收到消息，调用的图灵机器人就会自动分析消息的意思，做出简单的回应。