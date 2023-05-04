# PJe-BD
Projeto de apoio para execução da base de dados paradigma do PJe localmente, em container Docker.

## Pontos a ajustar, se necessário:

### docker-compose.yaml:

- `container_name: pje-bd`
O container será criado com o nome pje-bd, por padrão.

- `/usr/local/pje:/usr/local/pje`
Caminho do pje no ambiente padrão de desenvolvimento.
Necessário ser o mesmo também dentro do container, pois o script de instalação da base de dados vai apontar para esse caminho.

- `/usr/local/pje/PostgreSQL/13/data:/var/lib/postgresql/data` 
É onde a base de dados do pje será criada no host e está sendo mapeada para o caminho padrão da base Postegres no container.

- `/home/murilo/Downloads:/mnt/Downloads` 
Localização dos scripts de criação do ambiente padrão, baixados do CSJT e onde serão localizados dentro do container.

## Como utilizar
Esse projeto gera um container com o banco Postgres vazio para ser instalado o ambiente do PJe.
- Inicialmente deve-se efetuar os ajustes conforme acima.
- Em seguida deve-se inicializar o container com o comando `docker-compose up` na raiz do pje-bd. Se tudo correr bem, deve ser apresentada a mensagem `database system is ready to accept connections`.
- Abra outro terminal, acesse o container `docker exec -it pje-bd /bin/bash`.
- No container, atualizar o repositório do apt e instalar o file `apt update && apt install file`.
- No container, criar manualmente as pastas dos tablespace `mkdir -p /usr/local/pje/PostgreSQL/13/tablespace/spjtsd000` e `mkdir -p /usr/local/pje/PostgreSQL/13/tablespace/spjtsi000`. Depois alterar a propriedade dessas pastas `chown -R postgres. /usr/local/pje/PostgreSQL/13/tablespace `. Isso é necessário pois somos root dentro do container e o script foi criado com `sudo` na frente.
- No script **08-restaurar_backup_base_dados_pje.sh**, comentar as linhas que tenham `sudo mkdir`, `sudo chown` e `sudo pg_ctlcluster`.
- No container, acesse a pasta onde estão os scripts fornecidos pelo ambiente padrão `cd /mnt/Downloads/pje_linux_64bits_[DATA-DA-VERSAO] `.
- Execute o script de criação do banco `./08-restaurar_backup_base_dados_pje.sh`.

## Observação
O script de 2022 faz uso da role `pjero_local_usuario_consulta` mas não cria:
```
CREATE ROLE pjero_local_usuario_consulta WITH 
  NOSUPERUSER 
  NOCREATEDB
	NOCREATEROLE
	INHERIT
	NOLOGIN
	NOREPLICATION
	NOBYPASSRLS
	CONNECTION LIMIT -1;
```