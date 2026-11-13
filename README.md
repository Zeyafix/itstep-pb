# itstep-pb

Публичный репозиторий курса IT Step — Python Bootcamp. Материалы помогают
преподавателю проводить занятия, а студенту — изучать Python и практиковаться.

## Структура

- `AGENTS.md` — описание проекта и правила работы AI-агента.
- `itstep-pb-assets/runtime/` — общие CSS и JavaScript для HTML-материалов,
  обычная папка этого репозитория.
- `lessons/` — публичные репозитории уроков, подключаемые как Git-сабмодули.
  Каждый урок может содержать заметки, HTML-теорию, проверки и публичные
  сабмодули `pa` и `hw` с заданиями.
- `groups/` — приватные репозитории групп, подключаемые как Git-сабмодули.
  Работы студентов, оценки и история обучения хранятся только в них.
- `docs/` — документация для преподавателя и разработчика, план курса и схема.
  Зашифрованный `docs/syllabus.rar` предназначен только для преподавателей.

Структура соответствует [схеме репозитория](docs/excalidraw-scheme/pb-scheme.png).
Сабмодули уроков, заданий и групп пока не подключены. GitHub Pages для runtime
пока не настроен; локальные HTML-файлы могут подключать его по относительному пути.

## Начало работы

```sh
git clone https://github.com/Zeyafix/itstep-pb.git
cd itstep-pb
```

- [Репозитории, клонирование и обновления](docs/development/repositories.md).
- [Работа преподавателя и выдача материалов студентам](docs/teacher/README.md).
- [Подключение runtime](docs/development/runtime.md).
- [Локальные скилы AI-агента](docs/development/skills.md).
- [Текущий план курса](docs/current-roadmap.md).

## License

This work is licensed under
[CC BY-NC-SA 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/) — free for
non-commercial use, with adaptations distributed under the same license.
For commercial licensing, contact the author.
