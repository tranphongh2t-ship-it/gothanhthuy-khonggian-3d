# Không Gian 3D — Thử Vật Liệu Gỗ Thanh Thùy

Viewer 3D nội thất (phòng bếp) cho phép khách hàng **đi bộ trong không gian** và
**bấm vào bề mặt (sàn / tường / tủ bếp) để thử vân gỗ Thanh Thùy** ngay trên model.

Chạy hoàn toàn trên trình duyệt — Three.js + GLB (Draco nén), không cần server.

## Xem thử

GitHub Pages: `https://<user>.github.io/gothanhthuy-khonggian-3d/`

## Nhúng vào website

```html
<iframe
  src="https://<user>.github.io/gothanhthuy-khonggian-3d/"
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
└── README.md
```

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

## Vật liệu gỗ

18 vân gỗ Thanh Thùy (LP Oak / LP Walnut / TT Woodgrain / màu tự nhiên) — khai báo
trong mảng `WOODS` đầu file `index.html`, vân được sinh seamless bằng canvas nên
không cần file ảnh.