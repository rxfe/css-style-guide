### **准备（都必选）**

1. 安装3个包

   ```
   npm install --save-dev @mtfe/stylelint-config husky lint-staged
   ```

   ​

2. 代码配置

   - 在根目录增加.stylelintrc文件，文件内容为：

     ```
     {    "extends": "@mtfe/stylelint-config"  }  
     ```

     ​

   - 修改package.json

     --fix:  会尽可能修复你的错误。（实际并不多）

     git add: 修复的范围为要add的文件。

     ```
     "lint-staged": { 
       "client/**/*.scss": [ 
         "stylelint --syntax=scss --fix", 
         "git add" 
       ], 
       "client/**/*.js": [ 
         "eslint --fix", 
         "git add" 
       ] 
     } 
     "scripts": { 
       "precommit": "lint-staged" 
     }
     ```

     ​

3. IDE配置

   - webstorm -> Preferences -> 搜索stylelint -> enable勾上
   - vscode：安装插件'stylelint'进行实时检测，只要有配置文件则不需要插件的设置。
   - sublime，待补充

4. 检测方式

   1. IDE实时标红检测
   2. 执行git commit时，lint-staged进行检测

### 检测调研

市面上比较常用的两种方式：

1. git hook

   git提交后进行检测，也可以配置只检测更新的文件。

   优点：远程检测，开发人员不能私自修改配置

2. lint-staged

   本地git commit 前检测更新的文件，检测失败则无法提交。

   优点：配置简单，有错误时无需重复提交，git log没有冗余记录。

目前使用的是lint-staged



**注意！不允许同一文件部分/多次commit的开发方式。**

**因为lint-staged存在缺陷，会在第一次commit时把整个文件提交。**
