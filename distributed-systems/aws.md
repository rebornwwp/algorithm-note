# Amazon Web Services\(AWS\)

概念

Amazon Machine Image \(AMI\) 对应镜像，一种包含软件配置 \(例如，操作系统、应用程序服务器和应用程序\) 的模板。

instance（实例）是云中的虚拟服务器。镜像+硬件+配置就是一个instance。

_instance type_ 代表用户开启instance的时候硬件配置。

block device mapping：块储存设备映射，帮助本地存储卷的配置。临时数据，如果想长久保存需要s3或者是ebs。

AWS Identity and Access Management \(IAM\) 用于身份识别与访问管理。

**停止实例：**实例停止后，该实例将执行正常关闭操作，然后过渡到 `stopped` 状态。

**终止实例：**当终止实例后，实例将执行正常关闭操作。根设备卷在默认情况下会被删除，但任何附加的 Amazon EBS 卷在默认情况下会被保留，这由每个卷的 `deleteOnTermination` 属性设置确定。

根设备类型:**Storage for the Root Device.** \(`ebs` 或 `instance store`\)



## Regions and Availability Zones <a id="using-regions-availability-zones"></a>

regions指代一个独立的地理的区域，availability zones指代一个区域中独立的位置，所以一个region有多个zones，单个regions中可以实现实例和数据存放在多个zones中，在regions之间也能实现数据备份，但是需要用户设置。

可用区由区域代码后跟一个字母标识符表示；例如，`us-east-1a`。

可用区的_AZ ID，唯一表示，_`use1-az1` 是 `us-east-1` 区域的 AZ ID，它在每个 AWS 账户中的位置均相同。





