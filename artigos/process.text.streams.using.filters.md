# Processar stream de texto usando filtros

> O candidato deve ser capaz de aplicar filtros

## Objetivo

> Enviar *arquivos de texto* e *stream de sa&iacute;da* atrav&eacute;s de filtros para modificar o conte&uacute;do usando comandos UNIX do pacote textutils

## Termos e Utilit&aacute;rios


 + cat [ok]
 + cut
 + expand
 + fmt
 + head [ok]
 + join
 + less
 + nl
 + od [ok]
 + paste
 + pr
 + sed [ok]
 + sort
 + split [ok]
 + tail
 + tr
 + unexpand
 + uniq
 + wc

## Concatenando, imprimindo, substituindo e filtrando.

> O comando __cat__ tem como principal funcionalidade a impress&atilde;o do conte&uacute;do contido no arquivo, a partir dessa funcionalidade voc&ecirc;.

### Caso 1: Ler o texto na tela

> Exibe o conte&uacute;do do arquivo _/proc/__$$__/limits_

>> cat /proc/$$/limits
>>
>> ##### Resultado
>> Limit                     Soft Limit           Hard Limit           Units<br />
>> Max cpu time              unlimited            unlimited            seconds<br />
>> Max file size             unlimited            unlimited            bytes<br />
>> Max data size             unlimited            unlimited            bytes<br />
>> Max stack size            8388608              unlimited            bytes<br />
>> Max core file size        0                    unlimited            bytes<br />
>> Max resident set          unlimited            unlimited            bytes<br />
>> Max processes             31135                31135                processes<br />
>> Max open files            1024                 65536                files<br />
>> Max locked memory         65536                65536                bytes<br />
>> Max address space         unlimited            unlimited            bytes<br />
>> Max file locks            unlimited            unlimited            locks<br />
>> Max pending signals       31135                31135                signals<br />
>> Max msgqueue size         819200               819200               bytes<br />
>> Max nice priority         0                    0<br />
>> Max realtime priority     0                    0<br />
>> Max realtime timeout      unlimited            unlimited            us<br />

### Caso 2: Criando um novo arquivo

> Oldtimes

>> cat >/tmp/teste <\<EOF [enter]<br />
>> escreva qualquer coisa<br />
>> EOF<br />

### Caso 3: Compactando e Concatenando arquivos

> Compactando

>> cat /etc/mtab  | gzip -  > /tmp/mtab.gzip<br />
>> file /tmp/mtab.gzip<br />
>>> /tmp/mtab.gzip: gzip compressed data, last modified: Mon Oct 10 14:44:12 2016, from Unix<br />

> Dividindo o arquivo mtab.gzip

>> split -n 2 /tmp/mtab.gzip  mtab.gzip-<br />
>> ls -l mtab.gzip-*<br />
>>> -rw-rw-r-- 1 enilton enilton 411 Out 10 11:49 mtab.gzip-aa<br />
>>> -rw-rw-r-- 1 enilton enilton 411 Out 10 11:49 mtab.gzip-ab<br />

> Remove a referência mtab.gzip do sistema de arquivos

>> rm mtab.gzip

> Concatena os arquivos mtab.gzip-a{a,b}

>> cat /tmp/mtab.gzip-a{a,b} > /tmp/mtab.gzip ou cat /tmp/mtab.gzip-aa /tmp/mtab.gzip-ab  > /tmp/mtab.gzip<br />
>> file /tmp/mtab.gzip<br />
>>> /tmp/mtab.gzip: gzip compressed data, last modified: Mon Oct 10 14:44:12 2016, from Unix

### Caso 4: Descompactando o arquivo mtab.gzip 

> Descompactando o arquivo

>> cat mtab.gzip | gunzip --suffix=.gzip -<br />
>>> ...<br />

> Descompactando o arquivo, redirecionando a saída para outro arquivo

>> cat mtab.gzip | gunzip --suffix=.gzip  -  > /tmp/mtab<br />

### Caso especial: Baixando o index do site www.google.com

> Socket TCP

