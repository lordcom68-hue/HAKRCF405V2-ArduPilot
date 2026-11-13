# Сборка из исходников

Проверенный релиз собран из коммита
`221251fa69896d8f0d3ecb4d7b0bc6901bbd2074` пользовательского порта ArduPilot.
Публикационный коммит поверх него меняет только документацию.

## Получение исходников

```bash
git clone --recurse-submodules https://github.com/lordcom68-hue/HAKRCF405V2-ArduPilot.git
cd HAKRCF405V2-ArduPilot
git checkout v4.7.0-dev-hakrcf405v2-20260927
git submodule update --init --recursive
```

Для точного сравнения кода релиза можно использовать исходный коммит:

```bash
git diff 221251fa69896d8f0d3ecb4d7b0bc6901bbd2074 -- \
  ':!README.md' ':!docs' ':!assets' ':!CONTRIBUTING.md' ':!SECURITY.md'
```

## Toolchain

Проверенная сборка выполнялась с:

- GNU Arm Embedded Toolchain `10-2020-q4-major`;
- Python 3;
- Waf из подмодуля ArduPilot;
- ChibiOS из зафиксированного подмодуля;
- Windows/Cygwin.

Новая версия компилятора может создать другой бинарник или выявить новые
предупреждения. Для максимально близкого результата используйте указанную
версию toolchain.

## Сборка ArduCopter

Добавьте `arm-none-eabi-gcc` в `PATH`, затем выполните:

```bash
./waf configure --board HAKRCF405V2 --no-submodule-update
./waf copter
```

В Cygwin команда и путь выглядят, например, так:

```bash
export PATH="/cygdrive/c/Tools/gcc-arm-none-eabi-10-2020-q4-major/bin:$PATH"
python3 ./waf configure --board HAKRCF405V2 --no-submodule-update
python3 ./waf copter
```

Файлы появятся в:

```text
build/HAKRCF405V2/bin/arducopter.apj
build/HAKRCF405V2/bin/arducopter.bin
build/HAKRCF405V2/bin/arducopter_with_bl.hex
```

## Сборка загрузчика

```bash
./waf configure --board HAKRCF405V2 --bootloader --no-submodule-update
./waf bootloader
```

Перед распространением убедитесь, что загрузчик и приложение используют один
Board ID — `4202`.

## Проверка результата

Проверьте:

1. успешное завершение Waf без ошибок;
2. наличие `arducopter.apj`, `arducopter.bin` и объединённого HEX;
3. плату `HAKRCF405V2` и Board ID `4202` в APJ;
4. размер приложения — он не должен превышать доступную область Flash;
5. SHA-256 каждого распространяемого файла.

Пример для PowerShell:

```powershell
Get-FileHash .\build\HAKRCF405V2\bin\* -Algorithm SHA256
```

Сборка может не совпасть побайтно из-за версии toolchain и метаданных сборки.
Опубликованные контрольные суммы относятся к готовому релизу от 27 сентября
2026 года.
