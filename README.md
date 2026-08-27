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

## Đã cập nhật (2026-08-27)

- **Ảnh thật**: hero, 3 ảnh câu chuyện, và 14 ảnh thư viện (8 vị trí gốc +
  6 ảnh nội thất bổ sung: bàn thờ, hoành phi, không gian thờ cúng, phản gỗ
  gian tiếp khách, buồng ngủ, không gian sinh hoạt) đã được thay bằng ảnh
  thật lấy từ bộ ảnh sinh viên thu thập (`THU THAP THONG TIN/ẢNH` trong
  monorepo gốc), đã resize + nén (JPG, cạnh dài tối đa 2000px). `gallery-8.jpg`
  là bản vẽ phục dựng mặt bằng khuôn viên (render lại từ PDF). Bộ ảnh gốc có
  65 tấm theo `Checklist-anh-can-thu-thap.xlsx`, nhưng đó là số lượng ảnh
  *cần chụp* (nhiều tấm/vị trí để chọn ảnh đẹp nhất + ảnh tham chiếu canh mô
  hình 3D) — không phải số ảnh hiển thị trên web.
- **Mô hình 3D**: `3DNhaCo.glb` đã được dựng lại từ file quét
  `NHA ONG TRAN VAN HO.obj` (SketchUp, ~11,6 triệu đỉnh, ~1,26 GB). File gốc
  không kèm `.mtl`/texture nên mô hình mới **không có ảnh texture thật** — các
  mặt được tô màu theo hướng pháp tuyến (mái = màu ngói đất nung, tường = màu
  gỗ, sân = màu be, cây xanh = màu lá) để dễ đọc hình khối hơn màu xám trơn.
  File gốc chứa cả một dãy nhiều căn nhà lân cận; đã xác định và cắt riêng
  đúng khuôn viên nhà ông Trần Văn Hổ (cổng + nhà + sân vườn), rồi giảm từ
  ~10,5 triệu xuống còn ~800 nghìn mặt để tải được trên web/mobile.
  Toạ độ 3 hotspot mẫu cũng đã ước lượng lại cho khớp mô hình mới.

## Việc còn cần làm

Các mục còn lại đều có comment `TODO` ngay tại chỗ trong `index.html`:

- **Slider so sánh Xưa/Nay** (`assets/compare-old.svg` / `compare-new.svg`):
  vẫn là placeholder. Bộ ảnh đã thu thập (`compare-old_goc-a_01`,
  `compare-new_goc-a_01/02/03`) **không khớp góc chụp** (ảnh xưa chụp từ cổng
  ngoài đường, ảnh nay chụp từ trong sân) nên chưa dùng được — cần chụp lại
  ảnh "nay" đúng **cùng một góc** với ảnh "xưa" rồi thay 2 file SVG đó bằng
  ảnh JPG thật.
- **Văn bản lịch sử**: 3 đoạn trong mục "Câu chuyện lịch sử" và 3/5 mốc trong
  `TIMELINE_DATA` (đầu `script.js`, đánh dấu `19xx`/`20xx`) vẫn là placeholder
  — cần điền tư liệu lịch sử thật đã xác minh.
- **Nội dung chú thích hotspot**: toạ độ đã đúng vị trí (cổng/mái/sân), nhưng
  nội dung text trong mỗi `<div class="hotspot-annotation">` vẫn là
  placeholder — cần viết mô tả lịch sử/kiến trúc thật cho từng điểm. Toạ độ
  cũng chỉ ước lượng bằng mắt, có thể cần tinh chỉnh thêm vài chục cm khi mở
  thử trên site.
- **Vị trí**: khối `<iframe>` Google Maps đang nhúng theo địa chỉ text (chưa
  phải toạ độ khảo sát chính xác). Thay bằng toạ độ chính xác khi có, và điền
  giờ mở cửa thật (đang là placeholder).
- **Footer**: tên đơn vị quản lý di tích và thông tin liên hệ đang là
  placeholder trong `index.html` (mục `.footer__org`).
- **Meta chia sẻ**: `og:image` chưa có ảnh — thêm khi có ảnh đại diện chính thức.

## Ghi chú kỹ thuật

- Nút chia sẻ Facebook/Zalo dùng `window.location.href` của trang đang mở —
  hoạt động đúng ngay khi deploy lên domain thật, không cần cấu hình thêm.
- Slider so sánh ảnh xưa/nay và lightbox thư viện ảnh là code tự viết (không
  phụ thuộc thư viện ngoài) trong `script.js`.
- `3DNhaCo.glb` (~20 MB) được commit thẳng vào Git — vẫn trong giới hạn bình
  thường của GitHub (không cần Git LFS ở kích thước này), nhưng nếu sau này
  thay bằng model nặng hơn nhiều (>100 MB) thì cần cân nhắc Git LFS.
