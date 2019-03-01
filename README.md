### beego
--- 
https://github.com/astaxie/beego/tree/master/orm

```go
package main
import (
  "fmt"
  "github.com/astaxie/beego/orm"
  _ "github.com/go-sql-driver/mysql"
)

type User struct {
  Id int `orm:"auto"`
  Name string `rom:"size(100)"`
}

func init() {
  orm.RegisterModel(new(User))
  orm.RegisterDataBase("default", "mysql", "root:root@/my_db?charset=utf8", 30)
  orm.RunSyncdb("default", false, true)
}

func main() {
  o := orm.NewOrm()
  
  user := User{Name: "slene"}
  
  id, err := o.Insert(&user)
  
  user.Name = "astaxie"
  num, err := o.Update(&user)
  
  u := User{Id: user.Id}
  err = o.Read(&u)
  
  num, err = o.Delete(&u)
}

type Post struct {
  Id int `orm:"auto"`
  Title string `orm:"size(100)"`
  User *User `orm:"rel(fk)"`
}

var posts []*Post
qs := o.QueryTable("post")
num, err := qs.Filter("User__Name", "slene").All(&posts)


var maps []Params
num, err := o.Raw("SELECT id FROM user WHERE name = ?", "slene").Values(&maps)
if num > 0 {
  fmt.Println(maps[0]["id"])
}

o.Begin()

user := User{Name: "slene"}
id, err := o.Insert(&user)
if err == nil {
  o.Commit()
} else {  
  o.Rollback()
}

func main() {
  orm.Debug = true
}

[ORM] - 2018-08-09 13:18:15 - [Queries/default] = [  db.Exec / 0.4ms] - [INSERTINTO `user` (`name`) VALUES (?)] - `slene`
```

```
```

```
```


