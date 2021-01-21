
单Master版：

   ansible-playbook -i hosts single-master-deploy.yml -uroot -k


多Master版：

   ansible-playbook -i hosts multi-master-deploy.yml -uroot -k



===============================================


ansible定义cfg文件：
[defaults]
inventory      = /hosts
forks          = 5          # 并行数
become      = root
remote_port    = 22
host_key_checking = False
timeout = 10
log_path = /var/log/ansible.log
private_key_file = /root/.ssh/id_rsa





集群安装变量文件：
# 安装目录 
software_dir: '/root/binary_pkg'    # 统一存放所有二进制安装包的目录
k8s_work_dir: '/opt/kubernetes'
etcd_work_dir: '/opt/etcd'
tmp_dir: '/tmp/k8s'            # 临时存放文件目录




# 集群网络
service_cidr: '10.0.0.0/24'
cluster_dns: '10.0.0.2'   
# 与roles/addons/files/coredns.yaml中IP一致，并且是service_cidr中的IP
pod_cidr: '10.244.0.0/16' 
# 与roles/addons/files/kube-flannel.yaml中网段一致（必须与flannel插件网络一致）
service_nodeport_range: '30000-32767'
cluster_domain: 'cluster.local'         # 集群dns域





# 部署高可用需要的参数，（若部署单master节点请忽略）
vip: '172.16.60.150'
nic: 'ens33'       # keepalived心跳检查的网卡（按照实际网卡定义，比如eth0）


# 自签证书可信任IP列表，为方便扩展，可添加多个预留IP
cert_hosts:
  # 包含所有LB、VIP、Master IP和service_cidr的第一个IP
  k8s:
    - 10.0.0.1
    - 172.16.60.241
    - 172.16.60.242
    - 172.16.60.243
    - 172.16.60.244
    - 172.16.60.245
    - 172.16.60.246
  # 包含所有etcd节点IP
  etcd:
    - 172.16.60.241
    - 172.16.60.242
    - 172.16.60.243

=======================================================

