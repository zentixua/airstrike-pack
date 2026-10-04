# Airstrike Pack — сборка NeoForge 1.21.1 под мод Airstrike

Своя сборка «Airstrike Pack» в [packwiz](https://packwiz.infra.link/): Create, Create Aeronautics, Airstrike, Distant
Horizons, шейдеры, запись повторов. Что в ней и как её ставить — README.md. Репозиторий выделен из zentixua/airstrike
вместе с историей: номера PR до переезда — `zentixua/airstrike#N`. Мод — https://github.com/zentixua/airstrike,
мод ведущего (только сервер) — пока там же (`mod/gm`, выходит в выпусках Airstrike).

## Люди и правила
- Автор/хост: **Артём** (GitHub zentixua, в игре ZentixUA). Друзья: **ENOTzRPG**, **WallyFillmark**. Общаемся
  по-русски, кратко и по делу.
- **`pack/` на `main` — живая сборка**: экземпляры игроков с автообновлением (Prism, packwiz-installer перед каждым
  запуском) и выделенный сервер ставят её с `main` при следующем запуске. В `main` — только проверенное CI и только jar
  из уже вышедших выпусков (PR сборки — после выпуска мода).
- Сборка чистая и надёжная: стандартные механизмы, без обходов; настройки чужих модов — по умолчанию, а каждый файл
  в `config/` — с причиной в README.md. Новый обязательный мод — только по слову Артёма.
- Версию (`version` в `pack.toml`) поднимать при каждом изменении состава и строку в таблицу версий README.md:
  у всех игроков один набор.
- Публичные шаги (выпуск сборки на Modrinth, публикация) — только по прямому слову Артёма.
- Готовый PR вливать самому после зелёного CI, чистого независимого ревью и вердикта координатора.

## Где что лежит
```
pack/                    ← сама сборка: pack.toml (версии игры и NeoForge), index.toml, mods/*.pw.toml и
                           shaderpacks/*.pw.toml (откуда скачать и хеш), config/ (кладётся в игру как есть)
tools/prism_instance.py  ← экземпляр Prism, который ставит pack/ с main сам при каждом запуске (Airstrike Pack.zip)
tools/pack_dir.py        ← каталог игры из pack/ без Prism (моды по хешам) — для проверок клиентом Airstrike
.github/workflows/build.yml  ← CI «Сборка модов»: индекс свежий, .mrpack и экземпляр Prism (артефакт airstrike-pack)
```
Адрес сборки для packwiz-installer: `https://raw.githubusercontent.com/zentixua/airstrike-pack/main/pack/pack.toml`.
Сборка лежит в `pack/`, а не в корне: файлы репозитория (README, CI, инструменты) не попадают в индекс и в игру.

## Рабочий цикл
```sh
go install github.com/packwiz/packwiz@v0.0.0-20260906154125-ef87d964f8cb
cd pack && packwiz update <имя>           # мод с Modrinth; точная версия — packwiz modrinth add --project-id … --version-id …
cd pack && packwiz refresh                # индекс с хешами — тем же коммитом (CI проверяет)
cd pack && packwiz mr export              # .mrpack — разовая установка без автообновления
python3 tools/prism_instance.py           # dist/Airstrike Pack.zip — новый экземпляр с автообновлением
```
Новый выпуск Airstrike: `packwiz url add Airstrike https://github.com/zentixua/airstrike/releases/download/v<версия>/airstrike-<версия>.jar --meta-name airstrike`;
мод ведущего — так же, `airstrike-gm-<версия>.jar` из того же выпуска, `side = "server"`.

## Подводные камни
- GitHub отдаёт raw-файлы с кэшем до 5 минут: сразу после слияния `pack.toml` и `index.toml` могут быть из разных
  коммитов — packwiz-installer скажет про неверный хеш индекса («Quit», запустить позже). Сервер перезапускать через
  ~5 минут после слияния.
- `side` у модов должен быть верным: сервер ставит сборку с `-s server`, `client` он не ставит; мод только для сервера
  (мод ведущего) — `side = "server"`, игрокам он не скачивается.
- packwiz-installer удаляет только то, что ставил сам: экземпляр из `.mrpack` переводят на автообновление, убрав
  `mods` в сторону (README.md), иначе старый jar с другим именем остался бы рядом с новым.
- Путь в индексе не выходит за `pack/` (packwiz-installer отбрасывает лишние `..`), а `pack.toml` несёт хеш индекса:
  сборку нельзя перенаправить на другой адрес содержимым файлов — новый адрес вписывается в «Предстартовую команду»
  экземпляра. Переименование репозитория GitHub адрес не ломает: raw.githubusercontent.com отдаёт файлы и по старому
  имени (проверено 04.10.2026).
- Прежний адрес `zentixua/airstrike/main/pack/pack.toml` отдаёт сборку, замороженную на переезде, пока все экземпляры
  (игроки и сервер) не переведены на новый; убирать его — только по слову Артёма.
