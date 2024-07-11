# Установка основных компонентов LEMP-node.js


## Задачи:
- Установка MySQL 8.0, PHP 8.1, Nginx 1.18,  Nodejs 16 и 20 версии;
- Nodejs должен быть установлен только для этого пользователя.


### Настройка проводилась на следющем vagrant боксе:
### vm.box = "debian/bullseye64" 
### vm.box_version = "11.20230615.1"
### iptables отключен  <br/> 


#### Подключаемся к созданной машине
```vagrant ssh```
#### Получим рут права для начало работы
```sudo -i```
#### Обновим репозитории
```apt update```
#### Обновим пакеты
```apt upgrade -y```
#### Установим пакеты необходимые для работы
```apt install apt-transport-https curl gnupg2 ca-certificates lsb-release debian-archive-keyring vim -y```
### Nginx
#### Имортируем GPG ключ Nginx репозитория 
```
curl https://nginx.org/keys/nginx_signing.key | gpg --dearmor \
    | sudo tee /usr/share/keyrings/nginx-archive-keyring.gpg >/dev/null
```
#### Добавим репозиторий Nginx в список доступтных репозиториев
```
echo "deb [signed-by=/usr/share/keyrings/nginx-archive-keyring.gpg] \
http://nginx.org/packages/debian `lsb_release -cs` nginx" \
    | sudo tee /etc/apt/sources.list.d/nginx.list
```
#### Изменим приоритет Nginx репозитория
```
echo -e "Package: *\nPin: origin nginx.org\nPin: release o=nginx\nPin-Priority: 900\n" \
    | sudo tee /etc/apt/preferences.d/99nginx
```
#### Снова обновим репозитории и установим требуемую версию Nginx
```apt update```


```apt install nginx=1.18.* -y```


```apt-mark hold nginx=1.18.*```


### Mysql
#### Загрузим deb пакет mysql
```wget https://dev.mysql.com/get/mysql-apt-config_0.8.22-1_all.deb```
#### Запустим пакет для конфигурации репозитория Mysql
```dpkg -i mysql-apt-config_0.8.22-1_all.deb```
#### Т.к. при обновлении репозиториев возникала ошибка в связи с итсекшим ключом, добавим репозиторий в надежные источники
```apt-key adv --keyserver keyserver.ubuntu.com --recv-keys B7B3B788A8D3785C```
#### Проведем установку Mysql 8.0
```apt update```


```apt upgrade -y```


```apt install mysql-client mysql-community-server mysql-server -y```


#### Сконфигурируем первичные настройки безопасности
```mysql_secure_installation```
#### Добавим службу в автозагрузку
```systemctl enable mysql@.service```


### PHP
#### Скачаем GPG ключ для репозитория PHP (для доступа к ключу нужен VPN)
```wget -O /etc/apt/trusted.gpg.d/php.gpg https://packages.sury.org/php/apt.gpg```
#### Добавим репозиторий в список доступтных репозиториев
```sh -c 'echo "deb https://packages.sury.org/php/ $(lsb_release -sc) main" > /etc/apt/sources.list.d/php.list'```
#### Обновим репозитории
```apt update```
#### Произведем установку PHP8.1
```apt install php8.1-fpm php8.1-mysql -y```
#### Добавим службу в автозагрузку
```systemctl enable php8.1-fpm.service```


### Node.js
#### Добавим группу для пользователя node
```groupadd node```
#### Создадим нового пользователя
```useradd node -s /bin/bash -g node -m -p qwerty1234```
#### Перейдем в пользователя node
```su node```
#### Скачаем и установим nvm
```curl https://raw.githubusercontent.com/creationix/nvm/master/install.sh | bash```
#### Обновим profile
```source ~/.profile```
#### С помощью менеджера установим требуемые версии node.js
```nvm install 16```


```nvm install 20```
#### Зададим версию по умолчанию
```nvm use 20```
