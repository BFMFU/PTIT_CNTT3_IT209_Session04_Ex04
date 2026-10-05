# Bài 4: Quản lý file nhạy cảm với Git

## 1. Mục tiêu

- Cấu hình file `.gitignore` để Git bỏ qua các file nhạy cảm và file rác.
- Gỡ file `credentials.txt` khỏi Git nhưng không xóa file vật lý trên máy.
- Sử dụng lệnh `git commit --amend` để chỉnh sửa commit gần nhất.

## 2. Tình huống

Trong quá trình làm bài, file `credentials.txt` chứa thông tin bảo mật đã bị commit nhầm vào Git.

Yêu cầu cần thực hiện:

- Giữ nguyên file `credentials.txt` trên máy tính.
- Không tiếp tục theo dõi file này bằng Git.
- Cấu hình `.gitignore` để Git bỏ qua file này trong các lần commit sau.
- Sửa lại thông điệp của commit gần nhất.

## 3. Các bước thực hiện

### Bước 1: Thêm `credentials.txt` vào `.gitignore`

Nội dung của file `.gitignore`:

```gitignore
credentials.txt
```

Sau khi thêm, Git sẽ bỏ qua file `credentials.txt` trong các lần theo dõi và commit tiếp theo.

### Bước 2: Gỡ `credentials.txt` khỏi Git nhưng giữ file trên máy

Sử dụng lệnh:

```bash
git rm --cached credentials.txt
```

Tham số `--cached` chỉ xóa file khỏi Git Index (staging area), không xóa file vật lý trong thư mục làm việc.

Kiểm tra trạng thái:

```bash
git status
```

Kết quả mong đợi:

```text
Changes to be committed:
    modified:   .gitignore
    deleted:    credentials.txt
```

`deleted: credentials.txt` ở đây chỉ có nghĩa là file được xóa khỏi Git ở commit tiếp theo, không phải xóa file vật lý khỏi máy.

Sau khi commit, do `credentials.txt` đã được thêm vào `.gitignore`, Git sẽ không tiếp tục theo dõi file này.

### Bước 3: Commit thay đổi

```bash
git add .gitignore
git commit -m "Remove sensitive credentials file from tracking"
```

### Bước 4: Sửa thông điệp commit gần nhất bằng `--amend`

Nếu muốn đổi thông điệp của commit vừa tạo:

```bash
git commit --amend -m "Update Git ignore configuration"
```

Lệnh `--amend` sẽ thay thế commit gần nhất bằng một commit mới có nội dung và thông điệp đã được chỉnh sửa.

### Bước 5: Kiểm tra trạng thái

```bash
git status
```

Kết quả mong đợi:

```text
nothing to commit, working tree clean
```

File `credentials.txt` vẫn tồn tại trong thư mục làm việc cục bộ nhưng không còn được Git theo dõi.

### Bước 6: Kiểm tra commit gần nhất

```bash
git log -n 1
```

Kết quả ví dụ:

```text
commit abc123...
Author: ...
Date:   ...

    Update Git ignore configuration
```

Thông điệp `Update Git ignore configuration` chứng minh commit gần nhất đã được sửa bằng lệnh `git commit --amend`.

## 4. Các lệnh chính đã sử dụng

```bash
git rm --cached credentials.txt
git add .gitignore
git commit -m "Remove sensitive credentials file from tracking"
git commit --amend -m "Update Git ignore configuration"
git status
git log -n 1
```

## 5. Kết quả

- `credentials.txt` vẫn được giữ lại trên máy tính.
- `credentials.txt` không còn được Git theo dõi.
- File `.gitignore` đã được cấu hình để bỏ qua `credentials.txt` trong tương lai.
- Commit gần nhất đã được chỉnh sửa thông điệp bằng `--amend`.
- `git status` không còn hiển thị `credentials.txt` là file staged hoặc modified.
