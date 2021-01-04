[toc]

# 微信机器人 `WebApi`

> 服务端端口默认2222，header为：Content-Type:application/json
>
> 本文档需启动`WeChatServer.exe`

|    名称    | 类型 |                            *说明*                            |
| :--------: | :--: | :----------------------------------------------------------: |
|  `funid`   | int  |                           函数的ID                           |
| `WeChatID` | int  |                      进程微信的标识符ID                      |
|   `wxid`   | str  | 微信原始ID/群ID，如：**wxid_cxkcxkcxkcxkSB**/**12345678937@chatroom** |

> > 返回`{"code":0}`多指参数错误；返回`{"code":1}`代表代码执行成功，不代表发送成功，需要自行鉴定参数值是否正确

# `0x1`登录

## 初始化

`打开WeChatServer.exe`

## 获取当前启动微信数目和登录信息

```json
{"funid":10}
```

> 如果未登录，会返回二维码地址，生成二维码微信扫码即可登录

## 创建微信

```
{"funid":11}
```

> 无返回值，调用**获取当前启动微信数目和登录信息**查看

## 退出登录

```
{"funid":12,"WeChatID":1}
```

# `0x2`消息

## 发送文本消息

```
{"funid":20,"WeChatID":1,"wxid":"filehelper","content":"你好[耶]"}
```

|   *名称*   | 类型 |                            *说明*                            |
| :--------: | :--: | :----------------------------------------------------------: |
| `wxidlist` | str  | 可选，群艾特的wxid数组，文本内容需带@名字，可用`查询信息`获取名字 |

```
{"funid":20,"WeChatID":1,"wxid":"filehelper","content":"@cxx 你好[耶]","wxidlist":["wxid_cxkcxkcxkcxkSB"]}
```

## 发送xml数据（小程序/链接）

|  *名称*   | 类型 |                       *说明*                        |
| :-------: | :--: | :-------------------------------------------------: |
|  `type`   | int  |                 5为链接，21为小程序                 |
| `content` | str  | xml数据/附加数据，注意转义！！！只需要把content转义 |

```
{"funid":21,"WeChatID":1,"wxid":"filehelper","content":"<msg><fromusername>wxid_g68tncvydroh22</fromusername><scene>0</scene><commenturl></commenturl><appmsg appid=\"\" sdkver=\"0\"><title>m4v</title><des>https://m.baidu.com/s?from=1000539d&amp;word=m4v</des><action>view</action><type>5</type><showtype>0</showtype><content></content><url>https://m.baidu.com/s?from=1000539d&amp;word=m4v</url><dataurl></dataurl><lowurl></lowurl><thumburl></thumburl><messageaction></messageaction><recorditem><![CDATA[]]></recorditem><weappinfo><pagepath></pagepath><username></username><appid></appid><appservicetype>0</appservicetype></weappinfo><extinfo></extinfo><sourceusername></sourceusername><sourcedisplayname></sourcedisplayname><commenturl></commenturl><appattach><totallen>0</totallen><attachid></attachid><emoticonmd5></emoticonmd5><fileext></fileext><cdnthumburl>3054020100044d304b02010002046b55e22802032f57260204ce51277702045e95bcc10426777869645f673638746e63767964726f683232315f6d73655f313538363837313438383837370204010800050201000400</cdnthumburl><aeskey>456e10bba42c4ba0242e5ff39de8d646</aeskey><cdnthumbaeskey>456e10bba42c4ba0242e5ff39de8d646</cdnthumbaeskey><cdnthumblength>5154</cdnthumblength><cdnthumbheight>234</cdnthumbheight><cdnthumbwidth>234</cdnthumbwidth></appattach><websearch /></appmsg><appinfo><version>1</version><appname>Window wechat</appname></appinfo></msg>","type":5}
```

小程序：

```
{"funid":21,"WeChatID":1,"wxid":"filehelper","content":"","type":21}
```

## 发送名片

```
{"funid":22,"WeChatID":1,"wxid":"filehelper","content":"<?xml version=\"1.0\"?><msg bigheadimgurl=\"http://wx.qlogo.cn/mmhead/ver_1/JGSU7YZiaAdhyALl6UftpJBLHfbwIHfUhmmcR9LW3OZvctLLFNic43aK4aPU8nN3MNeK6qyU7NImeVhiaiamDYOsalLYyyOrcA3S7NvLGtmHzZE/0\" smallheadimgurl=\"http://wx.qlogo.cn/mmhead/ver_1/JGSU7YZiaAdhyALl6UftpJBLHfbwIHfUhmmcR9LW3OZvctLLFNic43aK4aPU8nN3MNeK6qyU7NImeVhiaiamDYOsalLYyyOrcA3S7NvLGtmHzZE/132\" username=\"填写WXID\" nickname=\"蔡徐坤\" fullpy=\"aoliao\" shortpy=\"\" alias=\"19991203\" imagestatus=\"2\" scene=\"17\" province=\"福建\" city=\"中国\" sign=\"\" sex=\"1\" certflag=\"0\" certinfo=\"\" brandIconUrl=\"\" brandHomeUrl=\"\" brandSubscriptConfigUrl= \"\" brandFlags=\"0\" regionCode=\"CN_Fujian_Sanming\" />
"}
```

## 发送图片

```
{"funid":23,"WeChatID":1,"wxid":"filehelper","content":"写路径，如C:\\Users\\1\\Desktop\\1.png"}
```

## 发送文件

```
{"funid":24,"WeChatID":1,"wxid":"filehelper","content":"写路径，如C:\\Users\\1\\Desktop\\1.mp4"}
```

## 消息回调

Socket连接，接收消息会发送到客户端

> 格式为 ：
>
> ```
> {wxid: "91213123123@chatroom", content: "你好", attach: "wxid_1234560swv12", type: 1, WeChatID: 1}
> ```
>
> 

# `0x3`群相关

## 群创建

```
{"funid":30,"WeChatID":1,"wxidlist":["filehelper","filehelper"]}
```

注明：群创建至少选择两个人，为了不必要的问题发生，不提供群ID返回值，请执行前备份好友列表对比创建后的好友列表

## 群邀请/大群邀请

```
{"funid":31/32,"WeChatID":1,"wxid":"12345678937@chatroom",wxidlist":["filehelper","filehelper"]}
```

> 注明：大群邀请（"funid":32），为发送xml邀请，需要用户同意

## 获取群成员列表

```
{"funid":33,"WeChatID":1,"wxid":"12345678937@chatroom"}
```

> 注明：**返回的wxid为群主wxid，wxidlist数组为群成员列表wxid**

## 修改群昵称

```
{"funid":34,"WeChatID":1,"wxid":"12345678937@chatroom","content":"写昵称，如我爱蔡徐坤"}
```

## 群公告

```
{"funid":35,"WeChatID":1,"wxid":"12345678937@chatroom","content":"这是群公告"}
```

## 移除群成员

```
{"funid":36,"WeChatID":1,"wxid":"12345678937@chatroom","wxidlist":["filehelper"]}
```

## 退出群聊

```
{"funid":37,"WeChatID":1,"wxid":"12345678937@chatroom"}
```

#`0x4`好友相关

## 获取好友列表

```
{"funid":40,"WeChatID":1}
```

> 包含微信、微信群、公众号等列表信息

## 添加好友

```
{"funid":41,"WeChatID":1,"wxid":"wxid_cxkcxkcxkcxkSB","content":"发送添加好友内容","type":14}
```

> 注明：群添加的type为14

## 删除好友

```
{"funid":42,"WeChatID":1,"wxid":"wxid_cxkcxkcxkcxkSB"}
```

## 查询信息

```
{"funid":43,"WeChatID":1,"wxid":"wxid_cxkcxkcxkcxkSB"}
```

# `0x5`其他功能

## 收款

```
{"funid":50,"WeChatID":1,"wxid":"wxid_cxkcxkcxkcxkSB","transferid":"cxkcxkcxkcxkcxkcxkcxkcxkcxkcxkcxkcxkcxkcxkcxkcxkcxkcxkcxkcxkcxkcxkcxkcxkcxkcxkcxkcxkcxkcxkcxkcxkcxkcxkcxkcxkcxkcxkcxkcxk"}
```

## 同意好友请求

```
{"funid":51,"WeChatID":1,"v1":"cxkcxkcxkcxkcxkcxkcxkcxkcxkcxkcxkcxkcxkcxkcxkcxkcxkcxkcxkcxkcxkcxkcxkcxkcxkcxkcxkcxkcxkcxkcxkcxkcxkcxkcxkcxkcxkcxkcxkcxk","v4":"cxkcxkcxkcxkcxkcxkcxkcxkcxkcxkcxkcxkcxkcxkcxkcxkcxkcxkcxkcxkcxkcxkcxkcxkcxkcxkcxkcxkcxkcxkcxkcxkcxkcxkcxkcxkcxkcxkcxkcxk"}
```

##打开浏览器

```
{"funid":52,"WeChatID":1,"url":"http://baidu.com"}
```
##浏览器当前URL

```
{"funid":54,"WeChatID":1}
```

## 从网络获取图片/视频/文件

```
{
    "funid": 53,
    "alen": 624897,
    "Orig": 1,
    "url": "3058020100044c304a020100020461b76c6c02032f565d02042ec9a3b402045fe989330425617570696d675f633965643037613531356264623236315f31363039313430353331373838020401052a010201000405004c54a100",
    "aeskey": "307da8c216404e08ea7de5f57b6e19da"
}
```

