# Executando um Docker Compose no Django
Esse repositório contêm um código padrão para um projeto Django com Docker Compose.

Será hospedado localmente usando o container de Gunicorn e Nginx.

# Uso

Executar serviços em plano de fundo:
`docker-compose up -d`

Executar serviços no primeiro plano:
`docker-compose up --build`

Inspecionar Volume:
`docker volume ls`
ou
`docker volume inspect <nome do volume>`

Apagar volumes não utilizados:
`docker volume prune`

Visão da Rede:
`docker network ls`

Parar serviços:
`docker-compose down`

Abrir uma sessão bash enquanto o container está executando:
`docker exec -it <ID do container> /bin/bash`


# Fluxo

1. O arquivo do yaml do Docker Compose irá primeiramente subir o container do Gunicorn que irá executar o projeto Django na porta 8000.

2. O ponto de entrada para o serviço *django_gunicorn* está servido no *entrypoint.sh*. Esse script irá fazer a migration do banco de dados e irá também coletar os arquivos estáticos usados pelo projeto Django.

3. Os arquivos estáticos irá ser coletado no *STATIC_ROOT*. o */static* será diretório no container.

4. Esse diretório será montado para um volume Docker em uma máquina virtual.

5. O próximo container que será levantado é o Nginx. O Dockerfile para esse container está na pasta */nginx*. A configuração do Nginx irá interagir com o serviço do Gunicorn na porta 800 e irá também servir os arquivos estáticos no */static* tamém montado pelo mesmo volume..


# Endpoints

* Você irá ser capaz de acessar o projeto Django no 0.0.0.0:80. Isso é o endpoint que interage com o Gunicorn no 0.0.0.0:8000.

* Para validar que os arquivos estão sendo servidos corretamente, você pode visitar 0.0.0.0:80/admin. Esse endpoint irá mostrar a tela de administrador com os estilos corretos usados.

* Gunicorn não serve os arquivos estáticos, por isso se você visita o 0.0.0.0:8000/admin (A página de administrador irá aparecer sem o estilo padrão)

### Modelo da infraestrutura

![Modelo da infraestrutura](.infragenie/infrastructure_model.png)