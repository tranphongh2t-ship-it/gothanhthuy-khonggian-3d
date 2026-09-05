# Không Gian 3D — Thử Vật Liệu Gỗ Thanh Thùy

Viewer 3D nội thất (phòng bếp) cho phép khách hàng **đi bộ trong không gian** và
**bấm vào bề mặt (sàn / tường / tủ bếp) để thử vân gỗ Thanh Thùy** ngay trên model.

Chạy hoàn toàn trên trình duyệt — Three.js + GLB (Draco nén), không cần server.

## Xem thử

GitHub Pages: **https://tranphongh2t-ship-it.github.io/gothanhthuy-khonggian-3d/**

## Nhúng vào website

```html
<iframe
  src="https://tranphongh2t-ship-it.github.io/gothanhthuy-khonggian-3d/"
  width="100%"
  height="600"
  style="border:0; border-radius:12px;"
  allowfullscreen
  loading="lazy"></iframe>
```

## Cấu trúc

```
├── index.html            Viewer (tự chứa: Three.js + thư viện load từ CDN)
├── models/kitchen-draco.glb   Model phòng bếp (6 MB, Draco)
├── textures/             119 ảnh vân gỗ thật (webp 1024px, ~6 MB tổng)
└── README.md
```

## Chia sẻ đúng vật liệu

Nút **🔗 Chia sẻ** (góc trên trái viewport) sao chép link dạng
`?wood=<slug>` — người mở link sẽ thấy **tự động áp sẵn vân gỗ đó lên sàn**
(bề mặt chính), bảng vật liệu mở sẵn và tô đậm mẫu tương ứng.

Ví dụ: `index.html?wood=lp-335` → tự áp Plum Walnut lên sàn.

## Đổi model khác

1. Nén GLB của bạn (nên < 15 MB):
   ```bash
   npx @gltf-transform/cli optimize input.glb output.glb --compress draco --texture-compress webp
   ```
2. Bỏ file vào `models/` và mở trang với tham số: `index.html?model=models/ten-file.glb`
   (không có tham số thì mặc định dùng `models/kitchen-draco.glb`).

## Điều khiển

- **Kéo chuột** → xoay camera quanh phòng
- **Cuộn chuột** → zoom vào/ra
- **🚶 Đi bộ (WASD)** → di chuyển tự do trong phòng
- **👆 Chọn bề mặt** → bấm vào sàn/tường/tủ để thay gỗ
- **📋 Bề mặt** → xem danh sách tất cả bề mặt có thể thay gỗ
- **☀ Ánh sáng** → chuyển 3 cấp: Sáng nhất / Sáng vừa / Tối
- **🔗 Chia sẻ** → sao chép link với gỗ đang chọn
- **⌂ Reset** → về góc nhìn mặc định

## Mẹo model chuẩn để thay vật liệu tốt

- Mỗi bề mặt nên là **1 mesh/material riêng** (sàn 1 material, tường 1 material, tủ 1 material…).
- Đặt tên material gợi ý bề mặt (`PISO`/sàn, `PARED`/tường, `TECHO`/trần, `MADERA`/tủ…) — khi bấm vào mesh, viewer hiển thị tên bề mặt đó.
- Tránh gộp cả phòng vào 1 mesh duy nhất (khi đó chỉ đổi được 1 bề mặt).

## Vật liệu gỗ — 110 sản phẩm Thanh Thùy

**110 vân gỗ** từ catalog Thanh Thùy, tổ chức theo nhóm:

| Nhóm | Số mẫu | Ví dụ |
|---|---|---|
| OAK · Vân Sồi (LP Laminate) | 12 | French Oak, Riviera Oak, Coburg Oak… |
| WALNUT · Vân Óc Chó (LP) | 12 | Classical Walnut, Plum Walnut, Lyon Walnut… |
| OAK · Vân Sồi (TTWO Melamine) | 27 | Roman Oak, Antique Oak, Vogue Oak… |
| WALNUT · Vân Óc Chó (TTWO) | 25 | Dark Walnut, Wenge, American Black Walnut… |
| BEECH · Vân Dẻ Gai | 3 | Bavarian Beech, White Beech, Jasmund Beech |
| HICKORY · Vân Dẻ Gai Mỹ | 3 | Natural Hickory, Breeze Hickory |
| ACACIA · Vân Acacia | 2 | Moldau Acacia |
| MAPLE / CHERRY | 8 | Wyoming Maple, Natural Cherry, Myoming Maple… |
| TEAK · Vân Tếch | 3 | Yellow Teak, Maldives Teak, Sunset Teak |
| SOLID · Màu Đặc | 10 | White, Magic Black, Latte, Shadow… |
| MARBLE · Vân Đá | 3 | Marquina Marble, Volakas Marble |

Tất cả khai báo trong mảng `WOODS` đầu file `index.html` — mỗi mẫu có đủ
`slug` (dùng cho link chia sẻ), `tex` (ảnh thật) và `src` (mã catalog hiện trên badge).

### Thêm vân gỗ mới (3 bước)

1. **Chuẩn bị ảnh**: cắt ảnh sản phẩm về tỉ lệ ~2:1 (khuyên 1024×512px), nén webp:
   ```bash
   python -c "from PIL import Image; im=Image.open('anh-goc.jpg').convert('RGB'); im=im.resize((1024,512), Image.LANCZOS); im.save('gothanhthuy-khonggian-3d/textures/<slug>.webp','WEBP',quality=82)"
   ```
2. **Bỏ ảnh vào** `textures/<slug>.webp` (đặt tên trùng `slug`, ví dụ `lp-500.webp`).
3. **Khai báo 1 dòng** trong mảng `WOODS` (thêm vào nhóm tương ứng trong `index.html`):
   ```js
   { name: 'Tên hiển thị', sku: 'LP 500EV', slug: 'lp-500', src: 'LP 500EV', tex: 'textures/lp-500.webp', c1: '#c8a06a', c2: '#8a5f33', grain: 3, twist: .05 },
   ```
   - `c1`/`c2`/`grain`/`twist` chỉ là màu dự phòng khi ảnh chưa tải (lấy màu chủ đạo của sản phẩm).
   - Nếu chưa có ảnh thật, bỏ qua `tex` — mẫu tự động dùng vân vẽ canvas, thêm ảnh sau cũng được.

Sau khi sửa, push lên GitHub (Pages tự rebuild ~1 phút) hoặc test local bằng
`.freebuff/start-kg.ps1` (http://127.0.0.1:54851).