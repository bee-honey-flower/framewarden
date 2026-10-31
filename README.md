# FrameWarden (Minecraft 26.3 · Fabric · client-only)

Bộ điều tiết chất lượng thích ứng. Mod đo FPS mỗi giây; nếu FPS thấp kéo dài thì hạ dần
các thiết lập tốn tài nguyên, nếu ổn định thì nâng lại. **Không thay thế Sodium, chạy song song với Sodium.**

## Build

Yêu cầu: JDK 25.

1. Tạo wrapper (một lần): `gradle wrapper --gradle-version 9.5.1`
   (hoặc copy thư mục `gradle/` + `gradlew*` từ https://github.com/FabricMC/fabric-example-mod)
2. `./gradlew build` -> file jar nằm ở `build/libs/framewarden-0.1.0.jar`
3. Thử nhanh: `./gradlew runClient`

Phiên bản trong `gradle.properties` (Loom, Fabric API) hãy đối chiếu lại tại https://fabricmc.net/develop

## Các bậc hạ chất lượng

| Bậc | Thay đổi (luôn tính từ giá trị bạn đã chọn) |
|-----|---------------------------------------------|
| 1 | Entity distance x0.75 |
| 2 | Entity distance x0.5, tắt bóng thực thể |
| 3 | Biome blend <= 1 |
| 4 | Render distance -2 (không dưới `minRenderDistance`) |
| 5 | Render distance -4 |

An toàn: không bao giờ vượt giá trị bạn chọn; bạn tự đổi thiết lập thì mod nhận làm mốc mới;
nâng thất bại thì chờ lâu gấp đôi (tối đa 15 phút); rời thế giới/tắt game thì trả lại thiết lập gốc.

## Cấu hình

`config/framewarden.properties` (tự tạo lần chạy đầu): `targetFps`, `downSeconds`, `upSeconds`,
`allowRecovery`, `allowDisableShadows`, `allowRenderDistance`, `minRenderDistance`.

## Nếu build lỗi

Mod chỉ dùng vài API của game. Nếu một dòng báo lỗi "cannot find symbol", khả năng cao là tên đã đổi ở 26.3:
- `Minecraft#getFps()`, `Minecraft#isWindowActive()`, `Minecraft#screen`
- `Options#entityDistanceScaling() / entityShadows() / biomeBlendRadius() / renderDistance()`
- `OptionInstance#get() / set()`

## Hướng phát triển (cần mixin, nên làm sau khi build được bản này)

- Entity / block-entity culling theo khoảng cách và tầm nhìn
- Giới hạn ngân sách particle theo từng frame
- Tìm điểm nghẽn bằng profiler rồi mới tối ưu (đừng đoán)
