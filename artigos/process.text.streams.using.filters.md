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

O comando __cat__ tem como principal funcionalidade a impress&atilde;o do conte&uacute;do contido no arquivo, a partir dessa funcionalidade voc&ecirc;.

(...)

### Caso 1: Imprimindo o texto na tela

Exibe o conte&uacute;do do arquivo _/proc/__$$__/limits_

Comando:

``` bash

    cat /proc/$$/limits

```

Retorno:

``` bash

    Limit                     Soft Limit           Hard Limit           Units
    Max cpu time              unlimited            unlimited            seconds
    Max file size             unlimited            unlimited            bytes
    Max data size             unlimited            unlimited            bytes
    Max stack size            8388608              unlimited            bytes
    Max core file size        0                    unlimited            bytes
    Max resident set          unlimited            unlimited            bytes
    Max processes             31135                31135                processes
    Max open files            1024                 65536                files
    Max locked memory         65536                65536                bytes
    Max address space         unlimited            unlimited            bytes
    Max file locks            unlimited            unlimited            locks
    Max pending signals       31135                31135                signals
    Max msgqueue size         819200               819200               bytes
    Max nice priority         0                    0
    Max realtime priority     0                    0
    Max realtime timeout      unlimited            unlimited            us

```

### Caso 2: Criando um novo arquivo

Comando:

``` bash

    cat >/tmp/teste <\<EOF [enter]
    escreva qualquer coisa
    EOF

```

### Caso 3: Compactando e Concatenando arquivos

> Compactando

Comando:

``` bash

    cat /etc/mtab  | gzip -  > /tmp/mtab.gzip

```

Comando:

``` bash

    file /tmp/mtab.gzip

```

Retorno:

``` bash

    /tmp/mtab.gzip: gzip compressed data, last modified: Mon Oct 10 14:44:12 2016, from Unix

```

> Dividindo o arquivo mtab.gzip

Comando:

``` bash

    split -n 2 /tmp/mtab.gzip  mtab.gzip-

```

> Confira os arquivos gerado pelos comando *_split_*

Comando:

``` bash

    ls -l mtab.gzip-*

```

Retorno:

``` bash

    -rw-rw-r-- 1 enilton enilton 411 Out 10 11:49 mtab.gzip-aa
    -rw-rw-r-- 1 enilton enilton 411 Out 10 11:49 mtab.gzip-ab

```

> Remova a referência mtab.gzip do sistema de arquivos

Comando:

``` bash

    rm mtab.gzip

```

> Concatene os arquivos mtab.gzip-a{a,b}

Comando:

``` bash

    cat /tmp/mtab.gzip-a{a,b} > /tmp/mtab.gzip ou cat /tmp/mtab.gzip-aa /tmp/mtab.gzip-ab  > /tmp/mtab.gzip

```

> Confira o tipo do novo arquivo *_mtab.gzip_*

Comando:

``` bash

    file /tmp/mtab.gzip

```

Retorno:

``` bash

    /tmp/mtab.gzip: gzip compressed data, last modified: Mon Oct 10 14:44:12 2016, from Unix

```

### Caso 4: Descompactando o arquivo mtab.gzip 

> Descompactando o arquivo *_mtab.gzip_*

Comando:

``` bash

    cat mtab.gzip | gunzip --suffix=.gzip -

```

Retorno:

