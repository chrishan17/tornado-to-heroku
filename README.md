# 如何将***tornado***应用部署至***Heroku***
<a href="https://www.heroku.com" target="blank"><img src="https://d1lpkba4w1baqt.cloudfront.net/heroku-logo-dark-300x100.png" alt ="Heroku" width='250' height='75'></a>
## 1. 使用前提：
#### 必须翻墙!  
- *Heroku*在翻墙后才能使用。
#### 安装***[git][1]***
#### 安装***[virtualenv][2]***  
- 创建独立的Python环境，多个Python应用互不影响。  
- 你可以先下载[Pip][3]，Python的软件安装管理包，然后`pip install virtualenv`。
#### 需要一个***Heroku account:***  
在[Heroku官网][4]注册账号。 

## 2. 下载***Heroku Toolbelt***并登录 **：**
* 在[Toolbelt][5]下载对应系统的Heroku Toolbelt，以便你在终端里使用heroku命令行。
* 下载并安装好后，打开终端登录：
	+ 输入`heroku login`, 然后输入你的邮箱和密码  

## 3. 把***tornado app***上传至___Heroku___的步骤**：**
#### 1.从***[Github][6]***上下载***tornado heroku***样例
	$ curl -L 'https://github.com/mikedory/Tornado-Heroku-Quickstart/tarball/master' | tar zx && cd mikedory-Tornado-Heroku-Quickstart-*

#### 2.修改样例的***main.py***及对应的***templates***, ***static***文件
- 原**main.py**

 		#!/usr/bin/env python
		import os.path
		import tornado.escape
		import tornado.httpserver
		import tornado.ioloop
		import tornado.options
		import tornado.web

		# import and define tornado-y things
		from tornado.options import define
		define("port", default=5000, help="run on the given port", type=int)


		# application settings and handle mapping info
		class Application(tornado.web.Application):
		    def __init__(self):
		        handlers = [
		            (r"/([^/]+)?", MainHandler)
		        ]
		        settings = dict(
		            template_path=os.path.join(os.path.dirname(__file__), "templates"),
		            static_path=os.path.join(os.path.dirname(__file__), "static"),
		            debug=True,
		        )
		        tornado.web.Application.__init__(self, handlers, **settings)


		# the main page
		class MainHandler(tornado.web.RequestHandler):
		    def get(self, q):
		        if 'GOOGLEANALYTICSID' in os.environ:
		            google_analytics_id = os.environ['GOOGLEANALYTICSID']
		        else:
		            google_analytics_id = False

		        self.render(
		            "main.html",
		            page_title='Heroku Funtimes',
		            page_heading='Hi!',
		            google_analytics_id=google_analytics_id,
		        )


		# RAMMING SPEEEEEEED!
		def main():
		    tornado.options.parse_command_line()
		    ***http_server = tornado.httpserver.HTTPServer(Application())
		    http_server.listen(tornado.options.options.port)

		    # start it up
		    tornado.ioloop.IOLoop.instance().start()


		if __name__ == "__main__":
		    main()
  
- 修改相应的Application类和MainHandler类并替换原static和templates文件夹中的css, js, html文件
	
#### 3.建立***Python***虚拟环境  
  `$ virtualenv venv --distribute `  
  `$ source venv/bin/activate`

#### 4.创建***[Procfile][7]***纯文本（用sublime text或其他编辑器）
- Procfile  
  `web: python main.py --port=$PORT`

#### 5.保存该***app***至***git***
`$ git init`  
`$ git add .`  
`$ git commit -m "init"`

#### 6.部署***app***至***Heroku***
  * 创建app在Heroku储存的空间  
  `$ heroku create appName`
  * 将***git***中的***app***推向***Heroku***  
  `$ git push heroku master`

#### 7.在浏览器中打开该***app***  
`$heroku open`

[1]: http://git-scm.com
[2]: https://pypi.python.org/pypi/virtualenv
[3]: https://pypi.python.org/pypi/pip
[4]: https://heroku.com
[5]: https://toolbelt.heroku.com
[6]: https://github.com/mikedory/Tornado-Heroku-Quickstart
[7]: https://devcenter.heroku.com/articles/procfile
