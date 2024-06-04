# Tự động triển khai DCDN bằng SDK với Free SSL từ Let's encrypt (Phần 1)

> Có nhiều cách để triển khai domain lên **DCDN** của Alibaba Cloud. Triển khai thông qua giao diện web console có thể thao tác trực quan và tiện lợi, nhưng để tự động hóa quá trình triển khai, hoặc muốn triển khai đồng loạt nhiều domain lên DCDN một cách nhanh chóng, thì có thể dùng các **API do Alibaba Cloud** cung cấp. Trong series này, mình thử nghiệm triển khai DCDN thông qua **Python SDK**, đồng thời tự động **enable HTTPS traffic** với domain đã triển khai qua việc tự động cấp **Free SSL từ Let's encrypt** (Alibaba Cloud có cung cấp SSL nhưng không có bản miễn phí). 

> Trước khi đến với DCDN, mình xin bắt đầu với việc đăng ký một chứng chỉ SSL miễn phí từ Let's encrypt qua ACME và tự động trỏ dns record để xác thực tên miền qua Alibaba Cloud DNS.  Để chuẩn bị cho việc enable HTTPS trên DCDN. 

### **1. Sơ lược về SSL, Let's Encrypt và ACME.**

- **SSL Certificate** là một chứng chỉ số được gắn với một domain, hostname,... Một SSL certificate gồm nhiều file chứa các thông tin, khóa mã hóa kỹ thuật số. Trong trường hợp sử dụng với web server, nó được dùng để thiết lập **kết nối HTTPS**. Về cơ bản, một SSL Certificate cần 1 cặp Keypair (Public Key và Private Key), cùng với CA Certificate (Certificate Authority - nhà cung cấp chứng chỉ kỹ thuật số) - là chứng chỉ của cơ quan đã cấp chứng chỉ SSL này. Các trình duyệt sẽ lưu một danh sách các CA "đáng tin cậy", khi xử lý một SSL certificate do một CA nằm ngoài danh sách này, hoặc các SSL certificate có vấn đề, hết hạn,... thì trình duyệt sẽ cảnh báo "Your connection is not private" hoặc một vài cảnh báo lỗi khác.
- **Let's Encrypt** là một nhà cung cấp chứng chỉ SSL "được tin cậy". Ngoài các chứng chỉ trả phí, nó vẫn cung cấp các bản chứng chỉ SSL miễn phí cho bất kỳ domain, nhưng chỉ có thời hạn trong thời gian ngắn (**3 tháng**), sau khi hết hạn, phải đăng ký hoặc renew lại chứng chỉ.
- **ACME (Automatic Certificate Management Environment)** là một giao thức, hay một chuẩn giao tiếp để client có thể tương tác với các CA (Certificate Authority) để thực hiện các thao tác như **issue, renew, và revoke**.
### **2.Một số khái niệm với ACME**
- **Account Key**: Để request lên CA, cần có một bộ key để xác thực người dùng và ký vào các request.
- **Certificate Key**: Đây là bộ key dùng để tạo CSR dùng cho việc request SSL, nó cũng chứa Private Key trong bộ SSL cert. CA không giữ Private Key này, nên cần phải lưu giữ kỹ, nếu bị mất, phải tạo lại chứng chỉ khác.
- **CSR - Certificate Signing Request**: Đây là file được mã hóa gửi đến CA để yêu cầu cung cấp chứng chỉ SSL. File này chứa thông tin về Domain cần cấp chứng chỉ, tên công ty, vị trí. Đối với Free Domain Valid SSL, hai thông tin quan trọng nhất là domain cần cấp chứng chỉ, các thông tin khác không quan trọng.
### **3. Các thư viện python cần thiết**
- **Thư viện python acme**: đây là một thư viện python được xây dựng sẵn, tuân theo giao thức ACME, dễ dàng sử dụng để tương tác với CA
- Ngoài ra cần thêm thư viện **cryptography** để làm việc với các key và mã hóa.
4. **Xây dựng các hàm liên quan đến ACME cơ bản:**
* **Hàm tạo Account Key**: Sử dụng thư viện **cryptography** để tạo key và nên lưu Key lại dùng về sau. Có thể lưu file với nhiều format, trong ví dụ sử dụng format TraditionalOpenSSL, ngoài ra có thể đặt mật khẩu cho file hoặc không, nếu đặt mật khẩu thì khi đọc file lên phải cung cấp mật khẩu giống như lúc lưu file. Ví dụ về hàm tạo Account Key:
```python
def GenAccountKey(FileName):
    account_key = rsa.generate_private_key(public_exponent=65537,key_size=2048,backend=default_backend())
    with open(FileName, "wb") as f:
        f.write(account_key.private_bytes(
        encoding=serialization.Encoding.PEM,
        format=serialization.PrivateFormat.TraditionalOpenSSL,
        encryption_algorithm=serialization.BestAvailableEncryption(b"passx"),))
```
*  **Hàm khởi tạo ACMEClient** - dùng để tương tác với CA. Để khởi tạo ACME client cần có **Account Key, email address**. Với Let Encrypt, có 2 endpoint để gửi request, một là endpoint thật, 2 là endpoint **staging**, endpoint staging cho phép các lập trình viên request ssl cert nhiều lần mà không bị giới hạn (dành cho giai đoạn code và chỉnh sửa code), sau khi code hoàn chỉnh có thể chuyển sang endpoint thật sự. Request trên endpoint thật sẽ bị giới hạn số lần request SSL cert. Hàm mẫu như sau:
```python
def GetACMEAccount(emailAddress,staging):
    if staging==True:
        path='https://acme-staging-v02.api.letsencrypt.org/directory'
    else:
        path='https://acme-v02.api.letsencrypt.org/directory'
    with open("Accout_Key", "rb") as keyfile:
        key_data=keyfile.read()
        pkey=serialization.load_pem_private_key(key_data,b"passx")
    acc_key = jose.JWKRSA(key=pkey)
    net = client.ClientNetwork(acc_key, user_agent="self-test")
    directory = client.ClientV2.get_directory(path, net)
    client_acme = client.ClientV2(directory, net=net)
    try:
      regr=client_acme.new_account(messages.NewRegistration.from_data(email=emailAddress, terms_of_service_agreed=True))
      return client_acme
    except acme_errors.ConflictError as e:
      print(e)
      return client_acme
```
* **Hàm tạo CSR:** để request 1 SSL cert, cần gửi cho CA một CSR chứa các thông tin cần thiết và được **ký bằng Certificate Key**. Đối với các chứng chỉ loại Domain Valid, cần cung cấp 2 thông tin là **domain name và email address**. Hàm cũng thực hiện **tạo Certificate Key**, lưu trữ private key của chứng chỉ. Trong hàm, biến CSR là một object của python,  để lưu trữ CSR dưới dạng text thông thường, cần encode nó bằng cú pháp như: **csr.public_bytes(serialization.Encoding.PEM)**. Cần lưu ý rằng phải lưu private key ở format **PrivateFormat.PKCS8** thay vì TraditionalOpenSSL để có thể sử dụng trên Alibaba Cloud. Khi tạo CSR, các thông tin khác cũng có thể thay đổi như COUNTRY_NAME, ORGANIZATION_NAME,... nhưng quan trọng nhất vẫn là COMMON_NAME - cũng là tên của domain cần cấp SSL.  Hàm mẫu như sau:
```python
def genCSR(domainName, email_address):
    """ Generate a certificate signing request """
    emailAddress=email_address
    KEY_FILE = domainName + '.key'
    CSR_FILE = domainName + '.csr'
    key = rsa.generate_private_key(public_exponent=65537,key_size=2048,)
    with open(KEY_FILE, "wb") as f:
        f.write(key.private_bytes(
            encoding=serialization.Encoding.PEM,
            format=serialization.PrivateFormat.PKCS8,
            encryption_algorithm=serialization.NoEncryption(),
    ))
    with open(KEY_FILE, "rb") as keyfile:
        key_data=keyfile.read()
        pkey=serialization.load_pem_private_key(key_data,None)
    csr = x509.CertificateSigningRequestBuilder().subject_name(x509.Name([
    # Provide various details about who we are.
      x509.NameAttribute(NameOID.COUNTRY_NAME, "VN"),
      x509.NameAttribute(NameOID.STATE_OR_PROVINCE_NAME, "HCM"),
      x509.NameAttribute(NameOID.LOCALITY_NAME, "HCM"),
      x509.NameAttribute(NameOID.EMAIL_ADDRESS, emailAddress),
      x509.NameAttribute(NameOID.ORGANIZATION_NAME, "Client"),
      x509.NameAttribute(NameOID.COMMON_NAME, domainName),
    ])).add_extension(x509.SubjectAlternativeName([x509.DNSName(domainName)]), critical=False,
    ).sign(pkey, hashes.SHA256())
    with open(CSR_FILE, "wb") as f:
      f.write(csr.public_bytes(serialization.Encoding.PEM))
    return pkey,csr
```
### **4. Tạm kết**
- **Tổng hợp các hàm:**
```python
from cryptography import x509
from cryptography.x509.oid import NameOID
from cryptography.hazmat.backends import default_backend
from cryptography.hazmat.primitives import hashes
from cryptography.hazmat.primitives import serialization
from cryptography.hazmat.primitives.asymmetric import rsa
from acme import errors as acme_errors
from acme import messages, client, crypto_util, challenges, jose
def GenAccountKey(FileName):
    account_key = rsa.generate_private_key(public_exponent=65537,key_size=2048,backend=default_backend())
    with open(FileName, "wb") as f:
        f.write(account_key.private_bytes(
        encoding=serialization.Encoding.PEM,
        format=serialization.PrivateFormat.TraditionalOpenSSL,
        encryption_algorithm=serialization.BestAvailableEncryption(b"passx"),))
def GetACMEAccount(emailAddress,staging):
    if staging==True:
        path='https://acme-staging-v02.api.letsencrypt.org/directory'
    else:
        path='https://acme-v02.api.letsencrypt.org/directory'
    with open("Accout_Key", "rb") as keyfile:
        key_data=keyfile.read()
        pkey=serialization.load_pem_private_key(key_data,b"passx")
    acc_key = jose.JWKRSA(key=pkey)
    net = client.ClientNetwork(acc_key, user_agent="self-test")
    directory = client.ClientV2.get_directory(path, net)
    client_acme = client.ClientV2(directory, net=net)
    try:
      regr=client_acme.new_account(messages.NewRegistration.from_data(email=emailAddress, terms_of_service_agreed=True))
      return client_acme
    except acme_errors.ConflictError as e:
      print(e)
      return client_acme
def genCSR(domainName, email_address):
    """ Generate a certificate signing request """
    emailAddress=email_address
    KEY_FILE = domainName + '.key'
    CSR_FILE = domainName + '.csr'
    key = rsa.generate_private_key(public_exponent=65537,key_size=2048,)
    with open(KEY_FILE, "wb") as f:
        f.write(key.private_bytes(
            encoding=serialization.Encoding.PEM,
            format=serialization.PrivateFormat.PKCS8,
            encryption_algorithm=serialization.NoEncryption(),
    ))
    with open(KEY_FILE, "rb") as keyfile:
        key_data=keyfile.read()
        pkey=serialization.load_pem_private_key(key_data,None)
    csr = x509.CertificateSigningRequestBuilder().subject_name(x509.Name([
    # Provide various details about who we are.
      x509.NameAttribute(NameOID.COUNTRY_NAME, "VN"),
      x509.NameAttribute(NameOID.STATE_OR_PROVINCE_NAME, "HCM"),
      x509.NameAttribute(NameOID.LOCALITY_NAME, "HCM"),
      x509.NameAttribute(NameOID.EMAIL_ADDRESS, emailAddress),
      x509.NameAttribute(NameOID.ORGANIZATION_NAME, "Client"),
      x509.NameAttribute(NameOID.COMMON_NAME, domainName),
    ])).add_extension(x509.SubjectAlternativeName([x509.DNSName(domainName)]), critical=False,
    ).sign(pkey, hashes.SHA256())
    with open(CSR_FILE, "wb") as f:
      f.write(csr.public_bytes(serialization.Encoding.PEM))
    return pkey,csr
def main():
    print("===========START-MAIN==============")
    genAccoutKey()
    acme_client=getACMEAccount('tem@gmail.com',True)
    pkey, csr= genCSR('vinahost.cloud','tempx@gmail.com')
    
main()
```
- Trên đây mình chỉ thực hiện các **vấn đề cơ bản**, còn rât nhiều tham số khác, và xử lý **các lỗi** có thể xảy ra trong quá trình giao tiếp, nhưng nó không nằm trong phạm vi bài viết.
- Phía trên mình đã viết về các hàm cơ bản để chuẩn bị cho việc request một chứng chỉ SSL. Quy trình cơ bản khi CA cấp chứng chỉ là client sẽ gửi CSR, CA sẽ **xác thực** rằng client là người sở hữu domain được ghi trong CSR bằng một trong những cách như TraditionalOpenSSL, HTTP, Email,... Sau khi xác thực thành công sẽ gửi ssl cert về cho người dùng. Người dùng sau đó có thể cài đặt ssl cert này và private key của mình lên web server để thiết lập kết nối https.
- Vậy nên, ở phần [thứ 2](#), mình xin được giới thiệu một số hàm Python SDK tương tác với Alibaba Cloud DNS để trỏ domain tự động, sau đó là request 1 chứng chỉ SSL và xác thực bằng **phương thức DNS**.
### **5. Tham khảo**
1. [Project letsencrypt-apis-cert-generation](https://github.com/saichander17/letsencrypt-apis-cert-generation/blob/main/src/main.py#L33)
2. [Python cryptography](https://cryptography.io/en/latest/hazmat/primitives/asymmetric/rsa/)
3. [Python ACME](https://pypi.org/project/acme/)
4. [Let's Encrypt ACME on Alibaba Cloud – Part 1](https://www.alibabacloud.com/blog/lets-encrypt-acme-on-alibaba-cloud-part-1_593777)
