# XÂY DỰNG HỆ THỐNG MẠNG BẢO MẬT ĐA LỚP CHO HỘI SỞ NGÂN HÀNG VỚI CISCO ASA & VPN

## Giới thiệu
Đề tài được triển khai trên nền tảng **EVE-NG** với mục tiêu xây dựng hệ thống mạng bảo mật đa lớp cho hội sở ngân hàng.  
Hệ thống sử dụng **Cisco ASA Firewall**, **IPSec VPN** và **AnyConnect VPN** để đảm bảo:
- 🔒 Tính bảo mật dữ liệu  
- 📡 Kết nối an toàn cho chi nhánh từ xa  
- 🌐 Truy cập từ xa qua VPN client  

---

## 🏗Mô hình triển khai
Mô hình mạng bao gồm:
- **BR (Branch Router)**: Cung cấp DHCP cho mạng LAN chi nhánh, thiết lập VPN chính & dự phòng về GW1/GW2.  
- **ISP Router**: Kết nối BR với GW1, GW2.  
- **GW1 & GW2**: Gateway trung tâm, thiết lập tunnel VPN IPSec với BR.  
- **CORE1 & CORE2**: Mạng lõi hội sở, chạy OSPF và định tuyến nội bộ.  
- **ASA Firewall**: Phân tách 3 vùng **Inside – DMZ – Outside**, triển khai AnyConnect VPN.  
- **SVR (Server trong DMZ)**: Cung cấp dịch vụ cho người dùng nội bộ & VPN client.  

📌 Topology:  
![image](https://github.com/asaiduc/vpn_asa_eve_lab/blob/main/images/topology.png)

---

## ⚙️ Các thành phần & IP
| Thành phần | Interface / IP |
|------------|----------------|
| BR | e0/0: 103.1.1.254 /24 <br> e0/1: 10.1.1.254 /24 |
| ISP | e0/0: 103.1.1.1 /24 <br> e0/1: 102.1.1.1 /24 <br> e0/2: 101.1.1.1 /24 |
| GW1 | e0/0: 102.1.1.254 /24 <br> e0/1: 172.17.2.254 /24 |
| GW2 | e0/0: 101.1.1.254 /24 <br> e0/1: 172.17.1.254 /24 |
| CORE1 | e0/0: 172.17.2.1 <br> e0/1: 172.17.1.1 <br> e0/2: 172.18.0.1 <br> e0/3: 172.17.0.1 |
| CORE2 | e0/0: 172.18.0.254 <br> e0/1: 172.16.1.254 <br> e0/2: 172.16.0.254 |
| ASA | Eth0 (outside): 192.168.1.10 <br> Eth1 (dmz): 172.16.2.1 <br> Eth2 (inside): 172.16.1.1 |
| SVR (DMZ) | 172.16.2.100/24, GW: 172.16.2.1 |
| Tunnel10 | 10.0.0.2/30 (BR – GW1) |
| Tunnel20 | 10.0.1.2/30 (BR – GW2) |

---

## 🔧 Các chức năng triển khai
- **VPN Site-to-Site (IPSec):**
  - BR ↔ GW1 (đường chính, cost 10)  
  - BR ↔ GW2 (đường dự phòng, cost 100)  

- **Routing:**
  - OSPF trên toàn bộ hệ thống (BR, GW1, GW2, CORE).  
  - Static route trên ASA về các mạng nội bộ.  

- **Cisco ASA Firewall:**
  - Zone: **Inside – DMZ – Outside**  
  - NAT + ACL kiểm soát lưu lượng giữa các zone  
  - AnyConnect VPN cho user truy cập từ xa  

- **AnyConnect VPN:**
  - Pool IP: `192.168.100.10 – 192.168.100.50`  
  - User: `vpnuser / 123456`  
  - Split-tunnel: chỉ cho phép VPN client truy cập vào DMZ (172.16.2.0/24)  

---

## 📂 Cấu trúc Repository
vpn_asa_eve_lab/

│── README.md # Giới thiệu

│── configurations/

│ └── asa_vpn_lab_config.txt # Toàn bộ cấu hình

│── report/

│ └── Report_Lab.pdf # Báo cáo chính thức

│── images/

│ └── topology.png # Sơ đồ mạng

---

## 🚀 Hướng dẫn sử dụng
1. Import cấu hình từ `configurations/asa_vpn_lab_config.txt` vào các thiết bị trong EVE-NG.  
2. Khởi động topology theo bảng IP.  
3. Kết nối VPN Client AnyConnect đến ASA:  
   - Server: `https://192.168.1.10`  
   - Username: `vpnuser`  
   - Password: `123456`  
4. Ping đến **SVR (172.16.2.100)** để kiểm tra kết nối.  

---

## 👨‍💻 Thành viên nhóm
- **Nguyễn Bá Trung – N21DCAT060**  
- **Lê Khánh Bình Đức – N21DCAT013**  
- **Đinh Văn Hậu – N21DCAT017**  

**GVHD:** ThS. Đàm Minh Lịnh  

---

## 📖 Tài liệu tham khảo
- Cisco ASA & AnyConnect VPN Configuration Guide  
- EVE-NG Official Documentation  
- Giáo trình An toàn thông tin – Học viện Công nghệ Bưu chính Viễn thông  

---
