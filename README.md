# Перезаливатор
Приложение для "перезаливки" баз данных 1С:Предприятия

## Описание
Под словом "перезалить" понимается процедура восстановления одной базы данных из резервных копий другой базы данных.
Например, если необходимо загрузить данные из базы продуктива в тестовую или разработочную.

Перезаливатор позволяет максимально автоматизировать процесс "перезаливки" баз данных 1С:Предприятия. 

Имеется GUI-интерфейс для выбора базы-приемника и базы-назначения, а также окно с выводом результата:

<img src="https://github.com/Tavalik/Perezalivator/raw/master/Screenshots/Perezalivator1.png" alt="Скриншот1">

## Установка

### Зависимости  

Необходима библиотека **json**: https://github.com/oscript-library/json

Установка:
``` cmd
opm install json
```

Необходима библиотека **gui**: https://github.com/oscript-library/oscript-simple-gui

Установка:
``` cmd
opm install gui
```

Необходим набор библиотек **TLib** : https://github.com/Tavalik/OS_TScripts/tree/master/Tlib

Установка копированием каталога Tlib в каталог библиотек (по умолчанию в C:\Program Files (x86)\OneScript\lib)

### Установка приложения

Установка копированием файлов текущего репозитория.

## Описание и работа с приложением

Запуск приложения осуществляется запуском файла **Perezalivator_Run.bat**
При первом запуске в текущем каталоге будет создан пустой файл настроек **Perezalivator_Params.json**. 

Необходимо заполнить все необходимые параметры, описав возможные базы-источники, базы-назначения и параметры для отправки электронных писем.

Проверить корректность введенных настроек можно запустив файл **Perezalivator_Run_Test.bat**. Перезаливатор будет запущен в режиме тестирования настроек.

При следующем запуске файла **Perezalivator_Run.bat** откроется окно, в котором необходимо выбрать базу-источник:

<img src="https://github.com/Tavalik/Perezalivator/raw/master/Screenshots/Perezalivator2.png" alt="Скриншот2">

базу-назначения:

<img src="https://github.com/Tavalik/Perezalivator/raw/master/Screenshots/Perezalivator3.png" alt="Скриншот3">

Если необходимо, можно указать дату, на которую необходимо получить данные (всегда используется конец дня).

После указания всех исходных параметров, перезаливатор начнет работу по следующему алгоритму:

1. Установка блокировки регламентных заданий и начала сеансов в базе-приемнике
2. Завершение активных сеансов (спустя несколько минут) в базе-приемнике
3. Расчет последовательности файлов резервных копий для **базы-источника** для восстановления на указанную дату
4. Восстановление базы-приемника по найденной последовательности файлов
5. Перевод базы-приемника в простую модель восстановления
6. Сжатие файлов журнала транзакций базы-приемника
7. Отключение базы-приемника от хранилища
8. Подключение базы-приемника к хранилищу
9. Обновление конфигурации базы данных базы-приемника
10. Снятие блокировки регламентных заданий и начала сеансов базы-приемника
11. Уведомление о результате по электронной почте

Если в базе-приемнике присутствуют активные соединения, будет показана таблица со всеми соединениями, а также будет предоставлена возможность завершить все активные сеансы.

<img src="https://github.com/Tavalik/Perezalivator/raw/master/Screenshots/Perezalivator4.png" alt="Скриншот4">

Отработав, Перезаливатор выдаст соответствующее сообщение (или сообщение об ошибке), а также отправит сообщение о результате работы на электронную почту.
