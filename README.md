# LicenseManager

> license模块，根据当前值与预设值对比判断状态并提供服务

### 文档
[Documentation for LicenseManager](http://wheellllll.github.io/LicenseManager/)

### 安装

#### Maven

```xml
<repositories>
    <repository>
        <id>tahiti-nexus-snapshots</id>
        <name>Tahiti NEXUS</name>
        <url>http://sse.tongji.edu.cn/tahiti/nexus/content/groups/public</url>
        <releases><enabled>false</enabled></releases>
        <snapshots><enabled>true</enabled></snapshots>
    </repository>
</repositories>

<dependencies>
    <dependency>
        <groupId>wheellllll</groupId>
        <artifactId>LicenseManager</artifactId>
        <version>1.0-SNAPSHOT</version>
    </dependency>
</dependencies>
```

#### 手工下载

从 https://github.com/Wheellllll/LicenseManager/releases 下载最新的jar包添加到项目依赖

### 使用

#### 实例化
提供capacity与throughput两种功能，可同时使用，也可以只使用其中之一，在初始化时需要指定license功能。
使用capacity功能时需设置容量上限；使用throughput功能时需设置流量上限和时间单位，默认时间单位为秒，可使用静态方法`setDefaultTimeUnit(TimeUnit defaultTimeUnit)`改变默认值。

 示例代码

 ```java
int capacityLimit = Config.getConfig().getInt("MAX_NUMBER_PER_SESSION", 100);
int throughputLimit = Config.getConfig().getInt("MAX_NUMBER_PER_SECOND", 5);

License license = new License(License.LicenseType.CAPACITY, capacityLimit); //只使用capacity功能

License license = new License(License.LicenseType.THROUGHPUT, throughputLimit); //只使用throughput功能，时间单位为默认值秒

License license = new License(License.LicenseType.BOTH, capacityLimit, throughputLimit); //同时使用两种功能，时间单位为默认值秒
 ```

*枚举值 LicenseType，TimeUnit，Availability的定义请见文档*

#### 从已有实例启用功能或关闭功能

实例化一个License之后，可以根据需要开启或关闭功能。

示例代码

```java
License license2 = new License(License.LicenseType.CAPACITY, capacityLimit);

license2.enableThroughput(throughputLimit, false, License.TimeUnit.MINUTES); //第二个参数表示是否从上次停下的地方继续计数，true表示继续，false表示重新开始计数；第三个参数为时间单位，可省略第三个参数，此时使用默认时间单位

license2.disableCapacity(); //关闭capacity功能
```

#### 许可服务
使用`use()`方法，此方法会根据启用的服务类型改变对应的当前值并与预设值作比较，返回可用性结果：
- AVAILABLE（license可用）
- CAPACITYEXCEEDED（容量超过限制）
- THROUGHPUTEXCEEDED（流量超过限制）
- BOTHEXCEEDED（容量流量均超过限制）

示例代码

```java
License.Availability availability = license.use();
if (availability == License.Availability.AVAILABLE) {
  //do some thing
} else {
  //do other things
}
```

需要重置license时可以使用`reset(LicenseType type)`方法,将指定类型的license的当前消息数量置为0。

示例代码
```java
license.reset(License.LicenseType.BOTH);
```
