# Обновление Geometa

### Обновление Geometa происходит следующим образом:
1. Перейти на страницу **pipeline CI/CD**: `https://gitlab.gemsdev.ru/gems/geometa/-/pipelines`
2. Если необходимо, то выполнить фильтрацию по автору либо ветке.
3. Найти нужный pipeline и скачать артефакты сборки (артефакты сборки находятся в stage с именем **build**). Конкретно для Geometa - это **build:geometa**.
4. Разархивировать скачанный архив. Артефакты Geometa находятся в папке: `install\Products\Tumen`.
5. Перенести артефакты на сервер в папку для установки, например: `/home/user/install/IAS`.
6. В командной строке выполнить установку. Пример: `sudo bash /home/user/install/IAS/IASUnixDeploy -installDir=/opt/IAS -connectionString='Server=172.16.55.112;Port=5432;Database=mrvl;User Id=postgres;Password=Qwerty123$;Connection Idle Lifetime=5;Connection Pruning Interval=3; Maximum Pool Size=500;' -gHost=localhost -iasPublicOrigin=https://mrvl.gemsdev.ru -monitorPublicOrigin=https://monitor-mrvl.gemsdev.ru -importerPublicOrigin=https://importer-mrvl.gemsdev.ru -extraModule=Gems.Module.ReportLogger"`
7. Если нам необходимы доп. модули, то необходимо обратиться к справке по их установке. Как правило, необходимо просто докинуть модуль в папку модулей и перезагрузить приложение.

### Еще парочка моментов:
1. По умолчанию на сервере Geometa устанавливается по пути: `/opt/IAS`
2. Артефакты для установки Geometa - это файлы: `full_search.zip`, `release.zip`, `IASUnixDeploy`, `templates.zip`. Последний из файлов отсутствует в сборке на GitLab. Необходимо его брать из релиза.
3. Для установки некоторых модулей необходимо вносить доп. переменные в строку установки. Более подробно можно прочитать в инструкции модуля.
4. Маршрутизация внутри сервера происходит через nginx. Основной файл конфигурирования: `/etc/nginx/sites-available/isogd`
5. Если наше приложение недоступно из браузера, то стоит проверить, есть ли соответствие доменного имени и IP-адреса в файле hosts (если на DNS-сервере информация об этом отсутствует).
6. Логи приложения находятся по пути: `/var/log/gems` Основной лог - `ias.log`
7. Перезапустить основное приложение: `sudo systemctl restart Gems.Ias.ApplicationServer.service`
8. Посмотреть логи основного приложения: `sudo journalctl -fu Gems.Ias.ApplicationServer.service`