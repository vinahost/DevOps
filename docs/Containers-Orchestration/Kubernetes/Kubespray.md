# Hướng dẫn cài đặt Kubernetes Cluster với Kubespray

**Kubernetes** là một phần mềm mã nguồn mở, có vai trò là một **Container Orchestration**, tức là một hệ quản trị các **Container** và các thành phần khác liên quan. Nó tương đương với **Docker Swarm** nhưng có nhiều tính năng ưu việt và mạnh mẽ hơn.

Bài viết sẽ hướng dẫn các bạn cài đặt một hệ thống **Kubernetes Cluster** bằng **Kubespray** với đầy đủ các thành phần giống như một hệ thống **Product**. Chúng ta sẽ xây dựng hệ thống **Kubernetes Cluster** gồm 03 **node Master**, 03 **node Worker** và các hệ thống phụ trợ như **LoadBalancer Api-Server**, **LoadBalancer ingress**

## Kuberspray là gì ?

**Kubespray** là một công cụ cài đặt cụm **Kubernetes** chuẩn. Được cộng đồng **Kubernetes** khuyến khích sử dụng. Đây không phải là **tool** hay phần mềm gì đặc biệt, nó không giống như **Rancher RKE**. Phần mềm **RKE** là viết **tool** và dùng các file **config** để **deploy** lên thì **kubespray** được viết bằng **Ansible Playbook** và **Vagrant** là chính.

Cách thức hoạt động thì nó được viết bằng **Ansible Playbook**. Nhìn qua bộ **Playbook** thực sự là khổng lồ và nhiều **task**, so với "**hard way**" cài đặt bằng **kubeadm** thì **Kubespray** cài đặt cũng tương tự sử dụng **kubeadm** nhưng chỉ cần chạy **ansible-playbook** là nó sẽ tự làm cho mình hết toàn bộ. Kể cả các phần trong **Requirement** của **Kubernetes** như **disable**, **swap**, bật **mod netfilter**, cài đặt **Docker**, **containerd**, khởi tạo **node**, **join node**, thêm, sửa xóa **node**. Đều được tự động hoàn toàn. Cấu hình cài đặt cụm bằng các **variable** trong **Ansible groups_vars**. Cần thông số gì thì **set** trong thư mục này là xong, hỗ trợ cài đặt luôn tất cả các **CNI**, **CSI** tự động, chỉ định được **runtime** mong muốn từ **crio**, **containerd**, **dockerd**. Nếu cài mới **OS** và chạy **Ansible Kubespray** gần như là cài chuẩn xác giống như "**hard way**" là dùng **kubeadm** trực tiếp.

So sánh với **kubeadm** thì mình đánh giá **Kubespray** dễ sử dụng hơn. So với **RKE** thì **RKE** sử dụng các **image** tùy chỉnh **100%**, **tool** cũng tự viết nhưng nhanh hơn, **image** nhẹ hơn. Nhưng độ ổn định thì chưa khẳng định được vì nó tùy biến quá nhiều. Sau khi dùng đủ các **tool** thì mình sẽ sử dụng  **Kubespray** trên môi trường **On-premise**, **bare-metal**. Thời gian chạy **Kubespray** để khởi tạo **Cluster** là **15 - 20 phút**, theo mình đánh giá là **khá nhanh**.

## Chuẩn bị

