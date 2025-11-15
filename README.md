charybdis-nano　を　zmk_firmware　で動かすようにしたファームウェアです。

## ピンアサイン
Seeed Studio XIAO BLE（nRF52840）の各ピンは以下のように割り当てています。

### 行（共通）

| 行番号 | XIAO ピン | 備考 |
| --- | --- | --- |
| Row0 | D1 | `config/boards/shields/Test/zmkcharybdis.dtsi` |
| Row1 | D2 | 同上 |
| Row2 | D3 | 同上 |
| Row3 | D6 | 同上 |

### 列（左手シールド）

| 列番号 | XIAO ピン / Port | 備考 |
| --- | --- | --- |
| Col0 | D10 | `config/boards/shields/Test/zmkcharybdis_L.overlay` |
| Col1 | D9 | 同上 |
| Col2 | D8 | 同上 |
| Col3 | D7 | 同上 |
| Col4 | P0.10 (`&gpio0 10`) | 同上 |
| Col5 | P0.9 (`&gpio0 9`) | 同上 |

### 列（右手シールド）

| 列番号 | XIAO ピン / Port | 備考 |
| --- | --- | --- |
| Col6 | D10 | `config/boards/shields/Test/zmkcharybdis_R.overlay` |
| Col7 | D9 | 同上 |
| Col8 | D8 | 同上 |
| Col9 | D7 | 同上 |
| Col10 | P0.10 (`&gpio0 10`) | 同上 |

### 周辺デバイス

| デバイス | 接続ピン | 備考 |
| --- | --- | --- |
| ロータリーエンコーダ（左手のみ） | A: D5 / B: D0 | `config/boards/shields/Test/zmkcharybdis.dtsi` |
| トラックボール PMW3610（右手） | SPI0: SCK=P0.5, MOSI=P0.4, MISO=P0.4 / CS=P0.9 / IRQ=P0.2 | `config/boards/shields/Test/zmkcharybdis_R.overlay` |
