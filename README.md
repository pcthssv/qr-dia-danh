# Landing Page địa danh qua mã QR

Trang web tĩnh một file động, đọc dữ liệu từ `data.json` theo tham số `?id=<slug>`. Ví dụ:

- `./?id=hoang-sa`
- `./?id=truong-sa`

Không truy cập URL trống hoặc slug sai: trang sẽ hướng dẫn quét lại mã QR.

## Chạy thử

1. Mở thư mục bằng Visual Studio Code.
2. Cài tiện ích **Live Server**.
3. Chuột phải `index.html` → **Open with Live Server**.
4. Truy cập `http://127.0.0.1:5500/?id=hoang-sa`.

Không nên mở trực tiếp bằng `file://` vì trình duyệt có thể chặn `fetch("./data.json")`.

## Thêm địa danh

Nhân bản object `_template` trong `data.json`, đổi tên khóa thành slug viết thường không dấu (ví dụ `thap-ba-ponagar`), rồi điền tên, tọa độ, ảnh, audio, highlights, overview, timeline, quote và gallery.

- `theme`: `theme-history`, `theme-ocean`, `theme-nature` hoặc `theme-culture`.
- `particleEffect`: `gold-embers`, `ocean-bubbles` hoặc `forest-leaves`.
- Mỗi content `name`, `subtitle`, `overview`, `timeline`... có thể là object `{ "vi": "...", "en": "..." }` để chuyển song ngữ.

## Quy tắc assets

- Ảnh cover và thư viện: đặt trong `assets/images/`, dùng định dạng `.webp`.
- Thuyết minh: đặt trong `assets/audio/`, dùng định dạng `.mp3`.
- Tên file bắt buộc:
  - `assets/images/<id>-hero.webp`
  - `assets/images/<id>-1.webp`
  - `assets/images/<id>-2.webp`
  - `assets/audio/<id>.mp3`
- Ví dụ: `hoang-sa-hero.webp`, `hoang-sa-1.webp`, `hoang-sa-2.webp`, `hoang-sa.mp3`.

Nén ảnh thành khoảng 120–300 KB khi có thể và cung cấp phiên bản cover ít nhất 1400 px.

## Bản đồ Natural Earth

Để có nền ranh giới quốc gia trên bản đồ Leaflet, tải dữ liệu Natural Earth 1:50m dạng GeoJSON và lưu tại:

- `assets/map/sea-region.geojson`

Nguồn Natural Earth chính thức:

- https://www.naturalearthdata.com/downloads/50m-cultural-vectors/

Mã nguồn GeoJSON từ kho Natural Earth Vector có thể tải trực tiếp file country (ví dụ):

```bash
curl -L "https://raw.githubusercontent.com/nvkelso/natural-earth-vector/master/geojson/ne_50m_admin_0_countries.geojson" -o assets/map/sea-region.geojson
```

Nếu chưa tải file này, bản đồ vẫn hoạt động với nền đại dương, khung quần đảo, đường nối đất liền, điểm đảo và bảng chú thích. Trang không yêu cầu thêm API key hoặc tile server.

## Deploy Cloudflare Pages

1. Tạo repository Git chứa `index.html`, `data.json`, `README.md` và thư mục `assets`.
2. Trong Cloudflare Dashboard, chọn **Workers & Pages** → **Create application** → **Pages** → kết nối repository.
3. Để trống **Build command**.
4. Đặt **Build output directory** là `/` (hoặc `.`).
5. Deploy. Cloudflare Pages sẽ phục vụ trang tĩnh trực tiếp, không cần bước build.
