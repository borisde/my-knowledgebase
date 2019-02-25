# Шпаргалка по установке Nginx, PHP 7, MySQL на Ubuntu 18 (bionic)

## Обновление пакетов

Приконнектимся к нашему серверу по SSH и обновим список пакетов:

```
sudo apt update
```

Установим текстовый редактор Vim, если еще не установлен:

```
sudo apt install vim
```


## Установка Nginx

```
sudo apt install nginx
```

Проверяем версию и статус nginx:

```
nginx -v
```

```
systemctl status nginx
```

Результат:

![Установка nginx](screenshots/ustanovka-nginx.jpg "Установка nginx")

```vim
Основные комманды для запуска/остановки/перезапуска сервера:
sudo systemctl start nginx
sudo systemctl stop nginx
sudo systemctl restart nginx 
sudo systemctl enable nginx
```

## Установка PHP 7

Добавим в систему репозиторий с последней версией PHP:

```
sudo add-apt-repository ppa:ondrej/php
```

```
sudo apt update
```

На момент написания мануала последняя версия PHP - 7.3, поэтому установим связку `php-fpm` с помощью команды `php7.*-fpm`, где `*` - нужная версия PHP:

```
sudo apt install php7.3-fpm
```

> Примечание: если выполнить команду `sudo apt install php7.3`, то по умолчанию будет установлен apache c библиотеками для обработки php запросов, 
а команда `php7.3-fpm` устанавливает `php-fpm` без apache.

Проверим версию PHP:

```
php -v
```

Результат:

![Установка PHP 7.3](screenshots/ustanovka-php-7-3.jpg "Установка PHP 7.3")

Установим различные полезные расширения для PHP 7.3:

```
sudo apt install php7.3-cli php7.3-json php7.3-pdo php7.3-mysql php7.3-zip php7.3-mbstring php7.3-curl php7.3-xml 
```

Перезагрузим nginx:

```
sudo systemctl restart nginx
```

Для безопасности нужно раскоментировать строку и установить `cgi.fix_pathinfo=0` в файле конфига PHP `/etc/php/7.3/fpm/php.ini`.

Это можно сделать следующей коммандой:

```
sudo sed -i 's/;cgi.fix_pathinfo=[1|0]/cgi.fix_pathinfo=0/g' /etc/php/7.3/fpm/php.ini
```

```
sudo systemctl restart php7.3-fpm
```

![Установка cgi.fix_pathinfo](screenshots/cgi-fix-pathinfo.jpg "Установка cgi.fix_pathinfo")


#### Проверка работы PHP

Nginx при установке создал сайт с приветствием, изменим дефолтный конфиг этого сайта для проверки работы PHP 7:

```
sudo vim /etc/nginx/sites-available/default
```

Очистим файл:

```vim
:1,$d <Enter>
```

Включим режим добавления в редакторе Vim:

```vim
a
```

И добавим туда простой nginx конфиг:

```nginxconf
server {
        listen 80 default_server;
        listen [::]:80 default_server;

		#корневая директория где хранятся файлы сайта
        root /var/www/html; 

		#возможные имена индексных файлов
        index index.php index.html index.htm index.nginx-debian.html;

        server_name _;

        location / {
				try_files $uri $uri/ =404;
				
				#сначало проверяем существует ли файл из запроса $uri,
				#если нет, проверяем наличие директории $uri/ 
				#если и директория не существует, то показать ошибку 404
                
        }

        location ~ \.php$ {
				#перенаправление php запросов на обработку php7.3-fpm						    
                fastcgi_pass unix:/var/run/php/php7.3-fpm.sock;
                fastcgi_split_path_info ^(.+\.php)(/.*)$; 
                include fastcgi_params;
                
                fastcgi_param SCRIPT_FILENAME $realpath_root$fastcgi_script_name;
                fastcgi_param DOCUMENT_ROOT $realpath_root;        
        }
}
```

Сохраним файл:

```vim
<Esc> :wq <Enter>
```

Перезапустим nginx:	

```
sudo systemctl restart nginx
```

Файлы сайта с приветствием лежат по адресу `/var/www/html`. Создадим в этой папке `phpinfo.php` для проверки работы PHP 7:

```
sudo vim /var/www/html/phpinfo.php
```

Добавим в файл:

```php
<?php
phpinfo();
```

Введем в браузере <ip_адресс_сервера>/phpinfo.php:

![Проверка работы PHP 7.3](screenshots/proverka-raboty-php-7-3.jpg "Проверка работы PHP 7.3")

Удалим созданный файл phpinfo.php:

```
sudo rm /var/www/html/phpinfo.php
```

Если дефолтный хост nginx больше не нужен, то лучше его отключить, чтобы не было конфликтов:

```
sudo rm -f /etc/nginx/sites-enabled/default  
```

```
sudo systemctl restart nginx
```


## Установка MySQL

```
sudo apt install mysql-server
```

Проверка версии:

```
mysql --version
```

Результат:

![Установка MySQL 5.7](screenshots/ustanovka-mysql-5-7.jpg "Установка MySQL 5.7")

Изменим дефолтные настройки для повышения безопасности MySQL:

```
sudo mysql_secure_installation
```

Во время выполнения нужно ответить на вопросы:

```
Would you like to setup VALIDATE PASSWORD plugin? -- Установить плагин для валидации новых паролей?
Change the root password? -- Изменить пароль для root? (начиная с MySQL 5.7 сначало нужно изменить способ авторизации root пользователя)
Remove anonymous user? -- Удалить анонимного пользователя?
Disallow root login remotely? -- Отключить удаленный доступ для root пользователя? 
Remove test database and access to it? -- Удалить тестовую базу?
Reloade privilege tables now? -- Перезагрузить таблицы доступа?
```


В версии MySQL 5.7 по умолчанию для root пользователя включена аутентификация с помощью плагина `auth_socket`

![Изменение метода аутентификации MySQL 5.7](screenshots/metod-autentifikacii-mysql.jpg "Изменение метода аутентификации MySQL 5.7")

Этот плагин не требует пароль, он только проверяет что пользователь коннектится через UNIX сокет и имя этого пользователя. 
Это хорошо для безопасности, но если используется phpMyAdmin или другая программа для удаленного доступа необходимо изменить метод аутентификации с `auth_socket` на `mysql_native_password` для `root` пользователя или создать нового пользователя с нужными правами.

#### Изменение метода аутентификации с `auth_socket` на `mysql_native_password` для `root`

```
sudo mysql
```

Команда для проверки текущего метода аутентификации:

```
SELECT user,authentication_string,plugin,host FROM mysql.user;
```

Команда для изменения метода аутентификации и установка пароля:

```
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'TestPassword';
```

```
FLUSH PRIVILEGES;
quit;
```

> Если был установлен плагин для валидации MySQL паролей на предыдущем шаге, то в зависимости от его настроек потребуется создавать более сложные пароли.  

Зайдем в консоль MySQL по паролю и проверим:

```
mysql -u root -p
```

```
SELECT user,authentication_string,plugin,host FROM mysql.user;
```

Результат:

![Изменение метода аутентификации MySQL 5.7](screenshots/metod-autentifikacii-mysql2.jpg "Изменение метода аутентификации MySQL 5.7")


#### Создание нового пользователя MySQL

Заходим в панель MySQL под root'ом. Если включена авторизация по паролю, нужно использовать команду
`mysql -u root -p`, если по плагину `auth_socket`, то достаточно `sudo mysql`

Создаем нового пользователя:

```sql
CREATE USER 'test_user'@'localhost' IDENTIFIED BY 'TestPassword';
```

>`test_user` - логин
>`localhost` - имя хоста
>`TestPassword` - пароль

Создадим тестовую базу:
 
```sql
CREATE DATABASE test;
```

Дадим новому юзеру привилегии на тестовую базу:

```sql
GRANT ALL PRIVILEGES ON test.* TO 'test_user'@'localhost';
```

> `GRANT ALL PRIVILEGES ON *.*` - эта команда дает **все** права на **<имя_базы> . <имя_таблицы>**  

```sql
FLUSH PRIVILEGES;
```

Выходим из панели MySql:

```sql
quit
```

Проверим нового юзера:

```
mysql -u test_user -p
```

```sql
show databases;
```

Результат:

![Проверка работы MySQL](screenshots/proverka-mysql.jpg "Проверка работы MySQL")

#### Создание нового хоста nginx и пример скрипта работы из PHP с MySQL


Перейдем в `/var/www/` и создадим там новую директорию:

```
cd /var/www/
```

```
sudo mkdir test_site
```

Создаем nginx конфиг для нового сайта:

```
sudo vim /etc/nginx/sites-available/testsite.conf
```

