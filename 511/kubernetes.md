- 所有机器`vi /etc/hosts`

  ```
  192.168.1.130 kube-master
  192.168.1.131 kube-node1
  192.168.1.132 kube-node2
  192.168.1.133 kube-node3
  ```

- 关闭防火墙

```
systemctl stop firewalld && systemctl disable firewalld

```



- k8s master节点

  - 安装

  ```
  yum install etcd kubernetes-master
  ```

  - 配置`vi /etc/etcd/etcd.conf`

master节点配置完flannel后要新建这个网络

`etcdctl set /atomic.io/network/config '{ "Network": "10.1.0.0/16" }' `

