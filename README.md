# krp-modpack

Состав модпака **Kingdom RP** (Minecraft 1.21.1 + NeoForge 21.1.233) — сторонние
моды поверх основного мода [`krp-mod`](https://github.com/Kingdom-RP/krp-mod).

## Как это устроено

- **Сами jar'ы НЕ в git** — они заливаются в **[Releases](../../releases)** этого
  репозитория (тег `v1`). В git состав не хранится: источник истины — список
  ассетов Release.
- Сборщик дистрибутива лаунчера (`krp-launcher/builder`, флаг `--mods-release
  Kingdom-RP/krp-modpack --mods-tag v1`) читает ассеты Release через GitHub API,
  скачивает каждый `.jar`, считает SHA-256 и вписывает в `manifest.json` с url
  ассета. Лаунчер качает моды напрямую отсюда в `mods/` игрока и сверяет хеш.

## Рабочий цикл (добавить/обновить мод)

1. Положить нужные jar'ы в локальную папку `mods/` (она в `.gitignore`).
2. Залить их в Release `v1`:
   ```
   gh release upload v1 mods/*.jar --clobber
   ```
   (или перетащить файлы в Release через веб-интерфейс GitHub).
3. Дальше — **автоматически**: публикация/обновление Release триггерит пересборку
   дистрибутива в `krp-mod` (workflow `Publish modpack`), новый `manifest.json`
   уезжает на Pages, лаунчер раздаёт моды игрокам.

Никакого `modlist.toml` вести не нужно — состав определяется тем, что лежит в
Release.

## ⚠️ Лицензии

Многие моды распространяются как **All Rights Reserved** и запрещают ре-хостинг.
Размещение их jar в этих Releases — это ре-хостинг. Проверяй условия каждого мода;
для спорных используй прямую ссылку на их CDN (Modrinth/CurseForge) через
опциональный `--modlist` сборщика (см. `krp-launcher/docs/modlist.example.toml`).
