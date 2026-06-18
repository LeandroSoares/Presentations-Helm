FROM nginx:alpine

# Copiamos nosso HTML para a pasta de templates do Nginx.
# A imagem oficial do Nginx no Alpine executa o 'envsubst' automaticamente nos arquivos 
# terminados em .template antes de iniciar, gerando o arquivo final na pasta padrão.
COPY index.html /etc/nginx/templates/index.html.template

# Valor padrão caso a variável não seja fornecida
ENV BG_COLOR="#f0f0f0"

EXPOSE 80