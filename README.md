# Passo 1 - Atualizar Pacotes
```
sudo apt update
sudo apt upgrade
```

## Passo 2 - Instalar Dependências
```
sudo apt install libnss3-tools
sudo apt install mkcert
mkcert --version
```

### Passo 3 - Gerar e Mover o Certificado
```
hostname -i
mkcert gcsi.local localhost yourIP ::1
mkcert -install
cd /etc/nginx/
mkdir ssl
mv /home/user/gcsi.local+3.pem /etc/nginx/ssl/
mv /home/user/gcsi.local+3-key.pem /etc/nginx/ssl/
```

#### Passo 4 - Configurar Nginx
```
cd /etc/nginx/sites-available/
nano default


server {
	listen 443 ssl;
	listen [::]:443 ssl;
	server_name gcsi.local localhost;

	root /var/www/html;
	index index.html index.htm;

	ssl_certificate /etc/nginx/ssl/gcsi.local+3.pem;
	ssl_certificate_key /etc/nginx/ssl/gcsi.local+3-key.pem;

	location / {
		try_files $uri $uri/ index.html;
	}	
}
```

##### Passo 5 - Configurar o arquivo de Hosts
```
nano /etc/hosts
127.0.0.1 gcsi.local
```

###### Passo 6 - Encerrar e Iniciar o Nginx
```
systemctl stop nginx
systemctl start nginx
```
