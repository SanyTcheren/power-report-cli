# CLI для генерации Excel-отчета по ээ
## guide
 Для создания отчета:
 1. Снимите профиль мощности со стчетчика электрической энергии СЭТ-4М, переименуйте его в `power.txt` и поместите в домашнюю папку пользователя вашей ОС
 2. Сконфигурируйте общие данные необходимые для отчета:
 ```sh
 node report -y 2021 
 node report -m 11
 node report -d "УРАЛМАШ 5000/320 ЭК-БМЧ" 14938
 node report -f "Тепловское" 108
 ```
 3. Задайте данные по скважинам:
 ```
 node report -w 2011Г 2021-10-10 12 2021-11-05 10
 node report -p 1903Г 2021-11-05 10 2021-11-05 22
 node report -w 1903Г 2021-11-05 22 2021-11-25 16
 node report -p 8888Г 2021-11-25 16 2021-11-26 02
 node report -w 8888Г 2021-11-26 02 
 ```
 4. Проверьте введеные данные:
 ```
 node report -v
 ```
- если необходимо изменить общие данные, то просто воспользуйтесь соответствующей командой ещё раз
- если необходимо изменить данные по скважинам то сначала выполните чистку, а потом повторно введите данные
``` 
node report -c 
``` 
5. Если данные верны, то сгенерируйте отчет
```
node report 
```
6. Заберите файл отчета `report.xlsx` из домашней папки пользователя вашей ОС 
---
## cli
- `node report` - создание отчета на основе установленных данных
- `node report -h` - вывод справки
- `node report -e` - показывает пример использования cli
- `node report -v` - показывает установленные данные для отчета
- `node report -c` - очищмет данные по скважинам
- `node report -d [type] [number]` - установка типа и номера буровой установки
- `node report -f [field] [bush]` - установка месторождения и номера куста
- `node report -m [month]` - установка отчетного месяца
- `node report -y [year]` - установка отчетного года
- `node report -w [number] [s-date] [s-hours] [f-date] [f-hours]`- добавление в отчет скважины 
- `node report -p [number] [s-date] [s-hours] [f-date] [f-hours]`- добавление в отчет пзр скважины
---
## архитектура
 - данные для создания отчета будем хранить в пользвательской папке в файле `power-report.json`
 - файл для шаблона будем хранить в рабочем пакете - `template.xsls`
 - файл с профилем мощнсти будем хранить в пользовательской ппке `power.txt`
 - итоговый файл будем создавать в пользовательской директории, название `report.xlsx`
 - создадим сервисы:
	- `log.service.js` - для логирования и работы с консолью
	- `storage.service.js` - для работы с файлом данных
 - создадим хелперы для:
	- работы с xls  файлами - `excelHelper.js`
	- работы с датой - `timeHelper.js`
 - будем использовать следующие пакеты:
	- exceljs - для работы с exel
	- moment.js - для работы с датой
	- chalk, dedent-js - для работы с текстом
	- yargs - для парсинга аргументов командной строки

