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
├── textures/             15 ảnh vân gỗ thật (webp 1024px, ~600 KB tổng)
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

## Vật liệu gỗ18 vân gỗ Thanh Thùy (LP Oak / LP Walnut / TT Woodgrain / màu tự nhiên) — khai báo
trong mảng `WOODS` đầu file `index.html`. 15 mẫu (LP + TT) dùng **ảnh sản phẩm thật**
trong `textures/` (webp 1024px, tile seamless), 3 mẫu màu tự nhiên còn lại sinh bằng
canvas — thêm ảnh mới chỉ cần bỏ file vào `textures/` và thêm dòng `tex: 'textures/ten-file.webp'`
vào mục tương ứng trong `WOODS`.