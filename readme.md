# git技术


1，版本管理 git 命令：
    
    1- git init 初始化，
    git init 初始化，确立git文件夹，工作文件夹
    git status 查看目前状态是否有需要放入版本库临时文件区的文件
    git add “文件名” 将文件放入版本库临时区
    git add . 将全部文件放入临时区域
    git commit -m "版本名称"      将临时区文件提交到版本库
    git remote add origin https://github.com/theKingOfMine/nodeLearning.git 将文件夹对应到远端的哪一个仓库
    git branch 分支号  创建分支
    git checkout 分支号  转入到要去的分支
    git push 上传
    git push -u origin 分支号   上传一个新分支的东西
    git log 查看所有版本号
    git log --pretty=oneline 只查看一行的版本号
    git reset --hard 版本号 返回版本
    git reflog 所有的恢复操作日志
    git fetch 查看服务器有哪些分支有更新
    git merge origin/分支名  合并分支内容
    git diff 查看分支文件是否有冲突

    2- 增删改查

    git rm 删除文件
    git rm -r 删除文件夹




# node.js技术


1，创建一个服务器 http模块

```js
    const http = require('http');
    http.createServer((req, res)=>{
        //req 获取客户端传过来的东西
        //res 给浏览器回应的信息
        console.log(req.url);
        //设置相应头  解决乱码
        res.write('<head><meta charset="UTF-8"></head>');
        res.writeHead(200, {"Content-type": "text/html;charset=utf-8"});
        res.write("马吉春你好可爱呀");

        res.end("funck you！"); //必须有.end
    }).listen(3000);//端口号

```
2，url模块
```js
    创建url对象
    const url = require('url');
    var api = 'https://www.duoyinchina.com?name=mjc&age=19';

    获取url携带的参数
    var getValue = url.parse(api, true).query;
    console.log(`姓名：${getValue.name}, 年纪：${getValue.age}`);

    示例
    --------
    const http = require('http');
    const url = require('url');

    http.createServer((req, res)=>{
        //req 获取客户端传过来的东西
        //res 给浏览器回应的信息

        //设置相应头  解决乱码 状态码是200， 文件类型是html，字符集是:utf-8
        res.writeHead(200, {"Content-type": "text/html;charset=utf-8"});
        
        if(req.url != '/favicon.ico'){
            var userinfo = url.parse(req.url, true).query;
            console.log(userinfo.name);
        }

        res.end("程序都执行完了！！"); //必须有.end
    }).listen(3000);
------------
```

