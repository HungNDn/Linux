# 📝 Vim / Vi Cheat Sheet

## 1. Modes (Chế độ)

🟩 **Normal mode** – mặc định khi mở vim  
🟦 **Insert mode** – lệnh vào insert:

```bash
i   # Enter insert mode trước con trỏ
a   # Enter insert mode append sau con trỏ
gg	# Đầu file
G	# Cuối file
:5  # Tới dòng số 5
0   # Di chuyển tới đầu dòng
$   # Di chuyển tới cuối dòng
:w	# Lưu
:q	# Thoát
:wq	# Lưu & thoát
Esc # Thoát insert
```

## 2. Visual Mode (Chọn vùng)

```bash
v       # chọn theo ký tự
V       # chọn theo dòng
command + left, right, down, up để chọn vùng
```

## 3. Delete (Xóa)

| Command | Description |
|---------|-------------|
| `dd` | Xóa 1 dòng |
| `3dd` | Xóa 3 dòng, ndd = n dòng |
| `d$` | Xóa đến cuối dòng |
| `d0` | Xóa về đầu dòng |
| `D` | Tương đương d$ |

---


## 4. Copy / Yank

| Command | Description |
|---------|-------------|
| `yy` | Copy 1 dòng |
| `3yy` | Copy 3 dòng, ndd = n dòng |
| `y$` | Copy đến cuối dòng |
| `y0` | Copy về đầu dòng |
| `Y` | Tương đương y$ |

---

## 5. Paste (Dán)

```bash
p   # dán sau con trỏ/dòng
P   # dán trước con trỏ/dòng
```

## 6. Search (Tìm kiếm) and Replace (Thay thế)

```bash
/text   # Tìm xuống
?text   # Tìm lên
Command + enter # Nhảy đển giá trị đầu tiên
n       # Next tới từ tiếp
N       # Trở lại từ vừa lướt qua
:s/old/new/ # Thay lần đầu tiên xuất hiện của old trên dòng con trỏ
:s/old/new/g # Thay tất cả lần xuất hiện trong dòng
:%s/old/new/g # Thay toàn file
:%s/old/new/gc # Thay với xác nhận từng lần
# Nhấn y → yes, n → no, a → tất cả, q → thoát.
```

## 7. Undo / Redo

```bash
u         # Undo
Ctrl + r  # Redo
```




