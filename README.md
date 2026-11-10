# Build firmware cho Viettel NR3053 (mod USB + quạt + I2C)

Build bằng GitHub Actions từ mã nguồn `chasey-dev/immortalwrt-mt798x-rebase` (nhánh 25.12).

## Nội dung
- `nr3053/mt7981b-viettel-nr3053.dts`: DTS đã bật USB (VBUS GPIO14), thêm quạt PWM2 (GPIO7), I2C0 (GPIO30 SCL / GPIO31 SDA), đèn xanh dương (GPIO5).
- `nr3053/sdmc-nr3053.config.txt`: cấu hình gốc.
- `nr3053/nr3053-extra.config`: gói thêm (driver modem, ModemManager, luci-app-5gmodem, driver quạt, i2c-tools, luci-app-oled).
- `nr3053/files/etc/uci-defaults/zzz-nr3053-local`: chạy ở lần khởi động đầu: giờ Việt Nam, máy chủ gói gốc của ImmortalWrt, bỏ feed `video`, dùng feed kmod riêng.

## Vì sao cần feed kmod riêng
Firmware build riêng có kernel riêng, nên kmod trên kho chính thức không cài được ("no such package").
Workflow đăng toàn bộ kmod build cùng lần lên GitHub Pages, và firmware tự dùng feed đó, nên `apk update && apk add kmod-...` chạy bình thường.

## Chuẩn bị (một lần)
1. Repo để **Public** (GitHub Pages miễn phí chỉ cho repo public).
2. **Settings → Pages → Build and deployment → Source: GitHub Actions**.

## Chạy build
1. Tab **Actions** → **Build NR3053 firmware** → **Run workflow**.
2. Chờ 2–4 tiếng.
3. Tải artifact **nr3053-firmware** (file `.zip`) ở trang lần chạy.
4. Flash `immortalwrt-mediatek-filogic-viettel_nr3053-squashfs-sysupgrade.itb` bằng `sysupgrade -n`.

Feed kmod chỉ giữ bản của lần build mới nhất. Mỗi lần build lại thì flash luôn firmware của lần đó, để kmod khớp kernel.
