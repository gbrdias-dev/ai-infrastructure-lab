# Configurando o balanceamento de carga com Nginx

Primeiro você precisa criar um arquivo de configuração com o seguinte comando: 
sudo nano /etc/nginx/conf.d/ollama.conf

Em seguida coloque essas configurações no arquivo:

```
upstream ollama_servers {
    hash $http_x_forwarded_for consistent;

    server "ip-do-servidor1:11434";
    server "ip-do-servidor2:11434";
    
    exemplo:
    server 192.168.0.200:11434;
    server 192.168.0.200111434;

}

server {

    listen 11434;

    location / {

        proxy_pass http://ollama_servers;

        proxy_http_version 1.1;

        proxy_set_header Host $host;
        proxy_set_header Connection "";

        proxy_buffering off;

        proxy_read_timeout 600s;
        proxy_send_timeout 600s;
    }

}
```
