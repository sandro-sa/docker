# Definição e Criação de um Dockerfile

## Primeiro Dockerfile

- Criando o Dockerfile manualmente

```bash
# Criando container Ubuntu
docker run -dti --name ubuntu-python ubuntu
# Executando bash
docker exec -ti ubuntu-python bash
# Atualizando pacotes
apt update
# Instalando Python 3 e o editor nano
apt install -y python3 nano
# Limpando os arquivos .deb que foram baixados
apt clean
# Acessando a pasta /opt
cd /opt
# Criando um arquivo .py
nano app.py
```

- Dados para inserir no editor

```python
nome = input("Qual seu nome?")
print(nome)
```

- Salvar e sair do editor.

- Executando o app

```bash
# Executando o app
python3 app.py
# Saindo do bash
exit
# Executando o app diretamente do host
docker exec -ti ubuntu-python python3 /opt/app.py
# Parando o container
docker stop ubuntu-python
# Excluindo o container
docker rm -f ubuntu-python
```

- Criando diretório para imagens

```bash
mkdir /images
cd /images
mkdir ubuntu-python
cd ubuntu-python
nano app.py
```

- Dados para inserir no editor

```python
nome = input("Qual seu nome?")
print(nome)
```

- Criando o Dockerfile

```bash
nano dockerfile
```

- Dados para inserir no editor

```dockerfile
# Qual a imagem base?
FROM ubuntu
# Quais comandos vamos executar
RUN apt update && apt install -y python3 && apt clean
# Copiando o arquivo para o container
COPY app.py /opt/app.py
# Executando o app
CMD python3 /opt/app.py
```

- Salvar e sair.
- Construindo a imagem

```bash
# O ponto (.) indica que estamos no mesmo diretório, e o nome da imagem
docker build . -t ubuntu-python
```

- Repare que ele vai executar todos os passos que fizemos manualmente.
- Com o comando `docker images`, podemos ver a imagem criada.
- Agora, para executar o container:

```bash
# Não utilizamos o parâmetro -d, pois não queremos o container em segundo plano
docker run -ti ubuntu-python
```

- Repare com o comando `docker ps -a` que o container para após você responder seu nome e apertar Enter.

---

## Criando uma imagem personalizada do Apache (Web Server)

- Acesse a pasta `images` criada anteriormente e execute os comandos:

```bash
# Criando diretório
mkdir debian-apache
# Acessando o diretório
cd debian-apache
# Criando diretório 'site' para armazenar os arquivos
mkdir site
# Acessando o diretório 'site'
cd site
```

- Abra outro terminal e instale o [WinSCP](https://winscp.net/eng/download.php), caso não tenha ele disponível.

```bash
# Aqui você pode pegar um exemplo de site pronto na web e salvar no seu PC, depois envie-o para a máquina virtual
scp "D:\front_end\site.zip" root@192.168.0.163:/images/debian-apache/site
```

- De volta ao terminal da máquina virtual:

```bash
# Descompactando o arquivo zip
unzip site.zip
# Apagando o arquivo zip
rm site.zip
# Criando um arquivo .tar com todos os arquivos da página. O Docker extrai automaticamente o arquivo tar.
tar -czf site.tar ./
# Copiando o arquivo .tar para o diretório debian-apache
cp site.tar ../
# Removendo o diretório site
rm -Rf site
# Criando o Dockerfile
nano dockerfile
```

- Criando o Dockerfile

```dockerfile

    FROM debian

    RUN apt-get update && apt-get install -y apache2 && apt-get clean

    ENV APACHE_LOCK_DIR="/var/lock"
    ENV APACHE_PID_FILE="/var/run/apache2.pid"
    ENV APACHE_RUN_USER="www-data"
    ENV APACHE_RUN_GROUP="www-data"
    ENV APACHE_LOG_DIR="/var/log/apache2"

    ADD site.tar /var/www/html

    LABEL description="Apache Webserver 1.0"

    VOLUME /var/www/html

    EXPOSE 80

    ENTRYPOINT ["/usr/sbin/apachectl"]

    CMD ["-D", "FOREGROUND"]

```

- Gerando a imagem, criando o container e verificando o IP

```bash
    docker image build -t debian-apache:1.0 .

    docker run -dti -p 80:80 --name meu-apache debian-apache:1.0

    ip a
```

- Acesse o navegador e visualize o site.
- Caso precise de outra aplicação, basta alterar a porta e o nome do container:

```bash
    docker run -dti -p 8080:80 --name meu-apache2 debian-apache:1.0
```

---

## Criando imagens personalizadas a partir de imagens de linguagens de programação

- Acesse o hub do Docker e procure pela imagem oficial do [Python](https://hub.docker.com/_/python). Role a página para ver como criar o seu Dockerfile e copie o conteúdo, deixando salvo em algum bloco de notas.

- Vamos criar a imagem com o comando mostrado no início da página. Antes disso, volte para o diretório `images` e execute os seguintes comandos:

- **OBS:** Esta imagem é bem maior do que a feita com o Ubuntu, mas é muito mais rápida para trabalhar.

```bash
    docker pull python
    mkdir python
    cd python
    nano app.py
```

- Dados para inserir no editor

```python
nome = input("Qual seu nome?")
print(nome)
```

- Criando o Dockerfile

```bash
nano dockerfile
```

- Copie e cole o conteúdo do bloco de notas e faça as alterações necessárias:

```dockerfile
FROM python
# Local onde estamos trabalhando
WORKDIR /usr/src/app
# Copiando o app para o local de execução
COPY app.py /usr/src/app

# CMD [ "python", "/usr/src/app/app.py" ] # Pode ser usado das duas maneiras
CMD [ "python", "./app.py" ]
```

- Gerando a imagem

```bash
docker image build -t app-python:1.0 .
```

- Executando a aplicação

```bash
docker run -ti --name runapp1 app-python:1.0
```

