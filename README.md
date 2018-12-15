redis-keepalived-playbook
===========

# 说明

这个playbook用于搭建redis主从+keepalived模式。

这个模式基于两台机器，两台机器上分别搭建redis和keepalived。并且将keepalived的主从和redis主从进行关联。

# 设置
需要设置两个文件：

## hosts

配置两台机器的连接信息（如果是使用ssh用户名密码，记得在/etc/ansible/ansible.cfg设置host_key_checking = False）

## group_vars/all

* deploy_user: "codis"  redis部署用户
* deploy_folder: "/home/codis/redis"  redis部署地址
* deploy_port: "6000"  redis部署端口
* script_folder: "/etc/keepalived/script/"  keepalive的redis脚本
* vrrp: "192.168.34.99"  keepalived的vip地址
* virtual_router_id: "19"  keepalived的虚拟地址
* eth: "eth2"  keepalived绑定网卡

## site.yml

* backup_server: "192.168.34.3" 主/备对应的另外一个机器的地址


# 前置条件

* /home/codis/redis/redis{{deploy_port}} 之前没有安装redis并启动过redis
* {{virtual_router_id}} 信道没有被其他的keepalived使用过


# 运行
`ansible-playbook -i hosts site.yml`

脚本是幂等的，可重复执行

建议第一次运行先单步执行：

`ansible-playbook --step -i hosts site.yml`
