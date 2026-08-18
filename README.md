# RANO - RAN Optimize

Phần mềm desktop phân tích dữ liệu CM (Configuration Management) của mạng vô tuyến: đọc các file export từ Ericsson Network Manager (ENM), Huawei MML, CSV/TXT, Excel... rồi chuyển vào database, lọc theo loại MO và xuất ra CSV/TXT/XLSX.

**Liên kết:**
- Website: <https://ranoptimize.web.app>
- GitHub: <https://github.com/iitold/rano>
- Tải bản mới nhất: <https://github.com/iitold/rano/releases>

## Cài đặt

1. Tải file **`rano.exe`** ở phần [Releases](https://github.com/iitold/rano/releases) (bản mới nhất).
2. Đặt `rano.exe` vào thư mục bạn muốn (ví dụ `C:\RANO`, Desktop...). **Không cần cài đặt**, chạy trực tiếp là được.
3. Lưu ý: thư mục chứa app phải có quyền ghi của người dùng (không nên đặt trong `Program Files` vì sẽ không tự cập nhật được).

## Sử dụng

Mở `rano.exe`, giao diện có các tab:

- **Parse**: thêm file/thư mục dữ liệu (Add Files / Add Folder), chọn parser (để mặc định *Auto-detect*), bấm **Parse**. Thanh tiến trình hiển thị khi đang chạy, có thể hủy (Cancel).
- **Records / MO**: duyệt dữ liệu đã parse, chọn các loại MO muốn xuất.
- **Export**: xuất ra **CSV / TXT / XLSX** vào thư mục bạn chọn.
- **License**: xem Machine ID, kích hoạt key, hoặc dùng trial.
- **About**: xem thông tin phiên bản, kiểm tra cập nhật.

Hỗ trợ nhập file nén `.gz`, `.zip` (tự động giải nén). File dữ liệu mẫu của ENM thường là các file dạng `enm*`, MML của Huawei là `.xml`/`.txt`.

## License

RANO được cấp phép theo từng máy.

1. Vào tab **License**, bấm **Copy** để lấy **Machine ID** (mỗi máy một mã riêng).
2. Gửi Machine ID (kèm tên/công ty nếu muốn) cho người bán qua email: **doancongthanh92@gmail.com**.
3. Nhận key từ người bán → nhập vào ô **License key** → bấm **Activate**.

Hoặc dùng bản dùng thử: bấm **Start Trial** (7 ngày, mỗi máy 1 lần), hoặc **Request License** để gửi yêu cầu mua.

> Lưu ý: máy đã dùng hết trial sẽ cần key chính thức. Khi có internet, app tự kiểm tra lại license với server; nếu license bị thu hồi/hết hạn, app sẽ yêu cầu kích hoạt lại.

## Cập nhật phiên bản

App tự cập nhật qua GitHub. Khi có bản mới:

1. Mở tab **About** → bấm **Check for updates**.
2. Nếu có bản mới → bấm **Download vX và khởi động lại**.
3. App tự tải, kiểm tra mã SHA-256, thay thế và khởi động lại.

Hoặc tải tay ở [Releases](https://github.com/iitold/rano/releases).

## Lịch sử phiên bản

Mỗi bản release trên [Releases](https://github.com/iitold/rano/releases) đều có **Release notes** ghi rõ thay đổi (kèm file `CHANGELOG.md`). Tóm tắt các bản gần đây:

- **v0.3.0 (18/08/2026)** — Tự động cập nhật, license mã hóa + kiểm tra online, liên kết website/releases trong About.
- **v0.2.0** — Đổi Operator → Company, tách thông tin liên hệ thành trường riêng.

## Dữ liệu phát sinh

- File license: `rano_license.json` (đặt cạnh `rano.exe`, được mã hóa theo máy).
- Database tạm khi parse: nằm trong thư mục temp của hệ điều hành (tự xóa).
- Log: thư mục `logs\` cạnh `rano.exe`.

## Hỗ trợ

Mọi thắc mắc về cài đặt, license hay lỗi phần mềm, liên hệ qua email: **doancongthanh92@gmail.com**.