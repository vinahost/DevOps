# Helm là gì ?

**Helm** là một công cụ quản lý gói cho **Kubernetes**. Nó cho phép bạn quản lý các gói **Kubernetes**, được gọi là **chart**, một cách có thể quản lý. **Helm chart** là các kho lưu trữ mã nguồn cho các ứng dụng **Kubernetes**, bao gồm tất cả các tài nguyên cần thiết để chạy ứng dụng, chẳng hạn như các **pod**, dịch vụ, bộ cân bằng tải và tài nguyên khác.

**Helm** được thiết kế để đơn giản hóa việc triển khai, quản lý và cập nhật các ứng dụng **Kubernetes**. Nó cung cấp một giao diện dòng lệnh cho phép bạn triển khai các **chart**, quản lý các phiên bản của các **chart** và cập nhật các ứng dụng. **Helm** cũng cung cấp một kho lưu trữ trực tuyến cho các **chart** có thể được sử dụng để triển khai các ứng dụng phổ biến.

**Helm** được viết bằng **Go** và được phát hành theo giấy phép **Apache 2.0**. Nó là một công cụ phổ biến và được sử dụng bởi nhiều công ty, bao gồm **Google**, **Amazon** và **Microsoft**.

## Dưới đây là một số tính năng của Helm:

- **Quản lý gói**: Helm cho phép bạn quản lý các gói Kubernetes bằng cách sử dụng các charts. Charts là các tệp cấu hình có thể được sử dụng để tạo các cài đặt Kubernetes hoàn chỉnh.

- **Tùy chỉnh**: Helm cho phép bạn tùy chỉnh các charts của mình bằng cách chỉ định các biến môi trường và các tùy chọn cài đặt khác.

- **Phân phối**: Helm cho phép bạn phân phối các charts của mình cho các nhóm người dùng khác bằng cách sử dụng kho lưu trữ Helm.

- **Tích hợp**: Helm có thể được tích hợp với các công cụ khác, chẳng hạn như Jenkins và Travis CI, để tự động hóa quá trình phát triển và triển khai Kubernetes.

## Các khái niệm

- **Chart**: tập hợp file YAML template, đây là các file khai báo các source cần thiết để khởi tạo 1 ứng dụng. VD: Triển khai WordPress trên k8s cần các Image Docker như Source code, database, thì Helm sẽ tổ chức các Image cần thiết để khởi tạo ra ứng dụng.

- **Config**: nằm trong file values.yml, chứa nhưng khai báo về Image sử dụng, các biến môi trường, các Secret cần thiết để từ Template Helm khởi tạo ra services hoàn chỉnh (VD: Ingress, Deployment, ….)

- **Release**: là một version của K8s application đang chạy dựa trên Chart và kết hợp với một Config cụ thể.

- **Repositories**: Helm Charts có thể được publish thông qua nhiều repo khác nhau. Nó có thể là những private repo chỉ dùng trong nội bộ công ty, hoặc public thông qua Helm Hub. Một số Chart có thể có nhiều phiên bản của từng công ty hoặc publisher khác nhau. Riêng những Chart trong repo Stable thì luôn phải đáp ứng được tiêu chí từ Technical Requirements của Helm.

## Kiến trúc

**Helm** sử dụng kiến trúc **client-server** gồm:

- **Client CLI**: cung cấp cho developer sử dụng nó một **command-line interface (CLI)** để làm việc với *Charts*, *Config*, *Release*, *Repositories*. **Helm Client** sẽ tương tác với **Tiller Server**, để thực hiện các hành động khác nhau như *install*, *upgrade* và *rollback* với những *Charts*, *Release*.

- **Tiller Server**: **Server** nằm trong **Kubernetes cluster**, tương tác với **Helm Client** và giao tiếp **Kubernetes API server**. Từ đó, **Helm** có thể dễ dàng quản lý **Kubernetes** với các tác vụ như *install*, *upgrade*, *query* và *remove* đối với **Kubernetes resources**.

Cấu trúc của một **Helm Package**

    [root@master1181 apache]# tree
    .
    ├── charts
    │   └── common
    │       ├── Chart.yaml
    │       ├── README.md
    │       ├── templates
    │       │   ├── _affinities.tpl
    │       │   ├── _capabilities.tpl
    │       │   ├── _errors.tpl
    │       │   ├── _images.tpl
    │       │   ├── _labels.tpl
    │       │   ├── _names.tpl
    │       │   ├── _secrets.tpl
    │       │   ├── _storage.tpl
    │       │   ├── _tplvalues.tpl
    │       │   ├── _utils.tpl
    │       │   ├── _validations.tpl
    │       │   └── _warnings.tpl
    │       └── values.yaml
    ├── Chart.yaml
    ├── ci
    │   └── ct-values.yaml
    ├── files
    │   ├── README.md
    │   └── vhosts
    │       └── README.md
    ├── README.md
    ├── requirements.lock
    ├── requirements.yaml
    ├── templates
    │   ├── configmap-vhosts.yaml
    │   ├── configmap.yaml
    │   ├── deployment.yaml
    │   ├── _helpers.tpl
    │   ├── ingress.yaml
    │   ├── NOTES.txt
    │   └── svc.yaml
    ├── values.schema.json
    └── values.yaml

    7 directories, 31 files

