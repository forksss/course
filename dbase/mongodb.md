# MongoDb

## MongoDb For Docker

    需要先安装好Docker

### MongoDb安装

```
docker pull mongo  // 安装mongo的官方镜像

docker images       // 查看是否已经安装好mongo

```

### 运行 mongo

```
# 不带授权的方式
docker run -d -p 27017:27017 -v mongo_configdb:/data/configdb -v mongo_db:/data/db --name mongo docker.io/mongo
# 需要授权的方式
docker run --name <YOUR-NAME> -p 27017:27017 -v /data/db:/data/db -d mongo:3.4 --auth

```
—name 指定库的名字，如果不指定会使用一串随机字符串。

-p 27017:27017 官方的镜像已经暴露了 27017 端口，我们将它映射到主机的端口上。如果你不使用默认端口，将 : 前面的数字改成自定义端口。

-v /data/db:/data/db 冒号前面的是主机上的文件路径，将它挂载到库中的文件夹下，实际对文件的读写就会在主机文件上操作。

-d 在后台运行。

mongo:3.4 指定镜像版本，默认是 latest 。建议总是自己指定版本。

—auth 以 auth 模式运行 mongo。

然后执行一下 docker ps 确认一下库已经正常运行起来。

### 新建管理员

现在我们需要进入 mongo shell 操作：

```
docker exec -it <YOUR-NAME> mongo admin
> db.createUser({ user: '<USER>', pwd: '<PASSWORD>', roles: [ { role: 'userAdminAnyDatabase', db: 'admin' } ]});
Successfully added user: {
    "user" : "<USER>",
    "roles" : [
        {
            "role" : "userAdminAnyDatabase",
            "db" : "admin"
        }
    ]
}
```
以后想以管理员身份登入 mongo shell 就可以运行：

```
docker exec -it <YOUR-NAME> mongo -u <USER> -p <PASSWORD> --authenticationDatabase admin
```

### Mongo 查询

```
左边是mongodb查询语句，右边是sql语句。对照着用，挺方便。
db.users.find() select * from usersdb.users.find({"age" : 27}) select * from users where age = 27db.users.find({"username" : "joe", "age" : 27}) select * from users where "username" = "joe" and age = 27db.users.find({}, {"username" : 1, "email" : 1}) select username, email from usersdb.users.find({}, {"username" : 1, "_id" : 0}) // no case  // 即时加上了列筛选，_id也会返回；必须显式的阻止_id返回db.users.find({"age" : {"$gte" : 18, "$lte" : 30}}) select * from users where age >=18 and age <= 30 // $lt(<) $lte(<=) $gt(>) $gte(>=)db.users.find({"username" : {"$ne" : "joe"}}) select * from users where username <> "joe"db.users.find({"ticket_no" : {"$in" : [725, 542, 390]}}) select * from users where ticket_no in (725, 542, 390)db.users.find({"ticket_no" : {"$nin" : [725, 542, 390]}}) select * from users where ticket_no not in (725, 542, 390)db.users.find({"$or" : [{"ticket_no" : 725}, {"winner" : true}]}) select * form users where ticket_no = 725 or winner = truedb.users.find({"id_num" : {"$mod" : [5, 1]}}) select * from users where (id_num mod 5) = 1db.users.find({"$not": {"age" : 27}}) select * from users where not (age = 27)db.users.find({"username" : {"$in" : [null], "$exists" : true}}) select * from users where username is null // 如果直接通过find({"username" : null})进行查询，那么连带"没有username"的纪录一并筛选出来db.users.find({"name" : /joey?/i}) // 正则查询，value是符合PCRE的表达式db.food.find({fruit : {$all : ["apple", "banana"]}}) // 对数组的查询, 字段fruit中，既包含"apple",又包含"banana"的纪录db.food.find({"fruit.2" : "peach"}) // 对数组的查询, 字段fruit中，第3个(从0开始)元素是peach的纪录db.food.find({"fruit" : {"$size" : 3}}) // 对数组的查询, 查询数组元素个数是3的记录，$size前面无法和其他的操作符复合使用db.users.findOne(criteria, {"comments" : {"$slice" : 10}}) // 对数组的查询，只返回数组comments中的前十条，还可以{"$slice" : -10}， {"$slice" : [23, 10]}; 分别返回最后10条，和中间10条db.people.find({"name.first" : "Joe", "name.last" : "Schmoe"})  // 嵌套查询db.blog.find({"comments" : {"$elemMatch" : {"author" : "joe", "score" : {"$gte" : 5}}}}) // 嵌套查询，仅当嵌套的元素是数组时使用,db.foo.find({"$where" : "this.x + this.y == 10"}) // 复杂的查询，$where当然是非常方便的，但效率低下。对于复杂查询，考虑的顺序应当是 正则 -> MapReduce -> $wheredb.foo.find({"$where" : "function() { return this.x + this.y == 10; }"}) // $where可以支持javascript函数作为查询条件db.foo.find().sort({"x" : 1}).limit(1).skip(10); // 返回第(10, 11]条，按"x"进行排序; 三个limit的顺序是任意的，应该尽量避免skip中使用large-number
```
