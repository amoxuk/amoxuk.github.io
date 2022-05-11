# MongoDB使用说明

## docker安装mongo镜像

```shell
# 拉取镜像
docker pull mongo
# 启动镜像
docker run --name mongodb -p 27017:27017 -d mongo
# 进入mongo镜像的命令行
docker exec -it mongodb bash
# 查看mongo的日志
docker logs mongodb
# 创建实例，设置账号密码
docker run -d --name mongodb \
 -e MONGO_INITDB_ROOT_USERNAME=mongoadmin \
 -e MONGO_INITDB_ROOT_PASSWORD=secret \
 mongo
# 进入实例的命令行，登录数据库
docker run -it --rm mongo \
 mongo --host localhost \
 -u mongoadmin \
 -p secret \
 --authenticationDatabase admin \
 some-db
```

doc: <https://github.com/docker-library/docs/tree/master/mongo>

## 本地安装-ubuntu

查看<https://pgp.mongodb.com/>中，获取最新的mongo版本，如今天2022.5.12看到最新的是6.0

1. 安装公共钥匙：  
    * `6.0` 是当前的mongo版本，需要注意替换

    ```shell
    wget -qO - <https://www.mongodb.org/static/pgp/server-6.0.asc> | sudo apt-key add -
    ```

2. 为MongoDB创建一个list源文件：  
    * `focal` 是20.04的版本代号，需要换成系统对应的版本，可以使用`lsb_release -a`或者`cat /etc/lsb-releas`文件查看当前系统的版本代号
    * `6.0` 是当前的mongo版本，需要注意替换

    ```shell
    echo "deb [ arch=amd64,arm64 ] https://repo.mongodb.org/apt/ubuntu focal/mongodb-org/6.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-6.0.list
    ```

3. 更新镜像源：  

    ```shell
    sudo apt-get update
    ```

4. 使用官方的源安装MongoDB：  

    ```shell
    sudo apt-get install -y mongodb-org　　　　
    ```

