
1.vscode中添加eslint和vetur插件：

2.在文件->首选项->用户设置中添加如下代码
```
{
  "eslint.autoFixOnSave": true,
  "eslint.validate": [
    "javascript",
    "javascriptreact",
    {
      "language": "html",
      "autoFix": true
    },
    {
      "language": "vue",
      "autoFix": true
    }
  ]
}
```
3.打开项目选中文件 一直ctrl+s 就可以自动格式化了
ps:需要多次ctrl+s