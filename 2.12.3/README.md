# Servidor OCS Inventory com Docker Compose

Esta configuração utiliza o Docker Compose para executar o servidor OCS Inventory (versão 2.12.3), seu banco de dados (MySQL 8.0) e um proxy reverso Nginx.

## Pré-requisitos

*   [Docker](https://docs.docker.com/get-docker/) instalado.
*   [Docker Compose](https://docs.docker.com/compose/install/) instalado (geralmente incluído no Docker Desktop).

## Estrutura do Projeto

*   `docker-compose.yml`: Define os serviços (aplicação, banco de dados, proxy).
*   `./sql/`: Contém scripts SQL de inicialização para o banco de dados (ex: criação do schema). Garanta que este diretório exista e contenha os arquivos `.sql` necessários, se aplicável para a configuração inicial.
*   `./nginx/conf/`: Contém o template de configuração do Nginx (`ocsinventory.conf.template`).
*   `./nginx/certs/`: Contém certificados SSL (se estiver usando HTTPS). Certificados de exemplo podem estar incluídos por padrão.
*   `./nginx/auth/`: Contém arquivos de autenticação para o Nginx (ex: `.htpasswd` para acesso à API).

## Como Executar

1.  **Clonar ou Baixar:** Certifique-se de que você tem este arquivo `docker-compose.yml` e os diretórios associados (`sql`, `nginx`) na pasta do seu projeto (`c:\Users\XXXXXXXXXXXX\Desktop\OCSInventory-Docker-Image\2.12.3`).
2.  **Navegar até o Diretório:** Abra um terminal ou prompt de comando e navegue até o diretório que contém o arquivo `docker-compose.yml`:
    ```bash
    cd c:\Users\XXXXXXXXXXXX\Desktop\OCSInventory-Docker-Image\2.12.3
    ```
3.  **Iniciar os Serviços:** Execute o seguinte comando para construir (se necessário) e iniciar os contêineres em modo detached (segundo plano):
    ```bash
    docker-compose up -d
    ```
    Isso iniciará a aplicação OCS, o banco de dados MySQL e o proxy Nginx. Os dados serão persistidos em volumes Docker.
4.  **Acessar o OCS Inventory:** Assim que os contêineres estiverem rodando, você poderá acessar a interface web do OCS Inventory navegando para:
    [http://localhost/ocsreports](http://localhost/ocsreports)
    *   *Observação:* Na primeira vez que acessar, o OCS pode executar um processo de configuração ou atualização.
5.  **Credenciais Padrão:**
    *   **Banco de Dados:** O banco de dados está configurado com o usuário `ocsuser` e senha `ocspass`. A senha do usuário `root` do MySQL é `rootpass` (conforme destacado no `docker-compose.yml`).
    *   **Interface Web OCS:** As credenciais padrão para a interface web do OCS Inventory são geralmente `admin` / `admin`. Por favor, altere-as após o primeiro login.

## Como Parar

1.  **Parar e Remover Contêineres:** Para parar e remover os contêineres, redes e volumes definidos no `docker-compose.yml`, execute:
    ```bash
    docker-compose down
    ```
    Se você quiser preservar os volumes de dados (como o conteúdo do banco de dados), use:
    ```bash
    docker-compose stop
    ```

## Serviços

*   **`ocsapplication`**: A aplicação principal do servidor OCS Inventory (Apache/Perl).
*   **`ocsdb`**: O banco de dados MySQL que armazena os dados do inventário.
*   **`ocsproxy`**: Um proxy reverso Nginx que expõe a aplicação OCS nas portas 80 e 443. Ele trata as requisições e as encaminha para o `ocsapplication`.