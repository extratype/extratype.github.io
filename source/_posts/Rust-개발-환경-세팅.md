---
title: Rust 개발 환경 세팅
date: 2016-10-22 12:47:41
categories:
- coding
- Rust
tags:
---
나의 Rust 개발 환경을 정리해서 적어둔다.

- Windows 10 x64
- MSYS2 + rustup
- Rust stable x86_64 GNU ABI

## MSYS2 설치하기
리눅스스러운 환경을 구축하고 MinGW GCC 툴체인을 쓰기 위해서 [MSYS2](https://msys2.github.io)를 먼저 설치한다. Rust와 GCC를 함께 설치할 수도 있지만 [문제](https://github.com/rust-lang/rust/issues/19519#issuecomment-246117842)가 있어 따로 설치한다. MSYS2 인스톨러를 실행하고 난 다음엔 사이트에 나와있는 대로 업그레이드를 해주도록 하자.

`msys2_shell.cmd` 스크립트를 돌려서 MSYS2나 MinGW-w64 환경 쉘을 띄울 수 있다. `-where` 옵션을 주면 작업 디렉토리를 지정할 수 있다. 다른 옵션은 스크립트 파일을 참고하기 바란다. 탐색기의 컨텍스트 메뉴에 MSYS2 쉘을 추가하려면 [여기](https://github.com/njzhangyifei/msys2-mingw-shortcut-menus)를 참고.

MSYS2 환경은 윈도우와 독립된 POSIX 환경처럼 보이는데, 실제로는 MSYS2 툴체인으로 컴파일된(MSYS2 DLL을 사용하는) 프로그램에 한해서 그러하다. MinGW-w64 툴체인으로 컴파일된 프로그램들은 MSYS2 DLL을 거치지 않고 native로 동작한다. 이런 이유로 MSYS2를 분리된 POSIX 개발 환경으로 꾸밀 수는 없다.

나는 아예 윈도우와 MSYS2 환경을 통합해서 사용하기로 결정했다. `PATH` 환경 변수에 `C:\msys64\mingw64\bin`과 `C:\msys64\usr\bin`을 순서대로 추가한다. 단, 충돌을 방지하기 위해 `%SystemRoot%\system32` 보다 아래에 둔다. MSYS2 쉘에서 다른 윈도우용 툴들을 쓰기 위해 `msys2_shell.cmd`에서 `MSYS2_PATH_TYPE=inherit`를 설정한다. 이렇게 하면 MSYS2 쉘에서는 MSYS2 바이너리들을 `system32` 바이너리보다 우선하되 다른 툴들도 사용할 수 있게 된다.

## Rust 설치하기
인스톨러보다 Rust 툴체인 관리가 편해지는 [rustup](https://www.rustup.rs/)을 권장한다. 인스톨러로 Rust를 설치해도 되는데 MSYS2와 같이 쓰려면 'Linker and platform libraries' 옵션을 해제하고 설치하면 된다. rustup으로 Rust를 설치하면 최소한의 MinGW 툴체인이 같이 설치되며 제거할 수 있는 옵션이 아직 [없다](https://github.com/rust-lang-nursery/rustup.rs/issues/718). 임시방편으로 이렇게 제거하면 된다.

1. MinGW 툴체인이 설치된 디렉토리(예: `%USERPROFILE%\.multirust\toolchains\stable-x86_64-pc-windows-gnu\lib\rustlib\x86_64-pc-windows-gnu`)로 이동한다.
2. `bin` 폴더의 이름을 `bin~`로 바꾼다.
3. `lib~` 폴더를 만들고 `lib` 폴더 안의 `*.a` 파일들을 `lib~`로 모두 옮긴다.

나중에 Rust 툴체인을 삭제하거나 업그레이드할 일이 생기면 이렇게 백업해둔 파일들을 복원한 다음 rustup을 실행하면 된다.

[여기](https://github.com/rust-lang/rust-wiki-backup/blob/master/Using-Rust-on-Windows.md)를 참고하여 MinGW 툴체인을 설치한다.

## IDE 선택하기
[여기](https://areweideyet.com/)에서 IDE들을 비교해볼 수 있다. 나는 VS Code와 Eclipse를 선택했다. VS Code는 outline 창이 없다(Go to Symbol이나 Fold All로 흉내는 낼 수 있다)는 점만 빼면 가장 쓸만했다. 디버깅도 되는데 멀티스레드를 지원하지 않는 문제가 있다. 반대로 RustDT는 outline 기능도 있고 멀티스레드 디버깅도 지원하지만 매개변수 툴팁이 안 나오는 등 편집기가 마음에 들지 않았다.

다른 편집기도 조금씩 써보기는 했다.

- Atom은 UI가 VS Code만큼 깔끔하지 않았다.
- IntelliJ는 racer에 기반을 두지 않고 자체적으로 자동 완성을 지원하고, outline 기능도 있다. 매개변수 툴팁이 없고 디버깅도 지원하지 않는다.
- Visual Studio는 cargo로 빌드가 되지 않았다(지금은 되는 듯 하다).

VS Code는 이렇게 세팅했다.

1. Rust 소스 코드 다운로드: `rustup component add rust-src`
2. RustyCode, Native Debug, TOML Language Support 확장 설치
3. rustup 설치시 PATH를 등록하지 않았다면(예: `%USERPROFILE%\.cargo\bin`) `rust.cargoHomePath` 설정

Eclipse는 이렇게 세팅했다.

1. Eclipse Java Neon 설치
2. CDT 설치, Marketplace에서 RustDT 설치
3. 설정에서 각종 경로 지정. Download 버튼으로 쉽게 설치할 수는 있는데 기본 `bin` 폴더가 아니라 `RustDT` 폴더에 설치된다. 나는 [Rainicorn](https://github.com/RustDT/Rainicorn)까지 `bin` 폴더에 직접 설치했다.

Racer는 릴리즈된 버전이 오래되었으니 Git 저장소에서 설치하는 것을 추천한다:
```
cargo install --git https://github.com/phildawes/racer
```

디버깅은 `gdb`로 이루어진다. Rust 소스 코드에 포함된 pretty printer를 쓰면 변수를 깔끔하게 볼 수 있다. 단 디버깅이 느려지거나 가끔 멈추는 문제가 있다. Pretty printer를 쓰려면 우선 `.gdbinit` 파일을 만든다.

```python
python
print('Loading Rust pretty-printers...')
import sys
import os
import subprocess 
sys.path.append(os.path.join(subprocess.check_output(['rustc', '--print=sysroot']).decode(sys.stdout.encoding).strip(), 'lib', 'rustlib', 'src', 'rust', 'src', 'etc'))
import gdb_rust_pretty_printing
gdb_rust_pretty_printing.register_printers(gdb)
end
```

VS Code의 경우 `launch.json`을 수정한다. (예시)
```json
{
    "version": "0.2.0",
    "configurations": [{
        "name": "Debug",
        "type": "gdb",
        "request": "launch",
        "autorun": [
            "source .gdbinit"
        ],
        "target": "./target/debug/hello_rust.exe",
        "cwd": "${workspaceRoot}"
    }]
}
```

Eclipse에서는 Debug Configuration에서 Rust Application을 만들면 된다. 기본적으로 `.gdbinit`를 실행하게 되어 있다.

