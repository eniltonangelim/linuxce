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

### O utilitário _named-checkconf_

O named-checkconf verifica somente o arquivo named.conf. Configure o arquivo _named.checkconf_ para incluir as demais configurações no PATH.

### Enviando sinais para o named

É possível enviar sinais para o processo que controla o daemon named. A lista completa pode ser encontrada no manual do named.

Exemplo

+ SIGHUP: Reload named.conf

```text
kill -HUP $pid
```

+ SIGTERM, SIGINT: Shutdown server

```text
kill -INT $pid
```

```text
kill -TERM $pid
```

### O utilitário _host_

O comando _host_ é usado para consultar os _nameservers_.

Opções

+ -t: Tipo da consulta
+ -v: verbose
+ -a: Equivalente para _-v -t ANY_
+ -d: verbose
+ -i: IPV6 reverso
+ -m: memory debugging flag (trace|record|usage)

ANY: host -a foobar.com.br *_ou_* host -v -t ANY foobar.com.br

```text
Trying "foobar.com.br"
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 38080
;; flags: qr rd ra; QUERY: 1, ANSWER: 4, AUTHORITY: 2, ADDITIONAL: 2

;; QUESTION SECTION:
;foobar.com.br.			IN	ANY

;; ANSWER SECTION:
foobar.com.br.		600	IN	CNAME	wikidot.com.
foobar.com.br.		600	IN	SOA	dns1.superdns.com.br. abuse.superdns.com.br. 2008082802 3600 600 1209600 86400
foobar.com.br.		600	IN	NS	dns2.superdns.com.br.
foobar.com.br.		600	IN	NS	dns1.superdns.com.br.

;; AUTHORITY SECTION:
foobar.com.br.		600	IN	NS	dns1.superdns.com.br.
foobar.com.br.		600	IN	NS	dns2.superdns.com.br.

;; ADDITIONAL SECTION:
dns1.superdns.com.br.	86400	IN	A	69.28.88.238
dns2.superdns.com.br.	86400	IN	A	104.219.55.212

Received 205 bytes from 192.168.124.1#53 in 168 ms
```

MX: host -v -t mx foobar.com.br

```text
Trying "foobar.com.br"
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 19834
;; flags: qr rd ra; QUERY: 1, ANSWER: 8, AUTHORITY: 4, ADDITIONAL: 4

;; QUESTION SECTION:
;foobar.com.br.			IN	MX

;; ANSWER SECTION:
foobar.com.br.		597	IN	CNAME	wikidot.com.
wikidot.com.		604782	IN	MX	5 alt2.aspmx.l.google.com.
wikidot.com.		604782	IN	MX	10 aspmx4.googlemail.com.
wikidot.com.		604782	IN	MX	10 aspmx2.googlemail.com.
wikidot.com.		604782	IN	MX	10 aspmx3.googlemail.com.
wikidot.com.		604782	IN	MX	1 aspmx.l.google.com.
wikidot.com.		604782	IN	MX	5 alt1.aspmx.l.google.com.
wikidot.com.		604782	IN	MX	10 aspmx5.googlemail.com.

;; AUTHORITY SECTION:
wikidot.com.		160532	IN	NS	ns-1873.awsdns-42.co.uk.
wikidot.com.		160532	IN	NS	ns-532.awsdns-02.net.
wikidot.com.		160532	IN	NS	ns-1221.awsdns-24.org.
wikidot.com.		160532	IN	NS	ns-461.awsdns-57.com.

;; ADDITIONAL SECTION:
ns-461.awsdns-57.com.	160532	IN	A	205.251.193.205
ns-532.awsdns-02.net.	160532	IN	A	205.251.194.20
ns-1221.awsdns-24.org.	160532	IN	A	205.251.196.197
ns-1873.awsdns-42.co.uk. 2798	IN	A	205.251.199.81

Received 433 bytes from 192.168.124.1#53 in 2 ms
```

### O utilitário _dig_

O comando _dig_ tem o mesmo proposito do comando _host_, porém tem mais recursos.

Opções:

+ -x: Atalho para consultar o reverso
+ -t: Tipo da consulta
+ -m: memory debugging
+ \+short: Resposta curta

PTR: dig -x 187.58.65.21

```text
; <<>> DiG 9.10.3-P4-Debian <<>> -x 187.58.65.21
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 33522
;; flags: qr rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 3, ADDITIONAL: 7

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 4096
;; QUESTION SECTION:
;21.65.58.187.in-addr.arpa.	IN	PTR

;; ANSWER SECTION:
21.65.58.187.in-addr.arpa. 42368 IN	PTR	187.58.65.21.static.gvt.net.br.

;; AUTHORITY SECTION:
65.58.187.in-addr.arpa.	42368	IN	NS	dns2.gvt.net.br.
65.58.187.in-addr.arpa.	42368	IN	NS	dns1.gvt.net.br.
65.58.187.in-addr.arpa.	42368	IN	NS	dns3.gvt.net.br.

;; ADDITIONAL SECTION:
dns1.gvt.net.br.	20354	IN	A	200.146.72.5
dns1.gvt.net.br.	20354	IN	AAAA	2804:7f4:2000:2000:200:146:72:5
dns2.gvt.net.br.	20354	IN	A	200.139.125.38
dns2.gvt.net.br.	20354	IN	AAAA	2804:7f4:2000:2000:200:139:125:38
dns3.gvt.net.br.	20354	IN	A	186.215.69.150
dns3.gvt.net.br.	20354	IN	AAAA	2804:7f4:2000:2000:186:215:69:150

;; Query time: 1 msec
;; SERVER: 192.168.124.1#53(192.168.124.1)
;; WHEN: Wed Jan 18 12:57:30 BRT 2017
;; MSG SIZE  rcvd: 287
```

ANY: dig -t any foobar.com.br

```text
; <<>> DiG 9.10.3-P4-Debian <<>> -t any foobar.com.br
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 6277
;; flags: qr rd ra; QUERY: 1, ANSWER: 4, AUTHORITY: 2, ADDITIONAL: 3

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 4096
;; QUESTION SECTION:
;foobar.com.br.			IN	ANY

;; ANSWER SECTION:
foobar.com.br.		600	IN	SOA	dns1.superdns.com.br. abuse.superdns.com.br. 2008082802 3600 600 1209600 86400
foobar.com.br.		600	IN	CNAME	wikidot.com.
foobar.com.br.		600	IN	NS	dns1.superdns.com.br.
foobar.com.br.		600	IN	NS	dns2.superdns.com.br.

;; AUTHORITY SECTION:
foobar.com.br.		600	IN	NS	dns2.superdns.com.br.
foobar.com.br.		600	IN	NS	dns1.superdns.com.br.

;; ADDITIONAL SECTION:
dns1.superdns.com.br.	84639	IN	A	69.28.88.238
dns2.superdns.com.br.	84639	IN	A	104.219.55.212

;; Query time: 176 msec
;; SERVER: 192.168.124.1#53(192.168.124.1)
;; WHEN: Wed Jan 18 13:06:04 BRT 2017
;; MSG SIZE  rcvd: 216
```