Vai trò của các file và thư mục như sau:

- charts: những chart phụ thuộc có thể để vào đây tuy nhiên vẫn nên dùng requirements.yaml để link các chart phụ thuộc linh động hơn.
- templates: chứa những template file để khi kết hợp với các biến config (từ values.yaml và command-line) tạo thành các manifest file cho Kubernetes. Lưu ý: template file sử dụng format của ngôn ngữ lập trình Go.
- Chart.yaml: yaml file chứa các thông tin liên quan đến định nghĩa Chart như tên, version, …
- LICENSE/: license cho việc sử dụng Chart.
- README.md: miêu tả thông tin và cách sử dụng Chart tương tự README.md trong các project trên Github.
- requirements.yaml: yaml file chứa danh sách các link của các chart phụ thuộc.
- values.yaml: yaml file chứa các biến config mặc định cho Chart.

### Cài đặt Helm CLI

    curl -sSL https://raw.githubusercontent.com/helm/helm/master/scripts/get-helm-3 | bash

Kiểm tra **version**

    helm version --short

Tải các kho lưu trữ **Chart**

    helm repo add stable https://kubernetes-charts.storage.googleapis.com/
    helm search repo stable

Kết quả như hình:

![Helm Chart](/Image/Helm-Chart01.png)

Như các bạn thấy rất nhiều **repo stable** để cài đặt.

### Deploy Nginx với Helm

Để đảm bảo đã có đủ các **repo stable** có sẵn chúng ta sẽ chạy lệnh sau

    helm repo update

![Helm Chart](/Image/Helm-Chart02.png)

Tìm kiếm repo **Nginx**

    helm search repo nginx

![Helm Chart](/Image/Helm-Chart03.png)

