
<img src="ilus-code.svg" min-width="300px" max-width="300px" width="300px" align="right" alt="logo iuricode">

## Olá Aventureiros! ☕

    Você está precisando de servidores de DNS confiáveis e escaláveis, está no local adequado para resolver sua necessidade.
O arquivo docker-compose conta com dois servidores dns-unbound endereçáveis. A comunicação desses servidores com o host Linux se dão pelos seguintes endereços de IP abaixo;
    ↔ 192.168.100.111 - primary-unbound     
    healthcheck:
      test: ["CMD", "dig", "@127.0.0.1", "cnn.com"]

    ↔ 192.168.100.112 - secondary-unbound
        healthcheck:
      test: ["CMD", "dig", "@127.0.0.1", "nasa.org"]

    ↔ Visando otimizar o tempo para resoluções de problemas e possíveis análises que serão necessários  temos disponíveis a imagem oficial do unbound-dns personalizada com "dnsutils". Você pode ficar a vontade para adicionar mais ferramentas no dockerfile-unbound. 

        Dica: tcpdump, net-tools é ótimo e com certeza te ajudará em algum momento!
    ↔ Ambos os serviores receberão as consultas DNS e o cache será realizado.

    ↔ Utilizei apenas um pasta com os arquivos de configuração para facilitar a gestão e migrações.

    ↔ Consultas serão realizadas direto nos root.hints

    ↔ Lembre-se que o DNSSEC está configurado e funcionando:

        dnssec-tools.org - Um domínio criado para testes e ferramentas relacionadas ao DNSSEC.
        verisignlabs.com - Utilizado em demonstrações de tecnologias DNSSEC.
        nic.br - O domínio do Registro Brasileiro também possui suporte a DNSSEC.
        dnssec-failed.org - Um domínio para testar falhas na validação DNSSEC (não deve ser resolvido corretamente por um servidor DNS que valida DNSSEC).
        ietf.org - Domínio oficial do Internet Engineering Task Force (IETF).

## Vamos subir os containers agora ?

    ↔ Passo 1 : Verifique os arquivos de configurações e mapeie de acordo com seu diretório

    ↔ Passo 2 : Certifique que você possui o docker instalado. 
          root@srv-docker:/home/oliveied/my-docker-setup# docker --version
          Docker version 27.3.1, build ce12230
          root@srv-docker:/home/oliveied/my-docker-setup# docker-compose --version
          Docker Compose version v2.3.3
          root@srv-docker:/home/oliveied/my-docker-setup#

    ↔ Passo 3 : Monte o container com a imagem personalizada;
    docker build -t dockerfile-unbound /caminho/para/o/dockerfile

    ↔ Passo 4 : docker-compose up -d --build

    ↔ Passo 5 : Testando os containers
    root@srv-docker:/home/oliveied/my-docker-setup# docker ps
CONTAINER ID   IMAGE                    COMMAND         CREATED          STATUS                    PORTS                                                                              NAMES
546fe06263f7   dockerfile-unbound       "/unbound.sh"   46 minutes ago   Up 46 minutes (healthy)   0.0.0.0:5301->53/tcp, 0.0.0.0:5301->53/udp, [::]:5301->53/tcp, [::]:5301->53/udp   srv01-unbound-primary
d90a3f749e4e   dockerfile-unbound       "/unbound.sh"   46 minutes ago   Up 46 minutes (healthy)   0.0.0.0:5302->53/tcp, 0.0.0.0:5302->53/udp, [::]:5302->53/tcp, [::]:5302->53/udp   srv02-unbound-secondary

    Veja status do container (UP) + (healthy) isso significa que a resolução já está sendo realizada.

    Acesse o container: docker exec -it srv01-unbound-primary /bin/bash
    root@546fe06263f7:/opt/unbound# dig @localhost dnssec-tools.org +dnssec +trace -4
    
root@546fe06263f7:/opt/unbound# dig @localhost dnssec-tools.org +dnssec

; <<>> DiG 9.18.28-1~deb12u2-Debian <<>> @localhost dnssec-tools.org +dnssec
; (2 servers found)
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 64589
;; flags: qr rd ra ad; QUERY: 1, ANSWER: 5, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags: do; udp: 1232
;; QUESTION SECTION:
;dnssec-tools.org.              IN      A

;; ANSWER SECTION:
dnssec-tools.org.       300     IN      A       185.199.108.153
dnssec-tools.org.       300     IN      A       185.199.110.153
dnssec-tools.org.       300     IN      A       185.199.111.153
dnssec-tools.org.       300     IN      A       185.199.109.153
dnssec-tools.org.       300     IN      RRSIG   A 13 2 300 20250106233514 20241223220514 16472 dnssec-tools.org. ABM8CY7n7LEVrQhtCLPwjcJAXNjdZFFD9ajXSp8CoQTxNWV9kQ1M3qps GwwH+F+t0tkQc8+/PW9QTm96ilU89w==

;; Query time: 242 msec
;; SERVER: 127.0.0.1#53(localhost) (UDP)
;; WHEN: Tue Dec 24 14:40:01 UTC 2024
;; MSG SIZE  rcvd: 221

 Note-se que a flag AD está presente.
 
 Interpretação das Flags
    qr: É uma resposta.
    rd: Indica que o cliente solicitou resolução recursiva.
    ra: O servidor forneceu a resolução recursiva.
    ad: A resposta foi autenticada usando DNSSEC, validando as assinaturas digitais no caminho até o domínio.


## O quê encontrará aqui:

✔️ Servidores de DNS-Unbound + DNSSEC

✔️ Portainer - Gerenciamento Web dos seus containers

✔️ srv01-primary-unbound

✔️ srv01-secondary-unbound

✔️ Servidores vão trabalhar ativo/ativo

## Outros Itens:

✔️ docker-compose.yml

✔️ dockerfile-unbound

✔️ a-records.conf

✔️ srv-records.conf

✔️ root.hints

✔️ unbound.conf

✔️ forward-records.conf


<blockquote>Let's connect and explore opportunities to collaborate in shaping robust network infrastructures for the future!</blockquote>
