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
 
 **6.在Studio中把在线代码编辑器block添加到课程的高级设置中。**

+ 登录到Studio,打开你的课程
+ settings->Advanced Setting
+ 在"advanced_modules"的值后添加"piazza"
+ 可以在Studio中看到并使用该组件

# 定时更新数据
定时更新的数据己保存在
[github仓库](https://github.com/xyongcn/piazza-data-tsinghua.edu.cn_spring2015_30240243x/tree/master/data)

###  case1:如果你需要自己新建github仓库保存，步骤如下：

1. 新建github仓库，并建立仓库与edx 的ssh链接，并在github新建目录结构```data/piazza-data``` 和 ```data/piazza-data-filter```
1. 首先，利用脚本：```mooc-PiazzaXBlock/script/更新一段时间内的数据/upd-data-during-time.py``` 更新一次有所的数据，并把数据保存到github仓库对应的目录下
1. 然后，把```mooc-PiazzaXBlock/staticfiles/example-together/piazza_together.js``` 文件中的url_github改为你的github仓库链接

### case2:如果直接用我们的[github仓库](https://github.com/xyongcn/piazza-data-tsinghua.edu.cn_spring2015_30240243x/tree/master/data)，则步骤如下：

1. 建立github仓库的ssh连接
1. 把https://github.com/xyongcn/piazza-data-tsinghua.edu.cn_spring2015_30240243x 仓库clone到edx本地的/mnt/piazza_update目录下
```
sudo su
cd /mnt
mkdir piazza_update
git clone git@github.com:xyongcn/piazza-data-tsinghua.edu.cn_spring2015_30240243x.git
```

### (case1&case2) 最后，设定好每日定时执行

   
  + 把 ```mooc-PiazzaXBlock/script/每日更新piazza数据/```下的两个脚本updata_piazza.py和pushToGithub.sh 放到edx机器 /mnt/piazza_update/目录下
  
  + 利用crontab创建定时任务：
  
   ```
 crontab -e
 30 23 * * * /usr/bin/python /mnt/piazza_update/update_piazza.py >/mnt/piazza_update/piazza_log_today.log 2>&1
 ```
 
每天的23:30，执行脚本 update_piazza.py。把本次执行该脚本输出的结果记录在piazza_log_today.log 所有的日志记录在piazza_log.log。
