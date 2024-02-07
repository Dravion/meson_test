# prepare
Get Python for Windows (don't use the Windows App store thing
https://www.python.org/downloads/windows/

#Install meson
pip3 install meson

1) Open a VS Command prompt and run command:
%VCINSTALLDIR%\Auxiliary\Build\vcvarsall.bat x64 && cls

#Cross compile for: ARM64
%comspec% /k "%VCINSTALLDIR%\Auxiliary\Build\vcvarsall.bat" amd64_arm64
meson setup --cross-file windows-amd64-cross-to-arm64.config build/windows/arm64
meson compile -C build/windows/arm64
dumpbin /nologo /headers build\windows\arm64\my_program.exe | findstr /i "machine"

#Cross compile for: ARM32
%comspec% /k "%VCINSTALLDIR%\Auxiliary\Build\vcvarsall.bat" amd64_arm
meson setup --cross-file windows-amd64-cross-to-arm32.config build/windows/arm32
meson compile -C build/windows/arm32
dumpbin /nologo /headers build\windows\arm32\my_program.exe | findstr /i "machine"

#MS-CLANG
%comspec% /k "%VCINSTALLDIR%\Auxiliary\Build\vcvarsall.bat" x64
meson setup --cross-file windows-amd64-cross-to-clang.config build/windows/x64_clang
meson compile -C build/windows/x64_clang
dumpbin /nologo /headers build\windows\x64_clang\my_program.exe | findstr /i "machine"

run for example: build\windows\x64_clang\my_program.exe

#clear build dir
rmdir /q /s build

#FreeBSD CLANG
export CC=clang
export CXX=clang++
meson setup build/freebsd/x86_64
meson compile -C build/freebsd/x86_64
build/freebsd/x86_64/my_program

#MacOS CLANG
meson setup build/macos/x86_64
meson compile -C build/freebsd/x86_64
