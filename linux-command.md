# Linux Command Cheat Sheet

## 1. File & Directory Commands

| Command | Description |
|---------|-------------|
| `ls` | Liệt kê file và thư mục |
| `ls -l` | Xem chi tiết file (permissions, owner, size, date) |
| `ls -a` | Xem tất cả file, kể cả ẩn |
| `ls -al` | Xem chi tiết và cả file ẩn |
| `cd <dir>` | Chuyển tới thư mục `<dir>` |
| `cd ~` | Về thư mục home của user |
| `cd ../..` | Thoát ra 2 cấp thư mục |
| `mkdir <dir>` | Tạo thư mục mới |
| `mkdir -p <dir>` | Tạo thư mục và subdirectory nếu chưa tồn tại |
| `pwd` | Hiển thị đường dẫn hiện tại |
| `vi <file>` | Chỉnh sửa file bằng vi editor |
| `touch <file>` | Tạo file rỗng, không mở editor |
| `cp <src> <dst>` | Copy file hoặc thư mục |
| `mv <src> <dst>` | Di chuyển hoặc đổi tên file/thư mục |
| `rm <file>` | Xóa file |
| `rm -ri <dir>` | Xóa thư mục **có hỏi xác nhận** |
| `cat <file>` | Xem nội dung file |
| `tail -f <file>` | Theo dõi log theo thời gian thực |
| `find <path> -name "<pattern>"` | Tìm file theo tên (ví dụ: `find . -name "*.txt"`) |
| `grep "<pattern>" <file>` | Tìm chuỗi trong file |
| `grep -r "<pattern>" <dir>` | Tìm đệ quy trong thư mục |
| `ln -s target link` | Tạo symlink tới file target |
| `ln -sf target link` | Tạo symlink và override nếu đã tồn tại |
| `Ctrl + L` | Clear terminal |
| `man <command>` | Xem hướng dẫn lệnh (ví dụ: `man grep`) |

---

## 2. User & System Commands

| Command | Description |
|---------|-------------|
| `hostname` | Hiển thị hostname của hệ thống |
| `passwd` | Đổi mật khẩu user hiện tại |
| `whoami` | Hiển thị user hiện tại |
| `who` | Hiển thị thông tin tất cả user đang đăng nhập |

---

## 3. Permissions

| Command | Description |
|---------|-------------|
| `chmod` | Thay đổi quyền file/thư mục |
| `chmod 755 filename` | Owner rwx=7, Group r-x=5, Others r-x=5 |
| `chmod g=r filename` | Chỉ nhóm được quyền đọc |
| `chmod a-x filename` | Remove execute permission cho tất cả |
| `chmod u=rwx,g=r,o= filename` | Owner rwx, group r, others không có quyền |

---

## 4. `grep` Options

| Command | Description |
|---------|-------------|
| `grep -w "linux" file.txt` | Chỉ tìm **từ nguyên vẹn** |
| `grep -i "linux" file.txt` | Không phân biệt hoa thường |
| `grep -r "linux" /path/to/folder` | Tìm tất cả file trong thư mục và subdir |
| `grep -E "linux|ubuntu|debian" file.txt` | Regex mở rộng, tìm nhiều pattern cùng lúc |

---

## 5. User & Group Management

```bash
sudo groupadd <groupname>          # Tạo group mới
sudo useradd -m -c "Full Name"     # Tạo user mới
sudo usermod -a -G <groupname>     # Thêm user vào group
sudo chown <user>:<group> <file_or_dir>  # Thay đổi owner/group
```

## 6. Symbolic Link Management

```bash
/opt/
  mylib/
    liba.so.1.0.0   # shared object file
    liba.so         # symbolic link tới liba.so.1.0.0
    include/        # header files

cd /opt/mylib
ln -s liba.so.1.0.0 liba.so     # tạo symlink
rm liba.so
ln -s liba.so.1.1.0 liba.so     # update symlink
# hoặc dùng 
ln -sf liba.so.1.1.0 liba.so    # update symlink no delete liba.so
```


## 7. ACL (Access Control List)

| Command                                    | Example                                       | Description                 |
| ------------------------------------------ | --------------------------------------------- | --------------------------- |
| `setfacl -m u:USERNAME:perms file_or_dir`  | `setfacl -m u:alice:rwx /project/report.txt`  | Modify/add quyền cho user   |
| `setfacl -m g:GROUPNAME:perms file_or_dir` | `setfacl -m g:devteam:rx /project/report.txt` | Modify/add quyền cho group  |
| `setfacl -R -m u:USERNAME:perms dir`       | `setfacl -R -m u:bob:rx /project`             | Apply đệ quy cho thư mục    |
| `setfacl -x u:USERNAME file_or_dir`        | `setfacl -x u:alice /project/report.txt`      | Xóa quyền của user          |
| `setfacl -b file_or_dir`                   | `setfacl -b /project/report.txt`              | Xóa tất cả ACL, về mặc định |
| `getfacl file_or_dir`                      | `getfacl /project/report.txt`                 | Kiểm tra ACL hiện tại       |
