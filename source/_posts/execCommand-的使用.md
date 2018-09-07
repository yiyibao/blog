---
title: JavaScript中的execCommand()api解析
date: 2018-09-06 15:26:00
comments: true
categories: "javascript陌生的api学习"
tags: 
	- javascript api
	- 富文本编辑器
---

## JavaScript中的execCommand()详细解析

### 前提

- 并不是所有的浏览器都支持execCommand()。 [exeCommand对浏览器的兼容性](https://developer.mozilla.org/zh-CN/docs/Web/API/Document/execCommand)
- 使用execCommand()的时候，需要对改dom节点设置contentEditable=true

### 语法

- execCommand(String aCommandName, Boolean aShowDefaultUI, String aValueArgume)

### 参数

- String aCommandName:是一个字符串,命令名称 可以查看[命令列表](https://developer.mozilla.org/zh-CN/docs/Web/API/Document/execCommand#%E5%91%BD%E4%BB%A4)
- Boolean aShowDefaultUI:是一个布尔值,是否展示用户界面
- String aValueArgume:是一个字符串,一些命令，需要额外的参数，默认为null

### 返回值

- 返回的是一个布尔值，如果是false则表示操作不能被支持或未被弃用。

### 例子

- 实现一个copy功能

  - 只需要将execCommand的第一个参数变成copy命令即可

    ```html
    <!DOCTYPE html>
    <html lang="en">
    
    <head>
    
        <meta charset="UTF-8">
    
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
    
        <meta http-equiv="X-UA-Compatible" content="ie=edge">
    
        <title>Document</title>
    
        <style>
    
        *{
    
            margin: 0;
    
            padding: 0;
    
        }
    
        div{
    
            margin: 50px auto;
    
        }
    
        button {
    
            width: 100px;
    
            height: 35px;
    
            margin: 20px auto;
    
            display: block;
    
        }
    
        </style>
    
    </head>
    
    <body>
    
        <button id="copyButton">复制</button>
    
        <div
    
        contenteditable="true"
    
        style="width:600px; height:400px;border: 1px solid #000000"
    
        id="editorDiv"
    
        \>
    
        </div>
    
    </body>
    
    <script>
    
        const div = document.getElementById('editorDiv');
    
        const copyBtn = document.getElementById("copyButton");
    
        copyBtn.onclick = function (){
    
            try{
    
                document.execCommand('copy', false,null);
    
                alert("复制成功");
    
            } catch(err){
    
                alert('无法copy'+err);
    
            }
    
        }
    
    </script>
    
    </html>
    ```






