# 1. Init system (Systemd) — Hệ thống khởi tạo của Linux

**Systemd** là tiến trình đầu tiên chạy khi Linux boot lên (PID = 1).  
Nó chịu trách nhiệm quản lý toàn bộ quá trình khởi động và vận hành các dịch vụ của hệ thống.

---

## Vai trò chính của Systemd

- Khởi chạy tất cả các services khi hệ thống boot  
- Dừng services khi shutdown hoặc reboot  
- Quản lý logs hệ thống và dịch vụ  
- Quản lý sự phụ thuộc (dependency) giữa các services  
- Theo dõi các dịch vụ bị crash và tự động restart nếu cần  
- Quản lý các target (tương tự runlevel truyền thống)

---
### Quản lý vòng đời service

Các lệnh thường dùng để quản lý service:

```bash
systemctl start <service>       # Bắt đầu service
systemctl stop <service>        # Dừng service
systemctl restart <service>     # Khởi động lại service
systemctl enable <service>      # Đặt service tự động khởi động cùng hệ thống
systemctl disable <service>     # Vô hiệu hóa tự động khởi động
systemctl status <service>      # Kiểm tra trạng thái service
```
### Xem log với journalctl
Systemd thu thập log của toàn hệ thống và dịch vụ:

```bash
journalctl -u <service>    # Xem log của service cụ thể
journalctl -b              # Xem log từ lúc hệ thống khởi động
journalctl -xe             # Xem log lỗi chi tiết gần nhất
```


# 2. Cấu trúc thư mục Linux (Filesystem Hierarchy Standard – FHS)

## Các thư mục chính trong Linux

### `/` – Root directory  
Thư mục gốc, tất cả các thư mục và file khác đều nằm dưới đây.

---

### `/bin` – Chương trình cơ bản  
Chứa các lệnh hệ thống cốt lõi, được sử dụng bởi người dùng và scripts cơ bản như:  
- `ls`  
- `cp`  
- `mv`  
- `cat`  
- `bash`

---

### `/sbin` – Lệnh dành cho administrator  
Chứa các lệnh quản trị hệ thống, thường yêu cầu quyền root để chạy như:  
- `reboot`  
- `fdisk`  
- `ip`  
- `route`

---

### `/etc` – File cấu hình của hệ thống  
Chứa các file cấu hình quan trọng của hệ thống, ví dụ:  

| File / Thư mục           | Ý nghĩa                   |
|-------------------------|---------------------------|
| `/etc/passwd`           | Danh sách người dùng      |
| `/etc/shadow`           | Mật khẩu đã mã hóa        |
| `/etc/fstab`            | Cấu hình tự động mount ổ  |
| `/etc/ssh/sshd_config`  | Cấu hình dịch vụ SSH      |

---

### `/var` – Dữ liệu thay đổi thường xuyên (variable data)  
Chứa dữ liệu được thay đổi liên tục trong quá trình hệ thống chạy:  
- `/var/log` → file log hệ thống  
- `/var/lib` → metadata của các ứng dụng  
- `/var/spool` → hàng đợi mail, hàng đợi in ấn  

---

### `/usr` – Ứng dụng và thư viện  
Chứa các ứng dụng và thư viện phổ biến:  
- `/usr/bin` → các ứng dụng thông thường  
- `/usr/sbin` → các công cụ dành cho administrator  
- `/usr/lib` → thư viện dùng chung (.so files)

---

### `/home` – Thư mục người dùng  
Chứa thư mục cá nhân của từng người dùng, ví dụ:  
- `/home/tuan`  
- `/home/linh`

---

### `/opt` – Ứng dụng cài thủ công  
Dùng để chứa các ứng dụng được cài đặt thủ công, ví dụ:  
- Google Chrome  
- TeamViewer

---

### `/tmp` – File tạm thời  
Chứa các file tạm thời, thường được xóa tự động khi khởi động lại hệ thống.

---

### `/dev` – Thiết bị  
Linux coi mọi thiết bị như file, ví dụ:  
- `/dev/sda` → ổ cứng  
- `/dev/tty` → thiết bị terminal  
- `/dev/null` → thiết bị “black hole” (xóa mọi dữ liệu ghi vào)

---

# 3. Xử lý ứng dụng bị treo trên Linux

Khi một ứng dụng hoặc dịch vụ bị treo, bạn có thể sử dụng các công cụ và lệnh dưới đây để phát hiện và xử lý vấn đề.

```bash
ps aux | grep <app>    # Tìm tiến trình theo tên ứng dụng
top                    # Xem trạng thái CPU, RAM, process theo thời gian thực
htop                   # Phiên bản nâng cao của top (cần cài đặt thêm)
kill <pid> (mặc định SIGTERM) # Dừng nhẹ nhàng tiến trình
kill -9 <pid> (SIGKILL) # Kill mạnh, buộc dừng
journalctl -u <service>   # Xem log của service liên quan
dmesg | tail              # Xem log kernel mới nhất (thường liên quan phần cứng hoặc driver)
iotop    # Theo dõi I/O theo tiến trình
iostat   # Thống kê I/O thiết bị lưu trữ
```

# 4. Networking trong Linux

### Kiểm tra trạng thái mạng cơ bản
```bash
ip addr show       # Hiển thị IP, trạng thái interface
ip link show       # Hiển thị trạng thái các network interface
ping 8.8.8.8       # Kiểm tra kết nối đến IP
ping google.com    # Kiểm tra kết nối DNS
```

### Quản lý interface
```bash
ip link set dev eth0 up     # Bật interface eth0
ip link set dev eth0 down   # Tắt interface eth0
# Cấu hình ip tạm
ip addr add 192.168.1.100/24 dev eth0
ip route add default via 192.168.1.1
# Định tuyến
ip route show # Kiểm tra bảng định tuyến
ip route add 10.0.0.0/24 via 192.168.1.1 # Thêm route
# DNS
cat /etc/resolv.conf # File cấu hình DNS (Ko request được trang wifi có thể do thiếu tên miền 8.8.8.8...)
cat /etc/hosts # Map tĩnh tên miền → IP, ưu tiên hơn DNS
systemd-resolve --status # Kiểm tra trạng thái DNS (systemd-resolved)

# Firewall
sudo ufw status
sudo ufw allow 22/tcp
sudo ufw deny 80/tcp

```
