# patch-package使用以及坑
### 安装时候的坑
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
