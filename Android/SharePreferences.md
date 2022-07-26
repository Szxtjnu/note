# Share Preferences(简单保存数据)

## getPreferences

参数只有(int mode)，名称系统将会根据Activity的名称命名

### 声明以及初始化

```java
    SharedPreferences shp = getPreferences(Context.MODE_PRIVATE);
    SharedPreferences.Editor editor = shp.edit();
    editor.putInt("NUMBER",100);
	//提交一般采用apply，非同步式，commit可能会冲突
    editor.apply();
```

### 读取

```java
    int x = shp.getInt("NUMBER", 0);
    String TAG = "mylog";
    Log.d(TAG, "onCreate: " + x);
```

这里读取可以直接采用`getInt()`方法来读取数据，这里填入的参数有标志字段（与数据绑定的，在初始化时自定义的）以及缺省值（为了避免数据读入出现错误而使得程序崩溃）。

可以采用Log来查看读入操作。

### 存储

写入的数据是保存在xml文件之中，名称为MainActivity。

## getSharedPreferences

参数有(String name, int mode)两项，支持用户自定义名称，一般用在共享的数据上，允许应用程序中其他的部件来访问

初始化与读取的操作类似于getPreferences，只需要在声明时候加入自定义名称字段即可。

同样，在存储数据时，所在xml文件的文件名即为自定义名称。

## 其他类调用

由于getSharedPreferences是继承context类而来，而MainActivity也是继承context的类

一般在编写程序的时候，我们通常习惯把数据写到一个类之中，方便管理。

Q：那么在其他没有继承context的类之中，如何才可以使用getSharedPreferences函数呢？

A：需要传递一个context。 首先需要定义一个Context类的变量，然后再传到构造函数中。

在MainActivity之中，就可以new一个数据类的实例，然后传入引用，这里的引用不能传入this，可能会造成内存的泄露（由于在横竖屏切换或其他的一些常见下，会导致MainActivity的重新启动，导致之前的引用没有被内存处理机制处理），一般选择传入getApplicationContext()这样一个全局引用
