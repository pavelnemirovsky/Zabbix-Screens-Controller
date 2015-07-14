# Zabbix-Screens-Controller
Makes screens navigation easy

http://zabbix.local/www10.local&nginx -> http://zabbix.local/screens.php?elementid=55&hostid=10713&groupid=35

Скрипт призван упростить навигацию по динамическим скринам Zabbix'а.
Если у вас несколько сотен/тысяч хостов/групп и на каждый хост существует стандартный скрин, то, вероятнее всего, вы укликивались до смерти: выбрать скрин, группу, хост; а также огребали проблемы, когда нужно рассказать, как посмотреть те или иные графики человеку, который Zabbix видит первый раз.

### Как работает
 * Nginx получает 404 ошибку, редиректит запрос в zabbix_screens_controller.php
 * zabbix_screens_controller.php парсит строчку из URL и редиректит на нужный скрин

### Что нужно сделать, чтобы использовать у себя
 * Убедитесь, что ./conf/zabbix.conf.php доступен для чтения (или поменяйте путь), в конфиге должны быть логин-пароль под которым веб-морда ходит в базу;
 * Отредактируйте $map_screens, прописав свои скрины;
 * Если необходимо - поправьте $map_shorts, для ещё более быстрого доступа к нужным скринам;
 * Положите zabbix_screens_controller.php в директорию с web интерфейсом zabbix;
 * Пропишите директиву error_page 404 в конфиге nginx, чтобы все неизвестные запросы роутились в наш скрипт.

### Debug
Используйте аргумент &debug на случай, если что-то идёт не так (редирект не происходит, или урл формируется не валидный)

### Алиасы / дополнительные хосты
На случай, если в вашей инфраструктуре встречается несколько доменных имён, ведущих на один сервер (при условии, что дополнительные записи в zabbix не заведены), предусмотрена карта таких исключений: $rewrites

### Примеры
 * http://zabbix.local/www1.local - редирект на стандартный Linux скрин для хоста www1.local
 * http://zabbix.local/www1.local&nginx - редирект на скрин Nginx для того же хоста
 * http://zabbx.local/db1.local&m - редирект на скрин MySQL Performance (в виде короткой записи) для хоста db1.local

P.S. Если в [/etc/resolv.conf](http://linux.die.net/man/5/resolv.conf) на Zabbix сервере будут указаны нужные domain и search, все записи можно сократить до:
 * http://zabbix/www1
 * http://zabbix/www1&nginx
 * http://zabbx/db1&m
