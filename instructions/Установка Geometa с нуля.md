# Установка Geometa с нуля

***В случае выбора операционной системы на сервере лучше остановиться на **CentOS**. С ней меньше всего проблем.***

### Перед установкой Geometa необходимо убедиться, что в системе присутствуют следующие программы/зависимости:
1. PostgreSQL с расширением PostGis:  
   Архив с файлами: `X:\GeoMeta\Релизы\Версия релиза\Средства установки\Linux\CentOS 7\centos_install_postgresql.tar.gz`  
   Скрипт установки: `X:\GeoMeta\Релизы\Версия релиза\Средства установки\Linux\CentOS 7\centos_install_postgresql.sh`
2. Dotnet версии 3.1:  
   Подкидываем пакет зависимостей: `sudo rpm -Uvh https://packages.microsoft.com/config/centos/7/packages-microsoft-prod.rpm`  
   Устанавливаем dotnet 3.1: `sudo yum install dotnet-sdk-3.1`
3. Nginx:  
   Архив с файлами: `X:\GeoMeta\Релизы\Версия релиза\Средства установки\Linux\CentOS 7\centos_install_nginx.tar.gz`  
   Скрипт установки: `X:\GeoMeta\Релизы\Версия релиза\Средства установки\Linux\CentOS 7\centos_install_nginx.sh`
4. Geoserver версии 2.18:  
   Архив с файлами: `X:\GeoMeta\Релизы\Версия релиза\Средства установки\Linux\CentOS 7\geoserver_2.18\centos_install_geoserver.tar.gz`  
   Скрипт установки: `X:\GeoMeta\Релизы\Версия релиза\Средства установки\Linux\CentOS 7\geoserver_2.18\centos_install_geoserver.sh`

### Шаги воспроизведения:
1. Получить необходмую версию Geometa для установки либо обновления.  
   Артефакты релиза: `X:\GeoMeta\Релизы\Версия релиза\Дистрибутивы\Products\IAS`  
   Сборка на GitLab: `https://gitlab.gemsdev.ru/gems/geometa/-/pipelines ---> Выбираем необходимый PipeLine ---> Скачиваем архив артефактов (build.geometa)`
2. Перенести артефакты на сервер, например в папку: `/home/user/install/IAS`
3. В командной строке запустить команду установки. Пример: `sudo bash /home/user/install/IAS/IASUnixDeploy -installDir=/opt/IAS -connectionString='Server=172.16.55.112;Port=5432;Database=mrvl;User Id=postgres;Password=Qwerty123$;Connection Idle Lifetime=5;Connection Pruning Interval=3; Maximum Pool Size=500;' -gHost=localhost -iasPublicOrigin=https://mrvl.gemsdev.ru -monitorPublicOrigin=https://monitor-mrvl.gemsdev.ru -importerPublicOrigin=https://importer-mrvl.gemsdev.ru -extraModule=Gems.Module.ReportLogger"`
4. Если нам необходимы доп. модули, то необходимо обратиться к справке по их установке. Как правило, необходимо просто докинуть модуль в папку модулей и перезагрузить приложение.

### Еще парочка моментов:
1. По умолчанию на сервере Geometa устанавливается по пути: `/opt/IAS`
2. Артефакты для установки Geometa - это файлы: `full_search.zip`, `release.zip`, `IASUnixDeploy`, `templates.zip`. Последний из файлов отсутствует в сборке на GitLab. Необходимо его брать из релиза.
3. Для установки некоторых модулей необходимо вносить доп. переменные в строку установки. Более подробно можно прочитать в инструкции модуля.
4. Маршрутизация внутри сервера происходит через nginx. Основной файл конфигурирования: `/etc/nginx/sites-available/isogd`
5. Если наше приложение недоступно из браузера, то стоит проверить, есть ли соответствие доменного имени и IP-адреса в файле hosts (если на DNS-сервере информация об этом отсутствует).
6. Логи приложения находятся по пути: `/var/log/gems` Основной лог - `ias.log`
7. Перезапустить основное приложение: `sudo systemctl restart Gems.Ias.ApplicationServer.service`
8. Посмотреть логи основного приложения: `sudo journalctl -fu Gems.Ias.ApplicationServer.service`