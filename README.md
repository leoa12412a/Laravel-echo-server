# 建構基於Laravel-echo-server的Websocket

## 安裝Laravel-echo-server

安裝參考<a href="https://learnku.com/laravel/t/13101/using-laravel-echo-server-to-build-real-time-applications">這裡</a>

使用NPM安裝laravel-echo-server
```
npm install -g laravel-echo-server
```

開啟Laravel新專案，若已經安裝好專案可以跳過這個步驟
```
composer create-project --prefer-dist laravel/laravel echo-test
```

laravel-echo-server會使用到redis，所以要安裝redis
```
sudo yum install redis
```
安裝好執行redis
```
systemctl start redis
```
redis的port的是6379，在防火牆開啟這個port
```
firewall-cmd --add-port=6379/tcp --permanent
firewall-cmd --reload
```

安裝<a href="https://github.com/predis/predis">Predis</a>
```
composer require predis/predis
```

安裝好開始初始化laravel-echo-server
```
laravel-echo-server init
```
初始化會有一些關於Socket的配置，根據自行的需求配置
```
[root@localhost html]# laravel-echo-server init
? Do you want to run this server in development mode? No
? Which port would you like to serve from? (6001)
[root@localhost html]# laravel-echo-server init
? Do you want to run this server in development mode? Yes
? Which port would you like to serve from? 6001
? Which database would you like to use to store presence channel members? redis
? Enter the host of your Laravel authentication server. http://localhost
? Will you be serving on http or https? http
? Do you want to generate a client ID/Key for HTTP API? No
? Do you want to setup cross domain access to the API? Yes
? Specify the URI that may access the API: http://localhost:80
? Enter the HTTP methods that are allowed for CORS: GET, POST
? Enter the HTTP headers that are allowed for CORS: Origin, Content-Type, X-Auth-Token, X-Requested-With, Accept, Authorizatio
n, X-CSRF-TOKEN, X-Socket-Id
? What do you want this config to be saved as? laravel-echo-server.json
Configuration file saved. Run laravel-echo-server start to run server.
```
第一次執行時遇到錯誤
```
[root@localhost html]# laravel-echo-server init
/usr/lib/node_modules/laravel-echo-server/node_modules/chalk/source/index.js:106
        ...styles,
        ^^^
SyntaxError: Unexpected token ...
    at Object.exports.runInThisContext (vm.js:76:16)
    at Module._compile (module.js:542:28)
    at Object.Module._extensions..js (module.js:579:10)
    at Module.load (module.js:487:32)
    at tryModuleLoad (module.js:446:12)
    at Function.Module._load (module.js:438:3)
    at Module.require (module.js:497:17)
    at require (internal/module.js:20:19)
    at Object.<anonymous> (/usr/lib/node_modules/laravel-echo-server/node_modules/inquirer/lib/objects/separator.js:2:13)
    at Module._compile (module.js:570:32)
```
解決方法
更新到最新版本
I solved it by upgrade node to latest version:
```
sudo npm install n -g
sudo n latest
```

配置完成後就可以執行laravel-echo-server
```
laravel-echo-server start
```
執行成功後就會如下圖
```
[root@localhost html]# laravel-echo-server start

L A R A V E L  E C H O  S E R V E R

version 1.6.2

⚠ Starting server in DEV mode...

✔  Running at localhost on port 6001
✔  Channels are ready.
✔  Listening for http events...
✔  Listening for redis events...

Server ready!
```
若Redis沒有安裝會有以下問題
```
[root@localhost html]# laravel-echo-server start

L A R A V E L  E C H O  S E R V E R

version 1.6.2

⚠ Starting server in DEV mode...

✔  Running at localhost on port 6001
✔  Channels are ready.
✔  Listening for http events...
[ioredis] Unhandled error event: Error: connect ECONNREFUSED 127.0.0.1:6379
    at TCPConnectWrap.afterConnect [as oncomplete] (node:net:1157:16)
[ioredis] Unhandled error event: Error: connect ECONNREFUSED 127.0.0.1:6379
    at TCPConnectWrap.afterConnect [as oncomplete] (node:net:1157:16)
[ioredis] Unhandled error event: Error: connect ECONNREFUSED 127.0.0.1:6379
    at TCPConnectWrap.afterConnect [as oncomplete] (node:net:1157:16)
```

## 配置 Laravel 使 Laravel Echo Server 正常工作

於/config/app.php 備註 App\Providers\BroadcastServiceProvider::class 這行
```
//App\Providers\BroadcastServiceProvider::class,
```
於/config/app.php 取消備註 // 'Redis' => Illuminate\Support\Facades\Redis::class,
```
'Redis' => Illuminate\Support\Facades\Redis::class,
```

修改 .env 的 關於redis的配置
```
BROADCAST_DRIVER=redis
```
```
QUEUE_CONNECTION=redis
```
```
REDIS_HOST=127.0.0.1
REDIS_PASSWORD=null
REDIS_PORT=6379
```
.env 新增一行
```
REDIS_CLIENT=predis
```

接下來我必須安裝 Socket.io 客戶端和 Laravel-Echo 包，你可以通過以下方法安裝
```
sudo npm install --save socket.io-client
sudo npm install --save laravel-echo
```
