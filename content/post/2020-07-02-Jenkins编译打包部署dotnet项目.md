# Jenkins编译打包部署dotnet项目

## 1. 创建任务

![jenkins-scm7编译](E:\Git仓库\luxsun\Jenkins编译打包部署dotnet项目\assets\png\jenkins-scm7编译.png)

## 创建子任务

![image-20200702111315861](E:\Git仓库\luxsun\Jenkins编译打包部署dotnet项目\assets\png\子任务.png)

### 修改子任务的一些配置

1. 修改位置   Build中的执行Windows 批处理命令

   ![image-20200702111644501](E:\Git仓库\luxsun\Jenkins编译打包部署dotnet项目\assets\png\image-20200702111644501.png)

​           修改JOB运行节点,重定向到WIN_BUILD_43  **固态盘**

2. 修改workspace  保持子任务和主任务使用同一块workspace

   ```
   set Source_dir="D:\Jenkins\workspace\Test-SCM7-Build_only_WithSSD"
   ```

3. 修改MSBuild路径，43服务器已切换至Visural Studio 2019

   ```
   set MSBuild_dir="C:\vscode\MSBuild\Current\Bin"
   ```

4. 修改MSBuild参数，添加 /nologo /maxcpucount /p:Configuration="Release"，如下：![image-20200702112459966](E:\Git仓库\luxsun\Jenkins编译打包部署dotnet项目\assets\png\image-20200702112459966.png)

5. 删除指定文件及文件夹

   ![image-20200702112738293](E:\Git仓库\luxsun\Jenkins编译打包部署dotnet项目\assets\png\image-20200702112738293.png)

6. 压缩js文件，需要在编译服务器上安装node，并安装ugligyjs插件，命令如下

   ```
    npm install uglify-js
   ```

   

7. 修改ugligyjs的路径，默认路径为

   ```
   C:\Users\Administrator\node_modules\uglify-js\bin
   ```

8. 修改bat脚本中的路径，如下![image-20200702113314726](E:\Git仓库\luxsun\Jenkins编译打包部署dotnet项目\assets\png\image-20200702113314726.png)

9. 修改主任务中打包命令，删除文件夹，只保留压缩生成的压缩包，如下：

   ```
   "C:\Program Files\WinRAR\Rar.exe" a -r  -mt16 -df  -inul  %VERSION%.rar %VERSION%
   ```

   ![image-20200702113555120](E:\Git仓库\luxsun\Jenkins编译打包部署dotnet项目\assets\png\image-20200702113555120.png)