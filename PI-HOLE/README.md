# Pi-hole com Docker Compose

Pi-hole é uma solução de bloqueio de anúncios e rastreadores para redes inteiras, que funciona como um servidor DNS local. Ele é ideal para melhorar a privacidade e o desempenho, bloqueando conteúdos indesejados antes mesmo de serem carregados.

Este repositório fornece um exemplo de configuração de `docker-compose.yml` para rodar o Pi-hole em um ambiente Docker.

---

## Pré-requisitos

Certifique-se de que você possui os seguintes requisitos instalados e configurados no seu sistema Linux antes de prosseguir:

### Docker
1. Verifique se o Docker está instalado:
   ```bash
   docker --version
   ```
2. Caso não esteja instalado, siga as [instruções oficiais de instalação do Docker](https://docs.docker.com/engine/install/).

### Docker Compose
1. Verifique se o Docker Compose está instalado:
   ```bash
   docker-compose --version
   ```
2. Caso não esteja instalado, siga as [instruções oficiais de instalação do Docker Compose](https://docs.docker.com/compose/install/).

---

## Configurações Necessárias

### 1. Garantir que a porta 53 não está sendo utilizada pelo sistema
O Pi-hole utiliza a porta 53 para resolução de DNS. No entanto, em distribuições Linux modernas, a resolução de DNS nativa pode já estar utilizando essa porta. Para evitar conflitos:

1. Verifique o status do serviço `systemd-resolved`:
   ```bash
   systemctl status systemd-resolved.service
   ```
2. Edite o arquivo de configuração para desativar a resolução de DNS nativa:
   ```bash
   sudo nano /etc/systemd/resolved.conf
   ```
   - Altere ou adicione a seguinte linha:
     ```
     DNSStubListener=no
     ```
3. Reinicie o serviço:
   ```bash
   systemctl restart systemd-resolved.service
   ```

---

## Passo a Passo para Configuração do Pi-hole

1. Clone este repositório ou copie o arquivo `docker-compose.yml` para o seu ambiente local.
2. No diretório onde o `docker-compose.yml` está localizado, execute:
   ```bash
   docker-compose up -d
   ```

---

## Acesso ao Painel Web

Após iniciar o Pi-hole, o painel administrativo estará disponível no seguinte endereço:

- **URL**: `http://<IP_DO_HOST>:8001/admin/`

Substitua `<IP_DO_HOST>` pelo endereço IP do servidor onde o Pi-hole está rodando.

---

## Suporte

Para dúvidas ou problemas, consulte a [documentação oficial do Pi-hole](https://docs.pi-hole.net/) ou abra uma issue neste repositório.

