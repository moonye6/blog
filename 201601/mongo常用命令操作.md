## mongo常用命令操作

help                                     `mongodb支持的命令`

show dbs                                 `查看所有数据库`

use test

db                                       `查看当前连接在哪个数据库`

show collections                         `查看所有的collection `

db.help()                                `当前数据库支持哪些方法`

db.user.help()                           `当前数据库下的表或者表collection支持的方法`

 
db.foo.find()                            `查找所有`

db.foo.findOne()                         `查找一条记录`

db.foo.find({'msg':'Hello 1'}).limit(10) `根据条件检索10条记录`

 
db.mail_addr.drop()                      `删除collection`

db.dropDatabase()                        `删除当前的数据库`

db.foo.remove({'yy':5})                  `删除yy=5的记录 `

db.foo.remove()                          `删除所有的记录`
 
#### 存储嵌套的对象
db.foo.save({'name':'ysz','address':{'city':'beijing','post':100096},'phone':[138,139]})
 
#### 存储数组对象
db.user_addr.save({'Uid':'yushunzhi[@sohu](/user/sohu).com','Al':['test-1[@sohu](/user/sohu).com','test-2[@sohu](/user/sohu).com']})
 
#### 根据query条件修改，如果不存在则插入，允许修改多条记录
db.foo.update({'yy':5},{'$set':{'xx':2}},upsert=true,multi=true)

http://www.cnblogs.com/xusir/archive/2012/12/24/2830957.html


> 坑1： mongoose连接mongo的时候，使用model('Apple', Apple)来可以创建表，但是创建出来的表名会转为小写了，并且在apple后面加了一个`s` （mongoose源码行为）
