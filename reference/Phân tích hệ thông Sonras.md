# Phân tích hệ thống SONRAS

SONRAS (Sound Registration and Analyzing System) là hệ thống thu thập và phân tích âm thanh dưới nước. Kiến trúc gồm hai khối chính hoạt động độc lập nhưng kết nối qua radio: **phao thu âm (Buoy)** và **trạm điều khiển (SCS)**.

## 1. Kiến trúc tổng thể

Hệ thống theo mô hình **Edge–Base Station**: phao đóng vai trò thiết bị edge device thu thập, xử lý sơ bộ và lưu trữ dữ liệu tại chỗ, trong khi SCS là trạm trung tâm nhận, hiển thị và hậu xử lý.

```
Hydrophone
    |
    v
+-------------------------------------------------------+
|                 Phao SONRAS (Buoy)                     |
|  Định vị GPS, hoạt động bằng pin                        |
|                                                          |
|  [Khuếch đại & ADC] -> [Xử lý & lưu trữ] -> [GPS/định vị]|
|         42Ks/s, 3Hz-100kHz    FFT, 1/3 octave  time-tag <1ms
|                                                          |
|  [Mã hóa & truyền UHF]      [Truyền thoại VHF]           |
|     Dữ liệu số, ~5km          Analog "listen in", ~2km   |
+-------------------------------------------------------+
                |  UHF data         |  VHF voice
                v                   v
+-------------------------------------------------------+
|            SCS - SONRAS Control Station                |
|  PC trên tàu, định vị GPS                                |
|                                                          |
|  [Giám sát]  [Phát lại]  [Định vị nguồn]  [Hậu xử lý]     |
|                                                          |
|  [Giao diện GUI & hiển thị phổ]  [Lưu trữ dữ liệu & log]  |
+-------------------------------------------------------+
```

## 2. Core components của phao (Buoy)

* **Hydrophone + tiền khuếch đại** — cảm biến đầu vào, dải tần 3 Hz – 100 kHz
* **Module ADC/lấy mẫu** — sampling 42 Ks/s cho băng 3Hz–20kHz, lọc số cho 20kHz–100kHz
* **Module xử lý tín hiệu (DSP)** — phân tích FFT, chia băng 1/3 octave, 
* **Module lưu trữ nội bộ** — ghi dữ liệu thô để lấy qua Ethernet sau khi thu hồi phao
* **Module định vị GPS** — xác định và báo cáo vị trí phao liên tục
* **Module mã hóa & truyền dữ liệu UHF** — gửi dữ liệu 1/3 octave mỗi 1.5s tới SCS, tầm 5km
* **Module truyền thoại VHF** — phát tín hiệu analog "nghe trực tiếp" (100Hz–3kHz), tầm 2km
* **Nguồn điện** — pin 8 giờ hoạt động đầy đủ, 24 giờ nếu chỉ phát vị trí

## 3. Core components của SCS (trạm điều khiển)

* **Module thu nhận radio (UHF/VHF)** — nhận dữ liệu và audio từ phao
* **Module giải mã & xác thực** — vì dữ liệu được mã hóa cả khi lưu và khi truyền
* **Engine 4 chế độ vận hành:**
  * Giám sát & điều khiển thu thập theo thời gian thực
  * Phát lại (replay) dữ liệu đã xử lý cùng âm thanh gốc
  * Định vị nguồn âm từ nhiều phao (multilateration dựa trên time-tag)
  * Hậu xử lý dữ liệu thô (FFT dải hẹp, độ phân giải 1.5 Hz)
* **GUI hiển thị phổ** — biểu đồ bar/envelope theo tần số, cấu hình scale dB
* **Module lưu trữ & quản lý file** — lưu dữ liệu phiên đo, log vị trí
* **Định vị GPS của tàu/trạm** — để tính khoảng cách buoy–SCS


## 4. Thông số kỹ thuật tham khảo

| Hạng mục | Thông số |
|----------|----------|
| Dải tần đăng ký | 3 Hz – 100 kHz |
| Độ chính xác mức âm | ±3 dB (toàn dải), ±1.5 dB (10Hz–10kHz) |
| Lấy mẫu  | 44\.1 kHz (<16kHz), 220 kHz (16–100kHz) |
| Truyền thông | UHF (dữ liệu), VHF (thoại analog) |
| Tầm hoạt động | 5 km (dữ liệu), 2 km (thoại) |
| Độ sâu hydrophone | Nổi: 40m, neo: cách đáy 1m |
| Dung lượng pin | 8 giờ (đầy đủ), 24 giờ (chỉ định vị) |
| Trọng lượng phao | 40 kg    |