``` bash

    sysfs /sys sysfs rw,nosuid,nodev,noexec,relatime 0 0
    proc /proc proc rw,nosuid,nodev,noexec,relatime 0 0
    udev /dev devtmpfs rw,nosuid,relatime,size=4018876k,nr_inodes=1004719,mode=755 0 0
    devpts /dev/pts devpts rw,nosuid,noexec,relatime,gid=5,mode=620,ptmxmode=000 0 0
    tmpfs /run tmpfs rw,nosuid,noexec,relatime,size=807776k,mode=755 0 0
    /dev/sda8 / ext4 rw,relatime,errors=remount-ro,data=ordered 0 0
    securityfs /sys/kernel/security securityfs rw,nosuid,nodev,noexec,relatime 0 0
    tmpfs /dev/shm tmpfs rw,nosuid,nodev 0 0
    tmpfs /run/lock tmpfs rw,nosuid,nodev,noexec,relatime,size=5120k 0 0
    tmpfs /sys/fs/cgroup tmpfs rw,mode=755 0 0
    cgroup /sys/fs/cgroup/systemd cgroup rw,nosuid,nodev,noexec,relatime,xattr,release_agent=/lib/systemd/systemd-cgroups-agent,name=systemd 0 0
    pstore /sys/fs/pstore pstore rw,nosuid,nodev,noexec,relatime 0 0
    efivarfs /sys/firmware/efi/efivars efivarfs rw,nosuid,nodev,noexec,relatime 0 0
    cgroup /sys/fs/cgroup/hugetlb cgroup rw,nosuid,nodev,noexec,relatime,hugetlb,release_agent=/run/cgmanager/agents/cgm-release-agent.hugetlb 0 0
    cgroup /sys/fs/cgroup/pids cgroup rw,nosuid,nodev,noexec,relatime,pids,release_agent=/run/cgmanager/agents/cgm-release-agent.pids 0 0
    cgroup /sys/fs/cgroup/freezer cgroup rw,nosuid,nodev,noexec,relatime,freezer 0 0
    cgroup /sys/fs/cgroup/cpu,cpuacct cgroup rw,nosuid,nodev,noexec,relatime,cpu,cpuacct 0 0
    cgroup /sys/fs/cgroup/net_cls,net_prio cgroup rw,nosuid,nodev,noexec,relatime,net_cls,net_prio 0 0
    cgroup /sys/fs/cgroup/cpuset cgroup rw,nosuid,nodev,noexec,relatime,cpuset,clone_children 0 0
    cgroup /sys/fs/cgroup/memory cgroup rw,nosuid,nodev,noexec,relatime,memory 0 0
    cgroup /sys/fs/cgroup/devices cgroup rw,nosuid,nodev,noexec,relatime,devices 0 0
    cgroup /sys/fs/cgroup/blkio cgroup rw,nosuid,nodev,noexec,relatime,blkio 0 0
    cgroup /sys/fs/cgroup/perf_event cgroup rw,nosuid,nodev,noexec,relatime,perf_event,release_agent=/run/cgmanager/agents/cgm-release-agent.perf_event 0 0
    systemd-1 /proc/sys/fs/binfmt_misc autofs rw,relatime,fd=34,pgrp=1,timeout=0,minproto=5,maxproto=5,direct 0 0
    hugetlbfs /dev/hugepages hugetlbfs rw,relatime 0 0
    mqueue /dev/mqueue mqueue rw,relatime 0 0
    debugfs /sys/kernel/debug debugfs rw,relatime 0 0
    fusectl /sys/fs/fuse/connections fusectl rw,relatime 0 0
    /dev/sda7 /boot ext2 rw,relatime,block_validity,barrier,user_xattr,acl,stripe=4 0 0
    /dev/sda10 /home ext4 rw,relatime,errors=remount-ro,data=ordered 0 0
    /dev/sda1 /boot/efi vfat rw,relatime,fmask=0077,dmask=0077,codepage=437,iocharset=iso8859-1,shortname=mixed,errors=remount-ro 0 0
    binfmt_misc /proc/sys/fs/binfmt_misc binfmt_misc rw,relatime 0 0
    cgmfs /run/cgmanager/fs tmpfs rw,relatime,size=100k,mode=755 0 0
    tmpfs /run/user/1000 tmpfs rw,nosuid,nodev,relatime,size=807776k,mode=700,uid=1000,gid=1000 0 0
    gvfsd-fuse /run/user/1000/gvfs fuse.gvfsd-fuse rw,nosuid,nodev,relatime,user_id=1000,group_id=1000 0 0

```

> Descompactando o arquivo, redirecionando a saída

Comando:

``` bash

    cat mtab.gzip | gunzip --suffix=.gzip  -  > /tmp/mtab

```

### Caso especial: Baixando o index do site www.google.com

> Socket TCP

Comando:

``` bash

    exec 3<>/dev/tcp/www.google.com/80

```

``` bash

    echo -e "GET / \n"   >&3

```

