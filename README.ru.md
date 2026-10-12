# Incoming Pack

[English](README.md) · Русский

Сборка NeoForge для Minecraft 1.21.1 вокруг мода [Incoming](https://github.com/zentixua/airstrike). До версии 0.1.24
сборка называлась Airstrike Pack, а мод — Airstrike.

Кроме Incoming в ней Create и Create Aeronautics для построек и летательных аппаратов, моды на самолёты и оружие,
Distant Horizons, шейдерпак и запись повторов.

Сборка описана файлами [packwiz](https://packwiz.infra.link/) в `pack/`: `pack.toml` (версии Minecraft и NeoForge),
`mods/*.pw.toml` и `shaderpacks/*.pw.toml` (откуда скачать каждый файл и его хеш) и `config/` (файлы, которые кладутся
в игру как есть).

## Установка

Скачать — из [последнего выпуска](https://github.com/zentixua/airstrike-pack/releases/latest), без входа на GitHub:

- [Incoming-Pack.zip](https://github.com/zentixua/airstrike-pack/releases/latest/download/Incoming-Pack.zip) —
  экземпляр [Prism Launcher](https://prismlauncher.org/download/), который сам обновляет сборку.
- [Incoming-Pack.mrpack](https://github.com/zentixua/airstrike-pack/releases/latest/download/Incoming-Pack.mrpack) —
  разовая установка одной версии в Prism Launcher или Modrinth App, без автообновления.

Выпуск `v<версия>` выходит, когда на `main` приходит новая версия сборки. Ссылки выше всегда ведут к последнему.

### Prism Launcher с автообновлением

В Prism: «Добавить экземпляр…» → «Импорт» → `Incoming-Pack.zip`. В экземпляре — Minecraft и NeoForge из
`pack.toml`, 6144 МБ памяти, `packwiz-installer-bootstrap.jar` в папке игры и команда перед запуском, которая его
запускает.

Перед каждым запуском игры [packwiz-installer](https://packwiz.infra.link/tutorials/installing/packwiz-installer/)
ставит `pack/` с ветки `main` этого репозитория: новое докачивает, убранное из сборки удаляет. Свои файлы игрока
(миры, настройки, свои моды) он не трогает. Файл из `config/` сборки перезаписывает, только когда сборка его меняет.

Первый запуск скачивает моды и спрашивает про [необязательные](#необязательные-моды). На следующих запусках в окне
packwiz-installer есть «Continue» — сразу в игру (само через 10 с) — и «Optional mods...» — включить или выключить
необязательные моды.

- Галочки модов сборки на странице «Моды» Prism не менять: убранный или выключенный мод сборки packwiz-installer
  вернёт. Необязательные моды переключаются в «Optional mods...».
- Когда сборка переходит на новую версию Minecraft или NeoForge, packwiz-installer спрашивает, обновить ли экземпляр.
- Файлы сборки GitHub раздаёт с кэшем до 5 минут. Сразу после обновления сборки `pack.toml` и `index.toml` могут
  прийти из разных коммитов, и packwiz-installer скажет про неверный хеш индекса. Нажать «Quit» и запустить игру
  через несколько минут.

### Разовая установка из .mrpack

Импортировать `Incoming-Pack.mrpack` в Prism Launcher или Modrinth App. Галочек по умолчанию в формате Modrinth нет,
и Prism предлагает все необязательные моды выключенными: Distant Horizons отметить (кроме macOS).

Экземпляр из `.mrpack` переводится на автообновление так, миры остаются:

1. В Prism открыть «Папка» экземпляра (папка игры, там `mods` и `saves`). Переименовать `mods` в `mods-old` и
   положить туда же [packwiz-installer-bootstrap.jar](https://github.com/packwiz/packwiz-installer-bootstrap/releases/latest).
2. «Изменить…» → «Параметры» → «Пользовательские команды»: включить и в «Предстартовая команда» вписать
   `"$INST_JAVA" -jar packwiz-installer-bootstrap.jar https://raw.githubusercontent.com/zentixua/airstrike-pack/main/pack/pack.toml`
3. Запустить. packwiz-installer скачает моды сборки заново и спросит про необязательные. Свои моды, если были,
   вернуть из `mods-old` в `mods`.

`mods` убирается в сторону, потому что packwiz-installer удаляет только то, что ставил сам. Jar мода из прежней версии
сборки (`airstrike-<версия>.jar`) называется иначе и остался бы рядом с новым, а с двумя копиями мода игра не
запускается.

### Экземпляры с прежним адресом

Экземпляры, поставленные до переезда сборки в этот репозиторий, ставят её из `zentixua/airstrike/main/pack/pack.toml`.
Этот адрес отдаёт сборку такой, какой она была при переезде, новых версий не получает и будет убран. Перевести:
«Изменить…» → «Параметры» → «Пользовательские команды» → в «Предстартовой команде» заменить
`zentixua/airstrike/main/pack/pack.toml` на `zentixua/airstrike-pack/main/pack/pack.toml`. Моды и миры остаются.

## Необязательные моды

| выбор | моды | по умолчанию |
|---|---|---|
| запись и повторы | Flashback, Flashback NeoForge Fixed, Sinytra Connector, Forgified Fabric API | выключено |
| дальняя прорисовка | Distant Horizons | включено |

Запись нужна только тем, кто снимает повторы или видео.

Distant Horizons на macOS (Apple Silicon) выключать. DH 3.3.3 запрашивает `GL_POLYGON_MODE` через `glGetInteger` в
место под одно число на стеке LWJGL, а драйвер OpenGL от Apple пишет два. Каждый кадр 4 байта уходят за конец буфера,
портится соседняя память, и Java падает (`SIGSEGV` в `Chunk::chop`, компилятор JIT). В ветке `main` DH этот запрос
тот же, падения на macOS — открытая [#1258](https://gitlab.com/distant-horizons-team/distant-horizons/-/issues/1258).
Мод, выключенный на странице «Моды» Prism, packwiz-installer вернёт, поэтому DH — необязательный мод сборки. Игрок без
DH заходит и на сервер с ним: пакеты DH зарегистрированы как необязательные (`PayloadRegistrar.optional()`).

## Что внутри

Minecraft 1.21.1, NeoForge 21.1.259. Точные версии модов — в `pack/mods/*.pw.toml`.

| группа | моды |
|---|---|
| основа | Create, Sable, Create Aeronautics, Incoming (jar выпуска с GitHub), Distant Horizons (необязательный, включён) |
| производительность | Sodium, Sodium Extra, Reese's Sodium Options, Iris, Colorwheel, Lithium, FerriteCore, ModernFix, ImmediatelyFast, Entity Culling, More Culling (+ Cloth Config), Dynamic FPS |
| дополнения Create | Create Propulsion: Simulated, Aeroworks, Tweaked Controllers, Big Cannons (+ Ritchie's Projectile Library), Connected, Deco, Diesel Generators, Aeronautics Hot Air Fix |
| самолёты и оружие | Immersive Aircraft, Man of Many Planes, Vic's Point Blank (+ GeckoLib), Point Blank Aeronautics compat |
| картинка и камера | шейдерпак Complementary Reimagined, Euphoria Patches, Aeronautics Camera Sync, 3D Skin Layers, Not Enough Animations |
| мультиплеер и удобство | Essential, TrueUUID (вход на сервер, [ниже](#аккаунты-trueuuid)), JEI (+ MezzConfig), Jade, Jade Sable Compat, Xaero's Minimap и World Map (+ MapSyncer: вся карта мира с сервера), Create - Xaero's map, GraveStone и его патч для Sable, Mouse Tweaks, No Chat Reports |
| запись (необязательно) | Flashback, Flashback NeoForge Fixed, Sinytra Connector, Forgified Fabric API |
| клавиши | Default Options (+ Balm): раскладка сборки ([ниже](#клавиши)) |
| замер производительности | spark: на сервере `/spark tps`, `/spark health`, `/spark profiler` (операторы), у себя в клиенте — `/sparkc` |
| только на сервере | Almighty (мост для ИИ-ведущего), ServerCore (производительность сервера) |

## Свой сервер

Сервер NeoForge 21.1.259 для Minecraft 1.21.1 ставит моды из той же `pack/`. Положить
[packwiz-installer-bootstrap.jar](https://github.com/packwiz/packwiz-installer-bootstrap/releases/latest) в папку
сервера и запускать там перед каждым запуском сервера:

```sh
java -jar packwiz-installer-bootstrap.jar -g -s server https://raw.githubusercontent.com/zentixua/airstrike-pack/main/pack/pack.toml
```

С `-s server` моды только для клиента не ставятся, а ставятся два мода, которые есть только у сервера (игрокам они не
скачиваются):

- [Almighty](https://github.com/zentixua/almighty) — мост, через который ИИ-агент (например, Claude Code) ведёт игру на
  сервере. HTTP-мост слушает только `host` и `port` из раздела `[bridge]` файла `config/almighty-common.toml` (по
  умолчанию `127.0.0.1` и `25650`), и каждый вызов несёт ключ из `config/almighty/token`, который создаётся при первом
  запуске. У моста права оператора, а со скриптами (`[scripts]`, `enabled = true` по умолчанию) он может выполнить
  любой код в процессе сервера. Держать его на петле или в сети, куда есть доступ только у вас. Не нужен —
  `enabled = false` в `[bridge]` выключает мост.
- [ServerCore](https://modrinth.com/mod/servercore) — производительность сервера, [настройки](#настройки) идут со
  сборкой.

### Аккаунты (TrueUUID)

TrueUUID — для сервера с `online-mode=false` в `server.properties`. Он стоит и на сервере, и у каждого игрока и
проверяет аккаунты вместо сервера. При входе сервер посылает клиенту одноразовый вызов, клиент отвечает через
`joinServer` своей сессии Mojang (токен доступа остаётся у игрока), сервер сверяет ответ с `hasJoined`. Игрок с
лицензией Minecraft входит со своим UUID и скином. Игрок без лицензии входит, только если его клиент прямо отвечает
«сессии нет», и только под ником, который ещё ни разу не входил с лицензией. Это значения TrueUUID по умолчанию:
`allowOfflineOnFailure`, `knownPremiumDenyOffline` и `allowOfflineForUnknownOnly` в его разделе `auth`.

### Карта мира (MapSyncer)

В одиночной игре Xaero's World Map читает карту прямо из файлов мира, а в мультиплеере рисует только чанки, которые
сервер прислал игроку. Всю карту игрокам отдаёт MapSyncer (стоит на сервере и у каждого игрока): сервер переводит
файлы регионов мира в формат карты Xaero и при входе присылает игроку недостающие и изменённые регионы. Xaero's World
Map и Minimap стоят и на сервере: так у каждого мира свой идентификатор, и карты и метки миров, которые по очереди
запускаются на одном адресе, не сливаются.

Для каждого мира один раз, в консоли сервера:

- `mapsyncer incremental scheduled` — карта сервера обновляется раз в сутки, по умолчанию в 04:00 по часам сервера
  (час и минуту можно дать после команды). Клиенты забирают изменения при входе.
- `mapsyncer generate` — вся карта сразу. Ход — `mapsyncer status`.

Раз в сутки, а не каждые несколько минут, потому что перед каждым проходом MapSyncer сохраняет мир целиком на диск в
потоке сервера.

## Настройки

Сборка кладёт в `config/` эти файлы, у каждого — своя причина:

- `entityculling.json` — значения Entity Culling по умолчанию плюс сущности Incoming (`airstrike:…`) в
  `entityWhitelist`. Такие сущности мод не прячет и не останавливает их тик на клиенте (в 1.11.3 список действует на
  оба). Факел снаряда рисуется вместе с моделью, а шлейф, облако пуска и дым горящих обломков рождаются в тике сущности
  на клиенте (`AirstrikeClient.entityTick`). Без исключения снаряд за холмом или за спиной игрока терял бы факел и рвал
  шлейф.
- `aero_cam_sync-client.toml` — значения Aeronautics Camera Sync по умолчанию и `configSchemaVersion = 5`. Версия
  1.4.0 сама пишет новый файл с версией схемы 0, а на следующем запуске считает его устаревшим и вместо главного меню
  открывает вопрос «Config Reset» (`ConfigMigrationManager`: файл есть и схема меньше 5) — до нажатия «Reset» или
  «Ignore», которые ставят 5. С файлом сборки вопроса нет. Обновляя мод, сверить его версию схемы.
- `moreculling.toml` — `useBlockStateCulling = false`, остальное More Culling дописывает сам (и, как у нового файла,
  переводит дальности рамок в блоки). Рамки он рисует по-своему: без задней стенки у рамок и карт, к которым игрок
  стоит спиной, у блока в рамке — три видимые грани, у плоского предмета дальше 16 блоков — только лицевая сторона
  (торец в 3 см). Отсечение граней блоков выключено: оно решает `Block.shouldRenderFace` в его начале и перебивает
  правку Aeronautics, по которой грань у левитита рисуется (`levitite/BlockMixin`). Экономит оно работу видеокарты с
  вершинами, а в замере 03.10.2026 видеокарта была занята на 34 %, пока поток отрисовки стоял на 100 %.
- `defaultoptions/keybindings.txt` — раскладка клавиш сборки ([ниже](#клавиши)).
- `servercore/config.yml`, `servercore/optimizations.yml` — значения ServerCore 1.5.19 по умолчанию, кроме дальности
  активации и динамики.
  - Дальность активации (как у Paper) включена: мобы и стойки для брони дальше 16–48 блоков от игроков тикают раз в
    20 тиков, пока их ничто не будит (падают, в воде, их бьют, у них цель). Всё, что не моб и не стойка (снаряды
    Incoming, контрапции Create, самолёты, снаряды пушек, предметы), из проверок исключено: у типа по умолчанию
    `tick-interval: 1`.
  - Динамика включена: когда тик дольше 35 мс, сервер сужает дальность тика чанков и симуляции до 6 чанков и лимит
    мобов до 50 %, а когда нагрузка спадает, возвращает 8 чанков и 100 %. 8 — дальность симуляции, под которую
    рассчитаны настройки сборки (`simulation-distance=8` в `server.properties`). Дальность прорисовки не меняется.
  - Замер 06.10.2026 на копии мира сервера, 6 игроков в разных городах, ночь: работа потока сервера на тик −33 %
    (с одной дальностью активации — −27 %).

## Клавиши

Раскладка сборки — `config/defaultoptions/keybindings.txt`, её делает раскладкой по умолчанию мод Default Options:
новый экземпляр получает её на первом запуске, «Сброс» у клавиши и «Сбросить настройки» в «Назначении клавиш»
возвращают её. В уже стоящем экземпляре клавиши, которые записаны в его `options.txt`, Default Options не меняет:
Minecraft пишет туда все клавиши, и «не менял» от «выбрал то же» не отличить. Туда раскладку один раз ставит
«Настройки → Управление → Назначение клавиш → Сбросить настройки» (свои переназначения при этом сбрасываются).
Клавиши из файла, которых в `options.txt` ещё нет (новый мод сборки), Default Options ставит сам. Файл перечисляет все
клавиши, кроме JEI: обновление мода со своими новыми клавишами раскладку не сдвинет.

Правила: клавиши Minecraft на своих местах; одна клавиша — одно действие там, где оба могли бы сработать; в разных
режимах клавиша значит одно и то же (X — выйти, Ctrl — быстрее, ЛКМ — огонь); что не нужно в этой игре — без клавиши.
NeoForge запускает все действия на нажатой клавише разом, поэтому до раскладки R (эмоции Essential, высадка из
самолёта, перезарядка) или B (гардероб Essential, ракеты самолёта, режим огня, новая метка) срабатывали вместе.

| клавиша | действие |
|---|---|
| ЛКМ | атака; в бинокле пульта — пуск (Incoming); на лафете — выстрел пушки (Big Cannons) |
| R | перезарядка (Point Blank); у пульта Tweaked Controllers — мышь в центр |
| B, V, Y | режим огня, смена прицела, обвесы (Point Blank); Alt+Y — его настройки |
| G | действие с блоком, пока в руках оружие и ПКМ занят прицелом (Point Blank) |
| C | приближение (Essential) |
| X | выйти: из самолёта (Immersive Aircraft, было R), из пульта Tweaked Controllers (было Tab; Esc тоже) |
| Ctrl | бег; в самолёте — разгонные ракеты (было B) |
| Пробел, Shift | прыжок и красться; в самолёте — тяга, вверх и вниз |
| H (держать) | мышь как джойстик пульта Tweaked Controllers (было левый Alt — он у ящиков Create); в самолёте — свободный обзор (Immersive Aircraft, было левый Alt) |
| \` (Ё) | лафет Big Cannons: наведение или движение (было C); в самолёте — руль мышью вкл/выкл (Immersive Aircraft) |
| левый Alt | меню схематики и ящики с инструментами (Create) |
| Tab | список игроков; с творческим посохом физики — режим вращения (клавиша из его подсказки) |
| M, ], [ | карта мира, её настройки, настройки мини-карты (было Y) |
| U, J, Z, Num + | список меток, новая метка (было B), большая мини-карта, метка на месте (Xaero) |
| N, «,», «.» | камера снаряда, карта наведения, второй ключ (Incoming) |
| I | наклон камеры вместе с аппаратом (Aeronautics Camera Sync) |
| K, O | шейдеры вкл/выкл, выбор шейдеров (Iris) |
| Shift, Num 0, Num 1 | Jade: подробности, настройки, показ подсказки |

Без клавиши нарочно: у Essential — друзья (было H), гардероб (B), скриншоты (I), подглядеть чат (Z), быстрое меню (G),
эмоции (R): всё это есть в его меню; сохранить и загрузить панель творческого режима (C, X); перезагрузка шейдеров
Iris (R, нужна авторам шейдеров); осмотр оружия Point Blank (I, только анимация); у Jade — жидкости, рецепты и
применения (есть в JEI) и диктор (Num 2–5). Остальное — как в Minecraft (WASD, E, Q, T, /, F, L, P, F2, F5, F11, 1–9)
и у модов: правый Shift — подтверждение в окнах Xaero, в окне рецептов JEI — R, U, A и мышь.

Красными в «Управлении» нарочно остаются пары: NeoForge подсвечивает клавишу у двух действий, даже когда у них разные
контексты и вместе они не срабатывают (`KeyMapping.same` в конце сравнивает сами клавиши). Это JEI в окне рецептов
(A, R, U, мышь), «Размышление» Create (W над предметом в инвентаре) и его копии Shift, Ctrl и Alt, «Подробности» Jade и
свободная камера джойстика Aeroworks (Shift), Alt+Y Point Blank рядом с Alt Create и ЛКМ, R, X, Ctrl, Tab, H и \` из
таблицы.

Default Options стоит только у игроков. Его библиотека Balm стоит и на сервере: Balm регистрирует свой канал
`balm:config_sync` обязательным, а NeoForge не пускает игрока, если обязательного канала нет у сервера
(`NetworkComponentNegotiator`).

## Как обновлять (для сопровождающих)

- Мод с Modrinth: `packwiz update <имя>` (или `packwiz modrinth add --project-id <slug> --version-id <id>` — точная
  версия), потом `packwiz refresh`. CI проверяет, что `index.toml` свежий.
- Новый выпуск Incoming:
  `packwiz url add Incoming https://github.com/zentixua/airstrike/releases/download/v<версия>/airstrike-<версия>.jar --meta-name airstrike`.
  Almighty — так же, `almighty-<версия>.jar` из его [выпусков](https://github.com/zentixua/almighty/releases),
  `side = "server"`.
- Версию сборки (`version` в `pack.toml`) поднимать при каждом изменении состава, чтобы у всех игроков был один набор,
  и добавлять её строку в таблицу [Versions](README.md#versions) в README.md (на английском). CI берёт оттуда заметки
  релиза, и без строки проверка PR не пройдёт. Новую версию на `main` CI выпускает как `v<версия>` с `.mrpack` и
  экземпляром Prism.
- `pack/` с `main` игроки ставят при следующем запуске игры, поэтому в `main` — только проверенное CI и только jar
  Incoming из уже вышедших выпусков.
- Файлы руками: `go install github.com/packwiz/packwiz@v0.0.0-20260906154125-ef87d964f8cb`, потом
  `cd pack && packwiz mr export` (→ `Incoming Pack-<версия>.mrpack`) и `python3 tools/prism_instance.py`
  (→ `dist/Incoming-Pack.zip`).

## Лицензия

Файлы этого репозитория — под [GPL-3.0-only](LICENSE). Моды и шейдерпак, которые скачивает сборка, — под своими
лицензиями.

## Версии

История версий — в таблице [Versions](README.md#versions) английского README.
