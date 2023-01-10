# esp32 書き込みlog
つくるっち/USB2BT/他のためのESP32書き込みlogデータ解析。  
つくるっちアプリの書き込みツールはesptool.pyとesptool.exeの書き込みlogデータをキャプチャ・リバエンして作成しています。  

## esptool logのキャプチャ方法
1. WireShark - USBPcap1でUSBキャプチャ開始。PCに接続されたすべてのUSBデバイスがキャプチャされるため、なるべく他のUSBデバイスを使わないこと
1. ArduinoIDE / Tukurutch.exe / esptool.py / esptool.exe のどれかでESP32書き込み実行
1. WireSharkキャプチャ停止 - [ファイル] - [..として保存] - [ファイルの種類 - SuSE 6.3 tcpdump]で保存
1. WSLで [pcap2txt](pcap2txt) 実行、`./pcap2txt xx.pcap 2 > xx.txt` でファイル変換

キャプチャしたデータは下記の通り。

## logデータ一覧
* ESP32  
[esp32.exe.pcap.txt](esp32.exe.pcap.txt)  
.  
つくるっち書き込みlogデータ  
[esp32.test.pcap.txt](esp32.test.pcap.txt)
```
書き込みコマンド
        tools\esp32\esptool.exe --chip esp32 --port COM14       --baud 921600 --before default_reset --after hard_reset write_flash -z --flash_mode dio --flash_freq 80m --flash_size detect 0x1000 tools/esp32/sdk/esp32/bin/bootloader_qio_80m.bin 0x8000 ext/libraries/esp32/src/src.ino.partitions.bin 0xe000 tools/esp32/partitions/boot_app0.bin 0x10000 ext/libraries/esp32/src/src.ino.esp32.bin
python3 tools/esp32/esptool.py  --chip esp32 --port /dev/ttyS14 --baud 921600 --before default_reset --after hard_reset write_flash -z --flash_mode dio --flash_freq 80m --flash_size detect 0x1000 tools/esp32/sdk/esp32/bin/bootloader_qio_80m.bin 0x8000 ext/libraries/esp32/src/src.ino.partitions.bin 0xe000 tools/esp32/partitions/boot_app0.bin 0x10000 ext/libraries/esp32/src/src.ino.esp32.bin

0x1000 tools/esp32/sdk/esp32/bin/bootloader_qio_80m.bin
0x8000 ext/libraries/esp32/src/src.ino.partitions.bin
0xe000 tools/esp32/partitions/boot_app0.bin
0x10000 ext/libraries/esp32/src/src.ino.esp32.bin
```

* ESP32C3  
[esp32c3.exe.pcap.txt](esp32c3.exe.pcap.txt)  
.  
つくるっち書き込みlogデータ  
[esp32c3.test.pcap.txt](esp32c3.test.pcap.txt)
```
書き込みコマンド
        tools\esp32\esptool.exe --chip esp32c3 --port COM24       --baud 921600 --before default_reset --after hard_reset write_flash -z --flash_mode dio --flash_freq 80m --flash_size detect 0x0 tools/esp32/sdk/esp32c3/bin/bootloader_qio_80m.bin 0x8000 ext/libraries/lovyanGFXc3/src/src.ino.partitions.bin 0xe000 tools/esp32/partitions/boot_app0.bin 0x10000 ext/libraries/lovyanGFXc3/src/src.ino.esp32c3.bin
python3 tools/esp32/esptool.py  --chip esp32c3 --port /dev/ttyS24 --baud 921600 --before default_reset --after hard_reset write_flash -z --flash_mode dio --flash_freq 80m --flash_size detect 0x0 tools/esp32/sdk/esp32c3/bin/bootloader_qio_80m.bin 0x8000 ext/libraries/lovyanGFXc3/src/src.ino.partitions.bin 0xe000 tools/esp32/partitions/boot_app0.bin 0x10000 ext/libraries/lovyanGFXc3/src/src.ino.esp32c3.bin

0x0 tools/esp32/sdk/esp32c3/bin/bootloader_qio_80m.bin
0x8000 ext/libraries/lovyanGFXc3u/src/src.ino.partitions.bin
0xe000 tools/esp32/partitions/boot_app0.bin
0x10000 ext/libraries/lovyanGFXc3u/src/src.ino.esp32c3.bin
```

