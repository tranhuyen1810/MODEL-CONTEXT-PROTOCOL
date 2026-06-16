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

## Lưu ý bảo mật
- Không commit `API Key` vào repo.
- Dùng biến môi trường/n8n credentials để lưu API Key an toàn.
