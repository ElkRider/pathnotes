### I. usefual common case 

#### 1. two collection aggregate 

```sql
db.createCollection("users");
db.createCollection("orders");

// 插入一个用户
db.users.insertOne({
  username: "张三",
  email: "zhangsan@example.com",
  phone: "13800138000"
})

// 插入一个订单（注意：这里用 userId 来关联上面的用户）
db.orders.insertOne({
  orderNo: "ORDER20260516001",
  userId: "user_zhangsan_001", // 关联用户的唯一标识
  amount: 299.00,
  status: "待支付",
  createTime: new Date()
})


// 查询张三的订单
db.orders.find({ userId: "user_zhangsan_001" })

// 查询金额大于 100 且状态为“待支付”的订单
db.orders.find({ amount: { $gt: 100 }, status: "待支付" })


// 将指定订单的状态更新为“已支付”
db.orders.updateOne(
  { orderNo: "ORDER20260516001" }, 
  { $set: { status: "已支付" } }
)


// 删除该订单
db.orders.deleteOne({ orderNo: "ORDER20260516001" })


// 为 userId 字段创建升序索引
db.orders.createIndex({ userId: 1 })



// 为 orderNo 字段创建唯一索引（保证订单号不重复）
db.orders.createIndex({ orderNo: 1 }, { unique: true })


db.orders.aggregate([
  // 1. 先匹配出“已支付”的订单
  { $match: { status: "已支付" } },
  
  // 2. 关联 users 集合（类似 SQL 的 LEFT JOIN）
  {
    $lookup: {
      from: "users",              // 要关联的集合名
      localField: "userId",       // 当前 orders 集合的关联字段
      foreignField: "_id",        // users 集合的关联字段（MongoDB默认主键是_id）
      as: "userInfo"              // 关联后的用户信息，存放在这个新字段里（是个数组）
    }
  },

  // 3. (可选) 如果确定一个订单只对应一个用户，可以用 $unwind 把 userInfo 数组展开成对象
  { $unwind: "$userInfo" },

  // 4. 只展示需要的字段
  {
    $project: {
      orderNo: 1,
      amount: 1,
      status: 1,
      "userInfo.username": 1,     // 展示用户名字
      "userInfo.phone": 1         // 展示用户手机号
    }
  }
])

```

