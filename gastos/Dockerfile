# Uso la imagen base de PHP 8 con Apache
FROM php:8.0-apache

# Instalar las extensiones necesarias de PHP
RUN docker-php-ext-install pdo_mysql

# Copiar el composer.json al contenedor antes de instalar Composer
COPY composer.json* /var/www/html/

# Instalar Composer y ejecutar install en lugar de update
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer && \
    composer --version && \
    cd /var/www/html && \
    composer install --no-scripts --no-autoloader && \
    composer dump-autoload --optimize

# Habilitar módulos de Apache
RUN a2enmod rewrite auth_digest ssl

# Copiar archivos y configuraciones al contenedor
COPY 000-default.conf /etc/apache2/sites-available/000-default.conf
COPY ssl/certificate.crt /etc/ssl/certs/
COPY ssl/certificate.key /etc/ssl/private/
COPY ssl/.htpasswd /etc/apache2/.htpasswd 
COPY error_pages/ /var/www/error_pages
COPY index.php /var/www/html/
COPY src/ /var/www/html/

# Configurar los permisos adecuados para la raíz del documento
RUN chown -R www-data:www-data /var/www/html

# Exponer puerto 443
EXPOSE 443

# Comando para iniciar Apache
CMD ["apache2-foreground"]



