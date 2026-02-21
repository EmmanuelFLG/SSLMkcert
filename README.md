# Configuração de HTTPS Local com MKCert e Nginx

Este tutorial ensina como configurar HTTPS local para uma aplicação utilizando **MKCert** e **Nginx**.  
O HTTPS será válido apenas para o seu computador (certificados locais).

---

## Pré-requisitos

- Sistema Linux (Ubuntu/Debian)
- Nginx instalado
- Acesso ao terminal com sudo

---

## Instalar MKCert

Atualize os pacotes do sistema e instale o MKCert:

```bash
sudo apt update
sudo apt install mkcert libnss3-tools -y

Crie a autoridade certificadora local:

mkcert -install

Isso cria uma CA confiável no seu computador, permitindo que o navegador aceite os certificados gerados.

 Gerar os certificados

Escolha os domínios que quer usar localmente, por exemplo:

mkcert meuprojeto.local localhost

Isso vai gerar dois arquivos:

meuprojeto.local.pem → certificado

meuprojeto.local-key.pem → chave privada

 Copiar os certificados para o Nginx

Copie os arquivos para o diretório de configuração do Nginx:

sudo mv meuprojeto.local.pem meuprojeto.local-key.pem /etc/nginx/
 Configurar o Nginx

Edite o arquivo de site do Nginx ou crie um novo:

sudo nano /etc/nginx/sites-available/default

Exemplo de configuração HTTPS:

server {
    listen 443 ssl;
    server_name meuprojeto.local;

    ssl_certificate /etc/nginx/meuprojeto.local.pem;
    ssl_certificate_key /etc/nginx/meuprojeto.local-key.pem;

    root /caminho/do/seu/projeto;
    index index.html;

    location / {
        try_files $uri $uri/ /index.html;
    }
}

Substitua /caminho/do/seu/projeto pelo diretório da sua aplicação (por exemplo dist/ ou build/).

 Reiniciar o Nginx

Após salvar as alterações, reinicie o Nginx para aplicar as mudanças:

sudo systemctl restart nginx
 Testar HTTPS

Abra o navegador e acesse:

https://meuprojeto.local

Se tudo estiver certo, você verá seu projeto com o cadeado de HTTPS indicando conexão segura.
