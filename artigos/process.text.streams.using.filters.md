# Processar stream de texto usando filtros

> O candidato deve ser capaz de aplicar filtros

## Objetivo

> Enviar *arquivos de texto* e *stream de sa&iacute;da* atrav&eacute;s de filtros para modificar o conte&uacute;do usando comandos UNIX do pacote textutils

## Termos e Utilit&aacute;rios


 + cat
 + cut
 + expand
 + fmt
 + head
 + join
 + less
 + nl
 + od
 + paste
 + pr
 + sed
 + sort
 + split
 + tail
 + tr
 + unexpand
 + uniq
 + wc

### Concatenando arquivos e imprimindo-os na sa&iacute;da padr&atilde;o

> O comando __cat__ tem como principal funcionalidade a impress&atilde;o do conte&uacute;do contido no arquivo, a partir dessa funcionalidade voc&ecirc; consegue realizar os procedimentos a seguir:

>> + Exibe o texto na tela
>> + Ler arquivos
>> + Cria um novo arquivo
>> + __Concatenar__ os arquivos
>> + Modificar um arquivo

#### Caso 1: Ler o texto na tela

> Para o nosso primeiro caso, vou exibir o conte&uacute;do do arquivo _/proc/__$$__/limits_

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

#### Case 2: Criando um novo arquivo

> Para o nosso segundo caso, vou criar um arquivo
