# Usar la imagen base de PHP 8 con Apache
FROM php:8.0-apache

# Instalar las extensiones necesarias de PHP
RUN docker-php-ext-install pdo_mysql

# Instalar Composer
RUN curl -sS https://getcomposer.org/installer -o composer-setup.php
RUN php composer-setup.php --install-dir=/usr/local/bin --filename=composer && composer update

# Habilitar el módulo de reescritura de Apache
RUN a2enmod rewrite

# Habilitar el módulo de autenticación digest de Apache
RUN a2enmod auth_digest

# Habilitar el módulo SSL de Apache
RUN a2enmod ssl

# Para la configuración personalizada de Apache
COPY ./configuracion-proyecto.conf /etc/apache2/sites-available/000-default.conf

# Para los certificados SSL (HTTPS)
COPY ./ssl/certificate.crt /etc/ssl/certs/
COPY ./ssl/certificate.key /etc/ssl/private/

# Para las páginas de error personalizadas
COPY ./error_pages/ /var/www/error_pages

# Copiar el composer.json, el index.php y el código fuente de la carpeta src/ de la aplicación al contenedor
COPY composer.json* /var/www/html/
COPY index.php /var/www/html/
COPY ./src/ /var/www/html/

# Instala las dependencias de php especificadas en composer.json 
RUN composer install --no-interaction --working-dir=/var/www/html

# Para configuar los permisos adecuados para la raíz del documento
RUN chown -R www-data:www-data /var/www/html

# Exponer el puerto 443 para el servicio web seguro
EXPOSE 443

# Comando para iniciar Apache
CMD ["apache2-foreground"]
