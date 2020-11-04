#! https://zhuanlan.zhihu.com/p/265746455
# play项目初始配置文档
- [新建play项目](#新建play项目)
- [运行项目](#运行项目)
- [配置数据库(mysql)](#配置数据库mysql)
  - [依赖](#依赖)
  - [配置](#配置)
  - [使用数据库自动演进（evolutions）](#使用数据库自动演进evolutions)
  - [使用slick自动生成数据库操作代码](#使用slick自动生成数据库操作代码)
## 新建play项目
```
sbt new playframework/play-scala-seed.g8
```
## 运行项目
terminal中运行
```
sbt run
```
vscode中运行
launch.json文件
```
{
    // Use IntelliSense to learn about possible attributes.
    // Hover to view descriptions of existing attributes.
    // For more information, visit: https://go.microsoft.com/fwlink/?linkid=830387
    "version": "0.2.0",
    "configurations": [
        {
            "type": "scala",
            "request": "launch",
            "name": "Play main",
            "mainClass": "play.core.server.ProdServerStart",
            "args": [],
            "jvmOptions": []
        }
    ]
}
```
application.conf文件添加screatkey
```
play.http.secret.key=${?PLAY_SECREAT_KEY}
```
~/.bashrc中添加环境变量:
```
export PLAY_SECREAT_KEY='QCY?tAnfk?aZ?iwrNwnxIlR6CTf:G3gf:90Latabg@5241AB`R5W:1uDFN];Ik@n'
```
刷新.bashrc文件.(关闭所有vscode界面和terminal界面)
```
source .bashrc
```
## 配置数据库(mysql)
### 依赖
在build.sbt中添加如下依赖
```
libraryDependencies ++= Seq(
  "com.typesafe.play" %% "play-slick" % "5.0.0",
  "com.typesafe.play" %% "play-slick-evolutions" % "5.0.0",
  "com.typesafe.slick" %% "slick-codegen" % "3.3.2",
  "mysql" % "mysql-connector-java" % "5.1.41"
)
```
### 配置
在application.conf中添加配置(db.url自行替换):
```
# Default database configuration
slick.dbs.default.profile="slick.jdbc.MySQLProfile$"
slick.dbs.default.db.driver="com.mysql.jdbc.Driver"
slick.dbs.default.db.url="jdbc:mysql://192.168.1.249:3306/seed-store?useSSL=false&characterEncoding=UTF-8"
slick.dbs.default.db.user=${?MYSQL_USER}
slick.dbs.default.db.password=${?MYSQL_PSD}
```
~/.bashrc中添加环境变量:
```
export MYSQL_USER='{用户名}'
export MYSQL_PSD='{密码}'
```
刷新.bashrc文件.(关闭所有vscode界面和terminal界面)
```
source ~/.bashrc
```
配置项目中的logback.xml文件，在`<root>`节点之前添加以下语句使其能在控制台中显示SQL语句:
```
<logger name="slick.jdbc.JdbcBackend.statement"  level="DEBUG" /> 
```
### 使用数据库自动演进（evolutions）
1.创建如下目录，其中default表示数据库名称（图中为默认数据库）,创建sql文件以1,2,3命名，表示时间先后顺序。

![evolutions](sql_evolutions.png)

2.sql文件样例：

样例1：
```
-- !Ups

DROP TABLE IF EXISTS `user`;
CREATE TABLE `user` (
    `id` INT AUTO_INCREMENT,
    `username` VARCHAR(50) NOT NULL unique,
    `password` VARCHAR(50),
    `phone_number` VARCHAR(50),
    PRIMARY KEY (`id`)
)ENGINE InnoDB DEFAULT CHARSET=UTF8;

-- !Downs

DROP TABLE `user`;
```
样例2：
```
-- !Ups

ALTER TABLE `product` ADD CONSTRAINT `prdct_foreign_key_prdct` FOREIGN KEY(`manufacturer_id`)
REFERENCES `manufacturer`(`id`);

-- !Downs

ALTER TABLE `product` DROP FOREIGN KEY `prdct_foreign_key_prdct`;
```
样例3：
```
-- !Ups
CREATE TABLE operator (
	`id` INT AUTO_INCREMENT,
    `username` VARCHAR(50) NOT NULL unique,
    `password` VARCHAR(100) NOT NULL,
     PRIMARY KEY (`id`)
)ENGINE InnoDB DEFAULT CHARSET=UTF8;

INSERT INTO operator(username, password)
VALUES('admin', '$2a$10$D.DPASo7cAvdU9b0URPbY.www8lLK6PkJPQUT/TlWO.S84CYslzZO');

-- !Downs

DELETE FROM operator where username = 'admin';

DROP TABLE operator;
```
写好sql之后运行一次项目，然后打开项目网页，会提示运行脚本，运行即可。
### 使用slick自动生成数据库操作代码
项目源码目录新建scala文件,如下图:

![code_gen](slick_code_gen.png)

`Codegen.scala`文件内容样例如下:
```
object CodeGen extends App {
  val dbName = "manager-sys-demo"
  val dbUserName = System.getenv("MYSQL_USER")
  val dbPass = System.getenv("MYSQL_PSD")
  slick.codegen.SourceCodeGenerator.main(
    Array(
      "slick.jdbc.MySQLProfile",
      "com.mysql.jdbc.Driver",
      s"jdbc:mysql://192.168.1.249:3306/${dbName}?useSSL=false&characterEncoding=UTF-8",
      "/home/chuanzhangjiang/Desktop/source-code/manager-sys-demo/manager-sys-demo/app",
      "models",
      dbUserName,
      dbPass
    )
  )
}
```
`/home/chuanzhangjiang/scala_project/seed-store/app/`源码目录,`models`为源码输出的包。
数据库配置完成之后运行Codegen.scala即可。