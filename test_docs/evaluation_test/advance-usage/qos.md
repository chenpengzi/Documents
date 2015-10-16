# QoS 功能

## 创建 QoS

### 创建绑定云主机的 QoS

* 前提：

  * 已经创建好**云主机。

* 操作：

  1. 登录到 Controller 节点；
  1. 获取云主机的端口 ID：

    ```
    # neutron port-list
    ```
  1. 从中读取云主机端口 ID 并记录，以云主机端口为 target 创建 QoS：

    ```
    # neutron eayun-qos-create --name <QoS_NAME> --target-type port --target-id <PORT_ID> <DIRECTION: egress or ingress> <RATE> <DEFAULT_RATE>
    ```

* 预期结果：

  * QoS 创建成功：

    ```
    # neutron eayun-qos-create --name coffee-qos-port --target-type port --target-id f2a6b8d5-a16f-4834-bf90-c903e39921bb egress 102400 20480
    Created a new qos:
    +--------------------+--------------------------------------+
    | Field              | Value                                |
    +--------------------+--------------------------------------+
    | burst              |                                      |
    | cburst             |                                      |
    | default_queue_id   | c8bdf555-fbd2-4b54-9f22-298b9583c1bd |
    | description        |                                      |
    | direction          | egress                               |
    | id                 | 1dc3acca-6aac-4d22-9912-941be6d8121c |
    | name               | coffee-qos-port                      |
    | qos_queues         | c8bdf555-fbd2-4b54-9f22-298b9583c1bd |
    | rate               | 102400                               |
    | target_id          | f2a6b8d5-a16f-4834-bf90-c903e39921bb |
    | target_type        | port                                 |
    | tenant_id          | bf87349ae6704a87af75ebe9546d4af6     |
    | unattached_filters |                                      |
    +--------------------+--------------------------------------+
    ```

### 创建绑定路由的 QoS

* 前提：

  * 已经创建好路由。

* 操作：

  1. 登录到 Controller 节点；
  1. 获取所要设置路由限制的路由 ID：

    ```
    # neutron router-list | grep XXX
    ```
  1. 创建绑定路由的 QoS：

    ```
    # neutron eayun-qos-create --name <QoS_NAME> --target-type router --target-id <ROUTER_ID> <DIRECTION: egress or ingress> <RATE> <DEFAULT_RATE>
    ```

* 预期结果：

  * 创建绑定路由的 QoS 成功：

    ```
    # neutron eayun-qos-create --name coffee-qos-router --target-type router --target-id 1b569ef2-4e11-40d6-bbb1-a326ad152a4e egress 102400 20480
    Created a new qos:
    +--------------------+--------------------------------------+
    | Field              | Value                                |
    +--------------------+--------------------------------------+
    | burst              |                                      |
    | cburst             |                                      |
    | default_queue_id   | beb0b287-36fe-4df0-bf02-acb819aad0bf |
    | description        |                                      |
    | direction          | egress                               |
    | id                 | 47f26654-90ec-4311-9f5f-4a04fd07c8e6 |
    | name               | coffee-qos-router                    |
    | qos_queues         | beb0b287-36fe-4df0-bf02-acb819aad0bf |
    | rate               | 102400                               |
    | target_id          | 1b569ef2-4e11-40d6-bbb1-a326ad152a4e |
    | target_type        | router                               |
    | tenant_id          | bf87349ae6704a87af75ebe9546d4af6     |
    | unattached_filters |                                      |
    +--------------------+--------------------------------------+
    ```

## 创建 QoS 队列

### 创建 QoS 下的父队列

* 前提：

  * 已经创建好一个 QoS，基于该 QoS 创建父队列。

* 操作：

  1. 登录到 Controller 节点；
  1. 获取之前所创建的 QoS 的 ID：

    ```
    ```
  1. 创建 QoS 下的父队列：

    ```
    # neutron eayun-qos-queue-create <QoS_ID> <RATE>
    ```

