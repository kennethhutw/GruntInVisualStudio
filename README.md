網頁前端自動化工具 – Grunt in Visual Studio
----

安裝 Node.js
----
如果你是使用 Visual Studio 2015 或都沒有安裝Visual Studio, 可以去 Node.js Tools for Visual Studio 去下載

https://www.visualstudio.com/en-us/features/node-js-vs.aspx

如果是Visual Studio 2013，請下載Extensions for Node.js
https://visualstudiogallery.msdn.microsoft.com/0f162ede-1772-4ef0-96f3-3d53c2622f1f?SRC=Home

我是下載Node.js Tools 1.1 RC3 for Visual Studio 2013
為什麼要裝Node.js Tools 1.1 RC3 for Visual Studio 2013呢? 
因為Visual Studio 2013預設是沒有project for nodejs的

安裝Extension
----
在目前的Visual Studio中，要整合Grunt到專案之中，必須要透過Extension的方式 （在不久的將來，Visual Studio的新版本會提供內建的整合），首先要將以下兩個Extension安裝到Visual Studio中
1.	Task Runner Explorer - 用來偵測、綁定和執行Grunt設定檔中的任務
2.	Package Intellisense - 提供package.json(npm)和bower.json的intellisense

設定Grunt基本任務
----
1.	建立一個Node.js application
2.	選擇空白專案 （因為只需要寫JavaScript)


![Image](http://kenneth2011.pixnet.net/album/photo/297328249-2015-11-13_141150.png)

3.	建好後
![Image](http://kenneth2011.pixnet.net/album/photo/297328252-2015-11-13_141602.png)

4.	打開Package.json 檔案，輸入專案需要的模組
![Image](http://kenneth2011.pixnet.net/album/photo/297328714-2015-11-13_154146.png)


```
{
  "name": "NodejsConsoleApp1",
  "version": "0.0.0",
  "description": "NodejsConsoleApp1",
  "main": "app.js",
  "author": {
    "name": "KennethHu",
    "email": "Kenneth.hu@hotmail.com"
   },
    "devDependencies": {
        "grunt": "~0.4.5",
        "grunt-contrib-jshint": "^0.11.3",
        "grunt-contrib-nodeunit": "^0.4.1",
        "grunt-contrib-uglify": "^0.5.0",
        "grunt-contrib-concat": "^0.5.1",
        "grunt-contrib-cssmin": "^0.14.0",
        "grunt-contrib-htmlmin": "^0.6.0"
 }
}

```

5.	輸入後就會看到所有模組了

![Image](http://kenneth2011.pixnet.net/album/photo/297328243-2015-11-13_143726.png)

6.	在專案目錄下建立 gruntfile.js 檔案，並輸入設定檔
![Image](http://kenneth2011.pixnet.net/album/photo/297328246-2015-11-13_142617.png)
![Image](http://kenneth2011.pixnet.net/album/photo/297328654-2015-11-13_153914.png)



```gruntfile
module.exports = function (grunt) {
    // Project configuration.
    grunt.initConfig({
        pkg: grunt.file.readJSON('package.json'),
        uglify: {
            options: {
                banner: '/*! <%= pkg.name %> <%= grunt.template.today("yyyy-mm-dd") %> */\n'
            },
            build: {
                src: 'src/<%= pkg.name %>.js',
                dest: 'dest/<%= pkg.name %>.min.js'
            }
        },
        concat: {
            dist: {
                options: {
                    separator: '\n\r',
                    banner: '/*\nConcatinated JS file \n' +
                    'Author: Mahesh \n' +
                    'Created Date: <%= grunt.template.today("yyyy-mm-dd") %>' +
                    '\n */ \n'
                },
                // select the files to concatenate
                src: ['src/js/JavaScript1.js', 'src/js/JavaScript2.js'],
                // the resulting JS file
                dest: 'dest/MyAngularLogic.js'
            }
        }
    });
    
    grunt.loadNpmTasks('grunt-contrib-concat');
    
    // Load the plugin that provides the "uglify" task.
    grunt.loadNpmTasks('grunt-contrib-uglify');
    
    // Default task(s).
    grunt.registerTask('default', ['concat', 'uglify']);

};
```
8.	然後依Grunt設定建立資料夾，如下圖
![Image](http://kenneth2011.pixnet.net/album/photo/297328240-2015-11-13_145451.png)
9.	開始Task Runner Explorer（在檢視 > 其他視窗 > Task Runner Explorer），在contcat使用滑鼠右鍵選擇Run 
![Image](http://kenneth2011.pixnet.net/album/photo/297328267-2015-11-13_150452.png)
10.	執行成功
![Image](http://kenneth2011.pixnet.net/album/photo/297328264-2015-11-13_150616.png)
同時也產生出檔案
![Image](http://kenneth2011.pixnet.net/album/photo/297328261-2015-11-13_150837.png)



Troubleshooting
----------
How to open “Task Runner Explorer ”
Solution:
 View –> Other Windows –> Task Runner Explorer menu.

 
 Task Runner Explorer 是空的，如下圖
 ![Image](http://kenneth2011.pixnet.net/album/photo/297328255-2015-11-13_145800.png)
Solution:
1.	Click “Refresh” button
2.	你的gruntfile.js 放錯地方


Task Runner Explorer (No Tasks found)
![Image](http://kenneth2011.pixnet.net/album/photo/297328258-2015-11-13_145940.png) 
Solution:
1.	你的gruntfile.js是空的或是設定有問題
