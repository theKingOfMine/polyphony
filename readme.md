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
```


# 12，vue技术

    1，npm install vue 下载vue包，会在node_modules文件夹里
    2，引包 在html 文件里通过 script引入vue.js 在body html代码的最后
```js
    3，创建实例化对象 

    <script src="../node_modules/vue/dist/vue.js"></script>
    <script>
        const app = new Vue({
            el: '.a',
            data: {
                msg: '马吉春是世界之神'
            }
        })
    </script>

    4, 插值表达式
    {{表达式}}
    - 表示对象要有空格{{ {name:'马吉春'} }}
    - 字符串也可以 {{'马吉春是世界之神'}}
    - 判断后的布尔值 {{ 1==1 }}
    - 三元表达式 {{ true?'正确的':'错误的'}}
        <div class="a">{{msg.split('').reverse().join('')}}</div>

    5，vue常用v-指令

        v-text：元素innerText属性，必须是双标签，跟{{}}效果一样，使用较少
        v-html:元素的innderHTML
        v-if: 判断是否插入这个元素，相当于对元素的销毁和创建
        v-else-if
        v-else
        v-show 会给元素的style加上display:none
        v-bing 绑定属性 可以用简写 : 只有一个冒号
        v-on 绑定点击事件 v-on：原生的事件名='函数名'  简写@
        ## 示范

                     const app = new Vue({
                            el: '#app',
                            data: {
                                msg: '马吉春是世界之神',
                                africa: "每年7月，我都会想到肯尼亚的马拉河。现在面对着河里的那些成群结对虎视眈眈的鳄鱼和河马们，迁徙的角马们还是会不顾一切的纵身一跃吧。",
                                theKing: `<div>我好喜欢你呀</div>`,
                                isThatYou: true,
                                isActive : false,
                                url: 'https://www.duoyinchina.com/article/东非动物大迁徙.files/东非动物大迁徙1953.png',
                                g: [
                                    {
                                        id: 1,
                                        name: '冰清玉洁',
                                        level : '1',
                                    },
                                    {
                                        id: 2,
                                        name: '泰3',
                                        level : '2',
                                    },
                                    {
                                        id: 3,
                                        name: '泰1',
                                        level : '3',
                                    }
                                ],
                                zidian: {
                                        id: 165464,
                                        name: '马吉春',
                                        level : '世界第一',
                                    }

                                },
                                template:`
                                        <div class="app">
                                            <h3>v-text示范</h3>
                                            <div v-text='africa'></div>
                                            <div class="line"></div>
                                            <h3>v-html示范</h3>
                                            <div v-html='theKing'></div>
                                            <div class="line"></div>
                                            <h3>三元运算示范</h3>
                                            <div class="a">{{msg.split('').reverse().join('')}}</div>
                                            <div class="line"></div>
                                            <h3>v-if示范</h3>
                                            <div v-if='Math.random()>0.5'>是的，我在呢</div>
                                            <div v-else>是的，我并不在</div>
                                            <div class="line"></div>
                                            <h3>v-show示范</h3>
                                            <div v-show='isThatYou'>当时间像风一样飞过黑夜</div>
                                            <div v-show='!isThatYou'>当时间像风一样飞过黑夜</div>
                                            <div class="line"></div>

                                            <h3>v-bind:class示范 v-bind可以绑定所有属性</h3>
                                            <div class="bind" v-bind:class={active:isActive}></div>
                                            <div class="line"></div>
                                            <h3>v-bind 其他属性示范 简写方式只用一个 : v-bind可以绑定所有属性</h3>
                                            <img :src="url" style="width:100%">
                                            <div class="line"></div>
                                            <h3>v-on示范 绑定事件</h3>
                                            <div v-bind:class={active:isActive} v-on:click='clickHandler'>当时间像风一样飞过黑夜</div>
                                            <div class="line"></div>
                                            <h3>v-for示范</h3>
                                            <ul>
                                                <li v-for='(girl, index) in g'>
                                                <h2>{{girl.name}}</h2>
                                                <h2>{{index}}</h2>
                                                </li>
                                            </ul>

                                            <ul>
                                                <li v-for='(value, key) in zidian'>
                                                <h2>{{key}} === {{value}}</h2>
                                                </li>
                                            </ul>
                                        </div>
                                    `,

                                    methods: {
                                        clickHandler(e){
                                            console.log(this);
                                            this.isActive = !this.isActive;
                                        }
                                    },
                            })

    6,vue的双向数据绑定 

            <div id="app">
            <h2> v-model只能用在有value属性的标签上，它是v-bind：value和v-on=input的语法糖</h2>
            <input type="text" v-model="msg">
            <h3>{{msg}}</h3>
            <div class='line'></div>
            <input type="text" v-bind:value='msg' v-on:input='valueChange'>
            <h3>{{msg}}</h3>
            </div>
            <script src="../node_modules/vue/dist/vue.js"></script>
            <script>
                const app = new Vue({
                                el: '#app',
                                data:{
                                    msg: '马吉春是世界之神'
                                },
                                methods: {
                                    valueChange(e){
                                        this.msg = e.target.value
                                    }
                                },
                })
            </script>