* 预期结果：

  * QoS 下的父队列创建成功：

    ```
    # neutron eayun-qos-queue-create 1dc3acca-6aac-4d22-9912-941be6d8121c 20480 --prio 0
    Created a new qos_queue:
    +------------------+--------------------------------------+
    | Field            | Value                                |
    +------------------+--------------------------------------+
    | attached_filters |                                      |
    | burst            |                                      |
    | cburst           |                                      |
    | ceil             |                                      |
    | id               | c28cc6d9-f464-413e-befa-8efc2dc740e6 |
    | parent_id        |                                      |
    | prio             | 0                                    |
    | qos_id           | 1dc3acca-6aac-4d22-9912-941be6d8121c |
    | rate             | 20480                                |
    | subqueues        |                                      |
    | tenant_id        | bf87349ae6704a87af75ebe9546d4af6     |
    +------------------+--------------------------------------+
    ```

> ###### 说明：
> * `--prio` 是可选参数，定义该队列的优先级；
> * `--prio` 优先级为 0-7，数字越小，优先级越高，如果创建时未定义，默认优先级为 7。

### 创建父队列下的子队列

* 前提：

  * 已经创建好 QoS 下的父队列。

* 操作：

  1. 登录到 Controller 节点；
  1. 获取之前所创建的 QoS 和该 QoS 的父队列 ID：

    ```
    ```
  1. 创建该 QoS 父队列的子队列：

    ```
    # neutron eayun-qos-queue-create --parent <PARENT_ID> --prio <PRIORITY_NUM> <QoS_ID> <RATE>
    ```

* 预期结果：

  * 父队列下的子队列创建成功：

    ```
    # neutron eayun-qos-queue-create --parent c28cc6d9-f464-413e-befa-8efc2dc740e6 --prio 1 1dc3acca-6aac-4d22-9912-941be6d8121c 10240
    Created a new qos_queue:
    +------------------+--------------------------------------+
    | Field            | Value                                |
    +------------------+--------------------------------------+
    | attached_filters |                                      |
    | burst            |                                      |
    | cburst           |                                      |
    | ceil             |                                      |
    | id               | fd7d83b7-5f5b-449e-bed5-b0c21e10b318 |
    | parent_id        | c28cc6d9-f464-413e-befa-8efc2dc740e6 |
    | prio             | 1                                    |
    | qos_id           | 1dc3acca-6aac-4d22-9912-941be6d8121c |
    | rate             | 10240                                |
    | subqueues        |                                      |
    | tenant_id        | bf87349ae6704a87af75ebe9546d4af6     |
    +------------------+--------------------------------------+
    ```

## 创建过滤器

### 创建基于 QoS 的过滤器

* 前提：

  * 已经创建好 QoS。

* 操作：

  1. 登录到 Controller 节点；
  1. 获取所要创建过滤器的 QoS 的 ID：

    ```
    # neutron eayun-qos-list | grep XXX
    ```
  1. 创建该 QoS 的过滤器：

    ```
    # neutron eayun-qos-filter-create --protocol <PROTOCOL_NUM>\
    --dst-port <PORT_NUM> --dst-addr <ADDR_CDIR> <QoS_ID> <PRIO>
    ```

* 预期结果：

  * 基于 QoS 的过滤器创建成功：

    ```
    # neutron eayun-qos-filter-create --protocol 5 --dst-port 5201 --dst-addr 172.16.200.0/24 cfd12bf4-62dc-420b-bca9-1dd41e8f4da2 100
    Created a new qos_filter:
    +-----------+--------------------------------------+
    | Field     | Value                                |
    +-----------+--------------------------------------+
    | dst_addr  | 172.16.200.0/24                      |
    | dst_port  | 5201                                 |
    | id        | 165f3edd-feb4-4b7c-a04f-85db678edc4b |
    | prio      | 100                                  |
    | protocol  | 5                                    |
    | qos_id    | cfd12bf4-62dc-420b-bca9-1dd41e8f4da2 |
    | queue_id  |                                      |
    | src_addr  |                                      |
    | src_port  |                                      |
    | tenant_id | bf87349ae6704a87af75ebe9546d4af6     |
    +-----------+--------------------------------------+
    ```

