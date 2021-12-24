# Laravel-echo-server

<a href="https://learnku.com/laravel/t/13101/using-laravel-echo-server-to-build-real-time-applications">參考</a>

錯誤
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
解決
更新到最新版本
I solved it by upgrade node to latest version:
```
sudo npm install n -g
sudo n latest
```
