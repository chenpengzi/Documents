# 运维工具

|功能|基本要求|测试情况|备注|
|----|--------|--------|----|
|初始化环境|能够初始化环境信息，生成运维工具的配置文件|通过||
|列出节点信息|能在任意节点上列出节点信息|通过||
|检查节点的健康状态|在 Fuel 节点上可以检查所有节点的健康状态，在其他节点上则检查自身的健康状态|通过||
|检查集群状态|在 Fuel 节点上可以检查所有节点的集群状态，在其他节点上则检查自身的集群状态|通过||
|检查节点组件和配置文件的状态|可以检查指定角色的节点的组件和配置文件状态|通过||
|检查基础环境状态|可以检查节点上基础环境的状态|通过||
|上传 AMI 镜像|可以便捷地上传 AMI 镜像|通过||
|删除异常卷|可以将错误卷和包含快照的卷删除|通过||
|删除异常云主机|可以将异常状态（如错误状态）的云主机删除|通过||
|备份 Fuel|可以备份、恢复 Fuel 节点，并列出 Fuel 节点中所有的备份|通过||
|部署监控插件|可以使用运维工具部署平台的监控插件|通过||
|升级环境|可以使用运维工具将环境升级到指定版本|通过||