### 创建基于队列的过滤器

* 前提：

  * 已经创建好 QoS 队列，且该队列下没有其他子队列。

* 操作：

  1. 登录到 Controller 节点；
  1. 获取之前所创建的 QoS 和所要创建过滤器的队列的 ID：

    ```
    ```
  1. 创建该 QoS 队列的过滤器：

    ```
    # neutron eayun-qos-filter-create --queue <QUEUE_ID> --protocol <PROTOCOL_NUM>\
    --dst-port <PORT_NUM> --dst-addr <ADDR_CDIR> <QoS_ID> <PRIO>
    ```

* 预期结果：

  * 过滤器创建成功：

    ```
    # neutron eayun-qos-filter-create --queue fd7d83b7-5f5b-449e-bed5-b0c21e10b318 --protocol 5\
    --dst-port 5001 --dst-addr 172.16.200.0/24 1dc3acca-6aac-4d22-9912-941be6d8121c 100
    Created a new qos_filter:
    +-----------+--------------------------------------+
    | Field     | Value                                |
    +-----------+--------------------------------------+
    | dst_addr  | 172.16.200.0/24                      |
    | dst_port  | 5001                                 |
    | id        | ab22540e-b99c-499d-93aa-018de003a255 |
    | prio      | 100                                  |
    | protocol  | 5                                    |
    | qos_id    | 1dc3acca-6aac-4d22-9912-941be6d8121c |
    | queue_id  | fd7d83b7-5f5b-449e-bed5-b0c21e10b318 |
    | src_addr  |                                      |
    | src_port  |                                      |
    | tenant_id | bf87349ae6704a87af75ebe9546d4af6     |
    +-----------+--------------------------------------+
    ```

## 测试限速功能

### 相同宿主机上的两台云主机之间的限速

* 场景描述：

  有一个内网，在这个内网里有两台虚拟机需要通信，instanceA 和 instanceB；其中 instanceA 作为这个内网的 FTP 服务器需要向 instnceB 提供服务。在 instanceA 上设置QoS，带宽为 100KB/s，默认队列的保证带宽为 10KB/s。现在让 instanceA 的 FTP 保证 40KB/s。

* 前提：

  * 创建一个 QoS，带宽为 100KB/s，默认队列带宽为 10KB/s，方向为上传（输出），指向 instanceA 的端口：

    ```
# neutron eayun-qos-create --name coffee-test-port --target-type port --target-id f2a6b8d5-a16f-4834-bf90-c903e39921bb egress 102400 10240
Created a new qos:
+--------------------+--------------------------------------+
| Field              | Value                                |
+--------------------+--------------------------------------+
| burst              |                                      |
| cburst             |                                      |
| default_queue_id   | 7076470e-6d5a-43e6-8b2f-298ff787b543 |
| description        |                                      |
| direction          | egress                               |
| id                 | a43338d7-4fd6-432e-910c-1d5f84ed5f53 |
| name               | coffee-test-port                     |
| qos_queues         | 7076470e-6d5a-43e6-8b2f-298ff787b543 |
| rate               | 102400                               |
| target_id          | f2a6b8d5-a16f-4834-bf90-c903e39921bb |
| target_type        | port                                 |
| tenant_id          | bf87349ae6704a87af75ebe9546d4af6     |
| unattached_filters |                                      |
+--------------------+--------------------------------------+
    ```
  * 创建 QoS 下的队列 queueA，速度限制为 40KB/s：

    ```
# neutron eayun-qos-queue-create a43338d7-4fd6-432e-910c-1d5f84ed5f53 40960 --prio 0
Created a new qos_queue:
+------------------+--------------------------------------+
| Field            | Value                                |
+------------------+--------------------------------------+
| attached_filters |                                      |
| burst            |                                      |
| cburst           |                                      |
| ceil             |                                      |
| id               | 2ca88d47-278a-4fa7-9e1f-e15807ec4f12 |
| parent_id        |                                      |
| prio             | 0                                    |
| qos_id           | a43338d7-4fd6-432e-910c-1d5f84ed5f53 |
| rate             | 40960                                |
| subqueues        |                                      |
| tenant_id        | bf87349ae6704a87af75ebe9546d4af6     |
+------------------+--------------------------------------+
    ```
  * 创建过滤器，匹配从 instanceA 向外提供的 FTP 流量，指向 queueA：

    ```
# neutron eayun-qos-filter-create --queue 2ca88d47-278a-4fa7-9e1f-e15807ec4f12 --protocol 6 --src-port 21 --src-addr 172.16.200.0/24 a43338d7-4fd6-432e-910c-1d5f84ed5f53 100
Created a new qos_filter:
+-----------+--------------------------------------+
| Field     | Value                                |
+-----------+--------------------------------------+
| dst_addr  |                                      |
| dst_port  |                                      |
| id        | 3fe2a418-1003-4735-9c03-dfe1bcbdc42c |
| prio      | 100                                  |
| protocol  | 6                                    |
| qos_id    | a43338d7-4fd6-432e-910c-1d5f84ed5f53 |
| queue_id  | 2ca88d47-278a-4fa7-9e1f-e15807ec4f12 |
| src_addr  | 172.16.200.0/24                      |
| src_port  | 21                                   |
| tenant_id | bf87349ae6704a87af75ebe9546d4af6     |
+-----------+--------------------------------------+
    ```
  * instanceA 和 instanceB 中都已经安装了 iperf3 网络性能测试工具。

