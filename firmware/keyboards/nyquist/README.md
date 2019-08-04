# Nyquist Wireless

## Video

[![](http://img.youtube.com/vi/35nr770rzKE/0.jpg)](https://youtu.be/35nr770rzKE)

## Keymap

See [keymap.cpp](keymaps/default/keymap.cpp)

## Building and uploading

Get prerequisites for core development here:

* https://github.com/adafruit/Adafruit_nRF52_Arduino

Get SWD programmer (I use Bluepill + Blackmagic firmware), run this batch:

```
@echo off

set root=D:/Projects/github/BlueMicro_BLE

cd %root%/build/windows

powershell ./build.ps1 nyquist all

set path=C:\SDK\gcc-arm-none-eabi-8-2018-q4-major-win32\bin;%path%

set port=COM8
mode %port% | find "RTS" > nul
if errorlevel 1 echo Port %port% not found && exit

copy %root%/output/nyquist/nyquist*.hex %~dp0

set file=%~dp0/s132_nrf52_6.1.1_softdevice.hex
arm-none-eabi-gdb.exe --batch -ex "target extended-remote \\.\%port%" -ex "mon swdp_scan" -ex "file %file:\=/%" -ex "att 1" -ex "mon erase" -ex load

set file=%~dp0/nyquist-default-left.hex
arm-none-eabi-gdb.exe --batch -ex "target extended-remote \\.\%port%" -ex "mon swdp_scan" -ex "file %file:\=/%" -ex "att 1" -ex load

set file=%~dp0/app_valid_setting_apply_nRF52832.hex
arm-none-eabi-gdb.exe --batch -ex "target extended-remote \\.\%port%" -ex "mon swdp_scan" -ex "file %file:\=/%" -ex "att 1" -ex load

```

## Troubleshooting

BlueMicro v2.0c I use has different mapping, D2 (RXD pin) is wired to P0.08 not the default 0.07, fixed in keymap:
```
#undef D2
#define D2 8
```

## References

* https://keeb.io/products/nyquist-keyboard
* https://github.com/jpconstantineau/BlueMicro_BLE
* https://github.com/jpconstantineau/NRF52-Board