``` bash

    timeout 2 cat <&3

```

Retorno:

``` html

    HTTP/1.0 302 Found
    Cache-Control: private
    Content-Type: text/html; charset=UTF-8
    Location: http://www.google.com.br/?gfe_rd=cr&ei=9rT7V6DOL8_M8AetuYKoDw
    Content-Length: 262
    Date: Mon, 10 Oct 2016 15:34:14 GMT
    <HTML><HEAD><meta http-equiv="content-type" content="text/html;charset=utf-8">
    <TITLE>302 Moved</TITLE></HEAD><BODY>
    <H1>302 Moved</H1>
    The document has moved
    <A HREF="http://www.google.com.br/?gfe_rd=cr&amp;ei=9rT7V6DOL8_M8AetuYKoDw">here</A>.
    </BODY></HTML>

```

### Troubleshooting - Laboratório DOS file

> "Sim, aquilo era uma banana! Ninguém espera por uma banana!"

No arquivo /tmp/mtab adicione o _carriage returns ('\r', 0x0D)_

para simular um arquivo DOS enviado via ftp de uma estação Windows para um servidor Linux.

O conteúdo original: _sysfs /sys sysfs rw,nosuid,nodev,noexec,relatime 0 0_

> A conversão Unix - DOS

Comando:

``` bash

    cat /tmp/mtab.gzip | gunzip --suffix=.gzip - | head -n 1 | sed 's/$/\r/g;s/ / _\t_\r/g'  > /tmp/mtab

```

> Ninguém espera por uma banana!

Comando:

``` bash

    cat /tmp/mtab

```

Retorno:

``` bash

    0 ,nosuid,nodev,noexec,relatime

```

> Como o 0 (zero) foi parar no início da linha?

Comando:

``` bash

    cat -E /tmp/mtab

```

Retorno:

``` bash

    $ ,nosuid,nodev,noexec,relatime

```

A opção _E_ do comando _cat_ mostra o _$_ no fim de cada linha

O _$_ está no início, mas diferente da inversão de Brunhes–Matuyama, sabemos o que motivou este evento.

> Você acredita em _ET_?

Comando:

``` bash

    cat -ET /tmp/mtab

```

Retorno:

``` bash

    $ ^Iosuid,nodev,noexec,relatime ^I

```

Estava escondido entre o texto, mas a opção _T_ abstraiu a tabulação e exibiu o caractere _^I_.

> "Me mostre a verdadeira identidade do ser que estou enfrentando!"

Comando:

``` bash

    cat -vET /tmp/mtab

```

Retorno:

``` bash

    sysfs ^I^M/sys ^I^Msysfs ^I^Mrw,nosuid,nodev,noexec,relatime ^I^M0 ^I^M0^M$

```

Comando:

``` bash

    cat /tmp/mtab | od -c

```

Retorno:

``` bash

    0000000   s   y   s   f   s      \t  \r   /   s   y   s      \t  \r   s
    0000020   y   s   f   s      \t  \r   r   w   ,   n   o   s   u   i   d
    0000040   ,   n   o   d   e   v   ,   n   o   e   x   e   c   ,   r   e
    0000060   l   a   t   i   m   e      \t  \r   0      \t  \r   0  \r  \n
    0000100

```

O comando _od -c_ tem como principal funcionalidade fazer o dump do arquivo em octal e no exemplo a opção _-c_ imprimiu os carecteres.

> A conversão DOS - Unix

Comado:

``` bash

    sed -r -i  's/(\t)?\r//g' /tmp/mtab

```

Comando:

``` bash

    cat /tmp/mtab

```

Retorno:

``` bash

    sysfs /sys sysfs rw,nosuid,nodev,noexec,relatime 0 0

```

Comando:

``` bash

    cat /tmp/mtab | od -c

```

Retorno:

``` bash

    0000000   s   y   s   f   s       /   s   y   s       s   y   s   f   s
    0000020       r   w   ,   n   o   s   u   i   d   ,   n   o   d   e   v
    0000040   ,   n   o   e   x   e   c   ,   r   e   l   a   t   i   m   e
    0000060       0       0  \n
    0000065

```

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