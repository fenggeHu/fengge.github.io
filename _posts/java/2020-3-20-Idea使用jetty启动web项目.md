1，在Run配置窗口new Configuration选择 Maven

2，在Parameters选项卡（必填）
Working directory（web工程目录）： /Users/your_home/workspace/your-app/web
Command line：org.eclipse.jetty:jetty-maven-plugin:9.4.27.v20200227:run

Before launch设置 (*** 多套环境配置文件夹需要设置前置项 ***)
增加Run Maven Goal，打包环境配置
方式一，打包的指定环境。
Run Maven Goal的 Command line：clean install -P test
方式二，jetty run的指定环境。
Run Maven Goal的 Command line：clean  
在jetty run的Command line：org.eclipse.jetty:jetty-maven-plugin:9.4.27.v20200227:run  -P test

3，在Runner选项卡（选填）
VM Options（设置JVM参数、Jetty参数等）:
如，Jetty运行端口(默认8080)可修改为其它端口， -Djetty.port=7000
如，JVM参数，-Xmx4g -Xms4g -Xmn2g -Xss128k 
