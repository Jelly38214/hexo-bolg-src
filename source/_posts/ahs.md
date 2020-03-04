---
title: VS Code 
date: 2018-08-11 20:34:50
categories:
- vscode
tags:
- vscode 
type: "categories"
---

1. 自定义React组件的在vscode下的智能提示

    要实现上述问题，首先得来解决vscode如何识别`webpack`配置好的路径别名。当你在webpack.config.js里面的alias给你路径配置别名来帮助webpack去识别路径打包，这个别名指向的路径，vscode是不能识别。你得去做的努力来帮助vscode去识别。

     1. 安装插件Path Intellisense
     2. 编写jsconfig.json,[详情点击查看官网](https://code.visualstudio.com/docs/languages/jsconfig);

     我的jsconfig.json
     ```json
        {
            "compilerOptions": {
                "target": "ES6",
                "jsx": "react",
                "experimentalDecorators": true,
                "baseUrl": ".",
                "paths": {
                    "@": "./src"
                }
            },
            "exclude": ["node_modules", "dist"]
        }
     ```

     完成上述步骤，当你在vscode敲"@/components/Icon",它就能给你提示，这个路径是根目录下的src文件夹下的components文件夹下面的Icon文件夹下的index.js，就是`/src/components/Icon`这个完整的路径。

     `注意`：这里有个点需要注意，有些人，可能使用了ts，也就是typescript，然后就会有个tsconfig.json，这时需要注意的是，貌似typescript.json的权限比jsconfig.json高，也就是说，vscode会忽略jsconfig.json里面的配置，只去读tsconfig.json。
     这时也不慌，只要把上述的jsconfig.js文件的compilerOptions直接复制到tsconfig.js就好啦

     完成上述步骤，就完成了vscode对别名路径的提示啦，但，react的智能提示还没完成。你此时，需要写注释啦。

     举个栗子

     ```javascript
        /**
         * arguments xxx
         * example xxx
         */
        class Demo extends React.Component {}
     ```
    然后你在引入这个组件，并在键入这个组件名的过程，就能给你提示，`arguments`, `example`，这两个提示啦。所以，你，或使用这个组件的人，就大概能知道，这个组件怎么使用，长啥样子。注释写得好，提示就更贴心，方便你我他。
