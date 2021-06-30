# patch-package使用以及坑
- 因需求，需要修改一下开源项目的源码，对于传统的方法例如:
   1. 把修改后的代码换个名字重新打个包提交到tnpm，然后直接引用这个新包。
   2. 把代码copy移出node_modules作为本地依赖。
- 使用patch-package会更加轻便，而且可以根据diff来追溯修改的内容。

#### 安装时候的坑
- 在安装的时候，一定要使用
   ```
    npm install patch-package --save-dev
   ```
   进行安装。否则回遇到以下的坑🕳。
- 直接安装或者--save安装。
   ```
    npm install patch-package
    npm install patch-package --save
   ```
   - 在npx进行使用的时候会报错。
      ```
        Cannot read property 'dependencies' of undefined
      ```
      
#### 使用时候的坑
- 使用npx来标识修改的内容。
   ```
      npx patch-package codemirror
   ```
   - 运行时报错，无法识别出修改了哪些内容。
      ```
         • Creating temporary folder
         • Installing codemirror@5.62.0 with npm
         • Diffing your files with clean files
         ⁉️  Not creating patch file for package 'codemirror'
         ⁉️  There don't appear to be any changes.
      ```
   - 解决方法：
      - 在命令行后面添加 --exclude 命令
      ```
         npx patch-package XXX --exclude
      ```

#### 参考
- [support changing package.json](https://github.com/ds300/patch-package/issues/250)


      
      
