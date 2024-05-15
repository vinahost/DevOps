# Hướng dẫn cấu hình Jenkins CI/CD pipeline 1 dự án thực tế
Tiếp tục trong series CI/CD mình sẽ làm một bài hướng dẫn **Hướng dẫn cấu hình Jenkins CI/CD pipeline 1 dự án thực tế** Chắc hẳn bạn đã biết CI/CD là một phần công việc cực kỳ quan trọng của DevOps engineer, có những dự án CI/CD chiếm tới 50-70% khối lượng công việc của DevOps, đây là một dự án mà trước đây mình đã từng làm để bạn có thêm cái nhìn cụ thể, thực tế hơn về CI/CD.

**Lưu ý (phải biết):**

*   Đây là một bài viết **dài**… **rất dài**… vì mình hướng tới sự hoàn thiện đầy đủ vậy nên **cực kỳ chi tiết**  lại rất cụ thể và dễ tiếp cận, tuy nhiên nếu bạn muốn hiểu qua về Jenkins CICD Pipeline thì mình khuyên không nên đọc mất thời gian vì mình tập trung vào **thực hành thực tế** không phải lý thuyết.
*   Ngoài hướng dẫn chi tiết mình sẽ có **file**, **code** cụ thể trong bài viết để bạn có thể dễ dàng tham khảo.
*   Bài viết sẽ được làm **step by step**, từng bước chỉnh sửa hoàn thiện nên bạn có thể yên tâm nếu cả bạn là người mới.
*   Bạn sẽ dễ dàng nắm bắt kiến thức hơn khi đã biết cơ bản về Linux, Docker, Jenkins CICD Pipeline (phù hợp **fresher**, **junior**).
*   Hơn hết, **bạn là người chiến thắng** nếu như xem hết bài viết này và thực hành thành công trên dự án của bạn.

