# 🌌 EnderHost Panel

O **EnderHost Panel** é um **painel de gerenciamento de servidores** inspirado no [Pterodactyl](https://pterodactyl.io), desenvolvido para facilitar a **criação, administração e monitoramento** de servidores de jogos e aplicações.  

⚡ Simples, moderno e otimizado para rodar em **Ubuntu, Debian e Linux Mint**.  

---

## ✨ Funcionalidades
- Interface web intuitiva para gerenciar servidores.  
- Criação e configuração rápida de instâncias.  
- Monitoramento em tempo real (CPU, RAM, rede).  
- Sistema de usuários com diferentes permissões.  
- Suporte a múltiplos tipos de aplicações/jogos.  

---

## 🛠️ Requisitos do Sistema
- Ubuntu 20.04+ / Debian 10+ / Linux Mint 20+  
- Banco de Dados: MariaDB ou MySQL  
- Servidor Web: Nginx (recomendado)  
- PHP 8.1+ com extensões necessárias  
- Node.js 18+ e Yarn  
- Composer  

---

## 📥 Instalação Passo a Passo

### 🔹 1. Atualizar o sistema
```bash
sudo apt update && sudo apt upgrade -y
```

### 🔹 2. Instalar dependências básicas
```bash
sudo apt install -y curl wget git unzip tar     software-properties-common     apt-transport-https     ca-certificates lsb-release
```

### 🔹 3. Instalar MariaDB
```bash
sudo apt install -y mariadb-server mariadb-client
sudo systemctl enable --now mariadb
```

Criar banco e usuário:
```sql
CREATE DATABASE enderhost;
CREATE USER 'enderuser'@'127.0.0.1' IDENTIFIED BY 'SENHA_FORTE_AQUI';
GRANT ALL PRIVILEGES ON enderhost.* TO 'enderuser'@'127.0.0.1' WITH GRANT OPTION;
FLUSH PRIVILEGES;
EXIT;
```

### 🔹 4. Instalar PHP 8.1 + extensões
```bash
sudo add-apt-repository ppa:ondrej/php -y
sudo apt update
sudo apt install -y     php8.1 php8.1-cli php8.1-common php8.1-mysql php8.1-gd     php8.1-mbstring php8.1-bcmath php8.1-curl php8.1-xml php8.1-zip     php8.1-fpm
```

### 🔹 5. Instalar Composer
```bash
curl -sS https://getcomposer.org/installer | php
sudo mv composer.phar /usr/local/bin/composer
```

### 🔹 6. Instalar Node.js + Yarn
```bash
curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
sudo apt install -y nodejs
npm install -g yarn
```

### 🔹 7. Instalar Nginx
```bash
sudo apt install -y nginx
sudo systemctl enable --now nginx
```

### 🔹 8. Baixar o EnderHost Panel
```bash
cd /var/www/
sudo git clone https://github.com/seu-usuario/enderhost.git painel
cd painel
```

Permissões:
```bash
sudo chown -R www-data:www-data /var/www/painel
sudo chmod -R 755 /var/www/painel/storage /var/www/painel/bootstrap/cache
```

### 🔹 9. Instalar dependências Laravel
```bash
composer install --no-dev --optimize-autoloader
cp .env.example .env
php artisan key:generate
```

Editar `.env`:
```env
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=enderhost
DB_USERNAME=enderuser
DB_PASSWORD=SENHA_FORTE_AQUI
```

Rodar migrações:
```bash
php artisan migrate --seed
```

### 🔹 10. Compilar frontend
```bash
yarn install
yarn build
```

### 🔹 11. Configurar Nginx
Criar arquivo de configuração:
```bash
sudo nano /etc/nginx/sites-available/enderhost.conf
```

Exemplo de configuração:
```nginx
server {
    listen 80;
    server_name seu-dominio.com;

    root /var/www/painel/public;
    index index.php index.html;

    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }

    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/var/run/php/php8.1-fpm.sock;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        include fastcgi_params;
    }
}
```

Ativar site:
```bash
sudo ln -s /etc/nginx/sites-available/enderhost.conf /etc/nginx/sites-enabled/
sudo systemctl restart nginx
```

### 🔹 12. (Opcional) Configurar HTTPS com Let's Encrypt
```bash
sudo apt install -y certbot python3-certbot-nginx
sudo certbot --nginx -d seu-dominio.com
```

---

## 🚀 Finalização
Acesse no navegador:  
```
http://seu-dominio.com
```

E pronto 🎉 seu **EnderHost Panel** está rodando!  

---

## 📌 Status
⚠️ O projeto ainda está em **desenvolvimento**, novas funcionalidades estão sendo adicionadas.  
Contribuições são bem-vindas 🙌  

---