Chuẩn bị **08 máy** chủ trên [vCloud](https://vcloud.vinahost.vn/) của **Vinahost** (là các **VM** chạy **Ubuntu 20.04**), cụ thể như sau:

- Master, Worker: 2 CPU, 4G RAM
- Kubespray: 1 CPU, 1G RAM
- LoadBalancer: 2 CPU, 2G RAM 

![Kubespray](/Image/Kubernetes-Cluster01.png)

Các máy chủ trên cần thực hiện các bước cầu hình ban đầu như sau:

- Tạo **Security Group**, các máy chủ sẽ dùng chung **Security Group** này để có thể giao tiếp được với nhau và bên ngoài dựa trên các **rule** 

![Kubespray](/Image/Kubernetes-Cluster02.png)

- Tạo **EIP** cho máy chủ **LoadBlancer** và máy chủ **Kubespray** để có thể kết nối từ **internet** đến 2 máy chủ này

![Kubespray](/Image/Kubernetes-Cluster03.png)

- Tạo 6 máy chủ cho các node **Kubernetes cluster**, cài đặt **Ubuntu 20.04**, **`lưu ý`** cần **add ssh-key** khi tạo máy chủ để đỡ phải copy **ssh-key** lên từng máy chủ 

![Kubespray](/Image/Kubernetes-Cluster04.png)

- Tạo thêm 2 máy chủ cho **`Kubespray`** và **`LoadBalancer`** và gắn **EIP** đã tạo cho 2 máy chủ này

![Kubespray](/Image/Kubernetes-Cluster05.png)

## Bắt đầu cấu hình

### Cài đặt **`Haporxy`**

Phần nàyy sẽ thực hiện trên máy chủ **LoadBalancer**, máy chủ này sẽ nhận các yêu cầu từ bên ngoài và chuyển các yêu cầu đó đến các **node** của **Kubernetes Cluster**

```
apt install --no-install-recommends software-properties-common
add-apt-repository ppa:vbernat/haproxy-2.4 -y
apt update -y
apt install haproxy=2.4.\*
```
Sau khi cài đặt xong, các bạn tiến hành chỉnh thêm đoạn cấu hình **LoadBlancer** sau:
```
# LoadBalancer api-server
frontend k8s-api
   bind 0.0.0.0:16443
   mode tcp
   option tcplog
   default_backend k8s-api

backend k8s-api
   mode tcp
   option tcp-check
   balance roundrobin
   default-server inter 10s downinter 5s rise 2 fall 2 slowstart 60s maxconn 250 maxqueue 256 weight 100
       server master01 172.31.0.5:6443 check
       server master02 172.31.0.6:6443 check
       server master03 172.31.0.7:6443 check

# LoadBalancer nginx ingress
frontend k8s-ingress-https
   bind 0.0.0.0:443
   mode tcp
   option tcplog
   default_backend k8s-ingress-https

backend k8s-ingress-https
   mode tcp
   option tcp-check
   balance roundrobin
   default-server inter 10s downinter 5s rise 2 fall 2 slowstart 60s maxconn 250 maxqueue 256 weight 100
       server worker01 172.31.0.8:443 check
       server worker02 172.31.0.9:443 check
       server worker03 172.31.0.10:443 check

frontend k8s-ingress-http
   bind 0.0.0.0:80
   mode tcp
   option tcplog
   default_backend k8s-ingress-http

backend k8s-ingress-http
   mode tcp
   option tcp-check
   balance roundrobin
   default-server inter 10s downinter 5s rise 2 fall 2 slowstart 60s maxconn 250 maxqueue 256 weight 100
       server worker01 172.31.0.8:80 check
       server worker02 172.31.0.9:80 check
       server worker03 172.31.0.10:80 check
``` 
Lưu lại và restart haproxy
```
systemc restart haproxy
```

Vậy là đã cấu hình xong trên máy chủ **LoadBalancer**

### Chỉnh sửa cấu hình cho Kubernetes cluster

Trong phần này sẽ thực hiện trên server **Kubespray**

Trước tiên, cần *download* **Kubespray**, lưu ý *download* đúng phiên bản bạn cần, Trong phần này mình cài **kubernetes version v1.26.5** nên sẽ *download* **kuberpay** phiên bản **release-2.21**

```
git clone https://github.com/kubernetes-sigs/kubespray.git --branch release-2.21
```

Lúc này **kubespray** sẽ được tải về máy tại thư mục `/root/kubespray`. Bạn cần cài đặt các thành phần cần thiểt cho **kubespray**:

```
apt install python3 python3-pip -y
cd /root/kubespray
pip3 install -r requirements.txt
```


Tiếp theo - tạo thư mục chứa thông tin cấu hình **Kubernetes Cluster** từ bộ mẫu của **kubespray** và tạo file `host.yaml`

```
cp -rf inventory/sample inventory/mycluster
cd /root/kubespray/inventory/mycluster
vi host.yaml
```

Nội dung file `host.yaml` cụ thể như bên dưới. Cần chỉnh sửa nội dung file này theo đúng **IP** của các node sẽ cài **Kubernetes**

```
all:
  hosts:
    master01:
      ansible_host: 172.31.0.5
      ip: 172.31.0.5
      access_ip: 172.31.0.5
    master02:
      ansible_host: 172.31.0.6
      ip: 172.31.0.6
      access_ip: 172.31.0.6
    master03:
      ansible_host: 172.31.0.7
      ip: 172.31.0.7
      access_ip: 172.31.0.7
    worker01:
      ansible_host: 172.31.0.8
      ip: 172.31.0.8
      access_ip: 172.31.0.8
    worker02:
      ansible_host: 172.31.0.9
      ip: 172.31.0.9
      access_ip: 172.31.0.9
    worker03:
      ansible_host: 172.31.0.10
      ip: 172.31.0.10
      access_ip: 172.31.0.10
  children:
    kube_control_plane:
      hosts:
        master01:
        master02:
        master03:
    kube_node:
      hosts:
        worker01:
        worker02:
        worker03:
    etcd:
      hosts:
        master01:
        master02:
        master03:
    k8s_cluster:
      children:
        kube_control_plane:
        kube_node:
    calico_rr:
      hosts: {}
```

- **`kube_control_plane`** là các node sẽ chạy với role kube_control_plane

- **`kube_node`** là các node chạy role worker

- **`etcd`** là các node sẽ chạy etcd, thường chọn là các node master luôn dù không bắt buộc

Tiếp đến nếu muốn đổi **CNI** (**network plugin** của **Kubernetes**) thì sửa file **config** sau:

`inventory/mycluster/group_vars/k8s_cluster/k8s-cluster.yml`

Có thể chọn các **network plugin** sau: `cilium, calico, kube-ovn, weave hoặc flannel`

Ví dụ sử dụng network plugin `flannel`:
```
kube_network_plugin: flannel
```

Bình thường tới đây là có thể cài đặt một **Kubernetes cluster** rồi, nhưng trong bài này mình sẽ cài đặt thêm **nginx ingress** và cấu hình **LoadBlancer** bên ngoài cho **`api-server`** 

Để cài đặt thêm **nginx ingress** cần chỉnh sửa file cấu hình **`group_vars/k8s_cluster/addons.yml`**:


Tìm kiếm dến ***Nginx ingress controller deployment***, bỏ *comment* đoạn cấu hình **nginx ingress** và sửa tham số sau:

`ingress_nginx_enabled: false`

thành 

`ingress_nginx_enabled: true`

Đoạn cấu hình **nginx ingress** sau khi chỉnh sửa như sau:

```
# Nginx ingress controller deployment
ingress_nginx_enabled: true
# ingress_nginx_host_network: false
ingress_publish_status_address: ""
ingress_nginx_nodeselector:
  kubernetes.io/os: "linux"
ingress_nginx_tolerations:
  - key: "node-role.kubernetes.io/master"
    operator: "Equal"
    value: ""
    effect: "NoSchedule"
  - key: "node-role.kubernetes.io/control-plane"
    operator: "Equal"
    value: ""
    effect: "NoSchedule"
ingress_nginx_namespace: "ingress-nginx"
ingress_nginx_insecure_port: 80
ingress_nginx_secure_port: 443
ingress_nginx_configmap:
  map-hash-bucket-size: "128"
  ssl-protocols: "TLSv1.2 TLSv1.3"
ingress_nginx_configmap_tcp_services:
  9000: "default/example-go:8080"
ingress_nginx_configmap_udp_services:
  53: "kube-system/coredns:53"
ingress_nginx_extra_args:
  - --default-ssl-certificate=default/foo-tls
ingress_nginx_termination_grace_period_seconds: 300
ingress_nginx_class: nginx
```

Tiếp theo, **LoadBalancer** cho **Api-server**

Để cấu hình **LoadBlancer** cho **Api-server**, mở file ***group_vars/all/all.yaml*** và chỉnh sửa các thông số sau:
```
## External LB example config
apiserver_loadbalancer_domain_name: "api-server.nametest.tech"  # LB domain
loadbalancer_apiserver:
  address: 172.31.0.13   # ip của VM Load Balancer
  port: 16443            # port nhận traffic của api-server trên VM Load Babancer
```

Sau khi hoàn tất chỉnh sửa cấu hình cho **Kubernetes cluster**, ta bắt đầu kiểm tra lại cấu hình **SSH** tới các **node** với cặp **ssh-key** đã add trong lúc tạo **VM**

Ví dụ, thử thực thi lệnh sau khi **ssh** vào **master01**, nếu chưa thể kết nối được với các **node** cần **add** lại **ssh-key** lên tất cả các **node**
```
root@security:~/kubespray/inventory/mycluster# ssh root@172.31.0.5 ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host 
       valid_lft forever preferred_lft forever
2: ens3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1450 qdisc mq state UP group default qlen 1000
    link/ether fa:00:b5:37:02:00 brd ff:ff:ff:ff:ff:ff
    inet 172.31.0.5/24 brd 172.31.0.255 scope global ens3
       valid_lft forever preferred_lft forever
    inet6 fe80::f800:b5ff:fe37:200/64 scope link 
       valid_lft forever preferred_lft forever
root@security:~/kubespray/inventory/mycluster# 
```

Sau khi kiểm tra kết nối đến các **node**, ta bắt đầu chạy lệnh để tạo **Kubernetes Cluster**

```
ansible-playbook -i inventory/mycluster/hosts.yaml --private-key /root/.ssh/id_rsa cluster.yml
```
Trong bước này **ansible** kết nối đến các **node** bằng **SSH** và cài đặt **Kubernetes**, cài đặt nhanh hay chậm sẽ phụ thuộc vào tốc độ **internet**

Kết quả hoàn thành cài đặt sẽ như sau:

![Kubespray](/Image/Kubernetes-Cluster06.png)

Giờ kiểm tra thành quả cài đặt, lưu ý sau khi cài đặt thì **kubectl** chỉ có trên các **node master** và các có thể lấy thông tin `kubectl config` tại `/etc/kubernetes/admin.conf`

![Kubespray](/Image/Kubernetes-Cluster07.png)

## Triển khai một ứng dụng web 

Triển khai một ứng dụng web trên **Kubernetes Cluster** và truy cập từ bên ngoài

Trong bài này mình sẽ triển khai một ứng dụng web bằng **file manifest**, có nội dung mặc định là trang "**Welcome to nginx!**"

Tạo file **manifest** `nginx-test.yaml` có nội dung như sau:

```
apiVersion: v1
kind: Service
metadata: 
  name: nginx-test-service
  labels:
    app: nginx-test
spec:
  ports:
  - port: 80
    protocol: TCP
    targetPort: 80
  selector:
    app: nginx-test
  sessionAffinity: None
  type: NodePort
---
apiVersion: apps/v1
kind: Deployment
metadata:  
  labels:
    app: nginx-test
  name: nginx-test-depployment
spec:
  replicas: 1
  selector:
    matchLabels:
      app: nginx-test
  strategy:
    type: Recreate
  template:
    metadata:
      labels:
        app: nginx-test        
    spec:
      containers:
      - image: nginx:latest
        name: nginx-test
        ports:
        - containerPort: 80
          protocol: TCP
```
Trong file này mình triển khai một **pod nginx** và một **service** , cài đặt lên **Kubernetes cluster** với lệnh `kubectl`.

``` 
kubectl apply -f nginx-test.yaml
```
Sau khi cài đặt, sử dụng lệnh sau để kiểm tra pod `nginx` đã được cài đặt thành công hay chưa:
```
kubectl get pod
```
![Kubespray](/Image/Kubernetes-Cluster08.png)

Kiểm tra **service** vừa tạo

```
kubectl get svc
```

![Kubespray](/Image/Kubernetes-Cluster09.png)

Tạo và triển khai **Ingress** với **manifest** file `nginx-test-ingress.yaml` , chuyển hướng truy vấn vào ứng dụng trên khi truy cập bằng tên miền **test.nametest.tech** với giao thức **http**. Khi truy cập **ingress** sẽ chuyển hướng đến **service** của ứng dụng **web** dã tạo trước đó.

```
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  annotations:
    kubernetes.io/ingress.class: "nginx"
  name: ingress-test
spec:
  
  rules:
  - host: test.nametest.tech
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service: 
            name: nginx-test-service
            port:
              number: 80
```
```
kubectl appy -f nginx-test-ingress.yaml
```
Kiểm tra **ingress** vừa tạo

![Kubespray](/Image/Kubernetes-Cluster010.png)

Vậy là xong phần triển khai ứng dụng **web** lên cụm **Kubernetes**, bắt đầu trỏ tên miền đến **IP** của máy chủ **LoadBalancer** để truy cập ứng dụng **web** vừa triển khai.

Và đây là kết quả truy cập địa chỉ `test.nametest.tech` trên trình duyệt

![Kubespray](/Image/Kubernetes-Cluster011.png)

## Tham khảo

- [vCloud](https://vcloud.vinahost.vn/)
- [Kubespray](https://kubespray.io/)
- [Kubernetes là gì](https://kubernetes.io/vi/docs/concepts/overview/what-is-kubernetes/)