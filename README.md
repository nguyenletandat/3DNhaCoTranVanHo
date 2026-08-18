# Nhà Cổ Trần Văn Hổ — Website storytelling

Trang web tĩnh giới thiệu Nhà cổ ông Trần Văn Hổ (di tích kiến trúc nghệ thuật
cấp Quốc gia, số 18 đường Bạch Đằng, phường Phú Cường, TP. Thủ Dầu Một, tỉnh
Bình Dương) theo hướng "kể chuyện": mô hình 3D tương tác, dòng thời gian,
thư viện ảnh, vị trí.

HTML + CSS + JavaScript thuần — không framework, không backend. Chỉ cần một
static file server bất kỳ (hoặc GitHub Pages) là chạy được.

## Cấu trúc

```
├── index.html                        # toàn bộ nội dung/section của trang
├── style.css                         # giao diện (tông màu ấm: nâu đất / vàng đồng / be)
├── script.js                         # AOS init, dữ liệu timeline, hotspot, lightbox, slider so sánh, share
├── 3DNhaCo.glb                       # mô hình 3D hiển thị qua <model-viewer>
├── assets/                           # ảnh placeholder (SVG) — xem mục "Việc cần làm" bên dưới
└── Checklist-anh-can-thu-thap.xlsx   # checklist ảnh cần chụp thực địa (vị trí, số lượng, tên file)
```

Thư viện dùng qua CDN (không cần cài đặt): [`@google/model-viewer`](https://modelviewer.dev/)
để hiển thị `.glb`, và [AOS](https://michalsnik.github.io/aos/) cho hiệu ứng
fade/slide khi cuộn trang.

## Chạy thử cục bộ

Cần serve qua HTTP (mở trực tiếp bằng `file://` sẽ bị chặn fetch mô hình 3D
do CORS). Dùng bất kỳ static server nào, ví dụ:

```bash
# Python
python -m http.server 8080

# hoặc Node
npx serve .
```

Rồi mở `http://localhost:8080`.

## Việc cần làm trước khi công bố chính thức

Toàn bộ nội dung placeholder đều có comment `TODO` ngay tại chỗ trong
`index.html` / `script.js`. Tổng hợp lại:

- **Ảnh**: `assets/hero-bg.svg`, `assets/story-1..3.svg`, `assets/gallery-1..8.svg`,
  `assets/compare-old.svg`, `assets/compare-new.svg` đều là ảnh minh hoạ tạm
  (SVG tông màu ấm) — thay bằng ảnh thật rồi trỏ lại `src` tương ứng trong
  `index.html`. Nên nén ảnh trước khi thêm vào (JPG/WebP, không quá vài trăm KB/ảnh)
  vì trang chưa có build step để tối ưu ảnh tự động. Danh sách chi tiết vị trí
  cần chụp, số lượng, và tên file bắt buộc cho từng tấm nằm trong
  `Checklist-anh-can-thu-thap.xlsx`.
- **Văn bản lịch sử**: 3 block trong mục "Câu chuyện lịch sử" (Khởi dựng /
  Biến động lịch sử / Trùng tu & bảo tồn) và mảng `TIMELINE_DATA` ở đầu
  `script.js` — hiện chỉ có 2 mốc đã xác nhận (1890 khởi dựng, 1993 xếp hạng
  di tích Quốc gia), các mốc còn lại (`19xx`, `20xx`) là placeholder cần điền
  năm + sự kiện thật.
- **Hotspot trên mô hình 3D**: 3 hotspot mẫu ("Cổng chính", "Mái ngói", "Sân
  trong") trong `index.html` (tìm `slot="hotspot-1|2|3"`). `data-position` /
  `data-normal` là toạ độ ước lượng — cần chỉnh lại cho khớp với mô hình
  `3DNhaCo.glb` thật (mở model-viewer, thử các giá trị x/y/z tới khi điểm nằm
  đúng vị trí mong muốn), và thay nội dung chú thích trong mỗi
  `<div class="hotspot-annotation">`.
- **Vị trí**: khối `<iframe>` Google Maps trong `index.html` đang nhúng theo
  địa chỉ text (chưa phải toạ độ khảo sát chính xác). Thay bằng toạ độ chính
  xác khi có (ví dụ lấy từ Google Maps: nhấp chuột phải đúng vị trí công
  trình → sao chép toạ độ), và điền giờ mở cửa thật (đang là placeholder).
- **Footer**: tên đơn vị quản lý di tích và thông tin liên hệ đang là
  placeholder trong `index.html` (mục `.footer__org`).
- **Meta chia sẻ**: `og:image` chưa có ảnh — thêm khi có ảnh đại diện chính thức.

## Ghi chú kỹ thuật

- Nút chia sẻ Facebook/Zalo dùng `window.location.href` của trang đang mở —
  hoạt động đúng ngay khi deploy lên domain thật, không cần cấu hình thêm.
- Slider so sánh ảnh xưa/nay và lightbox thư viện ảnh là code tự viết (không
  phụ thuộc thư viện ngoài) trong `script.js`.
- `3DNhaCo.glb` (~24 MB) được commit thẳng vào Git — vẫn trong giới hạn bình
  thường của GitHub (không cần Git LFS ở kích thước này), nhưng nếu sau này
  thay bằng model nặng hơn nhiều (>100 MB) thì cần cân nhắc Git LFS.
