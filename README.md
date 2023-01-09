## esp32 書き込みlog
つくるっち/USB2BT/他のためのESP32書き込みlogデータ解析。  
esptool.pyとesptool.exeの書き込みlogデータをキャプチャ・リバエンして書き込みツールを作成しています。  


## esptoolのキャプチャ方法
1. WireShark - USBPcap1でUSBキャプチャ開始。すべてのUSBデバイスがキャプチャされるためなるべくUSBデバイスを使わないこと
1. ArduinoIDE / Tukurutch.exe / esptool.py / esptool.exe のどれかでESP32書き込み実行
1. WireSharkキャプチャ停止 - [ファイル] - [..として保存] - [ファイルの種類 - SuSE 6.3 tcpdump]で保存
1. WSLでpcap2txt実行、`./pcap2txt xx.pcap 2 > xx.txt` でファイル変換

## logデータ一覧
* ESP32
```
        tools\esp32\esptool.exe --chip esp32 --port COM14       --baud 921600 --before default_reset --after hard_reset write_flash -z --flash_mode dio --flash_freq 80m --flash_size detect 0x1000 tools/esp32/sdk/esp32/bin/bootloader_qio_80m.bin 0x8000 ext/libraries/esp32/src/src.ino.partitions.bin 0xe000 tools/esp32/partitions/boot_app0.bin 0x10000 ext/libraries/esp32/src/src.ino.esp32.bin
python3 tools/esp32/esptool.py  --chip esp32 --port /dev/ttyS14 --baud 921600 --before default_reset --after hard_reset write_flash -z --flash_mode dio --flash_freq 80m --flash_size detect 0x1000 tools/esp32/sdk/esp32/bin/bootloader_qio_80m.bin 0x8000 ext/libraries/esp32/src/src.ino.partitions.bin 0xe000 tools/esp32/partitions/boot_app0.bin 0x10000 ext/libraries/esp32/src/src.ino.esp32.bin

0x1000 tools/esp32/sdk/esp32/bin/bootloader_qio_80m.bin
0x8000 ext/libraries/esp32/src/src.ino.partitions.bin
0xe000 tools/esp32/partitions/boot_app0.bin
0x10000 ext/libraries/esp32/src/src.ino.esp32.bin
```
[esp32.exe.pcap.txt](esp32.exe.pcap.txt)  

* ESP32C3
```
        tools\esp32\esptool.exe --chip esp32c3 --port COM24       --baud 921600 --before default_reset --after hard_reset write_flash -z --flash_mode dio --flash_freq 80m --flash_size detect 0x0 tools/esp32/sdk/esp32c3/bin/bootloader_qio_80m.bin 0x8000 ext/libraries/lovyanGFXc3/src/src.ino.partitions.bin 0xe000 tools/esp32/partitions/boot_app0.bin 0x10000 ext/libraries/lovyanGFXc3/src/src.ino.esp32c3.bin
python3 tools/esp32/esptool.py  --chip esp32c3 --port /dev/ttyS24 --baud 921600 --before default_reset --after hard_reset write_flash -z --flash_mode dio --flash_freq 80m --flash_size detect 0x0 tools/esp32/sdk/esp32c3/bin/bootloader_qio_80m.bin 0x8000 ext/libraries/lovyanGFXc3/src/src.ino.partitions.bin 0xe000 tools/esp32/partitions/boot_app0.bin 0x10000 ext/libraries/lovyanGFXc3/src/src.ino.esp32c3.bin

0x0 tools/esp32/sdk/esp32c3/bin/bootloader_qio_80m.bin
0x8000 ext/libraries/lovyanGFXc3u/src/src.ino.partitions.bin
0xe000 tools/esp32/partitions/boot_app0.bin
0x10000 ext/libraries/lovyanGFXc3u/src/src.ino.esp32c3.bin
```
[esp32c3.exe.pcap.txt](esp32c3.exe.pcap.txt)  

* ESP32S3
```
        tools\esp32\esptool.exe --chip esp32s3 --port COM22       --baud 921600 --before default_reset --after hard_reset write_flash -z --flash_mode dio --flash_freq 80m --flash_size detect 0x0 ext/libraries/esp32S3DevkitC/src/src.ino.bootloader.bin 0x8000 ext/libraries/esp32S3DevkitC/src/src.ino.partitions.bin 0xe000 tools/esp32/partitions/boot_app0.bin 0x10000 ext/libraries/esp32S3DevkitC/src/src.ino.esp32s3.bin
python3 tools/esp32/esptool.py  --chip esp32s3 --port /dev/ttyS22 --baud 921600 --before default_reset --after hard_reset write_flash -z --flash_mode dio --flash_freq 80m --flash_size detect 0x0 ext/libraries/esp32S3DevkitC/src/src.ino.bootloader.bin 0x8000 ext/libraries/esp32S3DevkitC/src/src.ino.partitions.bin 0xe000 tools/esp32/partitions/boot_app0.bin 0x10000 ext/libraries/esp32S3DevkitC/src/src.ino.esp32s3.bin

0x0 ext/libraries/esp32S3DevkitC/src/src.ino.bootloader.bin
0x8000 ext/libraries/esp32S3DevkitC/src/src.ino.partitions.bin
0xe000 tools/esp32/partitions/boot_app0.bin
0x10000 ext/libraries/esp32S3DevkitC/src/src.ino.esp32s3.bin
```
[esp32s3.exe.pcap.txt](esp32s3.exe.pcap.txt)  
[esp32s3.python.pcap.txt](esp32s3.python.pcap.txt)  
exeとpythonでstubCodeが不一致。

## bootloader変換
書き込み時2byteを置換する。
```
bootloader[2] = 0x02
bootloader[3] = 0x2f(ESP32とESP32C3)、0x3f(ESP32C3)
```

