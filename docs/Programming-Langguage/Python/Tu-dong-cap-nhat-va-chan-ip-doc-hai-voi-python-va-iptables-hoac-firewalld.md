# Tự động cập nhật và chặn IP độc hại với Python và Iptables hoặc Firewalld

## Giới thiệu về Spamhaus

**Spamhaus** là một tổ chức phi lợi nhuận quốc tế chuyên chống lại thư rác (spam) và các hoạt động mạng độc hại. Được thành lập vào năm 1998 bởi Steve Linford, Spamhaus cung cấp một loạt các dịch vụ bảo mật mạng nhằm bảo vệ người dùng Internet khỏi các mối đe dọa từ thư rác, phần mềm độc hại và các cuộc tấn công mạng.

## Các dịch vụ chính của Spamhaus

1.  **DNS Blocklists (DNSBLs)**:
    
    *   Spamhaus duy trì nhiều danh sách chặn DNS (DNSBLs), sử dụng để xác định và ngăn chặn thư rác và các email độc hại khác. Các danh sách này bao gồm các IP được biết đến là nguồn phát tán thư rác và các hoạt động mạng xấu khác.
2.  **DROP (Don't Route Or Peer)**:
    
    *   DROP là một danh sách các dải IP không nên được định tuyến hoặc ghép nối do chúng được xác định là nguồn của các hoạt động mạng độc hại. Spamhaus cung cấp danh sách DROP và eDROP (Extended DROP) để giúp các quản trị mạng ngăn chặn lưu lượng từ các dải IP độc hại.
3.  **ZRD (Zero Reputation Domains)**:
    
    *   Danh sách ZRD bao gồm các tên miền có uy tín bằng không, tức là các tên miền mới được tạo ra và chưa có lịch sử hoạt động, thường được sử dụng cho các chiến dịch thư rác hoặc tấn công mạng.

Dưới đây là đoạn mã Python để tự động tải xuống các danh sách IP từ Spamhaus, sau đó chặn các IP đó trên một linux server sử dụng iptables hoặc firewalld. Đoạn mã này đáp ứng nhu cầu chạy webserver (apache, nginx), mail server và các ứng dụng khác.

## Đoạn mã Python chặn bằng iptables:

```
import requests
import subprocess

def download_ip_list(url):
    response = requests.get(url)
    response.raise_for_status()
    ip_list = [line.split(';')[0].strip() for line in response.text.splitlines() if line and not line.startswith(';')]
    return ip_list

def clear_iptables_rules():
    subprocess.run(["sudo", "iptables", "-F"])
    print("Cleared all iptables rules.")

def block_ips_iptables(blocked_ips):
    for ip in blocked_ips:
        subprocess.run(["sudo", "iptables", "-A", "INPUT", "-s", ip, "-j", "DROP"])
    print("Blocked IPs using iptables.")

# Download IP lists
drop_url = "https://www.spamhaus.org/drop/drop.txt"
edrop_url = "https://www.spamhaus.org/drop/edrop.txt"

drop_ips = download_ip_list(drop_url)
edrop_ips = download_ip_list(edrop_url)

blocked_ips = drop_ips + edrop_ips
print(f"Downloaded {len(blocked_ips)} IPs to block.")

# Clear previous iptables rules
clear_iptables_rules()

# Apply new blocking rules
block_ips_iptables(blocked_ips)

```

## Giải thích:

1.  **download\_ip\_list(url)**: Tải xuống danh sách IP từ URL cung cấp.
2.  **clear\_iptables\_rules()**: Xóa bỏ tất cả các rule trong iptables.
3.  **block\_ips\_iptables(blocked\_ips)**: Thêm các rule mới từ danh sách IP cập nhật vào iptables.

## Đoạn mã Python chặn bằng firewalld:

```
import requests
import subprocess

def download_ip_list(url):
    response = requests.get(url)
    response.raise_for_status()
    ip_list = [line.split(';')[0].strip() for line in response.text.splitlines() if line and not line.startswith(';')]
    return ip_list

def clear_firewalld_rules():
    # This assumes all rules are part of the 'drop' zone.
    existing_rules = subprocess.run(["sudo", "firewall-cmd", "--zone=drop", "--list-sources"], capture_output=True, text=True)
    for ip in existing_rules.stdout.split():
        subprocess.run(["sudo", "firewall-cmd", "--zone=drop", "--remove-source", ip])
    print("Cleared all firewalld rules in the drop zone.")

def block_ips_firewalld(blocked_ips):
    for ip in blocked_ips:
        subprocess.run(["sudo", "firewall-cmd", "--zone=drop", "--add-source", ip])
    subprocess.run(["sudo", "firewall-cmd", "--runtime-to-permanent"])
    print("Blocked IPs using firewalld.")

# Download IP lists
drop_url = "https://www.spamhaus.org/drop/drop.txt"
edrop_url = "https://www.spamhaus.org/drop/edrop.txt"

drop_ips = download_ip_list(drop_url)
edrop_ips = download_ip_list(edrop_url)

blocked_ips = drop_ips + edrop_ips
print(f"Downloaded {len(blocked_ips)} IPs to block.")

# Clear previous firewalld rules
clear_firewalld_rules()

# Apply new blocking rules
block_ips_firewalld(blocked_ips)

```

## Giải thích:

1.  **download\_ip\_list(url)**: Tải xuống danh sách IP từ URL cung cấp.
2.  **clear\_firewalld\_rules()**: Xóa bỏ tất cả các IP đã được thêm vào zone 'drop' của `firewalld`.
3.  **block\_ips\_firewalld(blocked\_ips)**: Thêm các IP mới vào zone 'drop' của `firewalld`.

Bằng cách này, bạn sẽ xóa bỏ các IP và dải IP đã chặn trước đó và chỉ chặn các IP và dải IP mới cập nhật từ danh sách bằng cách sử dụng IPtables hoặc Firewalld.
