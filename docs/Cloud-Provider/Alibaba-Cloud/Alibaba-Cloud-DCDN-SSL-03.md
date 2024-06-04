# Tự động triển khai DCDN bằng SDK với Free SSL từ Let's encrypt (Phần 3)

> Ở các phần trước, mình đã hoàn thành việc đăng ký chứng chỉ SSL và upload lên Alibaba Cloud. Ở phần này, mình sẽ tập trung vào triển khai DCDN qua python SDK.

### **1. Sơ lược về DCDN của Alibaba Cloud**
- DCDN (**Alibaba Cloud Dynamic Route for Content Delivery Network**) là một dịch vụ phân phối nội dung mạnh mẽ, đưa website đến gần người dùng hơn, và nâng cao bảo mật của website. Không như CDN thông thường chỉ phân phối các nội dung tĩnh (static content) như hình ảnh, các file css, js,... DCDN tự động phân loại các nội dung "động" (dynamic) và "tĩnh", với các yêu cầu với dữ liệu tĩnh, nó tự động cache trên các node (Points of Presence) để tăng tốc độ truyền tải và giảm tải cho Origin Server. Với các dữ liệu đòi hỏi xử lý ở Origin Server (dynamic content), DCDN sẽ sử dụng thuật toán route thông minh để tối ưu nhất tốc độ truyền tải giữa end user và Origin Server dựa trên mạng lưới hạ tầng mạnh mẽ của mình. Điều này mang lại sự tối ưu và giảm sự phức tạp khi triển khai (không cần can thiệp nhiều ở backend server).
- Origin Server hay **Backend Server** là nơi lưu trữ dịch vụ của người dùng, có thể được triển khai trên hosting, VPS, Cloud hay server vật lý. 
- **DCDN** không chỉ mang lại lợi ích về **tốc độ truyền tải**, **giảm tải** cho Orign Server, nó còn nâng cao tính **bảo mật**, do các traffic từ người dùng đều phải đi qua các POP DCDN trước khi đến được Origin Server (về cơ bản nó giúp che giấu IP thật và tránh bị tấn công trực tiếp). Điều này cũng cho phép DCDN có thể tích hợp các giải pháp bảo mật nâng cao "**Edge Security**" như **WAF, anti DDoS** để bảo vệ các dịch vụ phía sau một cách hiệu quả nhất. 
- Để **triển khai DCDN**, người dùng cần có một dịch vụ đang chạy. Sau khi **Active** dịch vụ DCDN trên giao diện web của Alibaba Cloud, có thể tiến hành deploy một domain lên Alibaba Cloud DCDN. Các bước có thể tóm tắt như sau:
	- **Add domain name**: trong bước này, người dùng cần xác nhận với Alibaba Cloud rằng mình là người sở hữu domain này. Để làm được điều đó, Alibaba sẽ sinh ra một bản ghi TXT và người dùng phải thêm bản ghi dns này vào hệ thống dns của mình (tương tự như bước xác thực của Let's Encrypt lúc đăng ký SSL).
	- Khi Add domain, người dùng cần chọn **Backend server**. Nó có thể là IP hoặc site domain hay OSS domain. Ngoài ra cần cung cấp port và weight (nếu có nhiều server).
	- Sau khi hoàn tất, DCDN sẽ có một khoảng thời gian để config trên hệ thống của họ. Sau đó, một bản ghi **CNAME** được sinh ra, người dùng cần phải thay đổi domain của website về bản ghi CNAME này. Để có thể **điều hướng traffic** sang các POP DCDN.
	- Lúc đầu DCDN sẽ nhận các traffic **http**, khi này cần **enable HTTPS**, để enable HTTPS cần có một **SSL certificate** được thêm vào trên DCDN. 
	- Về cơ bản, đến bước này, DCDN đã hoạt động, sau khi DCDN hoạt động sẽ có rất nhiều cấu hình khác và cả tích hợp Edge Security, nhưng không nằm trong phạm vi bài này.
	> Lưu ý: DCDN có thể triển khai với second-level domain nhưng chỉ **xác thực với first-level domain**.
### **2. Xây dựng các hàm ví dụ**
- Hàm **GetDnsRecordValue** dùng trong việc kiểm tra record đã có trên hệ thống hay chưa, hàm nhận vào domain name, RR là name của bản ghi. Ví dụ như cần tìm bản ghi liên quan đến domain blog.vinahost.cloud thì tham số Domain='vinahost.cloud', thamm số RR='blog'. Hàm sẽ trả về một object chứa danh sách thông tin các bản ghi filter được, có thể xem giá trị của của từng bản ghi bằng cú pháp: res.body.domain_records.record[0].value. Lọc theo mode EXACT sẽ không áp dụng các bộ lọc như Type, RR_keyword,..., chỉ cần điền KeyWord là đủ. Kết quả sẽ liệt kê chính xác các domain cần tìm, và mặc định là cái nào thêm vào mới nhất thì xếp ở đầu tiên. Bỏ qua sự phức tạp, mình chỉ lấy về một domain (để ví dụ).
```python
def GetDnsRecordValue(AccessKey, SecretKey,PrimaryDomain,RR): # RR is hostname
  config = open_api_models.Config(
            access_key_id=AccessKey,
            access_key_secret=SecretKey,
            region_id="ap-southeast-1"
  )
  config.endpoint="alidns.ap-southeast-1.aliyuncs.com"
  #[product_code].[region_id].aliyuncs.com
  dns_client=AlidnsClient(config)
  describe_domain_record_request = alidns_models.DescribeDomainRecordsRequest(
    domain_name=PrimaryDomain,
    search_mode= "EXACT",
    page_number=1,
    page_size=1, # new record first
    key_word=RR,
  )
  try:
      repons = dns_client.describe_domain_records_with_options(describe_domain_record_request, util_models.RuntimeOptions())
      return repons
  except Exception as error:
      print(error)
```
- Hàm **DeleteDnsRecord**: tương tự như trên, mình cũng xây dựng hàm để xóa record, hàm xóa record này chỉ yêu cầu Record ID, Record ID có thể dùng lại hàm **GetDnsRecordValue** để tìm, res của hàm trên trả về rất nhiều thông tin, thay vì lấy Value, mình trích xuất RecordID của chúng để dùng trong hàm delete này. Cú pháp trích xuất tương tự như: "**res.body.domain_records.record[0].record_id**", Hàm mẫu ví dụ:
```python
def DeleteDnsRecord(AccessKey, SecretKey,RecordID): # RR is hostname
  config = open_api_models.Config(
            access_key_id=AccessKey,
            access_key_secret=SecretKey,
            region_id="ap-southeast-1"
  )
  config.endpoint="alidns.ap-southeast-1.aliyuncs.com"
  #[product_code].[region_id].aliyuncs.com
  dns_client=AlidnsClient(config)
  delete_domain_record_request = alidns_models.DeleteDomainRecordRequest(
    record_id=RecordID,
  )
  try:
      repons = dns_client.delete_domain_record_with_options(delete_domain_record_request, util_models.RuntimeOptions())
      return repons
  except Exception as error:
      print(error)
```
- Hàm **DcdnVerifyDomain**: hàm này sẽ thực hiện đọc **TXT record** mà Alibaba Cloud yêu cầu qua việc gọi **describe_dcdn_verify_content()**, hàm trả về giá trị của record TXT cần trỏ, sau đó gọi hàm addDomain  (ở phần trước) để thêm record, đồng thời gửi yêu cầu Verify lên Alibaba Cloud để xác nhận. Nếu thành công, res.status_code là 200. Tham số Domain name là doamin bất kỳ, và hàm phải tách lấy **first-level domain** để xác thực. Dưới đây là ví dụ về hàm:
```python
def DcdnVerifyDomain(AccessKey, SecretKey, DomainName): #Verify First Level-Domain
  config = open_api_models.Config(
            access_key_id=AccessKey,
            access_key_secret=SecretKey,
            region_id="ap-southeast-1"
  )
  dcdn_client=dcdnClient(config)
  parts=DomainName.split('.')
  primary_domain='.'.join(parts[-2:])
  describe_verify_content_request = dcdn_models.DescribeDcdnVerifyContentRequest(
        domain_name=primary_domain
  )
  check_dns=False
  try:
      repons = dcdn_client.describe_dcdn_verify_content_with_options(describe_verify_content_request, util_models.RuntimeOptions())
      value=repons.body.content
      addRecordResult=addDnsRecord(AccessKey,SecretKey,primary_domain,"TXT","verification",value)
      print("Check Record verification."+primary_domain, "Value: ",value)
      sleep(10)
      for i in range(1,5):
         res = GetDnsRecordValue(AccessKey,SecretKey,primary_domain,'verification')
         if (res.body.total_count > 0) and (res.body.domain_records.record[0].value==value):
            check_dns=True
            break
         else:
            if (i==4):
              print("Failed to GetDnsRecordValue")
              return 0
            else:
              sleep(10)
              continue
  except Exception as error:
            print(error)
  if check_dns:
    verify_content_own_request= dcdn_models.VerifyDcdnDomainOwnerRequest(
        domain_name=primary_domain,
        verify_type="dnsCheck"
    )
    try:
        repons_own = dcdn_client.verify_dcdn_domain_owner_with_options(verify_content_own_request, util_models.RuntimeOptions())
        return repons_own
    except Exception as error:
        print(error)
        return 0
  else:
    print("Failed to addDnsRecord")
    return 0
```
- Hàm **DcdnAddDomain**: hàm này được viết để thêm domain vào DCDN, trong đó backend server là **IPaddress**. Hàm có yêu cầu về **Scope** là phạm vi triển khai DCDN, với mặc định là option **Domestic** (tại Trung Quốc), **Overseas** (toàn Thế giới nhưng không gồm Trung Quốc) và cuối cùng là **Global** (Toàn thế giới). Việc phân chia như vậy do chính sách **IPA** của Trung Quốc. Về Scence, có nhiều ngữ cảnh triển khai khác nhau, trong trường hợp triển khai web có thể chọn "**webservicescene**", Source là là 1 chuỗi,  có thể tạo một biến để chứa các thông tin của Source sau đó dùng hàm **json.dump()** để truyền vào hàm. Tham số **DomainName** trong hàm này có thể là một domain bất kỳ muốn triển khai DCDN, như một second-level domain. Ví dụ mẫu như bên dưới: 
```python
def DcdnAddDomain(AccessKey, SecretKey, DomainName,OrginAddr,OrginType,OriginPort,Scope):
  config = open_api_models.Config(
            access_key_id=AccessKey,
            access_key_secret=SecretKey,
            region_id="ap-southeast-1"
  )
  dcdn_client=dcdnClient(config)

  originType=("ipaddr","domain","oss")
  scope_define=("global","overseas","domestic")
  Scene=('apiscene','webservicescene','staticscene','null')
  my_scope=""
  if OrginType in originType:
    Type=OrginType
  else:
    Type="ipaddr"
  if Scope in scope_define:
    my_scope=Scope
  else:
    Type="overseas"
  source=[{"content":OrginAddr,"type":Type,"priority":"20","port":OriginPort}]

  add_dcdn_domain_request = dcdn_models.AddDcdnDomainRequest(
        domain_name=DomainName,
        sources=json.dumps(source),
        scope=my_scope,
  )
  try:
      repons = dcdn_client.add_dcdn_domain_with_options(add_dcdn_domain_request, util_models.RuntimeOptions())
      return repons
  except Exception as error:
    print(error)
  return 0
```
- Hàm **DcdnConfigCname**: sau khi đã thêm domain vào DCDN, lúc này, domain phải trỏ về bản ghi mà DCDN yêu cầu. Thông thường, CNAME sẽ có dạng: <domain_name>.<w.cdngslb.com> DCDN SDK có hàm **describe_dcdn_domain_detail_request** chứa thông tin về domain vừa được thêm, trích xuất thông tin về CNAME cần trỏ. Tuy nhiên, code có tốc độ chạy rất nhanh, từ lúc gọi hàm **DcdnAddDomain** đến hàm **DcdnConfigCname** rất ngắn, lúc này, trên Alibaba Cloud vẫn đang cấu hình. Trong thời gian này, nếu gọi API sẽ trả về rỗng hoặc lỗi. Khi lấy được giá trị CNAME, thì tiếp tục gọi hàm add domain để thêm bản ghi này. Ví dụ mẫu:
```python
def DcdnConfigCname(AccessKey, SecretKey, DomainName):
    config = open_api_models.Config(
            access_key_id=AccessKey,
            access_key_secret=SecretKey,
            region_id="ap-southeast-1"
    )
    dcdn_client=dcdnClient(config)
    describe_dcdn_domain_detail_request = dcdn_models.DescribeDcdnDomainDetailRequest(
      domain_name=DomainName,
    )
    cname_record=''
    parts = DomainName.split('.')
    RR='@' #hostname, domain is vinahost.cloud, hostname blog -> blog.vinahost.cloud
    if len(parts) > 2: # parts <2 Primary Domain
      RR = '.'.join(parts[:-2])
      primary_domain='.'.join(parts[-2:])
    try:
      for i in range(1,5):
          repons = dcdn_client.describe_dcdn_domain_detail_with_options(describe_dcdn_domain_detail_request, util_models.RuntimeOptions())
          cname_record=repons.body.domain_detail.cname
          print(repons.body.domain_detail)
          print("Add CNAME Value: ",repons.body.domain_detail.cname," for domain: "+ primary_domain, " RR = ", RR)
          if cname_record != '':
             res=GetDnsRecordValue(AccessKey,SecretKey,primary_domain,RR)
             while res.body.total_count>0: # Clear all A record
               print('Find:',res.body.total_count,'Record',res.body.domain_records.record[0].record_id)
               rid=res.body.domain_records.record[0].record_id
               resp=DeleteDnsRecord(AccessKey,SecretKey,rid)
               res=GetDnsRecordValue(AccessKey,SecretKey,primary_domain,RR)
             result=addDnsRecord(AccessKey, SecretKey, primary_domain,"CNAME",RR,cname_record)
             return result
          else:
             if (i==4):
               if cname_record=="":
                   print("describe_dcdn_domain_detail failed!")
               return 0
             else:
               sleep(10*i)
               continue
    except Exception as error:
      print(error)
    return 0
```
- Hàm **DcdnEnableHTTPS**: Giá traffic của HTTP và HTTPS là khác nhau, nên khi thêm domain vào DCDN, cần chủ động **Enable HTTPS** để client có thể truy cập website qua HTTPS. SDK cung cấp hàm **set_dcdn_domain_sslcertificate()** để thiết lập việc này. Hàm enable bắt buộc yêu cầu cả **CertName và CertID** (option cert_id chỉ hiệu quả khi cert_type là cas). **SSLProtocol là "on"** để kích hoạt SSL certificate. Các tham số khác có thể tham khảo trong document của Alibaba Cloud. Code mẫu như sau:
```python
def DcdnEnableHTTPS(AccessKey, SecretKey, DomainName, CertName,CertID):
  config = open_api_models.Config(
            access_key_id=AccessKey,
            access_key_secret=SecretKey,
            region_id="ap-southeast-1"
  )
  dcdn_client=dcdnClient(config)
  set_dcdn_domain_SSL_certificate_request = dcdn_models.SetDcdnDomainSSLCertificateRequest(
        domain_name=DomainName,
        cert_type="cas",
        sslprotocol="on",
        cert_name=CertName,
        cert_region="ap-southeast-1",
        cert_id=CertID
  )
  try:
      repons = dcdn_client.set_dcdn_domain_sslcertificate_with_options(set_dcdn_domain_SSL_certificate_request, util_models.RuntimeOptions())
      print("Enable  DCDN Success")
      return repons
  except Exception as error:
      print(error)
  return 0
```
- Hàm **DcndCheckDomainDetail**: Hàm này đã có một đoạn trong hàm DcdnConfigCname, mình tách riêng ra để lấy thông tin về trạng thái của domain trên DCDN. Hàm **DescribeDcdnDomainDetail** của Alibaba Cloud trả về rất nhiều thông tin như Domain, Domain Status, SSL Protocal (nếu on là đã bật https), bản ghi CNAME. Khi cần quan tâm đến trạng thái của domain, có thể trích xuất phần "**DomainStatus**". Hàm ví dụ:
```python
def DcndCheckDomainDetail(AccessKey, SecretKey, DomainName):
    config = open_api_models.Config(
            access_key_id=AccessKey,
            access_key_secret=SecretKey,
            region_id="ap-southeast-1"
    )
    dcdn_client=dcdnClient(config)
    describe_dcdn_domain_detail_request = dcdn_models.DescribeDcdnDomainDetailRequest(
      domain_name=DomainName,
    )
    try:
      res = dcdn_client.describe_dcdn_domain_detail_with_options(describe_dcdn_domain_detail_request, util_models.RuntimeOptions())
      return res.body.domain_detail
    except Exception as error:
      print(error)
```
- Đến đây, hầu hết các hàm đã hoàn tất, DCDN đã hoạt động và **được kích hoạt https**, nghĩa là từ client đến DCDN là https, còn từ DCDN đến orgin server có sử dụng https hay không tùy thuộc vào cấu hình backend server lúc thêm domain.
- Lúc này, có thể viết thêm **hàm tổng hợp** các bước trên:
```python
def DcdnStartOnAlibabaCloud(AccessKey, SecretKey, DomainName,Endpoint,CertID):
    res=DcdnVerifyDomain(AccessKey, SecretKey,DomainName)
    if res==0:
      print("Failed to DcdnVerifyDomain")
      return 0
    ipaddr=Endpoint['ip']
    port=Endpoint['port']
    res=DcdnAddDomain(AccessKey, SecretKey, DomainName ,ipaddr,"ipaddr",port,"overseas")
    print("Add Dcdn Domain:",res)
    count=3
    flag_check=False
    while count:
      res=DcndCheckDomainDetail(AccessKey, SecretKey, DomainName)
      if(res.domain_status=="configuring"):
        flag_check=True
        break
      else:
        print("domain_status configuring check",count)
        sleep(10)
        count-=1
        continue
    if not flag_check:
      print("DcdnStartOnAlibabaCloud Failed!")
      return 0
    else:
      DcdnConfigCname(AccessKey, SecretKey, DomainName)
    count=20
    flag_check=False
    while count:
      res=DcndCheckDomainDetail(AccessKey, SecretKey, DomainName)
      if(res.domain_status=="online"):
        flag_check=True
        break
      else:
        print("domain_status online check",count)
        sleep(60)
        count-=1
        continue
    if not flag_check:
      print("DcdnConfigCname Failed!")
      return 0
    else:
      DcdnEnableHTTPS(AccessKey, SecretKey, DomainName,DomainName,CertID)
      sleep(10)
    res=DcndCheckDomainDetail(AccessKey, SecretKey, DomainName)
    print("DcdnStartOnAlibabaCloud Result:")
    print('Domain Name:', res.domain_name)
    print('Domain Status:', res.domain_status)
    print('DCDN Scope:', res.scope)
    return 0
```
### **4. Ví dụ về hàm Main.**
- Sau khi đã xây dựng các hàm như trên, mình viết hàm main mô tả ví dụ về việc gọi các hàm đã xây dựng:
```python
def main():
    print("===========START-MAIN==============")
    AccessKey='XXXXXX'
    SecretKey='mXXXXXXX'
    emailAddress = 'pmagic@gmail.com'
    accountKeyFile="Accout_Key"
    #Get SSL
    GenAccountKey(accountKeyFile)
    acme_client=GetACMEAccount(emailAddress,False)

    domainName = "blog.vinahost.cloud"
    endPoint={'ip':"0.0.0.0",'port':"443"}
    KEY_FILE = domainName + '.key'  #Private Key
    CSR_FILE = domainName + '.csr'
    CERT_FILE= domainName + '.cert' #FullChain.pem
    cert=GenSSLCert(acme_client,domainName,emailAddress,AccessKey, SecretKey)
    with open(CERT_FILE, "rt") as f:
          cert_file=f.read()
    cert_pem,chain_pem=SplitFullChainPem(cert_file)
    priv_pem=LoadPrivateKeyAsText(KEY_FILE)
    cert_id=UploadUserCertToCAS(AccessKey,SecretKey,cert_pem,priv_pem,chain_pem,domainName)
    DcdnStartOnAlibabaCloud(AccessKey,SecretKey,domainName,endPoint,cert_id)
    print("===========END-MAIN==============")
main()
```
### **5. Tạm kết**
- Các thư viện đã cài đặt:
```python
cryptography
acme
alibabacloud_alidns20150109
alibabacloud_cas20200407
alibabacloud_dcdn20180115
```
- Import:
```python
from cryptography import x509
from cryptography.x509.oid import NameOID
from cryptography.hazmat.backends import default_backend
from cryptography.hazmat.primitives import hashes
from cryptography.hazmat.primitives import serialization
from cryptography.hazmat.primitives.asymmetric import rsa
from acme import errors as acme_errors
from acme import messages, client, crypto_util, challenges, jose
from alibabacloud_tea_openapi import models as open_api_models
from alibabacloud_alidns20150109.client import Client as AlidnsClient
from alibabacloud_alidns20150109 import models as alidns_models
from alibabacloud_tea_util import models as util_models
from time import sleep
import json
import os
from alibabacloud_cas20200407.client import Client as casClient
from alibabacloud_tea_openapi import models as open_api_models
from alibabacloud_cas20200407 import models as cas_20200407_models
from alibabacloud_dcdn20180115.client import Client as dcdnClient
from alibabacloud_dcdn20180115 import models as dcdn_models
```
- Trên đây chỉ là các minh họa, mình thử nghiệm trên các hàm riêng rẽ, ít có tính tái sử dụng. Các lỗi khi gọi API vẫn không xử lý hết. Về CertID, khi enable HTTPS trên DCDN cần có certID, nhưng mình vẫn không tìm được cách lấy CertID từ API, nên phải lưu tạm vào file. 
### **6. Tham khảo**
1. [Let's Encrypt acme on Alibaba Cloud](https://www.alibabacloud.com/blog/let%27s-encrypt-acme-on-alibaba-cloud-%E2%80%93-part-1_593777)
2. [GitHub letsencrypt-apis-cert-generation](https://github.com/saichander17/letsencrypt-apis-cert-generation/)
3. [GitHub Certbot](https://github.com/certbot/certbot/blob/master/acme/examples/http01_example.py)
4. [Certificate Manager Service SDK Center](https://api.alibabacloud.com/api-tools/sdk/cas?version=2020-04-07&language=python-tea&tab=primer-doc)
5. [DCDN API Reference](https://www.alibabacloud.com/help/en/dcdn/developer-reference/api-reference-guide/)
6. [CAS endpoint](https://api.alibabacloud.com/product/cas)
7. [GitHub AlibabaCloud Python SDK 20200407](https://github.com/aliyun/alibabacloud-python-sdk/blob/master/cas-20200407/alibabacloud_cas20200407)
8. [Alibaba Cloud DNS Example](https://next.api.alibabacloud.com/api-tools/sdk/Alidns?version=2015-01-09&language=python-tea&tab=primer-doc)
9. [DCDN Document](https://www.alibabacloud.com/help/en/dcdn/)