# LicenseManager

> license模块，根据当前值与预设值对比判断状态并提供服务

### 安装
从 https://github.com/Wheellllll/LicenseManager/releases 下载最新的jar包添加到项目依赖

### 使用
#### 实例化
提供capacity与throughput两种license功能，可同时使用，在初始化时需要指定license功能。使用capacity功能时需设置容量上限；使用throughput功能时需设置流量上限和时间单位，默认时间单位为秒，可使用静态方法`setDefaultTimeUnit(TimeUnit defaultTimeUnit)`改变默认值。

 代码

 ```java
int capacityLimit = Config.getConfig().getInt("MAX_NUMBER_PER_SESSION", 100);
int throughputLimit = Config.getConfig().getInt("MAX_NUMBER_PER_SECOND", 5);
License license = new License(License.LicenseType.BOTH, capacityLimit, throughputLimit);
 ```


#### 许可服务
使用`use()`方法，此方法会根据启用的服务类型改变对应的当前值并与预设值作比较，返回可用性结果：
- AVAILABLE（license可用）
- CAPACITYEXCEEDED（容量超过限制）
- THROUGHPUTEXCEEDED（流量超过限制）
- BOTHEXCEEDED（容量流量均超过限制）

代码

```java
License.Availability availability = license.use();
if (availability == License.Availability.AVAILABLE) {
  //do some thing
} else {
  //do other things
}
```

需要重置license时可以使用`reset()`方法,将指定类型的license的当前消息数量置为0。

代码
```java
license.reset(License.LicenseType.BOTH);
```
