# 微信Api/WeChatApi

狗子微信Api、微信机器人、微信api、微信发卡机器人、微信聊天机器人。

基于Windows平台开发的微信Api，仅支持2.9.0.122版本。

仅供学习，禁止非法用途！

# 文件

`WeChat2.dll`：客户端

`WeChatclient.ini`：填写服务端地址，如：127.0.0.1:2222/192.168.1.2:2222

`WeChatServer.exe`：服务端（默认2222端口）

## 下载

`WeChatSetup (2.9.0.112).exe`麻烦进群下载

依赖： [点此下载Api.rar](https://github.com/FRz2one/WeChatApi/blob/master/Api.rar)

# Http通信

用户（调用HTTP api）>`WeChatServer.exe`>微信

> 以下内容基于`WeChatServer.exe`通信
## [文档](https://github.com/FRz2one/WeChatApi/blob/master/WeChat%20API.md)
[文档](https://github.com/FRz2one/WeChatApi/blob/master/WeChat%20API.md)
## Demo

### Java

感谢静夜思贡献[Java客户端代码](https://gitee.com/sglmsn/wechat-api)

### Python

> 自动同意好友并且回复内容，关键词拉人进群demo

```python
import requests
import  json
#import qrcode
import random
import threading
import time
import websocket
# -*- coding: utf-8 -*-

x="""回复12300邀请进狗子微信Api交流群"""#同意好友回复内容
dict0={"12300":"12312312388@chatroom"}#关键词邀请字典
#接口
def WeChatPost(dict):
    url = "http://127.0.0.1:2222/"
    header = {"Content-Type": "application/json"}
    body_1 =dict
    body_1 = str(body_1)
    body_1 = bytes(body_1, encoding="utf-8")
    response_1 = requests.post(url, data=body_1, headers=header)
    return response_1.content.decode('utf-8')
    pass
def Agreed(message):
    message0=message["content"]
    print(message0)
    WeChatPost({"funid": 51, "WeChatID": message["WeChatID"], "v1": message0[message0.find("encryptusername=") + 17:message0.find("fromnickname") - 2],"v4": message0[message0.find("ticket=") + 8:message0.find("opcode=") - 2]})
    time.sleep(2)
    WeChatPost({"funid":20,"WeChatID":message["WeChatID"],"wxid":message0[message0.find("fromusername") + 14:message0.find("fromusername") + 33],"content":x}) #发送同意好友回复
    pass
def msg(message):
    if message["wxid"].find("wxid") != -1 :
        for (key, value) in dict0.items():
            if key==message["content"] :
                WeChatPost({"funid": 32, "WeChatID": message["WeChatID"], "wxid": value,"wxidlist": [message["wxid"]]})
                pass
            pass
        pass
    pass

#Websocket回调和连接
def on_message(ws, message): #处理逻辑
    message_0 = json.loads(message)
    if message_0["type"]==1:
        threading.Thread(target=msg, args=(message_0,)).start()  # 线程执行消息处理函数

    if message_0["type"]==37: #好友申请
        print("好友申请")
        threading.Thread(target=Agreed, args=(message_0,)).start() #线程执行同意好友并回复消息函数
        pass

def on_error(ws, error):
    print(ws)
    print(error)
def on_close(ws):
    print(ws)
    print("### closed ###")
websocket.enableTrace(True)
ws = websocket.WebSocketApp("ws://127.0.0.1:2222/",on_message=on_message,on_error=on_error,on_close=on_close)
ws.run_forever()
```
# WebSocket通信

用户搭建服务端直接跟微信通信

> 自己搭建一个WebSocket服务端，微信客户端数据以json的格式发送给服务端，优点：多端群控更方便
>
> 服务端丢给客户端的数据应包含[Http文档](https://frz2one.github.io/Api.html)中JSON对应的Name字段（除`WeChatID`外）
>
> WeChat2.dll给你的JSON数据Name解释：
>
> > `funid:10001`：微信客户端信息（微信登录二维码；登录后会发送微信号、wxid等json数据）
> > `funid:10002`：微信客户端接收到的消息
>
> `robot_wxid`：微信客户端的账号wxid（未登录则没有或为空）
>
> `dwid`：标识符，如果服务端发送的json给客户端有dwid，客户端也会返回对应的dwid（否则dwid:0）
>
> `code`：为1为执行成功（不代表发送成功），否则有问题
# 更新日志

> 变得更牛逼了

0818：更新了

# 交流

请扫码微信加好友，到时我会拉入群，备注：bot。空了我会处理的。

![微信号：frz2one](https://cdn.jsdelivr.net/gh//FRz2one/WeChatApi/qrcode.png)
