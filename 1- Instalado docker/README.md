# instalando Docker

## **Liberar Acesso Remoto via SSH no Ubuntu Server**

- Para permitir o acesso remoto em uma máquina virtual Ubuntu Server, você pode configurar o **SSH (Secure Shell)**. Aqui estão os passos para garantir que o SSH esteja funcionando corretamente:

### 1. **Verificar se o SSH está instalado**

- O SSH nem sempre vem instalado por padrão em algumas distribuições do Ubuntu, então a primeira coisa a fazer é garantir que o **OpenSSH Server** esteja instalado. Para isso, execute os seguintes comandos:
```bash
   sudo apt update
   sudo apt install openssh-server
```

### 2. **Verificar o Status do Serviço SSH**

- Após a instalação, verifique se o serviço SSH está ativo. Execute:
```bash
   sudo systemctl status ssh
```

- Se o serviço não estiver ativo, você pode iniciá-lo com:
```bash
   sudo systemctl start ssh
```

- Para garantir que o serviço SSH inicie automaticamente ao reiniciar o sistema, use:
```bash
   sudo systemctl enable ssh
```

### 3. **Configurar o Firewall (Se Necessário)**

- Se você estiver usando o **ufw (Uncomplicated Firewall)** no seu Ubuntu Server, será necessário liberar a porta 22, que é a porta padrão do SSH. Para permitir o tráfego SSH, execute:
```bash
   sudo ufw allow ssh
```

- Ou, se o `ufw` não estiver configurado para liberar o SSH, você pode liberar manualmente a porta 22:
```bash
   sudo ufw allow 22/tcp
```

- Depois, verifique se a regra foi aplicada corretamente:
```bash
   sudo ufw status
```

### 4. **Acessar o Servidor Remotamente via SSH**

- Agora, para acessar o servidor Ubuntu remotamente, você precisa saber o **endereço IP** da sua máquina virtual. Para encontrar o IP, use:
```bash
   ip a
```

- Busque pelo endereço IP na interface de rede que sua máquina virtual está usando. 
- Depois, no seu computador cliente (Linux, macOS ou Windows), use o seguinte comando para se conectar ao Ubuntu Server via SSH:
```bash
- ssh usuario@ip_do_servidor
```

- Substitua `usuario` pelo nome de usuário no servidor e `ip_do_servidor` pelo endereço IP obtido.

### 5. **Configurações Adicionais (Opcional)**

- **Autenticação via Chave SSH**: Para aumentar a segurança, você pode configurar uma chave SSH para autenticação, em vez de usar senha. No seu cliente, gere a chave SSH com:
```bash
ssh-keygen
```

- Depois, copie a chave pública para o servidor com:
```bash
ssh-copy-id usuario@ip_do_servidor
```

- Isso permitirá que você se conecte sem precisar digitar a senha.

---

## **Verificando o Status do UFW (Firewall)**

- Para verificar se você está utilizando o **UFW (Uncomplicated Firewall)**, execute o seguinte comando:
```bash
   sudo ufw status
```

### Possíveis Saídas:

- 1. **Se o UFW estiver ativo**, você verá algo como:
```bash
   Status: active
   To                         Action      From
   --                         ------      ----
   22                         ALLOW       Anywhere
   22 (v6)                    ALLOW       Anywhere (v6)
```

- Isso indica que a porta 22 (para SSH) está liberada.
- 2. **Se o UFW estiver desativado**, você verá:
```bash
   Status: inactive
```

- Isso significa que o firewall não está em execução. Neste caso, não há necessidade de regras de firewall para o SSH, mas ativá-lo pode aumentar a segurança.

### Caso o UFW Não Esteja Instalado

- Se o comando `sudo ufw status` retornar algo como "comando não encontrado", significa que o UFW não está instalado. Para instalar o UFW e configurar o firewall, faça o seguinte:
```bash
   sudo apt install ufw
   sudo ufw enable
```

- Após ativar o UFW, libere a porta 22 para SSH:
```bash
   sudo ufw allow ssh
```

- Ou, se preferir, pode liberar manualmente a porta 22:
```bash
   sudo ufw allow 22/tcp
```

---

## **Habilitar o Acesso SSH Direto com o Usuário Root (Não Recomendado)**

- Embora não seja recomendável por questões de segurança, você pode habilitar o acesso SSH diretamente com o usuário `root` se necessário. 

### **Passos para Habilitar o Acesso SSH para o Usuário Root**

- 1. **Definir uma Senha para o Usuário Root**:
   
- Se o usuário `root` não tiver uma senha definida, use o comando abaixo para configurá-la:
```bash
   sudo passwd root
```

- 2. **Permitir o Login do Root via SSH**:

- Edite o arquivo de configuração do SSH:
```bash
sudo nano /etc/ssh/sshd_config
```

- Encontre a linha que define `PermitRootLogin` e altere para `yes`:
```bash
PermitRootLogin yes
```

- **Alternativa mais segura**: Se preferir permitir o login do root apenas via senha (e não por chave SSH), altere para:
```bash
   PermitRootLogin prohibit-password
```

- Isso permitirá o login como `root`, mas **somente via senha**, o que aumenta a segurança.

3. **Reiniciar o Serviço SSH**:

- Após alterar o arquivo, reinicie o serviço SSH para aplicar as mudanças:
```bash
   sudo systemctl restart ssh
```

4. **Verificar o Acesso via SSH**:

- Agora você pode tentar acessar o servidor usando o usuário `root`:
```bash
   ssh root@<IP_DO_SEU_SERVIDOR>
```

- Substitua `<IP_DO_SEU_SERVIDOR>` pelo IP do seu servidor. Você será solicitado a fornecer a senha do `root`.

---

## **Instalar o Docker no Ubuntu Server**

- Agora, vou mostrar como instalar o Docker no seu Ubuntu Server. Essa é a forma mais simples de instalação. Para outras opções, consulte a documentação oficial [aqui](https://docs.docker.com/engine/install/ubuntu/).

### 1. **Verificar se o `curl` está Instalado**

-Primeiro, verifique se o `curl` está instalado:
```bash
   curl --version
```

- Se o `curl` não estiver instalado, você pode instalá-lo com:
```bash
   sudo apt update
   sudo apt install curl
```

### 2. **Instalar o Docker**

- Execute os seguintes comandos para instalar o Docker:
```bash
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh
```

### 3. **Verificar a Versão do Docker**

- Após a instalação, verifique a versão do Docker com:
```bash
   docker version
```

### 4. **Verificar se o Docker está Rodando**

- Verifique o status do Docker para garantir que ele está em execução:
```bash
   sudo systemctl status docker
```

- A saída será algo como:
```bash
   docker.service - Docker Application Container Engine
  Loaded: loaded (/lib/systemd/system/docker.service; enabled; vendor preset: enabled)
  Active: active (running) since Thu 2024-11-07 00:04:53 UTC; 8min ago
  ...
```

### 5. **Iniciar o Docker (Se Necessário)**

- Caso o Docker não esteja rodando, inicie-o com:
```bash
   sudo systemctl start docker
```

---