>> exec 3<>/dev/tcp/www.google.com/80<br />
>> echo -e "GET / \n"   >&3<br />
>> timeout 2 cat <&3<br />
>>> HTTP/1.0 302 Found<br />
>>> Cache-Control: private<br />
>>> Content-Type: text/html; charset=UTF-8<br />
>>> Location: http://www.google.com.br/?gfe_rd=cr&ei=9rT7V6DOL8_M8AetuYKoDw<br />
>>> Content-Length: 262<br />
>>> Date: Mon, 10 Oct 2016 15:34:14 GMT
>>> <HTML><HEAD><meta http-equiv="content-type" content="text/html;charset=utf-8"><br />
>>> <TITLE>302 Moved</TITLE></HEAD><BODY><br />
>>> <H1>302 Moved</H1><br />
>>> The document has moved<br />
>>> <A HREF="http://www.google.com.br/?gfe_rd=cr&amp;ei=9rT7V6DOL8_M8AetuYKoDw">here</A>.<br />
>>> </BODY></HTML>

### Troubleshooting - Laboratório DOS file
> "Sim, aquilo era uma banana! Ninguém espera por uma banana!"

> No arquivo /tmp/mtab adicione o _carriage returns ('\r', 0x0D)_
> para simular um arquivo DOS enviado via ftp de uma estação Windows para um servidor Linux.<br />
> O conteúdo original: _sysfs /sys sysfs rw,nosuid,nodev,noexec,relatime 0 0_

> A conversão Unix - DOS

>> cat /tmp/mtab.gzip | gunzip --suffix=.gzip - | head -n 1 | sed 's/$/\r/g;s/ / _\t_\r/g'  > /tmp/mtab

> Ninguém espera por uma banana!

>> cat /tmp/mtab<br />
>>> 0 ,nosuid,nodev,noexec,relatime

> Como o 0 (zero) foi parar no início da linha?

>> cat -E /tmp/mtab<br />
>> _$_ ,nosuid,nodev,noexec,relatime<br />
>>> A opção _E_ do comando _cat_ mostra o _$_ no fim de cada linha<br />
>>> O _$_ está no início, mas diferente da inversão de Brunhes–Matuyama, sabemos o que motivou este evento.

> Você acredita em _ET_?

>> cat -ET /tmp/mtab<br />
>> $ _^I_osuid,nodev,noexec,relatime _^I_<br />
>>> Estava escondido entre o texto, mas a opção _T_ abstraiu a tabulação e exibiu o caractere _^I_.

> "Me mostre a verdadeira identidade do ser que estou enfrentando!"

>> cat -vET /tmp/mtab<br />
>>> sysfs ^I^M/sys ^I^Msysfs ^I^Mrw,nosuid,nodev,noexec,relatime ^I^M0 ^I^M0^M$

>> cat /tmp/mtab | od -c
>>> 0000000   s   y   s   f   s      \t  \r   /   s   y   s      \t  \r   s<br />
>>> 0000020   y   s   f   s      \t  \r   r   w   ,   n   o   s   u   i   d<br />
>>> 0000040   ,   n   o   d   e   v   ,   n   o   e   x   e   c   ,   r   e<br />
>>> 0000060   l   a   t   i   m   e      \t  \r   0      \t  \r   0  \r  \n<br />
>>> 0000100<br />
>>> O comando _od -c_ tem como principal funcionalidade fazer o dump do arquivo em octal e no exemplo<br />
>>> a opção _-c_ imprimiu os carecteres.

> A conversão DOS - Unix

> sed -r -i  's/(\t)?\r//g' /tmp/mtab

> cat /tmp/mtab <br />
>> sysfs /sys sysfs rw,nosuid,nodev,noexec,relatime 0 0

> cat /tmp/mtab | od -c<br />
>> 0000000   s   y   s   f   s       /   s   y   s       s   y   s   f   s<br />
>> 0000020       r   w   ,   n   o   s   u   i   d   ,   n   o   d   e   v<br />
>> 0000040   ,   n   o   e   x   e   c   ,   r   e   l   a   t   i   m   e<br />
>> 0000060       0       0  \n<br />
>> 0000065


> lista de comandos:

 + cut -v -E -T
 + sed -r -i
 + od -c
 + head -n
 + gunzip -
 + gzip -
 + ls -l
 + split -n
 + rm
 + file
 + echo -e
 + exec
 + Variável _$$_
 + Expansão {..}


