"# ESP32 GGWave Audio Receiver

Dự án thu nhận và giải mã tín hiệu âm thanh sử dụng thư viện GGWave và điều chế FSK trên ESP32 với microphone I2S SPH0645 và hiển thị kết quả trên màn hình LCD 16x2 I2C.

## 🎯 Tổng quan

ESP32 GGWave Audio Receiver là một dự án mã nguồn mở cho phép ESP32 thu nhận và giải mã dữ liệu được truyền qua tín hiệu âm thanh. Dự án sử dụng microphone I2S SPH0645 để thu âm chất lượng cao và thư viện [ggwave](https://github.com/ggerganov/ggwave) để thực hiện việc truyền dữ liệu qua sóng âm với hiển thị real-time trên màn hình LCD.

### 🎯 Ứng dụng
- 🎵 Truyền dữ liệu không dây qua âm thanh
- 🌐 IoT communication trong môi trường nhiễu RF
- 🎮 Demo truyền dữ liệu sáng tạo
- 📚 Nghiên cứu và giáo dục về xử lý tín hiệu số
- 💬 Nhận tin nhắn text qua sóng âm

## ✨ Tính năng

- 🎤 Thu nhận tín hiệu âm thanh qua microphone I2S SPH0645
- 🔄 Giải mã real-time sử dụng thuật toán GGWave
- 📱 Hiển thị kết quả trên LCD 16x2 I2C
- 🔍 Debug chi tiết qua Serial Monitor
- 💾 Tối ưu hóa bộ nhớ cho ESP32
- 🚀 Xử lý real-time với hiệu suất cao
- 💡 LED báo trạng thái hoạt động

## 🛠️ Yêu cầu phần cứng

### Board chính
- **ESP32 Development Board** (NodeMCU-ESP32-S hoặc tương tự)

### Microphone I2S
- **Adafruit I2S SPH0645** 
- Hoặc các microphone I2S khác tương thích

### Màn hình LCD
- **LCD 16x2** với module I2C
- **Địa chỉ I2C:** 0x27 (có thể thay đổi trong code)

### Linh kiện khác
- LED báo trạng thái (ESP32 có LED built-in GPIO 2)
- Breadboard và dây jumper
- Nguồn cấp cho ESP32 (USB hoặc 5V)

## 🔌 Sơ đồ kết nối

### Microphone I2S SPH0645
| ESP32 Pin | SPH0645 Pin | Mô tả |
|-----------|-------------|--------|
| **3.3V**  | **VCC**     | Nguồn cấp |
| **GND**   | **GND**     | Mass |
| **GPIO 26** | **BCLK**  | Bit Clock (Serial Clock) |
| **GPIO 33** | **DOUT**  | Data Output (Serial Data) |
| **GPIO 25** | **LRCL**  | Left/Right Clock (Word Select) |

### LCD I2C 16x2
| ESP32 Pin | LCD Pin | Mô tả |
|-----------|---------|--------|
| **3.3V**  | **VCC** | Nguồn cấp |
| **GND**   | **GND** | Mass |
| **GPIO 21** | **SDA** | I2C Data |
| **GPIO 22** | **SCL** | I2C Clock |

### LED Status
| ESP32 Pin | Thiết bị | Mô tả |
|-----------|----------|--------|
| **GPIO 2** | **LED** | LED trạng thái (built-in) |

## 📦 Cài đặt

### 1. Chuẩn bị môi trường

**Option A: PlatformIO (Khuyến nghị)**
```bash
# Cài đặt PlatformIO CLI
pip install platformio

# Hoặc sử dụng VS Code Extension
# Tìm "PlatformIO IDE" trong VS Code Extensions
```

**Option B: Arduino IDE**
- Cài đặt Arduino IDE 2.0+
- Thêm ESP32 board package

### 2. Clone và setup dự án

```bash
# Clone dự án
git clone https://github.com/QBL-23/fsk-receiver.git
cd esp32-ggwave-receiver

# Tạo file platformio.ini
cat > platformio.ini << EOF
[env:esp32dev]
platform = espressif32
board = esp32dev
framework = arduino
lib_deps = 
    ggerganov/ggwave@^0.4.2
    marcoschwartz/LiquidCrystal_I2C@^1.1.4
monitor_speed = 115200
build_flags = -DCORE_DEBUG_LEVEL=0
EOF
```

### 3. Build và upload

**PlatformIO:**
```bash
# Build dự án
pio run

# Upload lên ESP32
pio run --target upload

# Mở Serial Monitor
pio device monitor
```

**Arduino IDE:**
- Mở file `src/main.cpp`
- Chọn board: ESP32 Dev Module
- Upload code

## ⚙️ Cấu hình

### Cấu hình microphone trong `src/main.cpp`:

```cpp
// Chọn loại microphone (đã được cấu hình sẵn)
#define MIC_I2S_SPH0645  // Cho Adafruit SPH0645

// Nếu dùng microphone I2S khác
//#define MIC_I2S         // Cho microphone I2S thông thường
```

### Cấu hình hiển thị:

```cpp
// Bật/tắt hiển thị LCD
#define DISPLAY_OUTPUT 1  // Bật LCD

// Địa chỉ I2C của LCD (có thể cần thay đổi)
LiquidCrystal_I2C lcd(0x27, 16, 2);  // Thử 0x3F nếu 0x27 không hoạt động
```

### Cấu hình chế độ hoạt động:

```cpp
// Chế độ tầm xa (chậm hơn nhưng ổn định hơn)
//#define LONG_RANGE 1

// Tần số lấy mẫu cao (chất lượng tốt)
const int sampleRate = 24000;        // 24kHz
const int samplesPerFrame = 512;     // 512 samples/frame

// Hoặc tần số thấp hơn để tiết kiệm tài nguyên
//const int sampleRate = 12000;      // 12kHz
//const int samplesPerFrame = 256;   // 256 samples/frame
```

## 🚀 Sử dụng

### 1. Khởi động hệ thống

1. **Kết nối phần cứng** theo sơ đồ trên
2. **Upload code** lên ESP32
3. **Mở Serial Monitor** (115200 baud)
4. Quan sát quá trình khởi tạo:

```
Đang khởi tạo màn hình...
VXL ESP32 RX:
Dang lang nghe..
Đang cố gắng khởi tạo instance ggwave
Sử dụng độ dài payload: 16
Bộ nhớ yêu cầu bởi instance ggwave: XXXX bytes
Instance được khởi tạo thành công!
Đang khởi tạo giao diện I2S
Sử dụng đầu vào I2S
Áp dụng fix cho SPH0645
```

### 2. Truyền dữ liệu

Bên phát sẽ điều chế FSK và phát tín hiệu điều chế qua loa:
https://github.com/QBL-23/fsk-transmitter.git

**⚙️ Cài đặt trong Waver:**
```
✅ Bật "Fixed-length"
📏 Payload length: 16 bytes (hoặc 8 bytes cho LONG_RANGE)
📡 Protocol: DT_FASTEST hoặc MT_FASTEST
🔊 Volume: Tối đa cho chất lượng tốt nhất
```

### 3. Quan sát kết quả

**Serial Monitor sẽ hiển thị:**
```
Nhận được dữ liệu với độ dài 16 bytes:
Hello ESP32!
```

**LCD sẽ hiển thị:**
```
Receive:
Hello ESP32!
```

## 📊 Thông số kỹ thuật

### Cấu hình âm thanh
- **Tần số lấy mẫu:** 24,000 Hz (có thể điều chỉnh xuống 12,000 Hz)
- **Độ phân giải:** 32-bit I2S input → 16-bit processing
- **Samples per frame:** 512 (có thể điều chỉnh xuống 256)
- **Channel:** Mono (RIGHT channel only)
- **Format:** I2S với MSB shift cho SPH0645

### Cấu hình GGWave
- **Payload length:** 16 bytes (8 bytes cho LONG_RANGE)
- **Sample format input:** I16
- **Sample format output:** U8
- **Operating mode:** RX + TX + DSS + TX_ONLY_TONES

### Hiệu suất
- **Thời gian decode:** < 21ms (cho 512 samples @ 24kHz)
- **Memory usage:** Được tối ưu cho ESP32
- **Real-time processing:** Có
- **CPU usage:** Tối ưu với interrupt-driven I2S

## 🔧 Troubleshooting

### ❌ Không nhận được dữ liệu

**1. Kiểm tra kết nối microphone:**
```
Serial Monitor phải hiển thị:
✅ "Sử dụng đầu vào I2S"
✅ "Áp dụng fix cho SPH0645"
```

**2. Kiểm tra tín hiệu âm thanh:**
```cpp
// Uncomment dòng này để xem raw audio data
for (int i = 0; i < nSamples; i++) {
    Serial.println(sampleBuffer[i]);
}
```

**3. Kiểm tra giao thức:**
- Đảm bảo app gửi và ESP32 dùng cùng protocol
- Thử chuyển sang LONG_RANGE mode

### 📺 LCD không hiển thị

**1. Thêm I2C Scanner vào setup():**
```cpp
Wire.begin(21, 22);
for (byte i = 8; i < 120; i++) {
    Wire.beginTransmission(i);
    if (Wire.endTransmission() == 0) {
        Serial.print("Found I2C device at 0x");
        Serial.println(i, HEX);
    }
}
```

**2. Địa chỉ I2C phổ biến:**
- `0x27` (mặc định)
- `0x3F`
- `0x20`

**3. Kiểm tra kết nối:**
- VCC → 3.3V hoặc 5V
- GND → GND  
- SDA → GPIO 21
- SCL → GPIO 22

### 💾 Lỗi bộ nhớ

**Giảm tài nguyên:**
```cpp
// Giảm samples per frame
const int samplesPerFrame = 256;  // Thay vì 512

// Bật LONG_RANGE để giảm payload
#define LONG_RANGE 1

// Giảm sample rate
const int sampleRate = 12000;     // Thay vì 24000
```

### 🔊 Chất lượng tín hiệu kém

**Tăng chất lượng:**
```cpp
// Tăng sample rate (nếu đủ memory)
const int sampleRate = 48000;

// Sử dụng protocol ổn định hơn
#define LONG_RANGE 1
```

**Kiểm tra môi trường:**
- 🔇 Giảm tiếng ồn xung quanh
- 📏 Đưa nguồn âm gần microphone (30-50cm)
- 🔊 Tăng volume của thiết bị phát
- 🎯 Hướng microphone về phía nguồn âm

### ⚠️ Log "Thất bại khi giải mã" xuất hiện quá nhiều

```cpp
// Comment lại dòng này để giảm spam
// Serial.println("Thất bại khi giải mã");
```

**Nguyên nhân:**
- Không có tín hiệu âm thanh hợp lệ (bình thường)
- Tín hiệu quá yếu hoặc quá nhiễu
- Protocol không khớp giữa sender và receiver

## 🎯 Tối ưu hóa

### Cho hiệu suất cao:
```cpp
const int sampleRate = 48000;
const int samplesPerFrame = 1024;
// Không dùng LONG_RANGE
```

### Cho tiết kiệm tài nguyên:
```cpp
const int sampleRate = 12000;
const int samplesPerFrame = 256;
#define LONG_RANGE 1
```

### Cho độ ổn định cao:
```cpp
const int sampleRate = 24000;
const int samplesPerFrame = 512;
#define LONG_RANGE 1
```

## 🔄 Workflow thực tế

1. **Khởi động:** ESP32 khởi tạo I2S, GGWave, LCD
2. **Lắng nghe:** Liên tục đọc data từ microphone SPH0645
3. **Xử lý:** Convert 32-bit I2S data → 16-bit samples
4. **Giải mã:** GGWave decode samples thành data
5. **Hiển thị:** Xuất kết quả ra Serial và LCD
6. **Reset:** Quay lại bước 2

## 🤝 Đóng góp

Chúng tôi hoan nghênh mọi đóng góp! Vui lòng:

1. **Fork** dự án
2. Tạo **feature branch** (`git checkout -b feature/AmazingFeature`)
3. **Commit** changes (`git commit -m 'Add AmazingFeature'`)
4. **Push** to branch (`git push origin feature/AmazingFeature`)
5. Tạo **Pull Request**

### Hướng dẫn phát triển
- ✅ Tuân thủ code style hiện tại
- 📝 Thêm comments tiếng Việt cho functions mới
- 🧪 Test trên hardware thực tế với SPH0645
- 📖 Cập nhật README nếu thêm tính năng mới
- 🐛 Báo cáo bug qua Issues


## 🙏 Cảm ơn

- [ggerganov](https://github.com/ggerganov) cho thư viện GGWave tuyệt vời
- [Espressif](https://github.com/espressif) cho ESP32 framework
- [Adafruit](https://github.com/adafruit) cho SPH0645 microphone