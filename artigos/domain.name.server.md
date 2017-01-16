# Servidor DNS

Objetivos:

1. Configuração básica do servidor DNS
2. Criar e manter Zonas DNS
3. Segurança

## Configuração básica do servidor DNS

Termos e utilitário

+ /etc/named.conf  (LPI)
+ /etc/bind/named.conf (Debian like)
+ /var/cache/name
+ /usr/bin/rndc
+ kill
+ dig
+ host

### Principais componentes do *_BIND_*

+ /usr/sbin/named
+ /usr/sbin/rndc
+ /usr/sbin/named-checkconf
+ /etc/init.d/bind
+ /var/named
+ /var/cache/bind

> A resolução do nome é controlada pelo o arquivo _nsswitch.conf_

#### O arquivo named.conf

É o principal arquivo de configuração do _BIND_.

> Use o *_named-bootconf.sh_* para converter arquivo de configuração do _Bind 4_ para o _Bind 9_

### DNS caching-only

Um servidor de nomes configurado como _caching-only_, resolve os nomes que estão armazenados no _cache_, isto é feito para que sejam acessados rapidamente quandou o _named_ for consultado.

Exemplo de configuração de um *_caching-only_*.

```text
options {
    directory "/var/named";
};

logging {
    category lame-servers { null; };
    category cname { null; };
};

zone "." {
    type hint;
    file "/etc/bind/db.root";
};

zone "localhost" {
    type master;
    file "/etc/bind/db.local";
}

zone "127.in-addr.arpa" {
    type master;
    file "/etc/bind/db.127";
};

zone "0.in-addr.arpa" {
    type master;
    file "/etc/bind/db.0";
};

zone "255.in-addr.arpa" {
    type master;
    file "/etc/bind/db.255";
}
```

> Nota: Debian bind package

> Nota: Todos os tipos de comentários são permitidos, exemplo.., // e # no final de cada linha.

### A declaração *_options_*

+ *_directory_*  Especifica o diretório de trabalho para o daemon named.
+ *_forwarders_* Contém um ou mais endereço IP de servidores DNS para consulta.
+ *_forward_* Dois valores podem ser especificados _forward first_ e _forward only_
    + _first_ A consulta é enviada primeiro para o nameserver' especificado em *forwarders* e se este falhar procura em outro lugar;
    + _only_ A consulta é limitada somente para o nameserver especificado em *forwarders*
+ *_version_* É possível consultar a versão do bind
+ *_Dialup_* Quando um nameserver está atrás de um firewall, que conecta-se para o mundo através de um conexão _dialup_

Exemplo

+ Ambos *_forwarders_* e *_forward_*

```text
options {
    forwarders {
        123.12.134.2;
        123.12.134.3;
    }
    forward only;
};
```

+ Consulta da versão

```text
dig @ns2.foo.com.br version.bind chaos txt

; <<>> DiG 9.10.3-P4-Ubuntu <<>> @ns2.foo.com.br version.bind chaos txt
; (1 server found)
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 59094
;; flags: qr aa rd; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1
;; WARNING: recursion requested but not available

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 4096
;; QUESTION SECTION:
;version.bind.			CH	TXT

;; ANSWER SECTION:
version.bind.		0	CH	TXT	"9.7.3"

;; Query time: 407 msec
;; SERVER: 209.239.114.36#53(209.239.114.36)
;; WHEN: Sat Jan 14 17:09:53 BRT 2017
;; MSG SIZE  rcvd: 59
```

Altere o resultado da consulta sobrescrecendo a configuração no arquivo *_named.conf_*

```text
version none;
```

+ *_Dialup_*

```text
heartbeat-interval 0;
dialup yes;
```

### A declaração logging

O _channel_ especifica a saída. O _null_ channel, por exemplo, rejeita qualquer saída enviada para o _channel_.

O _category_ é um tipo de dados. O category _security_ é um de muitas categorias. 

Exemplo: Security

+ Category: Security
+ Channel: default_syslog

```text
logging { 
    category security { default_syslog; };
}
```

Exemplo: Desabilitar o logging

```text
logging {
    category lame-servers { null; };
    category cname { null ; };
}
```

### A declaração Zone

A _zone_ definida no _named.conf_ pode ser referenciada usando o simbolo "@" dentro do arquivo que corresponde a zona.

Exemplo:

```text
zone "127.in-addr.arpa" {
    type master;
    file "/etc/bind/db.127";
};
```

> Nota: O "@" é chamado de *_current origin_*


## O daemon named server

O _named_ é o programa que se comunica com outros servidores DNS para resolver nomes. Aceita consultas e pesquisas no cache e consultas em outros servidores de nomes.

### O rndc

O *_rndc_*  (*R*emote *N*ame *D*aemon *C*ontrol) é o programa para controlar o daemon *_named_* localmente e remotamente. Requer uma chave.

+ /etc/rndc.key

```text
key "rndc-key" {
    algorithm hmac-md5;
    secret "kudhfsdufhuihefdhfif==";
};

options {
    default-key "rndc-key";
    default-server 127.0.0.1;
    default-port 953;
};
```

+ /etc(/bind)?/named.conf

```text
key "rndc-key" {
    algorithm hmac-md5;
    secret "kudhfsdufhuihefdhfif==";
};

controls {
    inet 127.0.0.1 port 953
        allow { 127.0.0.1; } keys { "rndc-key"; };
};
```




