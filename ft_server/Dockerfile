# Установка контейнерной OS
FROM debian:buster

# Методанные
LABEL fngoc="fngoc@student.21-school.ru"

# Установка и обновление образов
RUN apt-get update
RUN apt-get upgrade -y
RUN apt-get -y install wget \
                        nginx \
                        mariadb-server \
                        php7.3 \
                        php-mysql \
                        php-fpm \
                        php-pdo \
                        php-gd \
                        php-cli \
                        php-mbstring \
                        php-zip \
                        php-xmlrpc \
                        php-xml \
                        php-soap \
                        php-intl \
                        php-curl

# Задаем рабочую директорию
WORKDIR /etc/nginx/sites-available/

# Копируем конфиг nginx
COPY ./srcs/nginx.conf /etc/nginx/sites-available/nginx.conf
RUN ln -s /etc/nginx/sites-available/nginx.conf /etc/nginx/sites-enabled

# Создание сервера
WORKDIR /var/www/server
RUN wget https://wordpress.org/latest.tar.gz
RUN tar -xzvf latest.tar.gz
RUN rm -rf latest.tar.gz
RUN chown -R www-data:www-data /var/www/server/wordpress
WORKDIR /var/www/server/wordpress
COPY ./srcs/wp-config.php .

WORKDIR /
COPY ./srcs/autoindex.sh .

# Установка php-myadmin
RUN wget https://files.phpmyadmin.net/phpMyAdmin/5.0.4/phpMyAdmin-5.0.4-all-languages.tar.gz
RUN tar -xzvf phpMyAdmin-5.0.4-all-languages.tar.gz
RUN rm -rf phpMyAdmin-5.0.4-all-languages.tar.gz
RUN mv phpMyAdmin-5.0.4-all-languages/ /var/www/server/phpmyadmin
COPY ./srcs/config.inc.php /var/www/server/phpmyadmin

#Скрипт устанавливающий ssl
COPY ./srcs/ssl.sh .
RUN bash ssl.sh
RUN rm -rf ssl.sh

# Инициализация database    
COPY ./srcs/db.sh .
RUN bash db.sh
RUN rm -rf db.sh

WORKDIR /
COPY ./srcs/init.sh .

# Открытие портов (не обязательно)
EXPOSE 80 443

# Выполняем команду во время запуска контейнера
CMD bash ./init.sh