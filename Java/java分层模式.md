## Java分层模式案例

### 房屋租赁程序

#### HouseView.java （界面）

1. 显示界面
2. 接受用户的输入
3. 调用HouseService完成对房屋信息的各种操作

#### HouseService.java （业务层）

1. 响应HouseView的调用
2. 完成对房屋信息的各种操作（增删改查CRUD）

#### House.java （模型层）

1. 一个House对象表示一个房屋信息

#### HouseRentAPP.java

1. 主方法中创建HouseView对象，并调用该对象，显示主菜单