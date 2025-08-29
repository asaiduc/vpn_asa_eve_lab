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

📌 Topology (tham khảo hình trong báo cáo):  