Nếu kiến thức này bạn thấy dễ dàng và bạn cần nâng cao và chuẩn hơn thì sắp tới mình cũng sẽ triển khai một CI/CD Pipeline deployment chi tiết khác tích hợp những công cụ khác ở on-premise như Ansible, Script, Unit Test,… trên AWS như Terraform, CodePipeline,… hoặc cơ bản hơn bạn có thể tham khảo bài viết này của mình [Setup gitlab ci/cd pipeline private registry](https://elroydev.tech/setup-gitlab-cicd-pipeline-private-registry/) sau đây chúng ta cùng đi thực hiện nội dung chi tiết.

Đầu tiên để có cái nhìn khái quát hơn mình sẽ đưa ra luồng xây dựng **Jenkins CICD Pipeline** cho dự án và đây cũng là điều quan trọng cần thiết bạn có thể nắm ngay được để tiện phát triển dự án của riêng bạn, theo cách của bạn.

Chuẩn bị tài nguyên
-------------------

Bạn cần những server nhất định để triển khai dự án Jenkins CICD Pipeline này, đó có thể là máy tính của bạn vậy thì bạn nên xem [Cách dùng Vagrant tạo máy ảo](https://elroydev.tech/cach-dung-vagrant-tao-may-ao/) để tiến hành tạo nhanh các server và chính xác nhất khi xóa tài nguyên cũng chỉ với 1 câu lệnh, hoặc trên những nền tảng khác ví dụ như [Tạo Linux/Ubuntu bằng VMWare](https://elroydev.tech/tao-linux-ubuntu-bang-vmware/). Các công nghệ, server dưới đây và các IP tương ứng bạn hãy xem và lưu ý nhé.

*   **Server 1: Gitlab server** – IP: **10.32.4.111:8090** (bạn có thể sử dụng Gitlab global hoặc có thể tham khảo bài viết này để tạo nhanh một Gitlab server [Setup Gitlab nội bộ](https://elroydev.tech/setup-gitlab-noi-bo-doanh-nghiep/))
*   **Server 2: Jenkins server** – IP: **10.32.3.170**
*   **Server 3: Sonarqube server** – IP: **10.32.3.171**
*   **Server 4: Database server** – IP: **10.32.3.160**
*   **Server 5: Staging server deploy** – IP: **10.32.3.172**
*   **Server 6: Product server deploy** – IP: **10.32.3.173**
*   **Server registry:** ở đây mình sử dụng **Dockerhub**, bạn cũng có thể tạo luôn một registry private để phù hợp tính bảo mật và thường registry sẽ được cấu hình, cài đặt trực tiếp trên Gitlab server và trên website mình cũng có làm hướng dẫn rồi.

Nếu tài nguyên máy tính bạn không đủ cài đặt tất cả các server đừng lo lắng, bạn cũng sẽ chỉ cần 2 server **Jenkins**, **Staging** và Gitlab global và Sonarqube cài trên cùng server Jenkins để thực hiện dự án vì mình đã chia ra vì mục đích sau này còn sử dụng cho nhiều dự án khác nữa.

Công nghệ:

*   Backend: Net 6
*   Frontend: Vue
*   Database: PostgreSql

Tổng quan các bước setup Jenkins CICD Pipeline
----------------------------------------------

*   **Step 1**: Setup các file cấu hình để chạy dự án bằng container (container hóa)
    *   Dockerize Backend.
    *   Dockerrize Frontend.
*   **Step 2**: Setup Jenkins server
    *   Cài đặt và thiết lập Jenkins server.
    *   Cài đặt Docker trên Jenkins server.
*   **Step 3**: Setup, tích hợp ban đầu Jenkins CICD Pipeline
    *   Tích hợp Gitlab với Jenkins bằng webhook.
    *   Thiết lập pipeline chạy job Jenkins CICD Pipeline đầu tiên.
*   **Step 4**: Setup Job test
    *   Thiết lập job test backend với NetCore.
    *   Thiết lập job test Sonarqube (test chất lượng code clean không).
*   **Step 5**: Setup migration database
*   **Step 6**: Setup build/packing project
    *   Dockerize dự án
    *   Push images lên Dockerhub
*   **Step 7**: Deploy staging
    *   Pull images về server
    *   Run container
*   **Step 8**: Deploy product

Thực hiện setup Jenkins CICD Pipeline
-------------------------------------

Trên đây là những gì cần lưu ý, chuẩn bị và các bước thực hiện, chúng ta sẽ tiến hành setup một Jenkins CICD Pipeline ngay sau đây.

**Những file cài đặt tại:** [file cài đặt Jenkins CI/CD Pipeline](https://elroydev.tech/file-cai-dat-jenkins-ci-cd-pipeline/)

### Step 1: Setup các file cấu hình để chạy dự án bằng container (container hóa)

Tại đây là dự án của mình và đang được lưu trên Gitlab với địa chỉ là: **10.32.4.111:8090**

![](http://elroydev.tech/wp-content/uploads/2023/04/1-project-setup-0-3.png)

Chạy dự án tại máy local và đây là BackEnd **NET 6** được tích hợp với swagger

![](http://elroydev.tech/wp-content/uploads/2023/04/1-project-setup-4.png)

Đây là FrontEnd **Vue**

![](http://elroydev.tech/wp-content/uploads/2023/04/1-project-setup-5.png)

Vào dự án tiến hành Dockerize dự án backend ở đây là một dự án Backend cơ bản chạy trong container port 80 bạn có thể tham khảo Dockerfile của mình được tạo tại **thư mục chứa file .sln** (với dự án của bạn nếu cũng là NetCore bạn có thể sử dụng chức năng tự động tạo ra Dockerfile tuy nhiên bạn nên tự viết để nắm rõ dự án và Docker)

![](http://elroydev.tech/wp-content/uploads/2023/04/1-project-setup-1.png)

Tương tự đây là Dockerfile của Frontend tại **thư mục cùng cấp với package.json** đây cũng là file cơ bản bạn có thể viết lại cho tối ưu tài nguyên và hiệu suất.

![](http://elroydev.tech/wp-content/uploads/2023/04/1-project-setup-2.png)

nginx.conf file đây là file được config với Dockerfile của Frontend bạn có thấy ở trên dòng cuối có sử dụng COPY file này không và nginx hiện tại chạy trong container là port 80 tức chạy http

![](http://elroydev.tech/wp-content/uploads/2023/04/1-project-setup-2-1.png)

docker-compose.yml file **tại thư mục gốc của dự án** đây là file cấu hình để chạy 2 service backend và frontend phía trên chút nữa chúng ta cũng sẽ thay đổi file này để phù hợp với các job phía sau và trông tổng quát hơn nhưng là step by step nên bạn cứ kiên nhẫn theo dõi nhé.

*   Backend port ở host (server) là **8081**
*   Frontend port ở host (server) là **80**

![](http://elroydevops.tech/wp-content/uploads/2023/04/1-project-setup-3-3.png)

Vậy là chúng ta đã thiết lập xong bước đầu Jenkins CICD Pipeline dự án bây giờ để tiến hành chạy thử tại thư mục gốc của dự án để xem dự án đã được dockerize chưa bạn sử dụng `docker-compose up -d` nếu thành công sẽ có 2 container được tạo tại thiết bị của bạn.

### Step 2: Setup Jenkins server trong Jenkins CICD Pipeline

Đây là bước tiếp theo setup Jenkins CICD Pipeline và như đã nói Jenkins server có IP là **10.32.3.170**

`$ ssh [[email protected]](https://elroydevops.tech/cdn-cgi/l/email-protection)` tiến hành ssh vào server

`$ mkdir /home/app-installed && cd /home/app-installed && touch jenkins-setup.sh && chmod +x jenkins-setup.sh && vim jenkins-setup.sh` tiến hành tạo thư mục để lưu cấu hình cài đặt các ứng dụng, di chuyển đến thư mục đó tạo một file script để cài đặt jenkins và cấp quyền thực thi file đó và bạn tiến hành viết nội dung mình highlight vào trong file đó rồi lưu lại, bạn có thể tham khảo các dùng vim tại [How to Use Vim – Tutorial for Beginners (freecodecamp.org)](https://www.freecodecamp.org/news/vim-beginners-guide/)

`$ ./jenkins-setup.sh` tiến hành chạy script để cài đặt jenkins

Nếu bạn thấy việc cài đặt này khó khăn bạn có thể cài đặt thủ công từng bước tham khảo tại: [How to Install Jenkins on Ubuntu](https://tecnstuff.net/how-to-install-jenkins-on-ubuntu-22-04/)

![](http://elroydevops.tech/wp-content/uploads/2023/04/2-jenkins-server-setup-1-6.png)

Cài đặt xong bạn truy cập **http://10.32.3.170:8080** để tiến hành setup hoàn thiện jenkins web

`$ cat /var/lib/jenkins/secrets/initialAdminPassword` command này sẽ lấy thông tin mật khẩu ban đầu của Jenkins server và tiến hành login

![](http://elroydev.tech/wp-content/uploads/2023/04/2-jenkins-server-setup-1-1.png)

Chọn install plugin mặc định cho đơn giản nhé

![](http://elroydev.tech/wp-content/uploads/2023/04/2-jenkins-server-setup-1-2.png)

Tạo tài khoản admin theo ý bạn nhé

![](http://elroydev.tech/wp-content/uploads/2023/04/2-jenkins-server-setup-1-3-1.png)

Cài đặt địa chỉ mặc định cho jenkins web bạn cứ để như vậy nhé

![](http://elroydev.tech/wp-content/uploads/2023/04/2-jenkins-server-setup-1-4-1.png)

Cài đặt thành công vào màn hình quản lý của jenkins web

![](http://elroydev.tech/wp-content/uploads/2023/04/2-jenkins-server-setup-1-5.png)

Tiếp theo, tương tự chúng ta sẽ cài đặt docker và docker-compose cho Jenkins server

`$ touch docker-setup.sh && chmod +x docker-setup.sh && vim docker-setup.sh` tương tự tạo file setup docker

`$ cat docker-setup.sh` được nội dung dưới đây

`$ ./docker-setup.sh` chạy script cài đặt docker và kiểm tra version được kết quả như dưới đây

`$ usermod -aG docker jenkins` thêm user jenkins vào group docker để jenkins có thể sử dụng docker trong Jenkins CICD Pipeline

Nếu bạn thấy việc cài đặt này khó khăn bạn có thể cài đặt thủ công docker từng bước tham khảo tại: [How to Install Docker on Ubuntu](https://phoenixnap.com/kb/install-docker-on-ubuntu-20-04)

![](http://elroydev.tech/wp-content/uploads/2023/04/2-jenkins-server-setup-2-1.png)

### Step 3: Setup, tích hợp ban đầu Jenkins CICD Pipeline

Trên giao diện jenkins web truy cập vào **Manage Jenkins -> Plugin Manage -> Available plugin** tiến hành cài đặt plugin **Gitlab** và **Docker**

![](http://elroydev.tech/wp-content/uploads/2023/04/2-jenkins-server-setup-3.png)

Tích chọn để jenkins web tự khởi động lại sau khi cài đặt xong plugin

![](http://elroydev.tech/wp-content/uploads/2023/04/2-jenkins-server-setup-4.png)

Trong lúc đó truy cập qua Gitlab tạo một Access tokens để thiết lập kết nối từ jenkins web đến Gitlab (chú ý người dùng tạo token này phải có quyền Admin)

![](http://elroydev.tech/wp-content/uploads/2023/04/2-jenkins-server-setup-5.png)

Copy và lưu trữ Token lại nhé

![](http://elroydev.tech/wp-content/uploads/2023/04/2-jenkins-server-setup-6-4.png)

Trong cài đặt **Admin Area -> Settings -> Network -> Outbound requests** tích chọn 2 phần này và nhấn lưu lại

![](http://elroydev.tech/wp-content/uploads/2023/04/2-jenkins-server-setup-7.png)

Quay lại giao diện Jenkins nhấn chọn **Manage Jenkins -> Configure System** tìm đến gitlab và tiến hành config như dưới đây

![](http://elroydev.tech/wp-content/uploads/2023/04/2-jenkins-server-setup-8.png)

Khi nhấn chọn thêm Jenkins phía trên sẽ mở ra popup để tạo Credential như dưới đây tiến hành nhập thông tin Token chính là token mà bạn vừa copy

![](http://elroydev.tech/wp-content/uploads/2023/04/2-jenkins-server-setup-8-1.png)

Quay lại chọn credentials mà chúng ta vừa tạo nhấn test và đã thành công

![](http://elroydev.tech/wp-content/uploads/2023/04/2-jenkins-server-setup-8-2-1.png)

Tiếp theo tiến hành tạo pipeline

![](http://elroydev.tech/wp-content/uploads/2023/04/2-jenkins-server-setup-10-13.png)

Điền tên dự án và chọn Pipeline

![](http://elroydev.tech/wp-content/uploads/2023/04/2-jenkins-server-setup-10-1-1.png)

Phần General tích chọn này là lưu lại 10 bản chạy gần nhất (bạn tùy chỉnh theo ý)

![](http://elroydev.tech/wp-content/uploads/2023/04/2-jenkins-server-setup-10-2.png)

Tiếp tục ở General, build triggers như dưới đây để Jenkins CICD Pipeline của bạn chạy cả được những action như push, merge code

![](http://elroydev.tech/wp-content/uploads/2023/04/2-jenkins-server-setup-10-3.png)

Tiến hành copy Url clone code của dự án

![](http://elroydev.tech/wp-content/uploads/2023/04/2-jenkins-server-setup-10-4.png)

Trong Advanced Project Options, phần pipeline chọn như sau và URL chính là URL vừa copy clone code

![](http://elroydev.tech/wp-content/uploads/2023/04/2-jenkins-server-setup-10-5.png)

Sau khi nhấn Jenkins thì hiện popup để thêm Credentials đây chính là account root của Gitlab, bạn có thể tạo một tài khoản admin như trên nhưng phải có quyền admin hoặc quyền đủ truy cập dự án nhé không Jenkins CICD Pipeline chạy không clone được code.

![](http://elroydev.tech/wp-content/uploads/2023/04/2-jenkins-server-setup-10-6.png)

Chọn lại user vừa tạo từ Credentials

![](http://elroydev.tech/wp-content/uploads/2023/04/2-jenkins-server-setup-10-7-1.png)

Đến phần Pipeline, branch ở đây mình chọn là **main** và **staging** vậy là khi merge request hoặc push từ 2 nhánh này Jenkins mới chạy nhé, bạn hãy tưởng tượng nếu config cho một nhánh develop hoặc nhánh nào đó mọi người đề có thể push được thì cái Jenkins CICD Pipeline pipeline của mình như cái chợ BUG.

Script Path Jenkinsfile nhé để chút chúng ta cấu hình nhấn lưu và đã hoàn thành xong setup Pipeline

![](http://elroydev.tech/wp-content/uploads/2023/04/2-jenkins-server-setup-10-8-1.png)

Bước cuối cùng tích hợp Gitlab với Jenkins chính là thiết lập webhook, bạn làm như sau

![](http://elroydev.tech/wp-content/uploads/2023/04/2-jenkins-server-setup-9.png)

Tạo token để tiến hành tích hợp trong Gitlab

![](http://elroydev.tech/wp-content/uploads/2023/04/2-jenkins-server-setup-9-1.png)

Trong dự án nhấn chọn **Settings -> Webhooks** cấu hình như sau:

*   URL: http://<account trên jenkins>:<token account jenkins>@<địa chỉ jenkins>/project/<tên project trên jenkins>

![](http://elroydev.tech/wp-content/uploads/2023/04/2-jenkins-server-setup-9-2.png)

Bỏ tích chọn SSL và nhấn Add webhook

![](http://elroydev.tech/wp-content/uploads/2023/04/2-jenkins-server-setup-9-3.png)

đến đây Jenkins CICD Pipeline của chúng ta đã tích hợp Gitlab và Jenkins.

Trong dự án tại thư mục gốc (cùng cấp docker-compose.yml file) của dự án bạn tiến hành tạo Jenkinsfile như dưới đây, mình chỉ thêm job đơn giản là in ra **người dùng** và **thư mục hiện tại**, bây giờ chúng ta sẽ thử commit code trên Gitlab, ở đây mình đang ở nhánh develop và tiến hành commit code.

![](http://elroydev.tech/wp-content/uploads/2023/04/2-jenkins-server-setup-11-8.png)

tạo merge request vào branch staging từ commit vừa push

![](http://elroydev.tech/wp-content/uploads/2023/04/2-jenkins-server-setup-11-1-2.png)

Tại đây viết thêm một số thông tin chi tiết thì viết nhé và nhấn tạo

![](http://elroydev.tech/wp-content/uploads/2023/04/2-jenkins-server-setup-11-2.png)

Tạo xong vì mình đang truy cập account có quyền maintain dự án nên được accept luôn như dưới đây

![](http://elroydev.tech/wp-content/uploads/2023/04/2-jenkins-server-setup-11-3.png)

Quay lại Jenkins và check vậy đã thành công

![](http://elroydev.tech/wp-content/uploads/2023/04/2-jenkins-server-setup-11-4.png)

Kết quả người dùng và thư mục hiện tại

![](http://elroydev.tech/wp-content/uploads/2023/04/2-jenkins-server-setup-11-5.png)

Vậy là trong step này chúng ta đã thiết lập thành công và chạy job đầu tiên của Jenkins CICD Pipeline.

### Step 4: Setup Job test trong Jenkins CICD Pipeline

#### Thiết lập test backend với dotnet test

`$ visudo` quay lại ssh server jenkins tiến hành chạy lệnh này và thêm dòng dưới đây vào file để User jenkins chạy những job Jenkins CICD Pipeline quyền root không cần hỏi password

![](http://elroydev.tech/wp-content/uploads/2023/04/2-jenkins-server-setup-12.png)

Tạo một Dockerfile.dotnet6 cùng cấp với Dockerfile tại thư mục Backend với nội dung sau, cụ thể là tải image dotnet sdk 6 về và cài đặt nuget dotnet-ef sau chúng ta sử dụng để migration và sonarscanner.

![](http://elroydev.tech/wp-content/uploads/2023/04/2-jenkins-server-setup-13.png)

Tại Jenkinsfile chỉnh sửa và thêm stage test như sau và tiến hành push, chú ý hiện tại chúng ta đang ở branch staging nên không cần gửi merge request như ban nãy mà chỉ cần push là sẽ được luôn nhé

![](http://elroydev.tech/wp-content/uploads/2023/04/2-jenkins-server-setup-14-1.png)

Quay lại jenkins thấy job chạy thành công

![](http://elroydev.tech/wp-content/uploads/2023/04/2-jenkins-server-setup-15.png)

#### Thiết lập test dự án với Sonarqube

Đây là bước tiếp theo setup Jenkins CICD Pipeline và như đã nói **Sonarqube server** có IP là **10.32.3.171**

`$ ssh [[email protected]](https://elroydevops.tech/cdn-cgi/l/email-protection)` tiến hành ssh vào server

Tiến hành cài đặt docker trên server này và chạy lệnh dưới đây để chạy Sonarqube

Hoặc bạn muốn lưu lại file docker-compose.yml để config sau này có thể tham khảo: [How to run SonarQube using Docker-Compose](https://blog.knoldus.com/how-to-run-sonarqube-using-docker-compose/)

![](http://elroydev.tech/wp-content/uploads/2023/04/3-sonarqube-server-setup-1-1.png)

Sau đó bạn sẽ truy cập **http://10.32.3.171:9000**

![](http://elroydev.tech/wp-content/uploads/2023/04/3-sonarqube-server-setup-2.png)

Vì dự án mình tạo trên gitlab nên chọn luôn còn không có thể chọn Manually để thiết lập thủ công hơn

![](http://elroydev.tech/wp-content/uploads/2023/04/3-sonarqube-server-setup-3-4.png)

Nhập thông tin sau:

*   GITLAB API URL: http://<ip or domain server gitlab>/api/v4/
*   Persional Access Token mà lúc nãy chúng ta đã tạo trên Gitlab

![](http://elroydev.tech/wp-content/uploads/2023/04/3-sonarqube-server-setup-4-1.png)

Chọn vào dự án

![](http://elroydev.tech/wp-content/uploads/2023/04/3-sonarqube-server-setup-5.png)

Bạn Click vào ảnh đại diện góc phải màn hình -> Security, tiến hành tạo Token ở đây mình khuyên bạn tạo token global còn ở đây mình chọn Project cụ thể

![](http://elroydev.tech/wp-content/uploads/2023/04/3-sonarqube-server-setup-6.png)

Quay lại server Jenkins cài đặt plugin **SonarQube Scanner**

![](http://elroydev.tech/wp-content/uploads/2023/04/3-sonarqube-server-setup-7-1.png)

Truy cập theo dưới đây tạo credentials xác thực với Sonarqube server (chú ý secret chính là token ở server Sonarqube mà chúng ta vừa tạo)

![](http://elroydev.tech/wp-content/uploads/2023/04/3-sonarqube-server-setup-8.png)

Config system Sonarqube dưới đây (chú ý phần server auten token: chọn credentials mới tạo phía trên)

![](http://elroydev.tech/wp-content/uploads/2023/04/3-sonarqube-server-setup-9.png)

Trong Jenkinsfile chúng ta thiết lập thêm những thông tin sau và tiến hành push code

![](http://elroydev.tech/wp-content/uploads/2023/04/sonarqube-1.png)

hoặc bạn có thể tham khảo cách quét sonar sau: ``docker run -v `pwd`:/app --workdir="/app" sonarsource/sonar-scanner-cli sonar-scanner -X $SONAR_SCANNER_OPTS -Dsonar.host.url="${SONAR_HOST_URL}" -Dsonar.login="${SONAR_TOKEN}" -Dsonar.sources="." -Dsonar.projectKey="${SONAR_PROJECT_KEY}" -Dsonar.projectName="${SONAR_PROJECT_KEY}" -Dsonar.projectVersion="V_${MIGRATION_NAME}"``

Thành công được kết quả dưới đây

![](http://elroydev.tech/wp-content/uploads/2023/04/3-sonarqube-server-setup-11.png)

Truy cập sang Sonarqube thấy code đã được check

![](http://elroydev.tech/wp-content/uploads/2023/04/sonarqube-2-2.png)

Vậy là chúng ta đã setup thành công test với Sonarqube tuy nhiên đây chỉ là bước vô cùng đơn giản và còn rất nhiều thứ trong này nhưng mình sẽ không đi sâu thêm. Và chúng ta cũng đã setup xong bước trong Jenkins CICD Pipeline này.

### Step 5: Setup migration database trong Jenkins CICD Pipeline

Ở đây mình có một server database và đã được config với backend, có 3 database chính là develop, staging, product nên trong dự án của bạn, bạn setup nhánh develop thì cấu hình database, staging cấu hình cho database staging và product tương tự, nên khi chúng ta chạy job migration code đã mới nhất thì cấu trúc database ở môi trường develop sẽ tương ứng với staging và production.

Đầu tiên chúng ta cài thêm plugin Blue Ocean một giao diện người dùng mới của Jenkins với mục đích cải thiện trải nghiệm người dùng, tăng tính trực quan và dễ sử dụng của Jenkins.

![](http://elroydev.tech/wp-content/uploads/2023/04/4-setup-migration-1-3.png)

Sau đó ta thấy lựa chọn **Open Blue Ocean** dưới đây

![](http://elroydev.tech/wp-content/uploads/2023/04/4-setup-migration-2.png)

Trong Jenkinsfile tạo thêm một stage Migration như dưới đây:

*   Ở đây mình viết stage cần xác nhận mới tiến hành thực thi migration còn không thì sẽ bỏ qua bước này chạy stage tiếp.
*   không hẳn lúc nào chúng ta chạy Jenkins CICD Pipeline cũng cần migration database (vì vậy để làm hướng dẫn mình đã comment lại chỗ migration)

Tiến hành commit code

![](http://elroydev.tech/wp-content/uploads/2023/04/4-setup-migration-3.png)

Xem kết quả đến job migration sẽ hiển thị thông báo lựa chọn

![](http://elroydev.tech/wp-content/uploads/2023/04/4-setup-migration-4.png)

Và mình nhấn **no** kết quả hiển thị dưới đây.

![](http://elroydev.tech/wp-content/uploads/2023/04/4-setup-migration-5.png)

Vậy là chúng ta đã setup xong stage migration database này trong Jenkins CICD Pipeline

### Step 6: Setup build/packing project

Tiến hành tạo tiếp tục Credentials để login vào Dockerhub như dưới đây

#### ![](http://elroydev.tech/wp-content/uploads/2023/04/5-setup-build-project-2.png)

Tạo thành công bạn hãy chú ý đến ID nhé vì chúng ta sẽ sử dụng nó trong Jenkinsfile

#### ![](http://elroydev.tech/wp-content/uploads/2023/04/5-setup-build-project-3.png)

Trong docker-compose.yml file chỉnh sửa như sau:

*   container\_name: tùy chỉnh sử dụng biến để phù hợp với từng dự án
*   image: định nghĩa image để tag của image được xác định mỗi lần commit code (có giá trị mặc định là “1.0.0”)
*   envinroment: biến môi trường mà đã thiết lập ở trên

#### ![](http://elroydev.tech/wp-content/uploads/2023/04/5-setup-build-project-5.png)

Thêm một số thông tin:

*   DOCKER\_HUB: tương ứng usename ở dockerhub
*   DOCKERHUB\_CREDENTIALS: là credentials mà nãy chúng ta tạo
*   NAME\_BACKEND và NAME FRONTEND là tên của Docker container và Image sau đây mình config
*   DOCER\_TAG: ở đây mình sẽ lấy theo branch commit và một mã băm (hash) được tạo ra bởi Git để đại diện cho commit cụ thể đó và mình tiến hành cắt string lấy 7 ký tự đầu tiên của mã băm từ biến GIT\_COMMIT
    *   Ví dụ: Nếu tên nhánh trên Git là “feature/new-feature” và mã băm commit là “abc123def456”, thì giá trị cuối cùng của biến DOCKER\_TAG sẽ là “new-feature-abc123d”.

#### ![](http://elroydev.tech/wp-content/uploads/2023/04/5-setup-build-project-6.png)

Thêm stage build and push images như dưới đây

#### ![](http://elroydev.tech/wp-content/uploads/2023/04/5-setup-build-project-6-1.png)

Tạo 2 repository trên dockerhub (chú ý là trên dockerhub chỉ có 1 repository được private còn lại sẽ public ở phiên bản miễn phí nhé)

#### ![](http://elroydev.tech/wp-content/uploads/2023/04/5-setup-build-project-4-1.png)

Tiến hành commit và kiểm tra Jenkins CICD Pipeline chạy thành công

![](http://elroydev.tech/wp-content/uploads/2023/04/5-setup-build-project-7-1.png)

Kiểm tra trên dockerhub thấy commit và repo backend đã có image

#### ![](http://elroydev.tech/wp-content/uploads/2023/04/5-setup-build-project-8-3.png)

Tương tự xem repo frontend image cũng đã lên thành công

#### ![](http://elroydev.tech/wp-content/uploads/2023/04/5-setup-build-project-9.png)

Vậy là đến đây chúng ta đã tiến hành xong bước đóng gói và đẩy image lên registry ở đây là dockerhub trong Jenkins CICD Pipeline

### Step 7: Deploy staging

Như đã nói Staging deploy server có địa chỉ IP **10.32.3.172** cũng là những bước cuối cùng trong Jenkins CICD Pipeline, tương tự trên server này bạn cũng phải cài đặt docker.

Tại Jenkins tiến hành cài đặt plugin **SSH Agent**

![](http://elroydev.tech/wp-content/uploads/2023/04/6-setup-deploy-1.png)

Trong server Jenkins tiến hành tạo khóa ssh và access để có thể truy cập sang Staging deploy server như sau:

`$ ssh-keygen -t rsa` tạo ra một cặp khóa công khai và khóa bí mật RSA cho việc xác thực truy cập từ xa khi Jenkins CICD Pipeline

`$ ssh-copy-id -i ~/.ssh/id_rsa.pub [[email protected]](https://elroydevops.tech/cdn-cgi/l/email-protection)` sao chép khóa công khai của người dùng lên server Staging

![](http://elroydev.tech/wp-content/uploads/2023/04/6-setup-deploy-2.png)

Thử ssh vào từ server Jenkins và không cần nhập mật khẩu đã thành công ssh vào server staging

![](http://elroydev.tech/wp-content/uploads/2023/04/6-setup-deploy-3.png)

`$ cd ~/.ssh && cat id_rsa` hiển thị nội dung của khóa riêng tư bạn hãy copy nó ra một chỗ

![](http://elroydev.tech/wp-content/uploads/2023/04/6-setup-deploy-4-1.png)

Tiếp tục chúng ta sẽ tạo credentials như sau:

*   Kind: SSH Username with private key
*   ID: jenkins-ssh-key (bạn hãy chú ý để chút nữa chúng ta sử dụng trong Jenkinsfile)
*   Username: root (user truy cập để tạo khóa vừa rồi)
*   Key: chính là nội dung của **id\_rsa** mà chúng ta vừa xem

![](http://elroydev.tech/wp-content/uploads/2023/04/6-setup-deploy-5.png)

Trong Jenkinsfile tiến hành tạo stage như sau:

*   Tạo 1 biến deploying với những dòng nội dung của bash script
    *   Xóa container cũ đang chạy.
    *   Pull image dựa theo tag mà chúng ta đã định nghĩa phía trên.
    *   Chạy container với image mới.
*   Sử dụng sshagent để tiến hành ssh vào Staging server
    *   Ghi nội dung của biến deploying vào file deploy.sh và tiến hành chạy file đó.

Tiến hành commit và push lại để chạy Jenkins CICD Pipeline

![](http://elroydev.tech/wp-content/uploads/2023/04/6-setup-deploy-6-3.png)

Kết quả đã deploy thành công trên Staging server

![jenkins cicd pipeline](http://elroydev.tech/wp-content/uploads/2023/04/6-setup-deploy-7-3.png)

Truy cập địa chỉ **http://10.32.3.172:8081/swagger/index.html** đã thấy backend chạy thành công.

![](http://elroydev.tech/wp-content/uploads/2023/04/6-setup-deploy-8-2.png)

Truy cập **http://10.32.4.172** đã thấy Frontend chạy thành công (nếu bạn thắc mắc tại sao không ghi cổng 80 như docker-compose.yml file thì là vì mặc định cổng 80 là http)

![](http://elroydev.tech/wp-content/uploads/2023/04/6-setup-deploy-9.png)

### Step 8: Deploy to Production

Stage này mình nhường cho bạn và mình sẽ dừng bài hướng dẫn tại đây, đây cũng như một phần thực hành cuối cùng bạn áp dụng để hoàn thành nó, mình mong rằng bạn sẽ hoàn thành Jenkins CICD Pipeline này.

Vậy là chúng ta đã thành công triển khai một Jenkins CICD Pipeline mà mình đã hướng dẫn cực kỳ chi tiết nếu bạn hoàn thành và hiểu toàn bộ thì bạn thật sự đã có kiến cơ bản về devops. Sau đây mình cũng sẽ hướng dẫn rất nhiều về CI/CD pipeline với những sự chỉnh chu và chất lượng, nâng cao hơn. Mong rằng cộng đồng DevOps của chúng ta ngày càng mở rộng và những bạn mới sẽ có nhiều cơ hội hơn để làm DevOps.

**Mọi thắc mắc bạn có thể liên hệ:**

- Email: elroydevops@gmail.com

- Link gốc: https://elroydev.tech/jenkins-cicd-pipeline-du-an-thuc-te-sieu-chi-tiet/