```
    7,局部组件
```js
            <div id="app">
            </div>
                <script src="../node_modules/vue/dist/vue.js"></script>
                <script>
                    // 1，组件的声明
                    var App = {
                            data(){
                                return{
                                    
                                }
                            },
                            methods: {
                                clickhandler(e){
                                    console.log(this);  
                                }
                            },  
                            template:`
                                <div>
                                    马吉春是世界之神
                                    <button @click='clickhandler'></button>
                                </div>
                                `
                            }
                            const app = new Vue({
                                    el:'#app',
                                    data() {
                                        return {
                                            
                                        }
                                    },
                                    // 2，组件的挂载
                                    components:{
                                        App
                                    },
                                    //3，组件的使用
                                    template: `
                                        <App></App>

                                    `
                                    })
                                </script>
```
    8,全局组件演示
            //全局组件
    Vue.component('Vbtn', {
    template:  `
        <button>我是全局组件按钮,马吉春是世界之神</button>
    `
    })


        局部组件
        var Vheader = {
            template:  `
            <div>
                <Vbtn />
                <div>我是头部组件</div>
                <Vbtn />
                <Vbtn />
    
            </div>
            `
        }

        var Vside = {
            template:`<div>我是侧边栏组件<Vbtn /></div>`
        }

        var Vcontent = {
            template:  `
            <div>我是内容组件<Vbtn /></div>
            `
        }

        var App = {
                template: `
                    <div> 
                        
                        <Vheader />
                        <div>
                        <Vside />
                        <Vcontent />
                        </div>
                    </div>
                `,
                components:{
                    Vheader,
                    Vside,
                    Vcontent
                }
            };

        const app = new Vue({
            el: '#app',
            data() {
                return {
                    
                }
            },
            components:{
                App
           
            },

            template:`
            <App />
           
            `
        })


    </script>
    
    9，组件之间的传值
```js
        代码演示
            1，父组件可以直接给子组件传值，子组件通过props接收，子组件给父组件传值需要定义一个自定义函数。

    <body>
        <div id="app">马吉春是世界之神</div>
        <script src="../node_modules/vue/dist/vue.min.js"></script>
        <script>
            /*  组件数据间的传值 1，先给父组件中绑定自定义属性
                            2，在子组件中使用props接受负组件的数据


            */
            Vue.component('Parent', {
                data() {
                    return {
                        father: '真几把吓人'
                    }
                },
                template:`
                    <div>
                        <p>我是父组件</p>
                        // childData这个变量将值传给了子组件，//@childHandler这个方法，将子组件的值传给了父组件
                        <Child :childData='father' @childHandler='childHandler'/>
                    </div>
                `,
                methods: {
                    childHandler(val){
                        console.log(val);
                    }
                }
            })

            Vue.component('Child', {
                template:`
                    <div>
                        <p>我是孩子组件</p>
                        <h2>{{childData}}</h2>
                        <input type='text' @input = 'changeValue(childData)' v-model='childData'/>
                    </div>
                `,
                //用来接受组件传值
                props:[
                    'childData'
                ],
                methods: {
                    changeValue(val){
                        this.$emit('childHandler', val)
                    }
                },
            
            })

            const app = new Vue({
                el: '#app',
                data() {
                    return {
                        
                    }
                },  
                template: `
                    <Parent />
                
                `
                

            })

        </script>
    </body>
