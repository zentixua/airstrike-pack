# Incoming Pack — сборка NeoForge 1.21.1 под мод Incoming (прежде Airstrike Pack и Airstrike)

Своя сборка «Incoming Pack» в [packwiz](https://packwiz.infra.link/): Create, Create Aeronautics, Incoming, Distant
Horizons, шейдеры, запись повторов. До 0.1.24 сборка называлась Airstrike Pack (репозиторий zentixua/airstrike-pack),
мод до 3.0.0 — Airstrike (zentixua/airstrike, id `airstrike`); переименованы целиком (решение Артёма 12.10.2026).
Старые адреса GitHub ведут на новые, экземпляры со старым адресом сборки обновляются дальше.
Что в ней и как её ставить — README.md (по-английски) и README.ru.md (русский перевод). Репозиторий выделен из
zentixua/incoming вместе с историей: номера PR до переезда — `zentixua/incoming#N`. Мод —
https://github.com/zentixua/incoming, мост ведущего Almighty (только сервер) — https://github.com/zentixua/almighty.

## Люди и правила
- Автор/хост: **Артём** (GitHub zentixua, в игре ZentixUA). Друзья: **ENOTzRPG**, **WallyFillmark**. Общаемся
  по-русски, кратко и по делу.
- **`pack/` на `main` — живая сборка**: экземпляры игроков с автообновлением (Prism, packwiz-installer перед каждым
  запуском) и выделенный сервер ставят её с `main` при следующем запуске. В `main` — только проверенное CI и только jar
  из уже вышедших выпусков (PR сборки — после выпуска мода).
- Публичные тексты — по-английски: README.md, docs/store (поля и описание для Modrinth и CurseForge), заметки релиза,
  описания необязательных модов; README.ru.md — русский перевод, правится в том же PR. Пишем как человек: просто,
  точно, без рекламных слов, повторов и воды; каждое утверждение сверено с файлами сборки.
- Сборка чистая и надёжная: стандартные механизмы, без обходов; настройки чужих модов — по умолчанию, а каждый файл
  в `config/` — с причиной в README.md и README.ru.md. Новый обязательный мод — только по слову Артёма.
- Версию (`version` в `pack.toml`) поднимать при каждом изменении состава и строку в таблицу `## Versions` README.md
  (по-английски; таблица только там, README.ru.md на неё ссылается): у всех игроков один набор.
- Выпуск на GitHub — сам: новая `version` на `main` выходит релизом `v<version>` (Артём, 09.10.2026: «нормально сборку
  опубликуй» — скачивание без входа по `releases/latest/download/…`; с 0.1.24 файлы — `Incoming-Pack.zip` и
  `Incoming-Pack.mrpack`, до того — `Airstrike-Pack.*`). Остальные публичные шаги (Modrinth и др.) — только по прямому
  слову Артёма.
- Готовый PR вливать самому после зелёного CI, чистого независимого ревью и вердикта координатора.

## Где что лежит
```
pack/                    ← сама сборка: pack.toml (версии игры и NeoForge), index.toml, mods/*.pw.toml и
                           shaderpacks/*.pw.toml (откуда скачать и хеш), config/ (кладётся в игру как есть)
tools/prism_instance.py  ← экземпляр Prism, который ставит pack/ с main сам при каждом запуске (Incoming-Pack.zip)
README.ru.md, docs/store/ ← русский README; поля и описание для Modrinth и CurseForge
LICENSE                  ← GPL-3.0-only (файлы репозитория; моды — со своими лицензиями)
tools/pack_dir.py        ← каталог игры из pack/ без Prism (моды по хешам) — для проверок клиентом Incoming
.github/workflows/build.yml  ← CI «Сборка модов»: индекс свежий, .mrpack и экземпляр Prism (артефакт incoming-pack);
                               новая version на main — релиз v<version> с ними (заметки — строка README.md
                               в разделе «## Versions»)
```
Адрес сборки для packwiz-installer: `https://raw.githubusercontent.com/zentixua/incoming-pack/main/pack/pack.toml`.
Сборка лежит в `pack/`, а не в корне: файлы репозитория (README, CI, инструменты) не попадают в индекс и в игру.

## Рабочий цикл
```sh
go install github.com/packwiz/packwiz@v0.0.0-20260906154125-ef87d964f8cb
cd pack && packwiz update <имя>           # мод с Modrinth; точная версия — packwiz modrinth add --project-id … --version-id …
cd pack && packwiz refresh                # индекс с хешами — тем же коммитом (CI проверяет)
cd pack && packwiz mr export              # .mrpack — разовая установка без автообновления
python3 tools/prism_instance.py           # dist/Incoming-Pack.zip — новый экземпляр с автообновлением
```
Новый выпуск Incoming: `packwiz url add Incoming https://github.com/zentixua/incoming/releases/download/v<версия>/incoming-<версия>.jar --meta-name incoming`;
Almighty — так же, `almighty-<версия>.jar` из выпуска https://github.com/zentixua/almighty, `side = "server"`.

## Подводные камни
- GitHub отдаёт raw-файлы с кэшем до 5 минут: сразу после слияния `pack.toml` и `index.toml` могут быть из разных
  коммитов — packwiz-installer скажет про неверный хеш индекса («Quit», запустить позже). Сервер перезапускать через
  ~5 минут после слияния.
- `side` у модов должен быть верным: сервер ставит сборку с `-s server`, `client` он не ставит; мод только для сервера
  (Almighty) — `side = "server"`, игрокам он не скачивается.
- packwiz-installer удаляет только то, что ставил сам: экземпляр из `.mrpack` переводят на автообновление, убрав
  `mods` в сторону (README.md), иначе старый jar с другим именем остался бы рядом с новым.
- Путь в индексе не выходит за `pack/` (packwiz-installer отбрасывает лишние `..`), а `pack.toml` несёт хеш индекса:
  сборку нельзя перенаправить на другой адрес содержимым файлов — новый адрес вписывается в «Предстартовую команду»
  экземпляра. Переименование репозитория GitHub адрес не ломает: raw.githubusercontent.com отдаёт файлы и по старому
  имени (проверено 04.10.2026).
- Прежний адрес сборки в репозитории мода (`zentixua/airstrike/main/pack/pack.toml`, замороженная копия) убран в
  zentixua/incoming#256: экземпляр с ним не обновляется — поставить заново из `Incoming-Pack.zip`.
- Мод до 3.0.0 — `mods/airstrike.pw.toml` (jar `airstrike-<версия>.jar`, id `airstrike`). Первый PR сборки с 3.0.0:
  `packwiz remove airstrike`, затем `packwiz url add` выше, а в `config/entityculling.json` и README (оба) —
  сущности `incoming:` вместо `airstrike:`. Мир сервера: `serverconfig/airstrike-server.toml` переименовать в
  `incoming-server.toml` до запуска (заметки к выпуску 3.0.0 мода).
