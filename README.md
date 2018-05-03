HttpRunnerManager
=================

Design Philosophy
-----------------

基于HttpRunner的接口自动化测试平台: `Requests`_, `unittest`_ and `Django`_. HttpRunner手册: http://cn.httprunner.org/

Key Features
------------

- 项目管理：新增项目，列表展示及相关操作
- 模块管理：为项目新增模块，用例和配置都归属于module
- 用例管理：分为添加config与test子功能，config定义全部变量和request等相关信息 request可以为公共参数和请求头，也可定义全部变量
- 业务组织方式：在include输入框中以 config>case1>case2...或者case1>case2
- 报告管理：单个运行会前端显示，批量的运行后可以在线查看
- 运行方式：可单个test，单个module，单个project，也可选择多个批量运行
- 分布执行：单个用例和批量执行结果会直接在前端展示，模块和项目执行为异步执行，完成后自动保存报告，可以在线查看
- 系统设置：可添加运行环境，case运行时可自由选择环境
- 报告查看：所有异步执行的用例均可在线查看报告，暂时未开放自主命名,默认时间戳命名
- 定时任务：可设置定时任务，遵循crontab表达式

Deployment Setp
---------------
1. 安装mysql数据库服务端(推荐5.7+),并设置为utf-8编码，创建相应HttpRunner数据库，设置好相应用户名、密码，启动mysql

2. 修改:HttpRunnerManager/HttpRunnerManager/settings.py里DATABASES字典相关配置：NAME(默认HttpRunner)
   USER(用户名，建议root用户，需要有增删改查权限！)、PASSWORD(对应登录用户名密码)、HOST(数据库所在服务器ip地址)
   PORT(数据库服务监听端口，默认3306)
3. 安装rabbitmq消息中间件，service rabbitmq-server start启动服务，访问：http://host:15672/#/ host即为你部署rabbitmq的服务器ip地址
   username：guest、Password：guest, 成功登陆即可

4. 修改:HttpRunnerManager/HttpRunnerManager/settings.py里BROKER_URL = 'amqp://guest:guest@127.0.0.1:5672//'将127.0.0.1替换为步骤3的host

5. 命令行窗口执行pip install -r requirements.txt 安装工程所依赖的库文件

6. 命令行窗口切换到HttpRunnerManager目录，执行python manage.py makemigrations ApiManager 生成数据库迁移脚本

7. 执行python manage.py migrate 对应HttpRunner数据库生成相应表结构

8. 执行python manage.py createsuperuser 根据提示输入用户名，邮箱，密码

9. 执行python manage.py runserver

10. shell或dos窗口切换到HttpRunnerManager目录执行：python manage.py celery -A HttpRunnerManager worker --loglevel=info 启动worker

11. shell或dos窗口切换到HttpRunnerManager目录执行：python manage.py celery beat --loglevel=info 开启定时任务配置

12. CLI窗口执行：celery flower 访问：http://localhost:5555/dashboard 即可查看任务列表和状态

13. 浏览器输入：http://127.0.0.1:8000/api/register/  注册用户，开始享用

14. 浏览器输入http://127.0.0.1:8000/admin/  输入步骤6设置的用户名、密码，登录后台运维管理系统
