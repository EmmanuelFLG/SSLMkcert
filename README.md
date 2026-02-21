Instalar MKCert

Atualize o sistema e instale:

sudo apt update
sudo apt install mkcert libnss3-tools -y
   Criar autoridade certificadora local
mkcert -install

Isso cria uma CA local confiável no seu computador.

  Gerar certificados

Escolha os domínios que quer usar, por exemplo:

mkcert meuprojeto.local localhost

Isso vai gerar dois arquivos:

meuprojeto.local.pem

meuprojeto.local-key.pem

 Copiar certificados para o Nginx
sudo mv meuprojeto.local.pem meuprojeto.local-key.pem /etc/nginx/
 Configurar Nginx

Edite o arquivo default ou crie um novo:

sudo nano /etc/nginx/sites-available/default

Exemplo de configuração simples:

server {
    listen 443 ssl;
    server_name meuprojeto.local;

    ssl_certificate /etc/nginx/meuprojeto.local.pem;
    ssl_certificate_key /etc/nginx/meuprojeto.local-key.pem;

    root /caminho/do/seu/projeto;
    index index.html;

    location / {
        try_files $uri $uri/ =404;
    }
}
 Reiniciar Nginx
sudo systemctl restart nginx
 Testar

Abra o navegador e digite:

https://meuprojeto.local

Se tudo estiver certo, você verá seu projeto com o cadeado de HTTPS.
