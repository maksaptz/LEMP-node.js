# LEMP-node.js
sudo -i
apt update
apt upgrade -y
dpkg-reconfigure locales
en_US.UTF-8.
apt install curl gnupg2 ca-certificates lsb-release debian-archive-keyring vim -y
curl https://nginx.org/keys/nginx_signing.key | gpg --dearmor \
    | sudo tee /usr/share/keyrings/nginx-archive-keyring.gpg >/dev/null
curl https://nginx.org/keys/nginx_signing.key | gpg --dearmor \
    | sudo tee /usr/share/keyrings/nginx-archive-keyring.gpg >/dev/null

echo "deb [signed-by=/usr/share/keyrings/nginx-archive-keyring.gpg] \
http://nginx.org/packages/debian `lsb_release -cs` nginx" \
    | sudo tee /etc/apt/sources.list.d/nginx.list
echo -e "Package: *\nPin: origin nginx.org\nPin: release o=nginx\nPin-Priority: 900\n" \
    | sudo tee /etc/apt/preferences.d/99nginx
apt update
apt install nginx=1.18.* -y
apt-mark hold nginx=1.18.*

wget https://dev.mysql.com/get/mysql-apt-config_0.8.24-1_all.deb
apt install ./mysql-apt-config_0.8.24-1_all.deb
apt update
