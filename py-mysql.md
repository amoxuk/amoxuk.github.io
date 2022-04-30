# python mysql 操作

## 示例

```python
import time

import pymysql


def sqltest():
    with pymysql.Connection(host='localhost', user='root', password='password', database='pyec') as sql:
        cur = sql.cursor()

        # cur.execute('insert into user(`id`,`name`,`date`) values(%s,%s,%s)', (123456, "张三", int(time.time())))
        # sql.commit()
        cur.execute('select * from user')
        data = cur.fetchone()
        timeArray = time.localtime(data[2])
        otherStyleTime = time.strftime("%Y-%m-%d %H:%M:%S", timeArray)
        print(otherStyleTime)


if __name__ == '__main__':
    sqltest()

```

---

> [跳转到目录](menu.md)
