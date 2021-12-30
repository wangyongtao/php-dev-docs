Webpack-Quick-Start.md


# Installation

使用 NPM 安装:  

```
$ npm install webpack -g
```

查看安装后的版本: 

```
$ webpack -v
2.6.1
```

# Usage


$ vi entry.js

```
document.write("Hello Webpack.");
```

$ vi index.html

```
<html>
    <head>
        <meta charset="utf-8">
    </head>
    <body>
        <script type="text/javascript" src="bundle.js" charset="utf-8"></script>
    </body>
</html>
```

```
$ webpack ./entry.js bundle.js
Hash: 32fd78bf2c63bbbf2d1b
Version: webpack 2.6.1
Time: 75ms
    Asset  Size    Chunks  Chunk     Names
bundle.js  2.66 kB 0      [emitted]  main
   [0] ./entry.js 29 bytes {0} [built]
```

使用浏览器打开 `index.html`, 会输出 `Hello Webpack.`.




npm install --save-dev style-loader css-loader


# Reference 

https://github.com/webpack/webpack  
https://webpack.js.org/guides
