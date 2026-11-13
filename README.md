<p align="center">
  <img src="assets/hakrcf405v2.svg" width="760" alt="HAKRCF405V2 ArduCopter">
</p>

<h1 align="center">ArduCopter для HAKRCF405V2</h1>

<p align="center">
  Проверенная неофициальная сборка ArduCopter из исходников для полётного
  контроллера HAKRCF405V2 на STM32F405.
</p>

<p align="center">
  <a href="https://github.com/lordcom68-hue/HAKRCF405V2-ArduPilot/releases"><img src="https://img.shields.io/github/v/release/lordcom68-hue/HAKRCF405V2-ArduPilot?include_prereleases&label=release" alt="GitHub release"></a>
  <img src="https://img.shields.io/badge/ArduCopter-4.7.0--dev-2563EB" alt="ArduCopter 4.7.0-dev">
  <img src="https://img.shields.io/badge/board-HAKRCF405V2-0F766E" alt="HAKRCF405V2">
  <img src="https://img.shields.io/badge/MCU-STM32F405-7C3AED" alt="STM32F405">
  <a href="COPYING.txt"><img src="https://img.shields.io/badge/license-GPLv3-E11D48" alt="GPLv3"></a>
</p>

<p align="center">
  <a href="#скачивание">Скачать</a> ·
  <a href="docs/INSTALL_RU.md">Установка</a> ·
  <a href="docs/HARDWARE_RU.md">Аппаратная часть</a> ·
  <a href="docs/BUILD_RU.md">Сборка из исходников</a> ·
  <a href="docs/TESTING_RU.md">Проверка</a>
</p>

> [!IMPORTANT]
> Это независимая неофициальная сборка. Она подтверждена владельцем как
> полностью рабочая на реальном контроллере, но разные ревизии платы могут
> отличаться. Первую проверку всегда проводите **без пропеллеров**.

## Что это за проект

Репозиторий содержит полный исходный код ArduPilot с пользовательской аппаратной
целью `HAKRCF405V2`, документацию и воспроизводимые инструкции сборки. Готовый
релиз включает прошивку вместе с загрузчиком для перехода с Betaflight и файл
`APJ` для последующих обновлений через Mission Planner.

| Параметр | Значение |
|---|---|
| Прошивка | ArduCopter `4.7.0-dev` |
| Целевая плата | `HAKRCF405V2` |
| Микроконтроллер | `STM32F405xx`, Flash 1 МБ |
| Board ID | `4202` |
| Исходный коммит порта | `221251fa69896d8f0d3ecb4d7b0bc6901bbd2074` |
| Размер приложения | 842 564 байта |
| Свободно во Flash | 140 476 байт |
| Дата проверенной сборки | 27 сентября 2026 года |

## Поддерживаемые возможности платы

- IMU `ICM42688` по SPI;
- барометр `DPS310` на поддерживаемых ревизиях;
- девять PWM-выходов;
- шесть последовательных интерфейсов плюс USB;
- SBUS/RC-вход через `USART2`;
- аналоговый контроль напряжения, тока и RSSI;
- экранное меню MAX7456 и журналирование во Flash;
- внешние компасы по I²C;
- загрузчик ArduPilot и обновление из Mission Planner.

Подробная карта интерфейсов находится в
[`docs/HARDWARE_RU.md`](docs/HARDWARE_RU.md), а первичная конфигурация платы — в
[`libraries/AP_HAL_ChibiOS/hwdef/HAKRCF405V2`](libraries/AP_HAL_ChibiOS/hwdef/HAKRCF405V2).

## Что проверено

| Статус | Проверка |
|---|---|
| ✅ | Исходники успешно собраны для цели `HAKRCF405V2` |
| ✅ | Загрузчик и ArduCopter объединены в установочный HEX |
| ✅ | Метаданные APJ содержат правильную плату и Board ID |
| ✅ | Готовые файлы сверены по SHA-256 |
| ✅ | Владелец подтвердил работу прошивки на реальном контроллере |
| ⚠️ | Каждую другую ревизию платы необходимо проверять отдельно |

## Скачивание

Откройте раздел **[Releases](https://github.com/lordcom68-hue/HAKRCF405V2-ArduPilot/releases)**
и выберите файл по назначению:

| Файл | Назначение |
|---|---|
| `arducopter_with_bl.hex` | Первая установка вместо Betaflight через STM32CubeProgrammer |
| `arducopter.apj` | Последующие обновления через Mission Planner |
| `arducopter.bin` | Сырой образ приложения для специальных сценариев |
| `HAKRCF405V2_bl.hex` | Только загрузчик; отдельно обычно не требуется |
| `SHA256SUMS.txt` | Контрольные суммы файлов релиза |

Полная пошаговая инструкция: **[Установка прошивки](docs/INSTALL_RU.md)**.

## Быстрая установка

### Первая установка

1. Снимите пропеллеры и сохраните конфигурацию Betaflight.
2. Переведите контроллер в режим DFU кнопкой `BOOT`.
3. В STM32CubeProgrammer выполните полную очистку микроконтроллера.
4. Запишите `arducopter_with_bl.hex` с включённой проверкой записи.
5. Переподключите USB и соединитесь с платой через Mission Planner.

### Последующие обновления

В Mission Planner откройте:

```text
SETUP → Install Firmware → Load custom firmware
```

и выберите `arducopter.apj`.

> [!WARNING]
> Не записывайте `arducopter.bin` или отдельный загрузчик, если точно не знаете,
> зачем это требуется. Ошибка выбора платы или адреса записи может временно
> сделать контроллер недоступным.

## Сборка из исходников

```bash
git clone --recurse-submodules https://github.com/lordcom68-hue/HAKRCF405V2-ArduPilot.git
cd HAKRCF405V2-ArduPilot
git checkout v4.7.0-dev-hakrcf405v2-20260927
git submodule update --init --recursive
./waf configure --board HAKRCF405V2 --no-submodule-update
./waf copter
```

Результаты появятся в `build/HAKRCF405V2/bin`. Настройка Linux, Windows/Cygwin,
toolchain и проверка результата описаны в
**[инструкции по сборке](docs/BUILD_RU.md)**.

## Происхождение исходников

Проверенная сборка основана на пользовательском порте
[`Jafarabat2025/AP`](https://github.com/Jafarabat2025/AP) и точном коммите
`221251fa69896d8f0d3ecb4d7b0bc6901bbd2074`. В публикационном коммите изменена
только документация и оформление; код, из которого собраны бинарники, сохранён.

ArduPilot распространяется по лицензии GNU GPLv3. Подробности и атрибуция:
[`docs/SOURCE_AND_LICENSE.md`](docs/SOURCE_AND_LICENSE.md). Оригинальная главная
страница ArduPilot сохранена в
[`docs/ARDUPILOT_UPSTREAM_README.md`](docs/ARDUPILOT_UPSTREAM_README.md).

## Безопасность

- снимайте пропеллеры перед прошивкой и первой настройкой;
- после прошивки заново проверьте ориентацию платы, датчики и порядок моторов;
- не переносите параметры с другой платы без проверки;
- сначала проводите стендовые испытания и только затем короткий безопасный полёт;
- сохраняйте журналы и резервную копию параметров перед обновлением.

Полный контрольный список: [`docs/TESTING_RU.md`](docs/TESTING_RU.md).

## Отказ от ответственности

Проект не является официальным продуктом ArduPilot, HAKRC или производителя
контроллера. Пользователь самостоятельно отвечает за проверку совместимости,
настройку failsafe и безопасность эксплуатации.
