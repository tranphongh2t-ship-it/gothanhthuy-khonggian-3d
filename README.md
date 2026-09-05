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
├── textures/             18 ảnh vân gỗ thật (webp 1024px, ~760 KB tổng)
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

## Mẹo model chuẩn để thay vật liệu tốt

- Mỗi bề mặt nên là **1 mesh/material riêng** (sàn 1 material, tường 1 material, tủ 1 material…).
- Đặt tên material gợi ý bề mặt (`PISO`/sàn, `PARED`/tường, `TECHO`/trần, `MADERA`/tủ…) — khi bấm vào mesh, viewer hiển thị tên bề mặt đó.
- Tránh gộp cả phòng vào 1 mesh duy nhất (khi đó chỉ đổi được 1 bề mặt).

## Vật liệu gỗ — 18/18 đều có ảnh sản phẩm thật

Toàn bộ 18 vân gỗ Thanh Thùy dùng **ảnh sản phẩm chụp thật** trong `textures/`
(webp 1024×512, tile seamless, preload song song với model) — không còn mẫu nào
sinh bằng canvas. Mỗi thumbnail hiển thị **badge mã nguồn catalog** ở góc dưới trái
để khách đối chiếu với catalog Thanh Thùy:

| Mã trong viewer | Tên hiển thị | Mã catalog thật |
|---|---|---|
| `lp-387` | French Oak | LP 387EV |
| `lp-388` | Oak Santana | LP 388EV |
| `lp-389` | Sonama Oak | LP 389EV |
| `lp-428` | Riviera Oak | LP 428T |
| `lp-611` | Banstead Oak | LP 611WN |
| `lp-240` | Metallic Oak | LP 240T |
| `lp-319` | Classical Walnut | LP 319EV |
| `lp-332` | Virgina Walnut | LP 332WN |
| `lp-333` | Columbia Walnut | LP 333WN |
| `lp-335` | Plum Walnut | LP 335T |
| `lp-612` | Snug Walnut | LP 612WN |
| `lp-448` | Lyon Walnut | LP 448EV |
| `tt-212` | Bavarian Beech | TT 212 |
| `tt-217` | White Beech | TT 217 |
| `tt-452` | Jasmund Beech | TT 452 |
| `te-01` | Gỗ tếch (Teak) | TTWO 609 Yellow Teak |
| `ch-01` | Anh đào (Cherry) | TTWO 385 Natural Cherry |
| `mp-01` | Phong (Maple) | TTWO 325 Maple |

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