# MySQL + phpMyAdmin com Docker Compose

Este projeto fornece um ambiente pronto para uso com MySQL (MariaDB) e phpMyAdmin utilizando Docker Compose.

## Pré-requisitos

- [Docker](https://www.docker.com/get-started)
- [Docker Compose](https://docs.docker.com/compose/install/)
- (Opcional) [Git](https://git-scm.com/)

## Como usar

1. Clone este repositório:

```bash
git clone https://github.com/vitfera/mysql_docker.git
cd mysql_docker
```

2. Copie o arquivo `.env.example` para `.env` e ajuste as variáveis se necessário:

```bash
cp .env.example .env
```

3. Suba os containers:

```bash
docker-compose up -d
```

4. Acesse o phpMyAdmin:

- URL: http://localhost:40001
- Usuário: conforme variável `PMA_USER` do `.env` (padrão: root)
- Senha: conforme variável `PMA_PASSWORD` do `.env` (padrão: root)

5. O banco de dados estará disponível na porta 40000.

## Acesso pela rede

O MariaDB está configurado para escutar em todas as interfaces do container (`bind-address=0.0.0.0`) e a porta `40000` é publicada no host.

Em outra máquina da mesma rede, conecte usando o IP da máquina que roda o Docker:

```bash
mysql -h 192.168.3.85 -P 40000 -u root -p
```

Se o volume `mysql_data` já existia antes desta configuração, libere o usuário remoto uma vez:

```bash
docker exec -it mysql_container mariadb -u root -p
```

```sql
CREATE OR REPLACE USER 'root'@'%' IDENTIFIED BY 'root';
GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' WITH GRANT OPTION;
FLUSH PRIVILEGES;
SHOW VARIABLES LIKE 'bind_address';
```

O `SHOW VARIABLES` deve retornar `0.0.0.0`.

## Parar e remover os containers

```bash
docker-compose down
```

---

Desenvolvido por [vitfera](https://github.com/vitfera)
