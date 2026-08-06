Dưới đây là bộ lệnh đầy đủ của Aider, chia theo nhóm chức năng:

## Quản lý file trong ngữ cảnh

| Lệnh | Chức năng |
|---|---|
| `/add <file>` | Thêm file để aider có thể đọc và sửa |
| `/drop <file>` | Bỏ file ra khỏi ngữ cảnh (không dùng `/drop` gì cả sẽ bỏ hết) |
| `/read-only <file>` | Thêm file chỉ để tham khảo, aider không sửa |
| `/ls` | Xem danh sách file đang trong chat |
| `/add <folder>/*.py` | Thêm nhiều file theo wildcard |

## Chế độ làm việc (chat modes)

| Lệnh | Chức năng |
|---|---|
| `/code` | Chế độ mặc định — sửa file trực tiếp |
| `/ask` | Chỉ hỏi/thảo luận, không sửa file |
| `/architect` | 2 model phối hợp: 1 lên kế hoạch, 1 viết code |
| `/chat-mode <mode>` | Chuyển mode tường minh (code/ask/architect/help) |

## Sửa code & Git

| Lệnh | Chức năng |
|---|---|
| `/diff` | Xem diff các thay đổi hiện tại |
| `/undo` | Hoàn tác commit gần nhất do aider tạo |
| `/commit` | Commit thủ công (aider tự sinh message) |
| `/reset` | Bỏ hết file khỏi chat + xóa lịch sử hội thoại |
| `/clear` | Xóa lịch sử hội thoại, giữ nguyên file trong chat |
| `/lint` | Lint và tự sửa lỗi file đang mở |
| `/test <cmd>` | Chạy lệnh test, đưa lỗi vào chat để aider tự sửa |

## Chạy lệnh & lấy dữ liệu ngoài

| Lệnh | Chức năng |
|---|---|
| `/run <cmd>` hoặc `!<cmd>` | Chạy lệnh shell, có thể thêm output vào chat |
| `/web <url>` | Cào nội dung trang web đưa vào chat |
| `/paste` | Dán nội dung clipboard (text) vào chat |
| `/clipboard` | Dán ảnh từ clipboard (hữu ích để đưa screenshot lỗi) |

## Quản lý model & phiên làm việc

| Lệnh | Chức năng |
|---|---|
| `/model <name>` | Đổi model chính giữa chừng |
| `/models <search>` | Tìm model khả dụng |
| `/tokens` | Xem số token đang dùng trong ngữ cảnh |
| `/map` | Xem repo map hiện tại |
| `/map-refresh` | Làm mới repo map |
| `/save <file>` | Lưu session hiện tại ra file |
| `/load <file>` | Nạp lại session đã lưu |

## Thoát & trợ giúp

| Lệnh | Chức năng |
|---|---|
| `/help <câu hỏi>` | Hỏi cách dùng aider (aider tự trả lời) |
| `/exit` hoặc `/quit` | Thoát aider |

## Quy trình thực tế khuyên dùng

```
/add index.html script.js
/ask Giải thích cấu trúc file này và đề xuất hướng sửa lỗi X
```
Sau khi thống nhất hướng làm:
```
/code Hãy sửa theo hướng vừa bàn
```
Kiểm tra kết quả:
```
/diff
```
Nếu hài lòng → aider tự commit; nếu không → `/undo`.

Với dự án của bạn (nhiều file, model local context ngắn), nên ưu tiên `/add` đúng 1-3 file cần sửa, dùng `/read-only` cho file cấu hình/schema chỉ cần tham khảo, tránh nhồi cả folder vào context.