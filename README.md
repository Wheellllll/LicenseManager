# LicenseManager

> license模块，根据当前值与预设值对比判断状态并提供服务

### 安装
从 https://github.com/Wheellllll/LicenseManager/releases 下载最新的jar包添加到项目依赖里去

### 使用
#### 实例化
提供capacity与throughput两种license，可同时使用，在初始化时需要指定license类型。使用capacity功能时需设置容量上限；使用throughput功能时需设置流量上限和时间单位，默认时间单位为秒，可使用静态方法`setDefaultTimeUnit(TimeUnit defaultTimeUnit)`改变默认值，若使用此方法改变默认时间单位，之后的默认值都会变为新的值。


#### 许可服务
使用`use()`方法，此方法会根据启用的服务类型改变对应的当前值并与预设值作比较，返回可用性结果：
- AVAILABLE（license可用）
- CAPACITYEXCEEDED（容量超过限制）
- THROUGHPUTEXCEEDED（流量超过限制）
- BOTHEXCEEDED（容量流量均超过限制）

需要重置license时可以使用`reset()`方法将指定类型的license当前值置为0
