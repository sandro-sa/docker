
## Desligar um servidor Ubuntu
Para desligar um servidor Ubuntu, você pode usar o comando `shutdown`. Aqui estão as formas mais comuns de fazer isso:

### 1. Desligar imediatamente:
Abra o terminal e execute o seguinte comando:

```bash
sudo shutdown now
```

Isso pedirá a senha do usuário e desligará o servidor imediatamente.

### 2. Desligar em um tempo específico:
Se você quiser agendar o desligamento para, por exemplo, 10 minutos a partir de agora, use:

```bash
sudo shutdown +10
```

Isso irá desligar o servidor após 10 minutos.

### 3. Desligar em um horário específico:
Você também pode especificar uma hora exata para o desligamento. Por exemplo, para desligar às 23:30, você pode usar:

```bash
sudo shutdown 23:30
```

### 4. Cancelar um desligamento agendado:
Se você mudou de ideia e deseja cancelar um desligamento agendado, use o comando:

```bash
sudo shutdown -c
```

### 5. Desligar imediatamente com `poweroff`:
Outro comando simples que também desliga o sistema imediatamente é:

```bash
sudo poweroff
```

### 6. Desligar imediatamente com `halt`:
Outro comando que interrompe todas as operações e desliga o sistema é:

```bash
sudo halt
```

Depois de rodar qualquer um desses comandos, o servidor Ubuntu será desligado de forma segura. Se tiver mais alguma dúvida ou precisar de assistência, fico à disposição!
