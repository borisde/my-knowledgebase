# Шпаргалка по установке nginx, PHP 7.3, MySQL, RabbitMQ на Ubuntu 18

## Обновление пакетов

Приконнектимся к нашему серверу по SSH и обновим список пакетов:

```
sudo apt update
```

Установим текстовый редактор Vim, если еще не установлен:

```
sudo apt install vim
```


## Установка nginx

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

![Установка nginx](2.jpg "Установка nginx")

```vim
Основные комманды для запуска/остановки/перезапуска сервера:
sudo systemctl start nginx
sudo systemctl stop nginx
sudo systemctl restart nginx 
sudo systemctl enable nginx
```

## Установка PHP 7.3

Добавим в систему репозиторий с PHP 7.3:

```
sudo add-apt-repository ppa:ondrej/php
```

```
sudo apt update
```

Установим связку PHP7.3-FPM для обработки динамических запросов (без apache):

```
sudo apt install php7.3-fpm
```

> Примечание: если выполнить комманду `sudo apt install php7.3`, то по умолчанию будет установлен apache c библиотеками для обработки php запросов.

Проверим версию PHP:

```
php -v
```

Результат:

![Установка PHP 7.3](3.jpg "Установка PHP 7.3")

Установим различные полезные расширения:

```
sudo apt install php7.3-cli php7.3-json php7.3-pdo php7.3-mysql php7.3-zip php7.3-mbstring php7.3-curl php7.3-xml 
```

Перезагрузим nginx:

```
sudo systemctl restart nginx
```

Для безопасности нужно раскоментировать строку и установить `cgi.fix_pathinfo=0` в файле php.ini.
Это можно сделать следующей коммандой:

```
sudo sed -i 's/;cgi.fix_pathinfo=[1|0]/cgi.fix_pathinfo=0/g' /etc/php/7.3/fpm/php.ini
```

```
sudo systemctl restart php7.3-fpm
```

![Установка PHP 7.3](4.jpg "Установка PHP 7.3")


nginx при установке создал сайт с приветствием, изменим дефолтный конфиг этого сайта для проверки работы PHP 7,3:

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
                fastcgi_split_path_info ^(.+\.php)(/.+)$;
                fastcgi_pass unix:/var/run/php/php7.3-fpm.sock; 
                fastcgi_index index.php;
                fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
                include fastcgi_params;
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

![Проверка PHP 7](5.jpg "Проверка PHP 7")

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