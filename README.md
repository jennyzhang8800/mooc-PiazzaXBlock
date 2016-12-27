# mooc-PiazzaXBlock
这是运行于MOOC平台的Piazza历史信息同步与展示的Block
#Xblock安装步骤

**1.把代码clone到服务器，并将所有者设为edxapp**
```
$ git clone https://github.com/jennyzhang8800/mooc-PiazzaXBlock.git
$ sudo chown -R edxapp:edxapp mooc-PiazzaXBlock/

```

**2.把mooc-PiazzaXBlock/PiazzaXBlock/piazza/static/html/piazza.html 文件中的iframe的src改为你的edx域名**
```
<option value="http://cherry.cs.tsinghua.edu.cn/static/example-feed/example.html" >show feed</option>
<option value="http://cherry.cs.tsinghua.edu.cn/static/example-cid/example.html" >show page center</option>
<option value="http://cherry.cs.tsinghua.edu.cn/static/example-together/example.html" >show all</option>
```
把上面代码中的cherry.cs.tsinghua.edu.cn换把你的edx域名

**3.安装XBlock**
 ```
 $ cd mooc-PiazzaXBlock/PiazzaXBlock
 $ sudo -u edxapp /edx/bin/pip.edxapp install .
 ```
 
 **4.将文件夹staticfiles/下的文件夹拷贝到/edx/var/edxapp/staticfiles/中，同时增加所有人对其的读权限**
 
 ```
$ sudo cp -r mooc-PiazzaXBlock/staticfiles/* /edx/var/edxapp/staticfiles/
$ sudo chmod a+r -R /edx/var/edxapp/staticfiles/example-together/
$ sudo chmod a+r -R /edx/var/edxapp/staticfiles/example-feed/
$ sudo chmod a+r -R /edx/var/edxapp/staticfiles/example-cid/
 ```
 
 **5.重启edx**
 ```
 sudo /edx/bin/supervisorctl restart edxapp:  
 ```
 
 **7.在Studio中把在线代码编辑器block添加到课程的高级设置中。**

+ 登录到Studio,打开你的课程
+ settings->Advanced Setting
+ 在"advanced_modules"的值后添加"piazza"
+ 可以在Studio中看到并使用该组件