Ở trên chúng ta có thể sử dụng **nginx-ingress** được **config** tích hợp sẵn với **kubernetes**. Đến đoạn này mình chỉ giới thiệu qua vì mình ko dùng **nginx-ingress** của **helm stable**. Cài đặt **nginx-ingress** thông tin chi tiết thêm [tại đây](https://github.com/helm/charts/tree/master/stable/nginx-ingress)

    helm install demo stable/nginx-ingress

Chúng ta muốn xóa **nginx-ingress** với **helm**

    helm uninstall demo

### Tự tạo chart nginx cho riêng mình và deploy chúng

**create chart**

chúng ta sẽ có 1 cấu trúc thư mục như sau

    /example-project
        /Chart.yaml # mô tả của chart
        /values.yaml # các giá trị mặc định, chúng ta có thể thay đổi trong khi cài đặt hay nâng cấp project của chúng ta
        /charts/ # subcharts
        /templates/ # template file

Bằng việc chạy lệnh

    helm create example-project

![Helm Chart](/Image/Helm-Chart04.png)

**Tùy chỉnh lại**

Trước khi tùy chỉnh chúng ta sẽ xem xét qua các nội dung của file ở phần *templates* chúng ta có các file như:

- deployment.yaml deployment với kubernetes
- _helpers.tpl mẫu có sẵn có thể tái sử dụng trong chart
- ingress.yaml liệt kê triển khai các quyền để ứng dụng của bạn truy cập được kubernetes
- serviceaccount.yaml tạo tài khoản cho service mình dùng 
- service.yaml triển khai các deployment với các service
- tests/ thư mục chứa các testing về chart

Chúng ta sẽ tự tạo **chart** từ đầu và chúng ta sẽ xóa các mục sau:

    rm -rf example-project/templates/
    rm -rf example-project/Chart.yaml
    rm -rf example-project/values.yaml

Tiếp theo chúng ta sẽ mô tả **chart** theo ý mình thích

    cat <<EoF > example-project/Chart.yaml
    apiVersion: v2
    name: example-project
    description: A Helm chart for Kubernetes Microservices application
    version: 0.1.0
    appVersion: 1.0
    EoF

Sau đó, chúng ta sẽ tạo các **deployment** và service đơn giản bên trong **templates**

    #tạo subfolders
    mkdir -p example-project/templates/deployment
    mkdir -p example-project/templates/service

Có 1 chút khác biệt so với deployment thông thường là có *config {{ .Values.replicas }}* hay *{{ .Values.hello.image }}:{{ .Values.version }}* được maping đến *values.yaml* sẽ khai báo sau

Tạo file: *templates/deployment/coffee-deployment.yaml*

    apiVersion: apps/v1
    kind: Deployment
    metadata:
    name: coffee
    spec:
    replicas: {{ .Values.replicas }}
    selector:
        matchLabels:
        app: coffee
    template:
        metadata:
        labels:
            app: coffee
        spec:
        containers:
        - name: coffee
            image: {{ .Values.hello.image }}:{{ .Values.version }}
            ports:
            - containerPort: 80

Tạo file: *templates/service/coffee-service.yaml*

    apiVersion: v1
    kind: Service
    metadata:
    name: coffee-svc
    spec:
    ports:
    - port: 80
        targetPort: 80
        protocol: TCP
        name: http
    selector:
        app: coffee

Tạo file values.yaml

    cat <<EoF > example-project/values.yaml
    # Default values for example-project.
    # This is a YAML-formatted file.
    # Declare variables to be passed into your templates.

    # Release-wide Values
    replicas: 3
    version: 'plain-text'

    # Service Specific Values
    hello:
    image: nginxdemos/hello
    EoF

**Deploy example-project Chart**

Trước khi deployment chúng ta sẽ kiểm tra **Chart** để xác nhận các **config** của chúng ta là chính xác

    helm install --debug --dry-run example-project example-project

![Helm Chart](/Image/Helm-Chart05.png)

Chúng ta sẽ **deploy**

    helm install example-project example-project


Chúng ta kiểm tra các **status** của **svc**,**deployment**,**pod**

    kubectl get svc,po,deploy

Chúng ta sẽ test thử nội dung cuả **nginx** này

    kubectl describe svc coffee-svc

![Helm Chart](/Image/Helm-Chart06.png)

    #chúng ta có 3 cái pods khác nhau, truy cập thử 1 cái
    curl 10.244.1.16:80

![Helm Chart](/Image/Helm-Chart07.png)

## Lợi ích của việc sử dụng helm qua ví dụ minh họa

Giả sử chúng ta **update** *example-project* của chúng ta với **image** không tồn tại vì 1 lý do nào đó thành n**ginxdemos/hello-non-existing** và cập nhật phiên bản mới của **project** của chúng ta

    helm upgrade example-project example-project

![Helm Chart](/Image/Helm-Chart08.png)

**Kubernetes** sẽ tạo **pod** khác với **image** mới đó nhưng có lỗi là **ImagePullBackOff**. Vậy chúng ta phải **Rollback** lại phiên bản

![Helm Chart](/Image/Helm-Chart09.png)

    helm history example-project

![Helm Chart](/Image/Helm-Chart010.png)

Rollback:

    helm rollback example-project 1

lỗi không còn nữa

![Helm Chart](/Image/Helm-Chart011.png)

**Xóa example-project**

Việc **uninstall** cũng nhanh không kém cạnh. Chúng ta không cần phải chạy 1 số câu lệnh lần lượt để **delete** *svc*, *deployment*, *ingress*, *secret* để xóa, mọi thứ chỉ 1 câu lệnh

    helm uninstall example-project

## Một số lợi ích của việc sử dụng Helm

- Quản lý các ứng dụng Kubernetes của bạn theo cách có cấu trúc và có thể mở rộng: Helm sử dụng các gói, được gọi là chart, để mô tả ứng dụng của bạn. Điều này giúp bạn quản lý các ứng dụng của mình một cách có tổ chức và dễ dàng hơn.

- Cài đặt các biểu đồ từ các kho lưu trữ: Helm cho phép bạn cài đặt các biểu đồ từ các kho lưu trữ. Điều này giúp bạn tiết kiệm thời gian và công sức khi cài đặt các ứng dụng mới.

- Tùy chỉnh các biểu đồ: Helm cho phép bạn tùy chỉnh các biểu đồ. Điều này giúp bạn phù hợp các biểu đồ với nhu cầu cụ thể của mình.

- Tạo các biểu đồ mới: Helm cho phép bạn tạo các biểu đồ mới. Điều này giúp bạn dễ dàng chia sẻ các ứng dụng của mình với người khác.

- Quản lý các phiên bản của các ứng dụng của bạn: Helm cho phép bạn quản lý các phiên bản của các ứng dụng của bạn. Điều này giúp bạn dễ dàng theo dõi các thay đổi đối với các ứng dụng của mình.

- Theo dõi các ứng dụng của bạn: Helm cho phép bạn theo dõi các ứng dụng của bạn. Điều này giúp bạn đảm bảo rằng các ứng dụng của bạn đang chạy bình thường.

## Tham khảo

- https://github.com/helm/helm