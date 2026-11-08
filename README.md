# ladybird-arch-build

Build paket AUR [`ladybird-git`](https://aur.archlinux.org/packages/ladybird-git)
(+ semua dependensi AUR-nya) di GitHub Actions, dalam container
`archlinux:base-devel`. Hasilnya file `*.pkg.tar.zst` yang bisa diinstall
langsung via `sudo pacman -U`.

## Cara pakai

1. (Sekali) push repo ini ke GitHub — workflow ada di
   `.github/workflows/build-ladybird.yml`.
2. Jalankan manual: tab **Actions** → **build-ladybird** → **Run workflow**,
   atau:
   ```
   gh workflow run build-ladybird.yml -R <owner>/ladybird-arch-build
   ```
3. Tunggu (bisa 3-6 jam; skia-static + ladybird sama-sama berat).
4. Download artifact `ladybird-arch-pkgs`, extract, lalu install semua sekaligus:
   ```
   cd ladybird-arch-pkgs
   sudo pacman -U ./*.pkg.tar.zst
   ```
   Kalau muncul prompt `libpng-apng and libpng are in conflict. Remove libpng? [y/N]`,
   jawab **y** — aman, karena `libpng-apng` itu paket libpng yang sama
   (drop-in replacement) plus patch APNG, yang memang dibutuhkan ladybird.

## Catatan

- Trigger juga jalan otomatis mingguan (cron `0 3 * * 0`).
- Semua paket AUR yang dibuild (yay-bin, skia-static, angle, libpng-apng,
  vulkan-memory-allocator, wuffs-lib, zmij, ladybird-git) ikut di-upload,
  karena ladybird-git butuh semuanya pas diinstall.
- Update mingguan: cukup `pacman -Syu` dulu, lalu download artifact terbaru
  dan `pacman -U` ulang (binary -git dibangun dari master Ladybird, jadi
  kadang bisa gagal kalau upstream lagi rusak — cek ulang run berikutnya).
