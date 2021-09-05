Laya开发常用配置
==
[toc]

##### chrome调试启动打开devtools参数 "--auto-open-devtools-for-tabs"
.vscode文件夹下，launch.json中"runtimeArgs"增加参数 **"--auto-open-devtools-for-tabs"**

##### 启用promise,await/async
在tsconfig.json 中增加参数
```js
"compilerOptions": {
    ...
    "importHelpers": true,
    "lib":[
      "es5",
      "dom",
      "es2015.promise"
    ]
  }
  ...
```