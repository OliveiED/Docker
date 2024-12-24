<img src="ilus-code.svg" min-width="300px" max-width="300px" width="300px" align="right" alt="logo iuricode">

# Bem-vindo, Aventureiros! ☕

Você está procurando servidores DNS confiáveis e escaláveis? Este é o lugar certo!
O projeto oferece um setup prático e eficiente com dois servidores **Unbound DNS** configurados para atender suas necessidades.

## Detalhes dos Servidores

- **192.168.100.111 - primary-unbound**  
  Healthcheck:
  ```yaml
  test: ["CMD", "dig", "@127.0.0.1", "cnn.com"]
  ```

- **192.168.100.112 - secondary-unbound**  
  Healthcheck:
  ```yaml
  test: ["CMD", "dig", "@127.0.0.1", "nasa.org"]
  ```

### Recursos Principais

- Utiliza a imagem oficial do Unbound DNS personalizada com **dnsutils**.
  - Sugestão: Adicione ferramentas como `tcpdump` e `net-tools` no `Dockerfile` para ajudar na resolução de problemas.
- Ambos os servidores operam no modo **ativo/ativo**, recebendo consultas DNS e armazenando cache.
- Configurações centralizadas em uma única pasta para facilitar a gestão e migrações.
- Consultas resolvidas diretamente nos **root.hints**.
- Suporte completo ao **DNSSEC**, com validação em domínios como:
  - `dnssec-tools.org`
  - `verisignlabs.com`
  - `nic.br`
  - `dnssec-failed.org` (para testar falhas na validação)
  - `ietf.org`

---

## Guia de Instalação

### 1. Verifique os arquivos de configuração
Certifique-se de que os arquivos de configuração estão mapeados corretamente para os diretórios do seu sistema.

### 2. Confirme a instalação do Docker
Verifique se o Docker e o Docker Compose estão instalados:
```bash
$ docker --version
Docker version 27.3.1, build ce12230

$ docker-compose --version
Docker Compose version v2.3.3
```

### 3. Compile a imagem personalizada

```bash
docker build -t dockerfile-unbound /caminho/para/o/dockerfile
```

### 4. Suba os containers

```bash
docker-compose up -d --build
```

### 5. Verifique o status dos containers

```bash
$ docker ps
CONTAINER ID   IMAGE                COMMAND         STATUS                   PORTS                         NAMES
546fe06263f7   dockerfile-unbound   "/unbound.sh"   Up 46 minutes (healthy)   0.0.0.0:5301->53/tcp, ...     srv01-unbound-primary
d90a3f749e4e   dockerfile-unbound   "/unbound.sh"   Up 46 minutes (healthy)   0.0.0.0:5302->53/tcp, ...     srv02-unbound-secondary
```
O status "UP" e "(healthy)" indica que os servidores estão funcionando.

### 6. Teste os servidores
Acesse o container:
```bash
docker exec -it srv01-unbound-primary /bin/bash
```
Teste uma consulta DNS com validação DNSSEC:
```bash
dig @localhost dnssec-tools.org +dnssec +trace -4
```
A flag **AD** na resposta indica que a validação DNSSEC foi bem-sucedida.

---

## Recursos Disponíveis

- **Servidores DNS com suporte a DNSSEC**:
  - `srv01-unbound-primary`
  - `srv02-unbound-secondary`

- **Configurações e arquivos essenciais**:
  - `docker-compose.yml`
  - `dockerfile-unbound`
  - `a-records.conf`
  - `srv-records.conf`
  - `root.hints`
  - `unbound.conf`
  - `forward-records.conf`

- **Portainer**: Interface web para gerenciamento de containers.

---

## Contribuições e Colaboração

Conecte-se comigo para explorar novas oportunidades de construir infraestruturas de rede robustas e escaláveis!

<blockquote>"Let's connect and explore opportunities to collaborate in shaping robust network infrastructures for the future!"</blockquote>

