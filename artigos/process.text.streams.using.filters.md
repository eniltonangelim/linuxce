# Processar stream de texto usando filtros

> O candidato deve ser capaz de aplicar filtros

## Objetivo

> Enviar *arquivos de texto* e *stream de sa&iacute;da* atrav&eacute;s de filtros para modificar o conte&uacute;do usando comandos UNIX do pacote textutils

## Termos e Utilit&aacute;rios

 + /bin/cat [ok]
 + /usr/bin/cut
 + /usr/bin/expand
 + /usr/bin/fmt
 + /usr/bin/head [ok]
 + join
 + less
 + nl
 + /usr/bin/od [ok]
 + paste
 + pr
 + /bin/sed [ok]
 + sort
 + /usr/bin/split [ok]
 + tail
 + tr
 + unexpand
 + uniq
 + wc

## Sobre o conteúdo

> Nesse artigo pretendo ser objetivo no desenvolvimento do conteúdo, criando exemplos práticos e aplicáveis na rotina de trabalho.

> Os itens da lista termos e utilitários não serão apresentandos isoladamente; execute atentamente o bloco de comandos.

> Dicas:

 + Execute o bloco de comando;
 + Faça um autoquestionamento sobre o que entendeu e o que não entendeu;
 + Se não entender uma sequencia de comandos, remova da esquerda para direita um comando de cada vez;


## Utilit&aacute;rios

### CAT -  Concatena arquivos

Sintaxe:

``` bash

    /bin/cat {(opção),..}  {[arquivo],..}

```

Opções:

 + -v: Mostra os símbolos não-imprimíveis
 + -E: Mostra o símbolo *$* no final da linha
 + -T: Mostra o símbolo *^I* para identificar uma tabulação
 + -e: equivalente para -vE
 + -t: equivalente para -vT

### CUT - Imprime as partes selecionadas da linha de cada arquivos para a saída padrão

Sintaxe:

```bash

    /usr/bin/cut {(opção),..} {[arquivo],..}

```

Opções:

 + -d: Use um delimitador diferente de tabulação
 + -f: Use para selecionar a lista de campos
 + --output-delimiter=STR: Substitua o delimitador na saída

### EXPAND - Converte tabulação em espaço


Sintaxe:

```bash

    /usr/bin/expand {(opção),..} {[arquivo],..}

```

Opções:

 + -a: Converte todos os espaços em brancos
 + -t, --tabs=N: Define a quantidade de espaços em vez do valor, padrão, 8.

### FMT - Formata e otimiza arquivo de texto


Sintaxe:

``` bash

    /usr/bin/fmt (option) [file]

```

Opções:

 + -t, --targed-paragraph: Indentation a primeira linha de forma diferente da segunda linha do parágrafo
 + -u, --uniform-spacing: Usa um espaço entre palavras, dois espaços após a sentença

### HEAD - Imprime o cabeçalho do arquivo na saída padrão

Sintaxe:

``` bash

    /usr/bin/head {(opção),..} {[arquivo],..}

```

Opções:

 + -n: Especifique o número de linhas do cabeçalho

### JOIN - Junta as linhas de dois arquivos que compartilham um campo comum

Sintaxe:

``` bash

    /usr/bin/join {(opção),...} {[arquivo],...}

```

Opções:

 + -i, --ignore-case: Ignora as diferenças entre os carecteres
 + --check-order: Se os campos estão ordenados

### LESS - Paginador

``` bash

    /usr/bin/less {(opção),...} [arquivo]

```