* ESP32S3  
[esp32s3.exe.pcap.txt](esp32s3.exe.pcap.txt)  
[esp32s3.python.pcap.txt](esp32s3.python.pcap.txt)  
exeとpythonでstubCodeが不一致。  
.  
つくるっち書き込みlogデータ  
[esp32s3.test.pcap.txt](esp32s3.test.pcap.txt)
```
書き込みコマンド
        tools\esp32\esptool.exe --chip esp32s3 --port COM22       --baud 921600 --before default_reset --after hard_reset write_flash -z --flash_mode dio --flash_freq 80m --flash_size detect 0x0 ext/libraries/esp32S3DevkitC/src/src.ino.bootloader.bin 0x8000 ext/libraries/esp32S3DevkitC/src/src.ino.partitions.bin 0xe000 tools/esp32/partitions/boot_app0.bin 0x10000 ext/libraries/esp32S3DevkitC/src/src.ino.esp32s3.bin
python3 tools/esp32/esptool.py  --chip esp32s3 --port /dev/ttyS22 --baud 921600 --before default_reset --after hard_reset write_flash -z --flash_mode dio --flash_freq 80m --flash_size detect 0x0 ext/libraries/esp32S3DevkitC/src/src.ino.bootloader.bin 0x8000 ext/libraries/esp32S3DevkitC/src/src.ino.partitions.bin 0xe000 tools/esp32/partitions/boot_app0.bin 0x10000 ext/libraries/esp32S3DevkitC/src/src.ino.esp32s3.bin

0x0 ext/libraries/esp32S3DevkitC/src/src.ino.bootloader.bin
0x8000 ext/libraries/esp32S3DevkitC/src/src.ino.partitions.bin
0xe000 tools/esp32/partitions/boot_app0.bin
0x10000 ext/libraries/esp32S3DevkitC/src/src.ino.esp32s3.bin
```

## ESP32 Flash書き込み
つくるっちではesptoolのlogデータを元にESP32書き込みを実行。read_reg, write_regは無視、詳細は各logデータ参照。  
ソースコードは [comlib.js](https://github.com/sohtamei/scratch-vm/blob/develop/src/extensions/scratch3_tukurutch/comlib.js)  
```
# SYNC

# MEM - STUB_CODE
mem_begin            size     blocks   blocksize offset
mem_data             data     seq
mem_begin            size     blocks   blocksize offset
mem_data             data     seq
mem_end                       entrypoint

# CHANGE_BAUDRATE

# SET_PARAMS
set_params           fl_id    total_size block_size sector_size page_size status_mask

# FLASH - bootloader_qio_80m.bin
flash_defl_begin     write_size num_blocks WRITE_SIZE offset
flash_defl_data      len      seq
flash_md5            addr     size

# FLASH - src.ino.partitions.bin
flash_defl_begin     write_size num_blocks WRITE_SIZE offset
flash_defl_data      len      seq
flash_md5            addr     size

# FLASH - boot_app0.bin
flash_defl_begin     write_size num_blocks WRITE_SIZE offset
flash_defl_data      len      seq
flash_md5            addr     size

# FLASH - src.ino.esp32.bin
flash_defl_begin     write_size num_blocks WRITE_SIZE offset
flash_defl_data      len      seq
flash_md5            addr     size

# REBOOT
flash_begin
flash_defl_end       reboot
```

## UART接続とUSB接続(ESP32S3)
S3はパソコンとマイコンの接続にUART接続とUSB接続２つの接続方法がある。  
USB接続でのFlash書き込みのとき書き込み前にBootモードへの切り替えが必要で、ボーレート1200でOpen/CloseすることによりBootモード切り替えを行う。
```
シリアルポート「COM20」を1200bpsで開いて閉じる事によって、リセットを行っています。
PORTS {COM20, } / {} => {}
PORTS {} / {COM20, } => {COM20, }
Found upload port: COM20
```
つくるっちアプリではwebSerialを使っており、ボーレート1200でOpenすると直後にデバイス側から切断される。  
つくるっちアプリ / ArduinoIDEどちらの場合でも書き込み後Bootモード → 通常モード切り替えは自動では出来ない。USB抜き差しやRESETが必要。

## UART接続とUSB接続(ESP32C3)
C3はパソコンとマイコンの接続にUART接続とUSB接続２つの接続方法がある。  
USB接続でのFlash書き込みのとき、C3はS3のようなボーレート1200によるBootモード切り替えは不要。なぜかつくるっちではうまくいかない、BootボタンによりBootモード切り替え。

## bootloader変換
書き込み時2byteを置換する。どういう意味があるかは確認中。
```
bootloader[2] = 0x02
bootloader[3] = 0x2f(ESP32とESP32C3)、0x3f(ESP32C3)
```

## 圧縮バイナリデータの復元
FLASHデータは圧縮されている。復元は [conv.py](conv.py) で行う。
```
python3 conv.py <圧縮文字列>
```
