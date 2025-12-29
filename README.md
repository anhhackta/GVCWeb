# GTA Vice City — HTML5 Port (DOS Zone)
**Dịch sang tiếng Việt:** Angry - HoàngX
[![Mở trên Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/13GFRIxTwVbixv0Vup9MSVXnB4SLmA3G7?usp=sharing)

> **Khởi động nhanh:** Chạy server chỉ với một cú nhấp chuột trên Google Colab. Nhấp vào badge ở trên, chạy cell, và sử dụng nút **"Launch Game"**. Mật khẩu tunnel sẽ được sao chép tự động — chỉ cần dán vào trang mở ra.

Phiên bản web của GTA: Vice City chạy trên trình duyệt thông qua WebAssembly.

## Yêu cầu

- Colab hoặc Docker hoặc Python 3.8+ hoặc PHP 8.0+
- Các thư viện phụ thuộc từ `requirements.txt`

## Bắt đầu nhanh

1.  **Clone repository**:
    ```bash
    git clone https://github.com/Lolendor/reVCDOS.git
    cd reVCDOS
    ```

2. **Cấu hình tài nguyên** (Tùy chọn):

   Theo mặc định, dự án sử dụng **DOS Zone CDN** — không cần tài nguyên cục bộ. Để lưu trữ offline, bạn có thể sử dụng:
   *   **Tệp nén** (`--packed` hoặc `--unpacked`) — tệp `.bin` duy nhất chứa tất cả tài nguyên
   *   **Thư mục cục bộ** (`--vcsky_local`, `--vcbr_local`) — thư mục tài nguyên đã giải nén
   *   **Chế độ cache** (`--vcsky_cache`, `--vcbr_cache`) — tải từ CDN một lần, sau đó phục vụ cục bộ

3. **Chạy ứng dụng**:
   Chọn một trong các phương pháp thiết lập dưới đây:
   * **Docker** (Khuyến nghị cho hầu hết người dùng) — nhanh và cô lập.
   * **PHP** — Chỉ cần tải thư mục lên server web của bạn (FTP/Hosting).
   * **Cài đặt thủ công** — dành cho phát triển và tùy chỉnh.

## Thiết lập & Chạy

### Tùy chọn 1: Sử dụng Docker (Khuyến nghị)
Cách dễ nhất để bắt đầu là sử dụng Docker Compose:

```bash
PACKED=https://folder.morgen.monster/revcdos.bin docker compose up -d --build
```

Để cấu hình các tùy chọn server thông qua biến môi trường:

```bash
# Đặt cổng, bật xác thực và lưu tùy chỉnh
IN_PORT=3000 AUTH_LOGIN=admin AUTH_PASSWORD=secret CUSTOM_SAVES=1 docker compose up -d --build
```

| Biến môi trường | Mô tả |
|------------------|-------|
| `OUT_HOST` | Host bên ngoài (mặc định: 0.0.0.0) |
| `OUT_PORT` | Cổng bên ngoài (mặc định: 8000) |
| `IN_PORT` | Cổng nội bộ container (mặc định: 8000) |
| `AUTH_LOGIN` | Tên người dùng HTTP Basic Auth |
| `AUTH_PASSWORD` | Mật khẩu HTTP Basic Auth |
| `CUSTOM_SAVES` | Bật lưu cục bộ (đặt giá trị `1`) |
| `VCSKY_LOCAL` | Phục vụ vcsky từ thư mục cục bộ (đặt giá trị `1`, hoặc đường dẫn như `/data/vcsky`) |
| `VCBR_LOCAL` | Phục vụ vcbr từ thư mục cục bộ (đặt giá trị `1`, hoặc đường dẫn như `/data/vcbr`) |
| `VCSKY_URL` | URL proxy vcsky tùy chỉnh |
| `VCBR_URL` | URL proxy vcbr tùy chỉnh |
| `VCSKY_CACHE` | Cache tệp vcsky cục bộ khi proxy (đặt giá trị `1`) |
| `VCBR_CACHE` | Cache tệp vcbr cục bộ khi proxy (đặt giá trị `1`) |
| `PACKED` | Phục vụ từ tệp nén (tên tệp hoặc URL, ví dụ: `revcdos.bin`) |
| `UNPACKED` | Giải nén tệp vào thư mục cục bộ (tên tệp hoặc URL, tự động đặt đường dẫn vcsky/vcbr) |
| `PACK` | Nén thư mục và phục vụ từ tệp nén kết quả (đường dẫn thư mục hoặc hash MD5) |

### Tùy chọn 2: Cài đặt cục bộ

1. Cài đặt các thư viện Python:
```bash
pip install -r requirements.txt
```

2. Khởi động server:
```bash
python server.py --packed https://folder.morgen.monster/revcdos.bin
```

Server sẽ chạy tại `http://localhost:8000`

### Tùy chọn 3: Hosting chia sẻ trên PHP (Không cần cài đặt)

Nếu bạn muốn chạy game từ môi trường hosting với `PHP 8.0` trở lên, chỉ cần sao chép nội dung của repo này vào hosting mong muốn.
Mặc định, `index.php` và `.htaccess` sẽ xử lý mọi thứ. 
## Tùy chọn server

| Tùy chọn | Kiểu | Mặc định | Mô tả |
|--------|------|---------|-------------|
| `--port` | int | 8000 | Cổng server |
| `--custom_saves` | cờ | tắt | Bật tệp lưu cục bộ (router lưu) |
| `--login` | chuỗi | không có | Tên người dùng HTTP Basic Auth |
| `--password` | chuỗi | không có | Mật khẩu HTTP Basic Auth |
| `--vcsky_local` | chuỗi/cờ | tắt | Phục vụ vcsky từ thư mục cục bộ. Sử dụng cờ cho `vcsky/` hoặc chỉ định đường dẫn |
| `--vcbr_local` | chuỗi/cờ | tắt | Phục vụ vcbr từ thư mục cục bộ. Sử dụng cờ cho `vcbr/` hoặc chỉ định đường dẫn |
| `--vcsky_url` | chuỗi | `https://cdn.dos.zone/vcsky/` | URL proxy vcsky tùy chỉnh |
| `--vcbr_url` | chuỗi | `https://br.cdn.dos.zone/vcsky/` | URL proxy vcbr tùy chỉnh |
| `--vcsky_cache` | cờ | tắt | Cache tệp vcsky cục bộ khi proxy |
| `--vcbr_cache` | cờ | tắt | Cache tệp vcbr cục bộ khi proxy |
| `--packed` | chuỗi | tắt | Phục vụ từ tệp nén. Chấp nhận đường dẫn tệp hoặc URL |
| `--unpacked` | chuỗi | tắt | Giải nén tệp vào `unpacked/{hash}/` và phục vụ từ đó. Chấp nhận đường dẫn tệp hoặc URL |
| `--pack` | chuỗi | tắt | Nén một thư mục và phục vụ từ tệp nén `{hash}.bin` kết quả. Chấp nhận đường dẫn thư mục hoặc hash MD5 |

**Ví dụ:**
```bash
# Bắt đầu trên cổng tùy chỉnh
python server.py --port 3000

# Bật lưu cục bộ
python server.py --custom_saves

# Bật xác thực HTTP Basic
python server.py --login admin --password secret123

# Sử dụng tệp vcsky và vcbr cục bộ
python server.py --vcsky_local --vcbr_local

# Cache tệp cục bộ khi proxy (chế độ lai) (khuyến nghị)
python server.py --vcsky_cache --vcbr_cache

# Phục vụ từ tệp nén (tệp cục bộ)
python server.py --packed revcdos.bin

# Phục vụ từ tệp nén (tải từ URL nếu không có)
python server.py --packed https://example.com/revcdos.bin

# Giải nén tệp và phục vụ từ tệp đã giải nén
python server.py --unpacked revcdos.bin

# Phát-stream và giải nén từ URL (tải xuống và giải nén đồng thời)
python server.py --unpacked https://example.com/revcdos.bin

# Nén một thư mục và phục vụ từ tệp nén kết quả
python server.py --pack /path/to/assets  # Tạo {folder_hash}.bin

# Nén từ thư mục đã giải nén theo hash MD5
python server.py --pack a1b2c3d4e5f6a7b8c9d0e1f2a3b4c5d6  # Sử dụng unpacked/{hash}/
```

> **Lưu ý:** HTTP Basic Auth chỉ được bật khi cả `--login` và `--password` đều được cung cấp.

> **Lưu ý:** Theo mặc định, vcsky và vcbr được proxy từ DOS Zone CDN. Sử dụng cờ `--vcsky_local` và `--vcbr_local` để phục vụ tệp từ thư mục cục bộ. Bạn cũng có thể chỉ định một đường dẫn tùy chỉnh.

> **Lưu ý:** Sử dụng cờ `--vcsky_cache` và `--vcbr_cache` để cache tệp được proxy cục bộ. Tệp sẽ được tải xuống một lần và phục vụ từ bộ nhớ cục bộ trong các yêu cầu tiếp theo.

> **Lưu ý:** `--packed` phục vụ tệp trực tiếp từ một tệp nén mà không cần giải nén (nhanh hơn và nén hơn). `--unpacked` giải nén tệp một lần và phục vụ từ tệp cục bộ (nếu bạn muốn chỉnh sửa tài nguyên).

> **Lưu ý:** Khi sử dụng URL với `--unpacked`, tệp lưu trữ sẽ được phát và giải nén đồng thời trong quá trình tải xuống bằng `downloader_brotli.py`.

> **Lưu ý:** Bạn có thể truyền một hash MD5 thô (32 ký tự hex) cho `--unpacked` để sử dụng một thư mục đã giải nén mà không cần tệp lưu trữ gốc. Ví dụ: nếu bạn có `unpacked/a1b2c3d4e5f6a7b8c9d0e1f2a3b4c5d6/`, bạn có thể khởi động server với `--unpacked a1b2c3d4e5f6a7b8c9d0e1f2a3b4c5d6`.

> **Lưu ý:** `--pack` tạo một tệp nén từ một thư mục chứa các thư mục con (như `vcsky/` và `vcbr/`). Mỗi thư mục con sẽ được nén theo thứ tự: thư mục đầu tiên tạo tệp nén, các thư mục sau sẽ được thêm vào. Sau khi nén, server sẽ tự động sử dụng tệp nén đã tạo qua chế độ `--packed`.

## Tham số URL

| Tham số | Giá trị | Mô tả |
|-----------|--------|-------------|
| `lang` | `en`, `ru` | Ngôn ngữ game |
| `cheats` | `1` | Bật menu gian lận (F3) |
| `request_original_game` | `1` | Yêu cầu tệp game gốc trước khi chơi |
| `fullscreen` | `0` | Vô hiệu hóa chế độ toàn màn hình tự động |
| `max_fps` | `1-240` | Giới hạn tỷ lệ khung hình (ví dụ: `60` cho 60 FPS) |
| `configurable` | `1` | Hiển thị giao diện cấu hình trước nút chơi |


**Ví dụ:**
- `http://localhost:8000/?lang=ru` — Phiên bản tiếng Nga
- `http://localhost:8000/?lang=en&cheats=1` — Tiếng Anh + gian lận
- `http://localhost:8000/?configurable=1` — Hiển thị giao diện cài đặt trước khi chơi

## Cấu trúc dự án

```
├── server.py           # FastAPI proxy server
├── index.php           # PHP proxy server
├── .htaccess           # Apache config for PHP
├── requirements.txt    # Python dependencies
├── revcdos.bin         # Packed archive (optional)
├── additions/          # Server extensions
│   ├── auth.py         # HTTP Basic Auth middleware
│   ├── cache.py        # Proxy caching and brotli decompression
│   ├── packed.py       # Packed archive serving module
│   └── saves.py        # Local saves router
├── utils/              # Utility modules
│   ├── packer_brotli.py # Archive packer with brotli compression
│   └── downloader_brotli.py # Archive packer with brotli compression
├── unpacked/           # Auto-created by --unpacked flag
│   └── {md5_hash}/     # Unpacked files organized by source hash
│       ├── vcsky/      # Decompressed game assets
│       └── vcbr/       # Brotli-compressed binaries
├── dist/               # Game client files
│   ├── index.html      # Main page
│   ├── game.js         # Game loader
│   ├── index.js        # Module loader
│   ├── GamepadEmulator.js  # Touch controls
│   ├── idbfs.js        # IndexedDB filesystem
│   ├── jsdos-cloud-sdk.js  # Cloud saves (DOS Zone)
│   ├── jsdos-cloud-sdk-local.js  # Local saves (--custom_saves)
│   └── modules/        # WASM modules
│       ├── runtime.js      # WASM runtime initialization
│       ├── loader.js       # Asset/package loading
│       ├── fs.js           # Virtual filesystem
│       ├── audio.js        # Audio system
│       ├── graphics.js     # Rendering pipeline
│       ├── events.js       # Input events handling
│       ├── fetch.js        # Network requests (Real-time asset streaming)
│       ├── syscalls.js     # System calls
│       ├── main.js         # Main entry point
│       ├── cheats.js       # Cheat engine (F3)
│       ├── asm_consts/     # Language-specific ASM constants
│       │   ├── en.js
│       │   └── ru.js
│       └── packages/       # Language-specific data packages
│           ├── en.js
│           └── ru.js
├── vcbr/               # Brotli-compressed game data (optional)
│   ├── vc-sky-en-v6.data.br
│   ├── vc-sky-en-v6.wasm.br
│   ├── vc-sky-ru-v6.data.br
│   └── vc-sky-ru-v6.wasm.br
└── vcsky/              # Decompressed assets (optional)
    ├── fetched/        # English version files
    │   ├── data/
    │   ├── audio/
    │   ├── models/
    │   └── anim/
    └── fetched-ru/     # Russian version files
        ├── data/
        ├── audio/
        └── ...
```

## Tính năng

- 🎮 Giả lập gamepad cho thiết bị cảm ứng
- ☁️ Lưu trữ đám mây qua js-dos key
- 💾 Lưu cục bộ (với cờ `--custom_saves`)
- 🌍 Hỗ trợ ngôn ngữ Tiếng Anh / Tiếng Nga
- 🔧 Công cụ gian lận tích hợp (quét bộ nhớ, gian lận)
- 📱 Điều khiển cảm ứng trên di động

## Lưu cục bộ

Khi lưu cục bộ được bật (cờ `--custom_saves`), nhập bất kỳ định danh 5 ký tự nào vào trường nhập "js-dos key" trên trang bắt đầu. Định danh này sẽ được sử dụng để lưu trữ các tệp lưu trong thư mục `saves/` trên server.

Ví dụ: Nhập `mykey` hoặc `12345` — các tệp lưu sẽ được lưu trữ dưới dạng `mykey_vcsky.saves` hoặc `12345_vcsky.saves`.

## Điều khiển (Cảm ứng)

Các điều khiển cảm ứng sẽ xuất hiện tự động trên các thiết bị di động. Các cần điều khiển ảo cho chuyển động và camera, các nút hành động nhạy cảm với ngữ cảnh.

## Gian lận

Bật với `?cheats=1`, nhấn **F3** để mở menu:
- Quét bộ nhớ (tìm/ chỉnh sửa giá trị)
- Tất cả các mã gian lận cổ điển của GTA VC
- AirBreak (chế độ không va chạm)

## Giấy phép

MIT. Làm những gì bạn muốn (nhưng hãy ghi công cho các tác giả port và tôi). Không liên quan đến Rockstar Games.

---

**Tác giả:** DOS Zone ([@specialist003](https://github.com/okhmanyuk-ev), [@caiiiycuk](https://www.youtube.com/caiiiycuk), [@SerGen](https://t.me/ser_var))

**Giải mã bởi**: [@Lolendor](https://github.com/Lolendor)

**Bản dịch tiếng Nga:** [GamesVoice](https://www.gamesvoice.ru/)

**Được thêm bởi cộng đồng:**
* Hỗ trợ PHP bởi [Rohamgames](https://github.com/Rohamgames)

## Hỗ trợ [tôi](https://github.com/Lolendor)

Nếu bạn thấy dự án này hữu ích:

- **TON / USDT (TON)**  `UQAyBchGEKi9NnNQ3AKMQMuO-SGEhMIAKFAbkwwrsiOPj9Gy`
- **ETH / USDT (ERC-20)** `0x69Ec02715cF65538Bb03725F03Bd4c85D33F8AC0`
- **TRX / USDT (TRC-20)** `THgNWT9MGW52tF8qFHbAWN25UR6WTeLDMY`