Opções:

 Pesquisa

 + /**_padrão_**: Busca normal
 + ?**_padrão_**: Busca recursiva/inversa
 + &/**_padrão_**: Mostra somente as linhas que combinam com o padrão
 + n: Repete a busca anterior, buscando novas ocorrências

### NL - Adiciona o número da linha

Sintaxe:

``` bash

    /usr/bin/nl {(opção),...} {[arquivo],..}

```

Opções:

 + -s, --number-separator=STRING: Adiciona uma _string_ após o número da linha
 + -v, --starting-line-number=NUMBER: Altera o valor da ordenação


### OD - Dump files in octal

Sintaxe:

``` bash

    /usr/bin/od {(opção),...} {[arquivo],...}

```

Opções:

 + -c: Imprime os carecteres, incluindo os não imprimíveis como: espaço, tabulação.
 + -b: Formato octal
 + -x: Formato hexadecimal

### PASTE - Unir as linhas dos arquivos

Sintaxe

``` bash

    /usr/bin/paste {(opção),...} {[arquivo],...}

```

Opções:

 + -d: delimitador
 + -s: Junta todas as linhas em uma
 + -: Uma coluna
 + - -: Duas colunas
 + - - -: Três colunas

### PR - Converte o arquivo para o Formato padrão das impressoras

Sintaxe:

``` bash

    /usr/bin/pr {(opção),...} {[arquivo],..}

```

Opções:

 + -n, --number-lines[=SEP[DIGITS]]: Adiciona a numeração da linha
 + -h, --header=HEADER: Usa o cabeçalho centralizado

### SED - stream editor

Sintaxe:

``` bash

    /bin/sed {(opção),..} {script} {[arquivo]}

```

Opções:

 + -r: Extensão da expressão regular
 + -i[SUFFIX], --in-place[=SUFFIX]: Edita o arquivo, criando um backup se o _SUFFIX_ for especificado

### SORT - Ordena as linhas do arquivo de texto

Sintaxe:

``` bash

    /usr/bin/sort {(opção),..} {[arquivo],..}

```

### SPLIT

Sintaxe:

``` bash

    /usr/bin/split {(opção),..} {[arquivo],..}

```

Opções:

 + -b, --bytes=SIZE: Tamanho em bytes por arquivo
 + -a, --suffix-length=N: Adiciona um sufixo para os arquivos
 + -n, --number=CHUNKS: Divide o arquivo em pedaços de acordo com o tamanho do arquivo

### TAIL

Sintaxe:

``` bash

    /usr/bin/tail {(opção),..} {[arquivo],..}

```

Opções:

 + -n, --lines=[+]NUM: Imprime às últimas NUM linhas, diferente do padrão 10. O _+_ indica a linha de inicio.

### TR - Traduz ou deleta caracteres

Sintaxe:

``` bash

    /usr/bin/tr {(opção)} expressão1 [expressão2]

```

Opções:

 + -d: Deleta caracteres da expressão1
 + -s: Comprime caracteres repetidos

### UNEXPAND - Converte espaço em tabulação

Sintaxe:

``` bash

    /usr/bin/unexpand {(opção),..} {[arquivo],..}

```

Opções:

 + -a: Converte todos os espaços em brancos
 + -t, --tabs=N: Define a quantidade de espaços em vez do valor, padrão, 8.

### UNIQ - Reporta ou omite linhas repetidas

Sintaxe:

``` bash

    /usr/bin/uniq {(opção),..} {[arquivo],..}

```

Opções:

 + -c: Conta a quantidade de ocorrências
 + -d: Imprime somente as linhas duplicadas, uma de cada grupo
 + -D: Imprime todas as linhas duplicadas
 + -u: Imprime somente as linhas que não são duplicadas

### WC - Mostra o número de linhas, palavras e bytes existentes dentro de cada arquivo

Sintaxe:

``` bash

    /usr/bin/wc {(opção),..} {[arquivo]}

```

Opções:

 + -c, --bytes: Quantidade de bytes
 + -l, --lines: Quantidade de linhas
 + -w, --words: Quantidade de palavras
 + -m, --chars: Quantidade de caracteres
 
## Concatenando, imprimindo, substituindo e filtrando

> Quero diminuir a curva de aprendizado, mas sem perder a qualidade do conteúdo. Então, vamos explorar o máximo possível as funcionalidades que esta sessão nos permite.

### Caso 1: Imprimindo o texto na tela

O uso básico do comando _/bin/cat_ permite-nos acessar o conteúdo do arquivo _/proc/__$$__/limits_ para leitura na tela

Comando:

``` bash

    /bin/cat /proc/$$/limits

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

Propósito: Conhecer o bloco de código especial _here document_. Usamos o redirecionamento de entrada e saída para fornecer dados a um programa ou comando

Comando:

``` bash

    /bin/cat >/tmp/teste << EOF [enter]
    bloco de código foobar
    EOF

```

 + \>/tmp/teste: Redirecionamento da saída padrão para o arquivo /tmp/teste
 + << _string_: Here document
 + EOF: Acrônimo para _End of File_, essa _string_ é responsável por limitar o bloco de código.
 + bloco: Entrada de dados

<hr>

> Importante: O limitador é de livre escolha

<hr>

### Caso 3: Compactando e Concatenando arquivos

Propósito: Praticar gerencimanto de arquivo e conhecer a função do hífen (-).


``` bash

    # Redirecionamento da saída do comando cat para a entrada padrão (-) do comando gzip

    /bin/cat /etc/mtab  | /bin/gzip -  > /tmp/mtab.gzip

```

``` bash

    # O comando file determina o tipo do arquivo

    /usr/bin/file /tmp/mtab.gzip


    # A saída do comando file confirma que /tmp/mtab.gzip é do tipo gzip

    /tmp/mtab.gzip: gzip compressed data, last modified: Mon Oct 10 14:44:12 2016, from Unix

```


``` bash

    # O arquivo /tmp/mtab.gzip será dividido em dois.

    /usr/bin/split -n 2 /tmp/mtab.gzip  mtab.gzip-

```


``` bash

    # Liste as duas partes

    ls -l mtab.gzip-*
    -rw-rw-r-- 1 enilton enilton 411 Out 10 11:49 mtab.gzip-aa
    -rw-rw-r-- 1 enilton enilton 411 Out 10 11:49 mtab.gzip-ab

```

``` bash

    # Remova a referência mtab.gzip do sistema de arquivos

    rm mtab.gzip

```

``` bash

    # Concatenando os arquivos mtab.gzip-aa e mtab.gzip-ab para gerar o novo mtab.gzip.

    /bin/cat /tmp/mtab.gzip-a{a,b} > /tmp/mtab.gzip ou cat /tmp/mtab.gzip-aa /tmp/mtab.gzip-ab  > /tmp/mtab.gzip

```

``` bash

    # Confirme o tipo do novo arquivo mtab.gzip

    /usr/bin/file /tmp/mtab.gzip

    # A saída do comando file confirma que /tmp/mtab.gzip é do tipo gzip

    /tmp/mtab.gzip: gzip compressed data, last modified: Mon Oct 10 14:44:12 2016, from Unix

```

<hr>

> Importante: A referência do arquivo mtab.gzip deixará de existir na área de controle de dados do _filesystem_ por intermédio do Kernel, mas os dados serão preservados até serem sobrescritos.

Pesquise:

 + wipe
 + foremost
 + Sleuth Kit
 + icat
 + tune2fs

<hr>

### Caso 4: Descompactando o arquivo mtab.gzip 

> Descompactando o arquivo *_mtab.gzip_*

Comando:

``` bash

    /bin/cat mtab.gzip | gunzip --suffix=.gzip -

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

    /bin/cat mtab.gzip | gunzip --suffix=.gzip  -  > /tmp/mtab

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

    /bin/cat /tmp/mtab.gzip | gunzip --suffix=.gzip - | /usr/bin/head -n 1 | /bin/sed 's/$/\r/g;s/ / _\t_\r/g'  > /tmp/mtab

```

> Ninguém espera por uma banana!

Comando:

``` bash

    /bin/cat /tmp/mtab

```

Retorno:

``` bash

    0 ,nosuid,nodev,noexec,relatime

```

> Como o 0 (zero) foi parar no início da linha?

Comando:

``` bash

    /bin/cat -E /tmp/mtab

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

    /bin/cat -ET /tmp/mtab

```

Retorno:

``` bash

    $ ^Iosuid,nodev,noexec,relatime ^I

```

Estava escondido entre o texto, mas a opção _T_ abstraiu a tabulação e exibiu o caractere _^I_.

> "Me mostre a verdadeira identidade do ser que estou enfrentando!"

Comando:

``` bash

    /bin/cat -vET /tmp/mtab

```

Retorno:

``` bash

    sysfs ^I^M/sys ^I^Msysfs ^I^Mrw,nosuid,nodev,noexec,relatime ^I^M0 ^I^M0^M$

```

Comando:

``` bash

    /bin/cat /tmp/mtab | /usr/bin/od -c

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

    /bin/sed -r -i  's/(\t)?\r//g' /tmp/mtab

```

Comando:

``` bash

    /bin/cat /tmp/mtab

```

Retorno:

``` bash

    sysfs /sys sysfs rw,nosuid,nodev,noexec,relatime 0 0

```

Comando:

``` bash

    /bin/cat /tmp/mtab | od -c

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

 + /bin/cat -v -E -T
 + /bin/sed -r -i
 + /usr/bin/od -c
 + /usr/bin/head -n
 + gunzip -
 + gzip -
 + ls -l
 + /usr/bin/split -n
 + rm
 + /usr/bin/file
 + echo -e
 + exec
 + Variável _$$_
 + Expansão {..}
