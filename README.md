# websocket-quickstart
WebSocket技术通常应用于需要服务器端快速向客户端（如浏览器）发送消息的场景。这些场景包括但不限于：

1. **网页游戏**：允许实时的多玩家互动和数据同步[1]。
2. **视频网站**：实现如弹幕等实时交互功能[1]。
3. **在线文档和协同编辑**：多用户可以实时编辑同一文档，共享即时更新[1][3]。
4. **运维工具**：实时监控和管理系统状态[1]。
5. **基于位置的应用**：如共享经济服务中的实时位置更新[2]。
6. **体育实况更新**：比如实时更新比分和比赛情况[2]。
7. **股票和基金报价实时更新**：快速传递市场变动信息[2]。
8. **聊天功能**：实现即时通讯，用户可以实时发送和接收消息[3]。
9. **实时数据展示**：如股票行情、实时天气等信息的实时更新[3]。
10. **微信小程序**：例如实现剪刀石头布等小游戏，需要实时同步玩家动作[4]。

WebSocket技术由于其全双工通信的特性，非常适合这些需要快速、实时交互的应用场景。

Citations:
[1] https://www.51cto.com/article/775910.html
[2] https://juejin.cn/post/7080511037320986660
[3] https://juejin.cn/post/7279290395737587712
[4] https://cloud.tencent.com/developer/article/1004362
[5] https://blog.csdn.net/m0_73687324/article/details/130650250
[6] https://developer.aliyun.com/article/1309727
[7] https://cloud.baidu.com/article/2896614

-----

开发一个web端的最简单的即时通讯应用通常涉及到前端和后端的开发。在这里，我将提供一个使用WebSocket技术的基本示例，因为WebSocket提供了一种在单个连接上进行全双工、双向交互的方式。这个例子将包括一个简单的服务器和客户端，使用JavaScript和Node.js。

### 服务器端（Node.js）

首先，你需要安装Node.js环境以及`ws`库，后者是一个WebSocket库，用于Node.js服务器。

1. 安装Node.js: 访问[Node.js官网](https://nodejs.org/)下载并安装。
2. 创建一个新的项目文件夹，例如`websocket-demo`。
3. 在该文件夹中打开终端或命令提示符，初始化npm并安装`ws`库：

   ```bash
   npm init -y
   npm install ws
   ```

4. 创建一个名为`server.js`的文件，并添加以下代码：

   ```javascript
   const WebSocket = require('ws');
   const wss = new WebSocket.Server({ port: 8080 });

   wss.on('connection', function connection(ws) {
       ws.on('message', function incoming(message) {
           console.log('received: %s', message);
           // 将消息广播回所有客户端
           wss.clients.forEach(function each(client) {
               if (client.readyState === WebSocket.OPEN) {
                   client.send(message);
               }
           });
       });

       ws.send('你已连接到服务器');
   });

   console.log('WebSocket server is running on ws://localhost:8080');
   ```

   这段代码创建了一个WebSocket服务器，监听8080端口。服务器接收到消息后会将其打印出来，并将相同的消息发送回所有已连接的客户端。

### 客户端（HTML + JavaScript）

1. 在同一项目文件夹中，创建一个名为`index.html`的文件，并添加以下代码：

   ```html
   <!DOCTYPE html>
   <html lang="en">
   <head>
       <meta charset="UTF-8">
       <meta name="viewport" content="width=device-width, initial-scale=1.0">
       <title>WebSocket Demo</title>
   </head>
   <body>
       <input type="text" id="messageInput" placeholder="输入消息">
       <button onclick="sendMessage()">发送</button>
       <ul id="messages"></ul>

       <script>
           var ws = new WebSocket('ws://localhost:8080');
           var messages = document.getElementById('messages');

           ws.onmessage = function (event) {
               var message = document.createElement('li');
               message.textContent = event.data;
               messages.appendChild(message);
           };

           function sendMessage() {
               var input = document.getElementById('messageInput');
               ws.send(input.value);
               input.value = '';
           }
       </script>
   </body>
   </html>
   ```

   这个HTML页面包含一个文本输入框和一个按钮，用于发送消息。它还包含一个列表，用于显示从服务器接收到的消息。

### 运行示例

1. 在服务器文件夹的终端中运行`node server.js`启动WebSocket服务器。
2. 在浏览器中打开`index.html`文件。

现在，你可以在文本框中输入消息，并点击发送按钮。消息将发送到服务器，然后服务器将消息广播回客户端，你将看到它出现在网页上。

这个示例展示了如何使用WebSocket在Web端创建一个简单的即时通讯应用。你可以根据需要扩展和修改这个基本的框架，例如添加用户身份验证、支持更复杂的消息类型等。

Citations:
[1] https://www.ruanyifeng.com/blog/2017/05/websocket.html
[2] https://www.liaoxuefeng.com/wiki/1252599548343744/1282384966189089
[3] https://www.jb51.net/article/237354.htm
[4] https://zenn.dev/nameless_sn/articles/websocket_tutorial
[5] https://apidog.com/jp/blog/python-websocket-tutorial/
[6] https://respond.io/zh/blog/top-messaging-apps
[7] https://developer.mozilla.org/ja/docs/Web/API/WebSockets_API/Writing_WebSocket_client_applications
[8] https://qiita.com/att55/items/da663f6e713c3bd073e8
[9] http://www.52im.net/thread-338-1-1.html
[10] https://press.monaca.io/atsushi/435
[11] https://sleekflow.io/zh-cn/blog/%E5%8D%B3%E6%97%B6%E9%80%9A%E8%AE%AF
[12] https://www.nxrte.com/jishu/im/32329.html
[13] https://blog.csdn.net/shadow_zed/article/details/123859651
[14] https://www.cm.com/zh-cn/blog/10-popular-ott-channels-in-oversea-market-of-2023/
[15] https://www.tiocloud.com
[16] https://www.sejuku.net/blog/70583
[17] https://www.workplus.io/h-nd-1975.html
[18] https://www.easemob.com/news/7416
[19] https://blog.csdn.net/zhoukaibai/article/details/130720937
[20] https://developer.aliyun.com/article/1196156