И добавляем туда базовый конфиг:

```nginxconf
server {
    listen 80 default_server;
    listen [::]:80 default_server;

    #корневая директория где хранятся файлы сайта
    root /var/www/test_site; 

    index index.php;
    
    server_name _;

    location / {
        try_files $uri $uri/ =404;                
    }

    location ~ \.php$ {			    
        fastcgi_pass unix:/var/run/php/php7.3-fpm.sock;
        fastcgi_split_path_info ^(.+\.php)(/.*)$; 
        include fastcgi_params;
                
        fastcgi_param SCRIPT_FILENAME $realpath_root$fastcgi_script_name;
        fastcgi_param DOCUMENT_ROOT $realpath_root;
    }
}
```

![Создание нового хоста nginx](screenshots/sozdanie-hosta-nginx.jpg "Создание нового хоста nginx")

Сохраним файл:

```vim
<Esc> :wq <Enter>
```

Активируем новый хост nginx:

```
sudo ln -s /etc/nginx/sites-available/testsite.conf /etc/nginx/sites-enabled/
```

Перезагрузим nginx:

```
sudo systemctl restart nginx
```

Для примера напишем простой скрипт на PHP который при заходе на страницу будет записывать в базу время, ip адрес и другую информацию о визите и сразу же выводить последние 5 записей из таблицы.

Зайдем в консоль MySQL под нашим тестовым юзером и создадим структуру таблицы:

```
mysql -u test_user -p
```  

Выберем тестовую базу:

```sql
USE test;
```

Создаем таблицу `visitors` со следующей структурой:

```sql
CREATE TABLE `visitors` (
  `id` int(8) unsigned NOT NULL AUTO_INCREMENT,
  `ip_address` varbinary(16) NOT NULL DEFAULT '',
  `user_agent` varchar(255) CHARACTER SET utf8 COLLATE utf8_unicode_ci NOT NULL DEFAULT '',
  `created_at` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP,
  PRIMARY KEY (`id`)
);
```

Теперь создадим PHP скрипт на нашем тестовом хосте, который будет получать данные о визите и записывать их в таблицу:

```
sudo vim /var/www/test_site/index.php
```

Добавим туда такой код:

```php

<?php
// Получаем ip адрес из суперглобальной переменной $_SERVER
// cначало проверяем REMOTE_ADDR, если он пустой - HTTP_X_FORWARDED_FOR и HTTP_CLIENT_IP
$ip = $_SERVER['REMOTE_ADDR'] ?? $_SERVER['HTTP_X_FORWARDED_FOR'] ?? $_SERVER['HTTP_CLIENT_IP'];

// Конвертируем IPv4 или IPv6 адресс в бинарную структуру для сохранения в поле ip_address таблицы
$ip = inet_pton($ip);
if (!$ip) {
    $ip = '';
}

// Присваеваем user_agent из суперглобальной переменной $_SERVER. Если переменная не содержит данных - присваеваем пустое значение
$user_agent = $_SERVER['HTTP_USER_AGENT'] ?? '';

//Создаем соединение с базой
$user = 'test_user';
$pass = 'TestPassword';
$pdo  = new PDO('mysql:host=localhost;dbname=test', $user, $pass);

// Создаем запрос на добавление данных
$q = "INSERT INTO visitors (ip_address, user_agent) VALUES (?,?)";
$stmt = $pdo->prepare($q);
$stmt->execute([$ip, $user_agent]);

// Выбираем 5 последних визитов
$q = $pdo->query("SELECT * FROM visitors ORDER BY id DESC LIMIT 5");

// закрываем соединение с базой
$stmt = null;
$pdo = null;

$i = 1;
?>
<table border="2" cellpadding="8" cellspacing="0">
    <tbody>
    <tr>
        <td></td>
        <td>Ip address</td>
        <td>User agent</td>
        <td>Visit date</td>
    </tr>
<?php while ($row = $q->fetch()) : ?>
    <tr>
        <td><?=$i++?></td>
        <td><?=inet_ntop($row['ip_address'])?></td>
        <td><?=$row['user_agent']?></td>
        <td><?=date('d.m.Y H:i:s', strtotime($row['created_at']))?></td>
    </tr>

<?php endwhile;

```

Сохраняем файл и смотрим результат:

![Пример скрипта PHP7 и MySQL](screenshots/primer-php7-mysql.jpg "Пример скрипта PHP7 и MySQL")