* 操作：

  1. 访问 instanceA，在 instanceA 中执行 `iperf3 -s`；
  1. 访问 instanceB，在 instanceB 中执行 `iperf3 -c <IP_OF_instanceA> -f K`，记录测试结果；
  1. 访问 instanceB，在 instanceB 中执行 `iperf3 -s`；
  1. 访问 instanceA，在 instanceA 中执行 `iperf3 -c <IP_OF_instanceB> -f K`，记录测试结果；
  1. 访问 instanceB，在 instanceB 中执行 `iperf3 -s -p 21`；
  1. 在带宽拥挤的情况下，访问 instanceA，在 instanceA 中执行 `iperf3 -c <IP_OF_instanceB> -f K -p 21`，记录测试结果。

* 预期结果：

  * 

### 不同宿主机上的两台云主机之间的限速

* 场景描述：

  有一个内网，同时有 A 和 B 两个 instance 需要对外提供 HTTP 服务，另外 instanceA 上面还要对外提供 FTP 服务。我在连接着这个内网的路由上设置了一个 QoS，带宽为 100KB/s，默认队列的保证带宽（给未归类的其它流量用）为 10KB/s。我现在希望 instanceA 能够使用 60KB/s 的带宽上传，其中 HTTP 保证 40KB/s，FTP 保证 20KB/s，instance B 使用 30KB/s 的带宽上传，全部给 HTTP 服务。

* 前提：

  * 创建 QoS，带宽设置为 100KB/s，默认队列带宽为 10KB/s，方向为上传，指向该内网的路由：
  * 在该 QoS 下创建队列 queueA，带宽设置为 60KB/s，提供给 instanceA 使用：
  * 在 queueA 下创建两个队列 queueA-1 和 queueA-2，速度分别设置为 40KB/s 和 20KB/s，分别提供给 HTTP 和 FTP 使用：
  * 在 QoS 下创建队列 queueB，带宽设置为 30KB/s，提供给 instanceB 使用，即 instanceB 的 HTTP 服务使用：
  * 创建过滤器，匹配从 instanceA 向外的 HTTP 流量，指向 queueA-1：
  * 创建过滤器，匹配从 instanceA 向外的 FTP 流量，指向 queueA-2：
  * 创建过滤器，匹配从 instanceB 向外的 HTTP 流量，执行 queueB：

* 操作：

* 预期结果：