3, supervisor node的自启动工具 
```js
    使用时在终端用 supervisor js文件名 来启动服务器，这样就可以随着保存自动重启服务器了
```
4， 自定义模块   ---模块分为两大类，核心模块和自定义模块  自定义模块都放入node_moduls文件夹 
```js
    创建新的js文件，将它做成一个模块用export方法导出想要分享的函数或是方法，再使用页，用require方法，通过地址引入需要的模块

    1-----request.js -----  module.export使用方法

        var obj = {
            get: function(){
                    console.log("从服务器获取数据");
                },
            post: function(){
                    console.log("提交数据");
                }
        }

        module.exports = obj;


    app.js-----
        const request = require('./module/request.js');
        console.log(request);

    2-----tools.js     exports 使用方法
        function formatUrl(adr){
            return "https://www.duoyinchina.com" + adr;
        }

        exports.formatUrl = formatUrl;

    ------app.js------
            var api=tools.formatUrl('他，这个世界的神是！！！：');
                console.log(api);


    //引入自定义模块 tools 和 request、 axios 等多种引入方法
    const tools = require('./module/tools.js');
    const request = require('./module/request.js');
    const axios = require('./module/axios/index.js');
    //如果自定义模块所在的文件夹是在文件夹为node_module下，同时文件本身被命名为index.js那么就可以直接用文件夹名称来一句话引入
    const db = require('db');

    console.log(db, 12);
    db.add();

    console.log(tools);
    console.log(request);
    request.get();
    request.post();
    axios.get();


    ******  一句话引入  npm init --yes 配置package.json文件 ******
    如果自定义模块所在的文件夹是在文件夹为node_module下，同时文件本身被命名为index.js那么就可以直接用文件夹名称来一句话引入,
    如果文件名不是index.js可以引入package.json文件，设置文件入口

    const db = require('db');
    console.log(db, 12);
    db.add();
```
5,下载包 npm 详解 
```js

    npm install 包名 --save     https://www.npmjs.com 下载这个网站里可以用的包
    npm install md5 --save 这样存可以自动在package.json文件里，记录了你下载了什么包，如果删除了node_modules，可以用npm i 下载所有的需要下载的包。
    现在最新的不需要写--save了，但是写了保险

    npm i 安装所依赖的包，在package.json里有记录的
    npm uninstall ModuleName 删除包的命令
    npm list 查看文件夹下的包列表
    npm info ModuleName 查看包信息
    npm install ModuleName@包的版本号 装指定版本的包
    npm install -g cnpm --registry=https://registry.npm.taobao.org  安装国内淘宝npm镜像 用cnpm执行上述命令
    cnpm i 包名 --save 下载包的标准语法
```
6，package.json详解
```js
    package.json定义了这个项目所需要的各种模块，以及项目的配置信息，比如（名称，版本，许可证等元数据）
    
    1-创建package.sjon
        npm init 或者 npm init --yes

    2-内容详解
        -dependencies里保存的是项目里所需要用的包
        -devDependencies里保存的是工具？？  用npm install babel-cli --save-dev会存到这个下面
        -版本号 ^ 表示第一位版本号不变后两位取最新,~表示前两位不变，最后以为取最新，*表示全部取最新
```
7,fs 核心模块
```js
    ---fs.stat 检测是文件还是目录

        fs.stat('./package.json', (err,data)=>{
            if(err){
                console.log('错误的问题是：', err);
                return;
            }
            
            console.log(`是文件：${data.isFile()}`);
            console.log(`是目录：${data.isDirectory()}`);

        })


    ---fs.mkdir 创建目录

        三个参数 path 创建目录的路径
                mode 目录的权限，默认为777
                callback 回调，传递异常参数err

        fs.mkdir('lover', (err)=>{
            if(err){
                console.log(err);
                return;
            }
            console.log("创建成功");
        })
    

    ---fs.writeFile 创建写入文件

        创建以及写入文件 如果以前的文件不存在，则创建及写入文件，如果文件存在，则替换

        参数:filename string 文件名称
            data string | buffer 将要写入的内容
            callback   function 回调函数，传递一个一场参数err
            options object 数组对象，包含
                encoding string可选值utf-8
                mode number 文件读写权限，默认438
                flag string 默认值'w'
            

        fs.writeFile('./html/writeFile.html', '马吉春是世界之神', (err)=>{
            if (err){
                console.log('写入失败');
                return;
            }
            console.log('创建写入文件成功');
        })


    ---fs.appendFile 追加文件,再次写入文件 参数都差不多

        fs.appendFile('./html/writeFile.html', '马吉春是世界之神,你信不信吧', (err)=>{
            if (err){
                console.log('写入失败');
                return;
            }
            console.log('写入成功');
        })

    ---fs.readFile 读取文件

        fs.readFile('./html/writeFile.html', (err, data)=>{
            if (err){
                console.log('读取文件失败');
                return;
            }
            console.log(data.toString()); //把Buffer 转化成string类型
        })


    ---fs.readdir 读取目录

        fs.readdir('../../learning', (err, data)=>{
            if (err){
                console.log('失败');
                return;
            }
            console.log(data);  返回一个文件和文件夹的数组信息
        })

    ---fs.rename 功能1，重命名，2，移动文件
        
        参数， 老路径，新路径，回调函数

        fs.rename('./html/writeFile.html', './html/new.html', (err)=>{
            if (err){
                console.log('失败', err);
                return;
            }
            console.log('重命名，还有移动成功'); 
        })

        fs.rename('./html/new.html', './lover/theKing.html', (err)=>{
            if (err){
                console.log('失败', err);
                return;
            }
            console.log('重命名，还有移动成功'); 
        })



    ---fs.rmdir 删除目录

        fs.rmdir('./html', (err)=>{
            if (err){
                console.log('删除失败', err);
                return;
            }
            console.log('删除成功'); 
        })


    ---fs.unlink 删除文件

        fs.unlink('./lover/theKing.html', (err)=>{
        if(err){
            console.log('删除失败');
            return;
        }
        console.log('删除成功');
    })
```
8, mkdirp包的使用 可有创建多级目录
```js
    var mkdirp = require('mkdirp');
    mkdirp('./upload/img/data');
```
9, 异部处理 用回调函数处理异步
```js
    function getData(callback){
        setTimeout(()=>{
            var la = '辣妹';
            callback(la);
        }, 3000)
    }

    getData(function(aaa){
        console.log(aaa);
    })

```
10, 用Promise方法来处理异步

    
```js
    方法一：
    var p = new Promise(function(resolve, reject){
                            setTimeout(()=>{
                                var la = '辣妹';
                                resolve(la);
                            }, 3000)
                        })

                        p.then((data)=>{
                            console.log(data);
                        })

    方法二，用封装的方法：
    function as(resolve, reject){
        setTimeout(()=>{
            var la = '辣妹';
            resolve(la);
        }, 3000)
    }

    var p = new Promise(as);
    p.then((data)=>{
        console.log(data);
    })

```
11, async 和 await 
```js
    await必须得用在async方法里
    function test(){
        return new Promise((resolve, reject)=>{
            setTimeout(function(){
                var n = '马吉春';
                resolve(n);
            }, 2000);
        })
    }

    async function main(){
        var data = await test();
        console.log(data);
    }

    main();