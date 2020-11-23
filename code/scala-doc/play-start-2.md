#! https://zhuanlan.zhihu.com/p/286630566
# scala使用playframwork开发webApp系列二——slick使用
- [系列目录](#系列目录)
- [前言](#前言)
- [dao类模板](#dao类模板)
- [查](#查)
- [增](#增)

## 系列目录
[scala使用playframwork开发webApp系列壹——项目搭建](https://zhuanlan.zhihu.com/p/265746455)

[**scala使用playframwork开发webApp系列二——slick使用**](https://zhuanlan.zhihu.com/p/286630566)

[scala使用playframwork开发webApp系列三——CSRF过滤器配置](https://zhuanlan.zhihu.com/p/301350500)
## 前言
本文假设你已经完成[系列一的配置](https://zhuanlan.zhihu.com/p/265746455)

## dao类模板
```
import play.api.db.slick.DatabaseConfigProvider
import scala.concurrent.ExecutionContext
import com.google.inject.Inject
import play.api.db.slick.HasDatabaseConfigProvider
import slick.jdbc.JdbcProfile
import scala.concurrent.Future

class SampleDao @Inject() (
    protected val dbConfigProvider: DatabaseConfigProvider
)(implicit executionContext: ExecutionContext)
    extends HasDatabaseConfigProvider[JdbcProfile] {
  import profile.api._
  // TODO
}
```

## 查
```
val query = User.filter(_.email === mEmail).result
db.run(query)
```
`User`对象由`CodeGen.scala`自动生成，参考系列一。

## 增
情况1:直接新增，返回被修改的行数。
```
def saveUser(newUser: UserRow): Future[Int] = {
  val action = User += newUser
  db.run(action)
}
```
情况2:只提供部分列的数据,返回被修改的行数。
```
val action = User.map(user => (user.username, user.password, user.email)) += (
      username,
      password,
      email
    )
db.run(action)
```
情况3:只提供部分列的数据，返回新增数据的自增id
```
val returnId = User
    .map(user => (user.username, user.password, user.email))
    .returning(User.map(_.id))

val action = returnId += (
    username,
    password,
    email
)      
```

持续更新中...