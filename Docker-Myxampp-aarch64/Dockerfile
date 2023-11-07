# Utilizamos una imagen base ARM64 para Debian
FROM arm64v8/debian:buster

# Actualizamos el sistema y instalamos paquetes necesarios
RUN apt-get update && apt-get install -y \
    apt-utils \
    apache2 \
    php7.3 \
    php7.3-mysql \
    mariadb-server \
    unzip \
    wget

# Configuramos MariaDB (asegúrate de establecer una contraseña segura)
RUN service mysql start && mysql -e "ALTER USER 'root'@'localhost' IDENTIFIED BY '1234'; FLUSH PRIVILEGES;"

# Habilitamos módulos de Apache necesarios
RUN a2enmod php7.3
RUN a2enmod rewrite

# Habilitar la navegación por directorios
RUN sed -i 's/Options Indexes FollowSymLinks/Options Indexes FollowSymLinks/' /etc/apache2/apache2.conf

# Descargamos y configuramos phpMyAdmin
RUN mkdir /var/www/html/phpmyadmin && \
    cd /var/www/html/phpmyadmin && \
    wget https://files.phpmyadmin.net/phpMyAdmin/5.1.1/phpMyAdmin-5.1.1-all-languages.zip && \
    unzip phpMyAdmin-5.1.1-all-languages.zip && \
    mv phpMyAdmin-5.1.1-all-languages/* . && \
    rm -rf phpMyAdmin-5.1.1-all-languages phpMyAdmin-5.1.1-all-languages.zip && \
    cp config.sample.inc.php config.inc.php

# Exponemos los puertos
EXPOSE 80 3306

# Iniciamos Apache y MariaDB al arrancar el contenedor
CMD service apache2 start && service mysql start && tail -f /dev/null