```
10，插槽的应用
```js
        <body>
    <div id="app"></div>


    <script src="../node_modules/vue/dist/vue.min.js"></script>
    <script>
        //全局组件 插槽的用法 <slot>标签
        Vue.component('Vbtn', {
            template: `
                <button class='btn' :class='type'>
                    <slot></slot>    
                </button>
            `,
            props: ['type']
        })

        var Vcontent = {
            template: `
                <div>
                    <h2>我是内容组件</h2>
                    <Vbtn>登陆</Vbtn>
                    <Vbtn type='active'>注册</Vbtn>
                </div>
            `
        }

        const app = new Vue({
            el: '#app',
            data() {
                return {
                    
                }
            },
            components: {
                Vcontent
            },
            template: `
                <Vcontent />
            
            `



            })


        </script>
    </body>

    插槽 2

    <body>
    <div id="app"></div>


    <script src="../node_modules/vue/dist/vue.min.js"></script>
    <script>
        //全局组件 插槽的用法 <slot>标签
        Vue.component('myLi', {
            template: `
                <li>
                    <slot name='two'></slot>
                    <slot name='three'></slot>
            
                </li>
            `,
            props: ['type']
        })

        var Vcontent = {
            template: `
                <div>
                    <ul>
                        <myLi>
                        <h2 slot='two'>我草泥马，我有点不开心</h2>
                        <h2 slot='three'>我草泥马，我有点不开心3</h2>
                        </myLi>
                    </ul>
                </div>
            `
        }

        const app = new Vue({
            el: '#app',
            data() {
                return {
                    
                }
            },
            components: {
                Vcontent
            },
            template: `
                <Vcontent />
            
            `
        })
        </script>
    </body>
```
    11， 过滤器,专门修理字符串的东西
```js

    <body>
        <div id="app">马吉春是世界之神</div>
        <script src="../node_modules/vue/dist/vue.min.js"></script>
        <script>
        

            //过滤器的作用，为页面中的数据进行添油加醋的作用，有两种：
            
            //全局过滤器，局部过滤器；// 过滤器 filters:{}

            //1，全局过滤器
            Vue.filter('my', function (value) {
                    return value + '，我听说过这句话，当初，我只知道，马吉春是个传说，没想到，他又出现在这个世界上啦！！！'
                    })


            
            /*
            1,声明过滤器，
            2，{{数据 ｜ 过滤器的名字}}
            */

            var App = {
                data() {
                    return {
                        price: 0,
                        msg: '神之界世是春吉马 卧槽居然成功了'
                    }
                },
                template: `
                    <div>
                        <h1>马吉春，我会把你照顾好的</h1>    
                        <input type='number' v-model='price'/>
                        <h3>{{price | myCurrentcy}}</h3>
                        <h3>{{msg | my}}</h3>
                    </div>
                `,
                filters: {
                    //声明过滤器
                    myCurrentcy: (val)=>{
                        return '¥' + val
                    },
                    myReverse: (val)=>{
                        return val.split('').reverse().join('');
                    }            
                }


            }
            //入口
            const app = new Vue({
                    el: '#app',
                    data() {
                        return {
                        }
                    },
                
                    template: `
                        <App></App>
                    `,
                    
                    components:{
                        App
                    }
                })

        </script>
    </body>
