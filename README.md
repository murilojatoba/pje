# PJe-BD
Projeto de apoio para execução da base de dados paradigma do PJe localmente, em container Docker.

## Pontos a ajustar, se necessário:

### docker-compose.yaml:

- ```container_name: pje-bd```
O container será criado com o nome pje-bd, por padrão.

- ```/usr/local/pje:/usr/local/pje```
Caminho do pje no ambiente padrão de desenvolvimento.
Necessário ser o mesmo também dentro do container, pois o script de instalação da base de dados vai apontar para esse caminho.
- ```/usr/local/pje/PostgreSQL/13/data:/var/lib/postgresql/data``` 
É onde a base de dados do pje será criada no host e está sendo mapeada para o caminho padrão da base Postegres no container.
- ```/home/murilo/Downloads:/mnt/Downloads``` 
Localização dos scripts de criação do ambiente padrão, baixados do CSJT e onde serão localizados dentro do container.

## Como utilizar
Esse projeto gera um container com o banco Postgres vazio para ser instalado o ambiente do PJe.
- Inicialmente deve-se efetuar os ajustes conforme acima.
- Em seguida deve-se inicializar o container com o comando ```docker-compose up``` na raiz do pje-bd. Se tudo correr bem, deve ser apresentada a mensagem ```database system is ready to accept connections```
- Abra outro terminal, acesse o container ```docker exec -it pje-bd /bin/bash```
- No container, acesse a pasta onde estão os scripts fornecidos pelo ambiente padrão ```cd /mnt/Downloads/pje_linux_64bits_[DATA-DA-VERSAO] ```
- Execute o script de criação do banco ```./08-restaurar_backup_base_dados_pje.sh```.