5. 启动及配置

    配置文件在`/etc/mongod.conf`中，详细的配置可参考:[配置文件官方说明文档](https://www.mongodb.com/docs/manual/reference/configuration-options/#std-label-conf-file)

    ```shell
    sudo service mongod start
    ```

doc: <https://www.mongodb.com/docs/manual/tutorial/install-mongodb-on-ubuntu/>

## python使用mongo

1. 安装pymongo，默认安装最新版

    ```shell
    python -m pip install 'pymongo[srv]'
    ```

2. 连接mongo服务

    ```python
    import pymongo
    # 连接字段说明: https://www.mongodb.com/docs/manual/reference/connection-string/
    # https://pymongo.readthedocs.io/en/stable/api/pymongo/mongo_client.html#pymongo.mongo_client.MongoClient
    conn_str = "mongodb+srv://<username>:<password>@<cluster-address>/test?retryWrites=true&w=majority"
    # 设置超时时间为5s
    client = pymongo.MongoClient(conn_str, serverSelectionTimeoutMS=5000)
    # 也可以是通过参数的方式传递登录参数
    # client = pymongo.MongoClient('<cluster-address>', username='admin',password='password',retryWrites=True)

    try:
        print(client.server_info())
    except Exception:
        print("Unable to connect to the server.")
    ```

    也可以使用稳定版的接口：

    ```python
    # 您可以从MongoDB Server版本5.0和PyMongo驱动程序版本3.12开始使用稳定API功能。使用稳定 API 功能时，可以更新驱动程序或服务器，而不必担心稳定 API 涵盖的任何命令的向后兼容性问题。
    from pymongo.mongo_client import MongoClient
    from pymongo.server_api import ServerApi

    conn_str = "<connection string>"

    # 设置api的版本
    client = pymongo.MongoClient(conn_str, server_api=ServerApi('1'), serverSelectionTimeoutMS=5000)
    ```

3. 获取数据库

    ```python
    # 使用`.`访问数据库
    db = client.test_database
    # 或者这样使用字典的方式
    db = client['test-database']
    ```

4. 获取集合[表]

    ```python
    # 使用`.`访问集合
    collection = client.test_collection
    # 或者这样使用字典的方式
    collection = client['test_collection']
    ```

5. 插入文档

    ```python
    import datetime
    # 请注意，数据中可以包含本机Python类型（如datetime），这些类型将自动转换为相应的BSON类型。
    # BSON类型说明：https://bsonspec.org/
    post = {"author": "Mike",
        "text": "My first blog post!",
        "tags": ["mongodb", "python", "pymongo"],
        "date": datetime.datetime.utcnow()}
    posts = db.posts
    post_id = posts.insert_one(post).inserted_id
    print(post_id)
    >> ObjectId('...')
    ```

    插入文档时，如果文档尚未包含key，则会自动添加特殊key，key在整个集合中必须是唯一的。

    我们可以通过list_collection_names列出数据库中的所有集合：

    ```python
    db.list_collection_names()
    >> ['posts']
    ```

6. 查询单个文档

    * 使用`find_one()`查询单条数据

    ```python
    import pprint
    res = posts.find_one()
    pprint.pprint(res)
    >> {'_id': ObjectId('...'),
     'author': 'Mike', 
     'date': datetime.datetime(...),
     'tags': ['mongodb', 'python', 'pymongo'],
     'text': 'My first blog post!'}
    ```

    * `find_one()`中还支持匹配特定的数据，返回符合条件的数据

    ```python
    res = collect.find_one({"class_name":"高三（1）班","score":{"$gt":90},"$or":[{"student_name":"张三"},{"student_name":"李四"}]})
    pprint.pprint(res)
    ```

    比较操作符：`{"class_name":"高三（1）班","score":{"$gt":90}}`

    符号 | 含义
    ---- | ----
    $lt  | 小于
    $lte | 小于等于
    $gt | 大于
    $gte | 大于等于
    $ne | 不等于
    $in | 在范围内
    $nin | 不在范围内

    逻辑操作符表示条件之间的关系： `{"$or":[{"student_name":"张三"},{"student_name":"李四"}]}`

    符号|  含义
    ---- | ----
    $and|  按条件取 交集
    $not|  单个条件的 相反集合
    $nor|  多个条件的 相反集合
    $or | 多个条件的 并集

    除此之外还有：

    符号| 含义| 示例| 示例含义
    ---- | ----| ----| ----
    $regex| 正则匹配| {"student_name":{"regex":".∗三"}} | 学生名以 “三” 结尾
    $expr| 允许查询中使用| 聚合表达式 |{"expr":{"gt":["spent","budget"]}}|  查询 花费 大于 预算 的超支记录
    $exists| 属性是否存在| {"date":{"$exists": True}}|  date属性存在
    $exists| 属性是否存在| {"date":{"$exists": True}} | date属性存在
    $type| 类型判断| {"score":{"$type":"int"}} | score的类型为int
    $mod| 取模操作| {'score': {'$mod': [5, 0]}} | 分数取5、0的模

7. 批量查询

    使用`find()`查询多条数据，用法和`find_one()`基本一致，但是返回的是一个可迭代的对象。

    ```python
    res = collect.find({"class_name":"高三（1）班","score":{"$gt":90},"$or":[{"student_name":"张三"},{"student_name":"李四"}]})

    for val in res:
        pprint.pprint(val)
    ```

    MongoDB服务器批量返回查询结果。批处理中的数据量不会超过最大BSON文档大小。

    要覆盖批处理的默认大小，请参见`batchSize()`和`limit()`。

    新版本3.4:类型为`find()`、`aggregate()`、`listIndexes`和`listCollections`的操作每批最多返回16兆字节。batchSize()可以执行较小的限制，但不能执行较大的限制。

    find()和`aggregate()`操作的初始批处理大小默认为`101`个文档。针对生成的游标发出的后续`getMore`操作没有默认的批处理大小，因此它们仅受`16mb`消息大小的限制。

    对于包含没有索引的排序操作的查询，服务器必须在返回任何结果之前加载内存中的所有文档来执行排序。

    ```python
    data = []
    for info in db["dbs"].find().batch_size(5000): #修改最大限制阈
        data.append(info)
        print("info nums=",len(info))
    ```

    但是这种方法是每次游标返回5000条数据，循环遍历，如果单词查找50000次应该怎么写呢？如下

    ```python
    data = []
    cousor=db["dbs"].find().batch_size(5000)
        for i in range(50000): #修改最大限制阈
            data.append(next(cousor))
    ```

8. 插入数据

doc: <https://www.mongodb.com/docs/drivers/python/>

## FAQ

1. wsl中Ubuntu 22.04中使用apt安装mongo报libssl1.1 (>= 1.1.1) but it is not installable

    > The following packages have unmet dependencies:
    > mongodb-org-mongos : Depends: libssl1.1 (>= 1.1.1) but it is not installable
    > mongodb-org-server : Depends: libssl1.1 (>= 1.1.1) but it is not installable
    > mongodb-org-shell : Depends: libssl1.1 (>= 1.1.1) but it is not installable

    这是因为mongo官方还没有针对22.04的构建(today:2022.5.12，mongo最新版6.0)，22.04已经使用ssl3所以这个依赖没有，推荐的方式是使用docker镜像使用mongo，当然也可以强制安装低一点的版本libsssl，方法如下：

    ```shell
    echo "deb http://security.ubuntu.com/ubuntu impish-security main" | sudo tee /etc/apt/sources.list.d/impish-security.list
    sudo apt-get update
    sudo apt-get install libssl1.1
    ```

    安装之后记得禁用该依赖

    ref:<https://www.mongodb.com/community/forums/t/installing-mongodb-over-ubuntu-22-04/159931>

---

> [跳转到目录](menu.md)
