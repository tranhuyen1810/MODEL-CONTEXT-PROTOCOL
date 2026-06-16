# MODEL-CONTEXT-PROTOCOL

Tự động thu thập file PDF được người dùng tải lên thông qua Form, trích xuất các thông tin cần thiết từ nội dung PDF và lưu dữ liệu vào Google Sheets để phục vụ quản lý, tra cứu và báo cáo.

## Nội dung repo
- `claude.md`: Hướng dẫn sử dụng Claude Code với Model Context Protocol (MCP) và n8n.
- `n8n-workflow-template.json`: Mẫu cấu hình workflow n8n để upload PDF, gọi Unstract API và ghi dữ liệu vào Google Sheets.

## Hướng dẫn nhanh
1. Mở workspace trong VS Code và bật Claude Code.
2. Dùng `claude.md` làm chỉ dẫn khi yêu cầu AI tạo workflow n8n.
3. Trong n8n:
   - Tạo webhook hoặc form upload PDF.
   - Thêm HTTP Request node tới Unstract API.
   - Thêm Google Sheets node để lưu dữ liệu.
4. Cập nhật `n8n-workflow-template.json` với `sheetId` và endpoint Unstract.

## Cấu trúc workflow mẫu
- `Webhook`: nhận file PDF từ người dùng.
- `Unstract API`: gửi file PDF sang Unstract để trích xuất.
- `Set`: chuẩn hóa dữ liệu trả về.
- `Google Sheets`: ghi dữ liệu vào bảng tính.

## Hướng dẫn import workflow vào n8n
1. Đăng nhập vào n8n Cloud hoặc n8n self-hosted.
2. Vào màn hình `Workflows`.
3. Chọn `Import from file` hoặc `Import`.
4. Chọn tệp `n8n-workflow-template.json` từ repo.
5. Kiểm tra lại các node và sửa các giá trị placeholder:
   - `sheetId`: ID của Google Sheet.
   - `x-api-key`: tham chiếu tới `{{$env.UNSTRACT_API_KEY}}` hoặc credential n8n.
6. Lưu và bật workflow.

## Cấu hình `UNSTRACT_API_KEY`
1. Lấy API Key từ Unstract sau khi deploy API.
2. Trong n8n:
   - Nếu dùng `Environment Variables`, thêm `UNSTRACT_API_KEY` vào file `.env` của n8n hoặc vào phần `Environment Variables` của n8n Cloud.
   - Nếu dùng `Credentials`, tạo credential `HTTP Request` hoặc `API Key` và trỏ header `x-api-key` tới credential đó.
3. Trong `Unstract API` node, `Authentication` để `No Auth` và chỉ dùng header:
   - `x-api-key: {{$env.UNSTRACT_API_KEY}}`

## Cấu hình `sheetId`
1. Mở Google Sheets muốn ghi dữ liệu.
2. Lấy `sheetId` từ URL, ví dụ:
   - `https://docs.google.com/spreadsheets/d/1AbCdEfGhIjKlMnOpQrStUvWxYz1234567890/edit`
   - `sheetId` là `1AbCdEfGhIjKlMnOpQrStUvWxYz1234567890`
3. Thay giá trị `<YOUR_GOOGLE_SHEET_ID>` trong node `Google Sheets` của workflow.
4. Nếu cần, đổi `range` thành `Sheet1!A:E` hoặc tên sheet tương ứng.

## Kiểm tra và chạy thử
1. Thử upload một file PDF qua webhook/form của workflow.
2. Xem kết quả trả về ở node `Unstract API`.
3. Kiểm tra node `Google Sheets` để đảm bảo giá trị `vendorName`, `invoiceNumber`, `invoiceDate`, `dueDate`, `totalAmount` được map đúng.

## Lưu ý bảo mật
- Không commit `API Key` vào repo.
- Dùng biến môi trường/n8n credentials để lưu API Key an toàn.
