# Hướng dẫn sử dụng Claude Code với Model Context Protocol (MCP) và n8n

## Mục tiêu
Xây dựng workflow n8n tự động:
- Form upload PDF từ người dùng
- Gọi API Unstract để trích xuất dữ liệu hóa đơn
- Ghi dữ liệu vào Google Sheets

## Bước 1: Chuẩn bị môi trường làm việc
1. Mở VS Code và bật Claude Code.
2. Tạo thư mục làm việc mới, ví dụ: `N8N WORKFLOWS`.
3. Tạo tệp `claude.md` trong thư mục đó để định nghĩa vai trò trợ lý AI tạo workflow n8n.
4. Trong `claude.md`, xác định rõ yêu cầu:
   - `Form upload PDF` → `Unstract API` → `Google Sheets`
   - Cung cấp Endpoint và API Key của Unstract
   - Cung cấp link Google Sheets, tên sheet, và cột cần ghi

## Bước 2: Cài đặt MCP và n8n Skills
1. Kết nối Claude Code với repo cấu hình MCP và n8n Skills.
2. Cung cấp các bước cần thiết cho Claude Code:
   - Git clone repo cấu hình MCP
   - Chạy `npx mcp setup`
   - Tải `n8n skills`
3. Kết nối n8n Cloud bằng URL và API Key.
4. Khởi động lại Claude Code sau khi cấu hình MCP xong.

## Bước 3: Thiết lập API trích xuất dữ liệu (Unstract)
1. Đăng ký và đăng nhập Unstract Cloud.
2. Tạo project mới và upload mẫu PDF hóa đơn, ví dụ `invoice_sample.pdf`.
3. Khai báo các trường cần trích xuất:
   - Nhà cung cấp / Tên nhà cung cấp
   - Số hóa đơn
   - Ngày lập hóa đơn
   - Hạn thanh toán
   - Tổng tiền
4. Deploy API dưới dạng REST API.
5. Lấy Endpoint URL và API Key.

## Bước 4: Yêu cầu Claude tạo workflow n8n
1. Chuyển chế độ chat sang Plan Mode.
2. Yêu cầu Claude tạo workflow n8n với các bước:
   - Form upload PDF
   - HTTP Request đến Unstract API
   - Xử lý dữ liệu trả về
   - Google Sheets ghi dữ liệu
3. Cung cấp thông tin:
   - Endpoint URL, API Key của Unstract
   - Google Sheets credential và link sheet
   - Tên sheet và cấu trúc cột cần ghi

## Bước 5: Chỉnh sửa và gỡ lỗi
1. Trong node HTTP Request:
   - `Authentication`: `No Auth`
   - Header: `x-api-key: <UNSTRACT_API_KEY>`
   - Chỉ định đúng `Binary Property` là trường tải file lên, ví dụ `data`.
2. Trong node Google Sheets:
   - Chọn đúng credential đã cấu hình
   - Chọn đúng Google Sheet và sheet name
   - Map dữ liệu từ output của Unstract đến cột tương ứng
3. Kiểm tra các lỗi thường gặp:
   - HTTP Request không gửi binary file
   - Sai `Binary Property`
   - Sai endpoint hoặc API key
   - Sai tên sheet hoặc range

## Bước 6: Chạy thử nghiệm
1. Mở workflow n8n đã tạo.
2. Chạy thử `Execute Workflow`.
3. Upload một file PDF mẫu qua form web.
4. Kiểm tra:
   - PDF được nhận đúng
   - Unstract trả về dữ liệu trích xuất
   - Dữ liệu được ghi vào Google Sheets

## Gợi ý cấu trúc dữ liệu trích xuất
- `vendorName` → Tên nhà cung cấp
- `invoiceNumber` → Số hóa đơn
- `invoiceDate` → Ngày hóa đơn
- `dueDate` → Hạn thanh toán
- `totalAmount` → Tổng tiền

## Lưu ý
- Giữ bí mật `API Key` và không commit vào repo.
- Dùng biến môi trường hoặc credential n8n để lưu API key an toàn.
- Nếu cần, kiểm tra log n8n và yêu cầu Claude giúp gỡ lỗi node.
