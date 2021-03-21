# Tutorial LPIC-1

[TOC]

# Projeto Linux mais fácil

Esta série de tutoriais, ajuda você a aprender as tarefas de administração do sistema Linux. O material desses tutoriais corresponde à versão 4.0 e 5.0 dos exames da LPIC-1.

Os tópicos se espelham nos exames LPIC-1: Linux Server Professional Certification do Linux Professional Institute. Você pode usar os tutoriais para se preparar para a certificação ou pode usá-los para aprender sobre o Linux.

Existem atualmente, dois exames para a certificação LPIC-1: exame 101 e exame 102, e você deve passar em ambos para obter a certificação LPIC-1. Cada tópico tem vários subtópicos e cada subtópicos tem vários objetivos. 

Na produção deste documento, buscou-se obter o máximo de qualidade possível, em respeito ao leitor e o conhecimento que será adquirido. **Entretanto, apesar de ter buscado a perfeição, não é algo fácil de ser alcançado. Por esse motivo, erros podem ter passado despercebidos. Qualquer ajuda no sentido de identificá-los é muito bem vinda**, porém o leitor deveria estar consciente de que **este documento é distribuído sem qualquer garantia, implícita e/ou explícita da Linux Professional Institute**.

É concedida permissão para copiar, distribuir e/ou modificar este Tutorial da LPIC-1.

***



### Nomeclaturas

Segue abaixo as convenções usadas no decorrer do documento:

`EXEMPLO` = Comando ou código curto.
<span style="color:#FFFFFF">**EXEMPLO**</span> = Destaque, para manter o foco no que está sendo explicado.





# 103.1 - Trabalhando na linha de comando

Este tópico aborda os comandos usado no shell, tudo o que você deve saber para operar o sistema, conhecendo seus comandos e funcionalidades.

A maneira usada para interagir com um servidor linux é através do prompt de comando do bash, o prompt possui 3 níveis de "permissões", que são: usuário root (super usuário), usuário comum e usuário comum mas com poderes de usuário root. Porque um usuário comum iria ter poderes de root?, porque quando você usa o comando *sudo*, você invoca o super usuário para realizar a tarefa para você, isso é usado quando se precisa de mais permissão para executar alguma tarefa.

A PS1 do shell exibe informações do nível de privilégio de seu usuário:

<span style="color:#FFFFFF">**$**</span> - Indica um usuário comum;
<span style="color:#FFFFFF">**#**</span> - Indica um super usuário (normalmente o *root*).

A PS1 pode ser editada, e pode omitir essa informação, mas uma PS1 padrão, sempre irá exibir o nível de permissão do seu usuário.

  

## Sequência de comandos

A grande maioria das tarefas no shell depende da execução de mais de um comando, para isso, usamos AND lógico, OU lógico para fazer a execução de vários comandos consecutivos.

<span style="color:#FFFFFF">**;**</span> - O ponto e vírgula executam tudo que vier depois dele, não importa se o comando anterior foi bem ou mal executado (`pwd ; echo "O comando PWD funcionou"` e `arquivo ; echo "O comando arquivo não funcionou"`);

<span style="color:#FFFFFF">**&&**</span> - AND lógico, só executa o comando, se o comando anterior funcionar (`pwd && echo "funcionou"`), outra forma de ver é 'executa 1 e executa 2';

<span style="color:#FFFFFF">**||**</span> - OU lógico, só executa o comando se o comando anterior não funcionou (`arquivo || echo "Comando nao existe"`), outra forma de visualizar é 'executa comando 1 ou executa comando 2'.

  

## Variáveis de ambiente

Aspectos de ambiente do shell é definido e controlado através de variáveis. Uma variável é um espaço alocado na memória *RAM* que recebe um valor, e fica alocado lá por tempo determinado (chamado tempo de execução), toda variável tem um nome e um valor.

**Locais** - Variáveis disponíveis apenas para a sessão atual (cada shell é um novo processo (nova sessão));

**Exportadas** - Disponíveis para a sessão atual e subseções (seções filhas da sua sessão atual), em outras palavras, para processo atual e para subprocessos (**processos filhos**).

 

#### 														Criar variáveis

No linux, a criação de uma variável é bem simples, basta você dar um nome e um valor, como no exemplo abaixo:   

```bash
linux:~/$ POSICAO="4.3"

linux:~/$ POSICAO='echo 4.3'
```

No segundo caso, já adicionei o <span style="color:#FFFFFF">*echo*</span> ao valor da variável, então basta digitar **$POSICAO** e o comando <span style="color:#FFFFFF">*echo*</span> irá mostrar o <span style="color:#FFFFFF">4.3</span>.



Para exibir o valor da variável usamos o comando *echo*, segue abaixo:

```bash
linux:~/$ echo "$POSICAO"
# Exibe o valor da variável;

linux:~/$ $POSICAO
# Nesse caso, como foi colocado um comando dentro da variável, 
# esse comando será executado.
```



#### 																						Exportar variáveis

Isso irá criar o que chamamos de variáveis exportadas, tornando a variável visível para subprocessos do seu shell.

Temos duas formas de exportar, uma é exportar declarando a variável e outra é exportar uma variável já criada, segue exemplo:

```bash
linux:~/$ export POSICAO="4.3" 
# Exportando a variável enquanto cria-se ela;

linux:~/$ export POSICAO 
# Exportando uma variável já existente.
```

  

## SET ou ENV

Para ver as variáveis de ambiente, temos alguns comandos que podem ser usados, os mais conhecidos são <span style="color:#FFFFFF">**env**</span> e <span style="color:#FFFFFF">**set**</span>, esses comandos não tem apenas essa função.

**SET** = Server para definir ou ativar/desativar opções de shell.

**ENV** = É usado para exibir as variáveis de ambiente, também é usado para executar um utilitário ou comando em um ambiente customizado.

Obs.: Os comandos `printenv` e `export` sem argumentos também exibem as variáveis.

O comando `env` lista somente as variáveis "Globais", ou seja, as variáveis que foram exportadas, enquanto o comando `set` lista todas as variáveis.
Para exibir as variáveis digite no terminal `env` ou `set`.



**Remover uma variável**

```bash
linux:~/$ env -u VARIAVEL 
# Remove a variável temporariamente;

linux:~/$ env -u VARIAVEL ./meu_script 
# Remove temporariamente para execução de um script.
```

  

### UNSET - Apagar variáveis de ambiente

Para apagarmos uma variável, usamos o comando '*unset*', ele vem com dois argumentos, o '-f' que remove uma função e o '-v' que remove uma variável, não sendo obrigatório a escolha do -v para remover uma variável.

As variáveis só existem enquanto o Shell existir, para torná-las "imortais", elas devem ser colocadas em arquivos como */etc/profile*, */etc/bash.bashrc* ou nos arquivos *.bashrc* ou *.profile* de sua *HOME*.

- **/etc/profile** - Contém dados que serão carregados para todos os usuários, esse arquivo é executado sempre que um shell de login do bash é carregado, como ao fazer login no console ou através do ssh, também é carregado quando fazemos login pela interface gráfica.

- **/etc/environment** - É usado para definir variáveis para programas que geralmente não são iniciados a partir de um shell, aqui temos a definição da ***PATH***.

```bash
linux:~/$ unset POSICAO

# A variável foi removida, mas se ela estiver sendo declarada em algum lugar, ao abrir um 
# novo shell, essa mesma variável volta a existir.
```



**Algumas variáveis importantes do sistema**

| Nome         | Descrição                                                    |
| ------------ | ------------------------------------------------------------ |
| PATH         | Armazena a localização de diretórios que contém executáveis, a PATH é definida no arquivo /etc/environment; |
| PS1          | Forma como o Shell irá aparecer (NOME@HOST-NAME:~$ );        |
| PS2          | Mesma coisa do PS1, mas ela entra em ação quando um comando tem mais de uma linha, por padrão o caractere é ''>"; |
| HOME         | Mostra o diretório do usuário atual;                         |
| LOGNAME      | Nome do usuário logado atualmente;                           |
| USER         | Nome do usuário logado atualmente, variável padrão para sistemas BSD; |
| SHELL        | Mostra o Shell em uso;                                       |
| HISTFILE     | Arquivo que irá conter os históricos do que for digitado no terminal (padrão é *.bash_history*); |
| HISTFILESIZE | Quantidade máxima de comando que serão gravados no arquivo;  |
| HISTSIZE     | Quantidade máxima de comando que serão gravados na memória;  |
| HOSTTYPE     | Exibe a arquitetura do sistema.                              |



Algumas variáveis embutidas no bash atuam como comandos e retornam um valor que são bem úteis em scripts, são elas:

| Variável | Descrição                                                    |
| -------- | ------------------------------------------------------------ |
| $#       | Armazena o número de argumentos da linha de comando que foram passados para o programa shell. |
| $0       | Armazena a primeira palavra do comando inserido (o nome do programa shell). |
| $*       | Armazena todos os argumentos que foram inseridos no linha de comando ($1 $2 ...). |
| $@       | Armazena todos os argumentos que foram inseridos na linha de comando, citada individualmente ("$1" "$2" ...). |
| $_       | Último argumento do último comando executado.                |
| $!       | PID do último processo executado e background;               |
| $$       | PID do shell atual;                                          |
| $?       | Retorna 0 se o último comando foi bem-sucedido, caso seja diferente de 0 ele não foi. |



## ECHO

Exibe na tela o que for digitado na frente do comando, exemplo:

```bash
linux:~/$ echo "isso aparecerá"
isso aparecerá
```

Esse comando é bem útil em *scripts* e é usado para verificar o que tem dentro de uma variável, exemplo:

```bash
linux:~/$ echo "$SHELL"
/bin/bash

# Nesse caso, o echo está exibindo o valor da variável SHELL.
```



## TYPE

Verifica se o comando é interno do shell ou é um comando externo.

**Interno**: Está embutido dentro do shell;
**Externo**: Foi instalado no Linux, não faz parte do shell;

Exemplo:

```bash
linux:~/$ type echo
echo é um comando interno do shell

# Isso significa que o comando é interno do Shell.
```

Outra saída que você pode ter é: <span style="color:#FFFFFF">echo is a shell builtin</span>, que também significa que é um comando interno do Shell.

```bash
linux:~/$ type tar
tar é /bin/tar

# Nesse caso, o comando tar, é um comando externo do Shell.
```

Ao usar o mesmo programa algumas vezes, esse programa entra em *cache*, para ser carregado mais rápido, usando o tar como exemplo, recebemos o resultado: <span style="color:#FFFFFF">tar está hasheado (/bin/tar)</span> ou <span style="color:#FFFFFF">tar is hashed (/bin/tar)</span> , significando que o programa está em *Hash* e será carregado mais rápido.

**Curiosidade**
A nomeclatura é hashed porque o comando hash verifica no banco de dados db.h, quantas vezes um programa foi usado, caso esse programa não seja interno do bash, ele ficará em cache para que sua execução seja mais rápida.

```bash
# Exemplo do comando hash (mostrando aplicações em cache):
linux:~/$ hash
hits	command
   3	/usr/bin/tar
```



## UNAME

Exibe informações do sistema, como: nome, versão do kernel, arquitetura entre outros.

**Opções:**

```
-a, --all                Exibe todas as informações
-s, --kernel-name        Exibe o nome do kernel
-n, --nodename           Exibe o nome do host do nó da rede (Nome do PC)
-r, --kernel-release     Exibe o kernel release (Versão do kernel de distribuição)
-v, --kernel-version     Exibe a versão do kernel
-m, --machine            Exibe o nome do hardware da máquina
-p, --processor          Exibe o tipo de processador
-i, --hardware-platform  Exibe a plataforma de hardware
-o, --operating-system   Exibe o sistema operacional.
```



**Usando o `-a`:**

```bash
# Exibindo tudo:
linux:~/$ uname -a
Linux wcscpy 4.4.0-151-generic #178-Ubuntu SMP Tue Jun 11 08:30:22 UTC 2019 x86_64 x86_64 x86_64 GNU/Linux'

# Exibindo o nome do kernel:
linux:~/$ uname -s
Linux

# Exibindo a versão de lançamento do kernel:
linux:~/$ uname -r
5.4.0-42-generic

# Exibindo a versao do kernel:
linux:~/$ uname -v
\#46-Ubuntu SMP Fri Jul 10 00:24:02 UTC 2020

# Exibindo o tipo de processador:
linux:~/$ uname -p
x86_64

#### Exibindo informações de um raspberry pi
# Exibindo tudo:
linux:~/$ uname -a
Linux raspberrypi 5.4.51-v7l+ #1327 SMP Thu Jul 23 11:04:39 BST 2020 armv7l GNU/Linux

# Exibindo o nome do kernel:
linux:~/$ uname -s
Linux

# Exibindo a versão de lançamento do kernel:
linux:~/$ uname -r
5.4.51-v7l+

# Exibindo a versao do kernel:
linux:~/$ uname -v
\#1327 SMP Thu Jul 23 11:04:39 BST 2020

# Exibindo o tipo de processador:
linux:~/$ uname -p
unknown
```



## ALIAS

O alias funciona como um "apelido" para comandos, um único *alias* pode executar uma sequência indeterminada de comandos.

#### Declarando um Alias

```bash
# Formato de declaração de alias:
alias NOME='AÇÃO'

# Criando um alias chamado google, que vai pingar para o google.
linux:~/$ alias google='ping www.google.com.br'
```

Agora basta digitar ***google*** no terminal e vai começar a pingar para o endereço do google.



#### Remover um Alias

```bash
# Removendo o alias chamado google:
linux:~/$ unalias google
```



**Deixar o alias permanente**

Para criar o alias e deixar ele permanente, você deve criar ele no seu <span style="color:#FFFFFF">~/.bashrc</span>, mas o mais correto é criar o arquivo <span style="color:#FFFFFF">~/.bash_aliases</span>, isso inclusive e avisado no arquivo <span style="color:#FFFFFF">~/.bashrc</span>.



## QUOTING

É usado para remoção do significado especial de determinados caracteres ou palavras no shell. Para isso, podemos usar um encapsulamento entre aspas ou apóstrofos, tendo diferença no uso de cada um deles.

O encapsulamento pode ser utilizado para desabilitar a interpretação de caracteres especiais, impedindo que palavras especiais sejam reconhecidas como tais.

Em regra, temos dois métodos de encapsulamento e um de escape, sendo: encapsulamento simples, encapsulamento duplo e caractere de escape.



#### Caractere de Escape

Usamos a barra '\\' invertida como um caractere de escape do Bash. Ele atua preservando o valor literal do próximo caractere (aquele que está logo depois da barra invertida). Aqui só temos uma pequena exceção que o uso da barra invertida para quebra de linha, mais conhecido como ***newline***.

Como funciona o *newline* (`Por favor, digite cada caractere para não haver erros`):

```bash
linux:~/$ echo "teste de quebra \
> de linha"
teste de quebra de linha
```

Usando o comando *echo*, adicionamos uma barra invertida e damos um enter, o *newline* é tratado como uma continuação da linha, isso porque, a barra invertida escapa o "enter", impedindo que o Shell interprete ele.



#### Encapsulamento simples

Para isso, usamos o **apóstrofos** (aspas simples como é mais comumente conhecido). O uso do encapsulamento é mais usado, pois, ao usar ele, se mantém o valor literal de tudo que está dentro do encapsulamento. 

Não se deve usar um encapsulamento precedido de uma barra invertida `echo '\$SHELL'`(O próprio encapsulamento simples anula o poder do que estiver dentro dele, nesse caso, a barra invertida é somente uma barra invertida). Cuidado com esse método, pois, ele impede a interpretação total do que estiver dentro dele, por exemplo:

```bash
linux:~/$ echo '$SHELL'
$SHELL
```

Como funciona o Encapsulamento Simples:

Crie uma variável conforme abaixo `(Por favor, digite cada caractere para não haver erros)`:

```bash
linux:~/$ var='ola
> oi
> hello'
```

Digite o comando abaixo:

```bash
linux:~/$ echo '$var'
$var
```

Observe que ele manteve o valor literal de todo conteúdo que estava dentro do encapsulamento, o mesmo acontece se, ao invés de usar o encapsulamento, você usar o caractere de escape (barra invertida `\`).

Se for usado barra invertida dentro do encapsulamento, a barra invertida será apenas uma barra invertida e não um caractere de escape, pois o encapsulamento irá manter o valor literal da barra, que neste caso é apenas ser uma barra invertida.

```bash
linux:~/$ echo 'ola \
> oie'
ola \
oie
```



#### Encapsulamento Duplo

Esse encapsulamento funciona exatamente igual ao encapsulamento simples, porém, ele mantém o valor especial do cifrão `$`, da crase `´` e da barra invertida `\`.

Isso significa que se você encapsular o cifrão, a crase ou a barra invertida dentro de aspas duplas, esses caracteres serão caracteres especiais dentro do encapsulamento.

```bash
linux:~/$ echo "$USER"
joao

# Observe que o encapsulamento duplo mantém o valor do cifrão.
```



## Histórico de comandos

O comando *history* exibe um histórico dos comandos digitados no terminal, o histórico fica “dividido”, uma cópia do histórico fica no arquivo '*.bash_history*' e outra cópia fica na memória (Em cache).

Se você digitar '***history -c***' o comando *history* não vai exibir nada, porque o '-c' apaga o histórico que está na memória, se após isso você digitar '***history -r***' tudo o que estiver no '*.bash_history*' será enviado para a memória.

**Obs**.: Cuidado ao digitar '***history -c***', o sistema não descarrega o que está na memória (histórico em cache) automaticamente no arquivo '.*bash_history*', para que isso ocorra, digite '***history -a***' e tudo o que estiver na memória irá ser jogado no arquivo. 

Caso queira digitar comandos sem que fique gravado no *'history'* você pode usar '***set +o history***' ou dar um espaço antes do comando, isso faz com que o comando não seja enviado para memória/'.*bash_history*'.

#### Executando um comando direto do history

**!!** - Executa o último comando digitado;

**!NUM*** - Executa o comando cujo número você escolheu;

**!texto** - Executa o último comando que começa com o texto;

**!?texto** - Executa o último comando que tenha o termo texto;



## PATH

A *PATH* é definida no arquivo '*/etc/environment*', ela serve para buscar executáveis em diretórios que estejam definidos nela, tornando possível a execução de qualquer executável apenas com o nome dele, não importando sua localização na árvore de diretórios. 

**Definir caminhos**

Temos que tomar cuidado ao definir algum caminho na *PATH*, principalmente se colocarmos isso em algum arquivo como *.bashrc* ou *.profile*.

Para definir um novo diretório dentro da *PATH* use o exemplo abaixo:

```bash
linux:~/$ PATH=$PATH:/mnt

# Use sempre o caminho absoluto, quando for adicionar algum diretório na PATH.
```



## WHICH

O comando *which* exibe a localização de binários, o que isso quer dizer? 
Ele usa o *PATH* para procurar onde está o programa (Esse programa é mais conhecido como Binário).

Exemplo:

```bash
linux:~/$ which tar
/bin/tar
```

O comando *which* é semelhante ao comando *whereis*, porém, o comando *which* só exibe o binário.



## WHEREIS
Seu uso é semelhante ao comando which, porém, ao invés de retornar apenas o binário, ele retorna tudo o que está vinculado a esse programa, como a página man, bibliotecas entre outras coisas.
Exemplo:

```bash
linux:~/$ whereis tar
tar: /usr/lib/tar /bin/tar /usr/share/man/man1/tar.1.gz
```



## APROPOS

Busca as seções de manual do termo que foi digitado.

```bash
linux:~/$ apropos security
apparmor.d (5)         - syntax of security profiles for AppArmor.
chcon (1)              - change file security context
hardening-check (1)    - check binaries for security hardening features
nmap (1)               - Network exploration tool and security / port scanner
ntfs-3g.secaudit (8)   - NTFS Security Data Auditing
pam_selinux (8)        - PAM module to set the default security context
perlsec (1)            - Perl security
runcon (1)             - run command with specified security context
security (2)           - unimplemented system calls
subdomain.conf (5)     - configuration file for fine-tuning the behavior of the...
sudoers (5)            - default sudo security policy plugin
unattended-upgrade (8) - automatic installation of security (and other) upgrades
Xsecurity (7)          - X display access control

```



## MAN
Abre o manual do programa solicitado.



**Seções do Comando MAN**

Ao digitar o man para algum binário, é apresentado um número entre parenteses, esse número identifica um tipo de seção, dentre 9 tipos que esse binário se adequa.

**Tipos de seções:**

```
1 - Binários comuns (Disponíveis ao usuário);
2 - Chamadas do Sistema (Rotinas do sistemas Unix e C);
3 - Bibliotecas;
4 - Dispositivos (Arquivos especiais como  dispositivos em */dev*);
5 - Arquivos de configuração;
6 - Jogos;
7 - Miscelânea (Diversos);
8 - Binários do Root (Procedimentos administrativos como daemons);
9 - Rotinas do kernel.
```

**Exemplo:**

```bash
# Vendo os tipos de seções existentes para o
# passwd.
linux:~/$ whatis passwd
passwd (5)           - arquivo de senhas
passwd (1)           - change user password
passwd (1ssl)        - compute password hashes

# Listando a 5° seção do passwd.
linux:~/$ man 5 passwd

# Listando a seção 1ssl do passwd.
linux:~/$ man 1ssl passwd
```

Por padrão, os arquivos de manuais ficam armazenados em ***/usr/man/*** e ***/usr/share/man/***, dentro dessas pastas o manual deve ficar dentro de sua respectiva seção, outros locais podem ser especificados, para isso usamos a variável ***MANPATH***, que fica localizada no arquivo ***/usr/lib/man.conf*** ou ***/etc/man.conf***.

```bash
linux:~/$ man sed
```

Podemos usar a opção `-L` para especificar a lingua a ser usada.

```bash
linux:~/$ man -L en sed
linux:~/$ man -L pt_BR sed

# Você deve ter o manual nas pastas da linguagem escolhida, caso não tenha, 
# será retornado o manual em inglês (en).

## Algumas linguagens (Utiliza-se o código das línguas ou código de idiomas):
# pt = Português de Portugual
# pt_BR = Portguês do Brasil
# en = Inglês
# nl = Holandês
# hr = Croata
# cs = Checa
# gl = Galego
# fr = Francês
# es = Espanhol
# da = Dinamarquês
# de = Alemão
```



# <span style="color:#d86c00">**103.2 - Processar fluxo de texto usando filtro**</span>
Esse tópico aborda os comandos do shell, o que você deve saber para operar o sistema, conhecendo seus comandos e funcionalidades.




## CAT

Exibe o conteúdo dos arquivos na tela.

**Opões:**
```
-A    Exibe caracteres especiais;
-E    Exibe somente caractere de fim de linha ($);
-T    Exibe apenas os tabs (^I);
-b    Numera as linhas (exceto as linhas em branco);
-n    Enumera tudo;
-s    Remove linhas em branco que se repetem.
```



**Crie um arquivo contendo o texto abaixo:**

--- Conteúdo do arquivo qualquer_nome 

```
aqui tem espaco: espaco


aqui tem um TAB:	tabulacao


fim de linha:
```



**Exibindo caracteres especiais**:

```bash
linux:~/$ cat -A arquivo
aqui tem espaco: espaco$
$
$
aqui tem um TAB:^Itabulacao$
$
$
fim de linha:$
```



**Exibe somente o caractere que mostra fim de linha:**

```bash
linux:~/$ cat -E arquivo
aqui tem espaco: espaco$
$
$
aqui tem um TAB:	tabulacao$
$
$
fim de linha:$
```



**Mostra apenas os tabs:**

```bash
linux:~/$ cat -T arquivo
aqui tem espaco: espaco


aqui tem um TAB:^Itabulacao


fim de linha:
```



**Enumera as linhas, menos as linhas em branco:**

```bash
linux:~/$ cat -b arquivo
     1	aqui tem espaco: espaco


     2	aqui tem um TAB:	tabulacao


     3	fim de linha:
```



**Enumerar tudo:**

```bash
linux:~/$ cat -n arquivo
     1	aqui tem espaco: espaco
     2	
     3	
     4	aqui tem um TAB:	tabulacao
     5	
     6	
     7	fim de linha:
```



**Remove linhas em branco repetidas:**

```bash
linux:~/$ cat -s arquivo
aqui tem espaco: espaco

aqui tem um TAB:	tabulacao

fim de linha:
```



## CUT
Usado para cortar bytes, caracteres ou campos de arquivo.

**Opções:**

```
-b    Seleciona bytes;
-c    Seleciona Caracteres; 
-d    Escolhe um delimitador (default = TAB)
-f    Escolhe o campo.
```


**Imprimindo o primeiro campo do passwd:**

```bash
linux:~/$ cut -d ':' -f1 /etc/passwd
root
daemon
bin
sys
sync
games
man
lp
mail
news
uucp
proxy

# Apenas alguns dos nomes que podem/irão aparecer.
```
Esse comando vai imprimir somente os nomes de usuários contidos no passwd. 
O passwd é separado por dois pontos ':', o comando diz para cortar usando como delimitador os dois pontos `cut -d ':'`, e para pegar o primeiro campo `-f1`.



## EXPAND - <span style="color:red">Desclassificado</span>

Converte TABs em espaços.

**Opções:**

```
-i		Converte apenas os TABs do início da linha;
-t N	Determina quantos espaços um TAB vai virar.
```

--- Crie um arquivo contendo a linha abaixo:

```
aqui tem 4 tabs:				.
```

--- Mostrando os tabs do arquivo:

```bash
linux:~/$ cat -A arquivo 
aqui tem 4 tabs:^I^I^I^I.$
```



**Converter tabs em espaços**

```bash
linux:~/$ expand arquivo
aqui tem 4 tabs:                                .

# Se você selecionar com o mouse o espaço em branco, verá que não tem mais tab,
# mas sim, vários espaços, para uma melhor verificação, faça isso usando o terminal.
```



## UNEXPAND - <span style="color:red">Desclassificado</span>

Converte espaços em TABs.

**Opções:**
```
-a		Converte os espaços no início da linha;
-t N	Define a quantidade de espaços (-t 3 = A cada 3 espaços temos 1 tab).
```
Usando o exemplo acima, copie a saída gerada pelo *expand* e cole dentro do 'arquivo'.



**Convertendo espaços em tabs**

```bash
linux:~/$ unexpand -t 8 arquivo

# Estamos informando que a cada conjunto de 8 espaços, ele vira um tab.
```



## FMT - <span style="color:red">Desclassificado</span>

Formata uma saída de texto, usado para arquivos longos, tem um padrão de 75 caracteres por linha.

**Opções:**

```
-w N	Define o número de caracteres por linha;
-s		Quebra linhas grandes sem preencher;
-u		Um espaço entre palavras e dois espaços entre sentenças.
```


**Exemplo:**

```bash
linux:~/$ fmt -w 50 arquivo_long

# Formatando as linhas com 50 caracteres apenas.
```



## HEAD

Exibe o começo de arquivos, por padrão as 10 primeiras linhas de um arquivo.

**Opções:**

```
-c N    Exibe por bytes (Caracteres);
-n N    Escolhe a quantidade de linhas.
```


**Exemplo:**

```bash
linux:~/$ head -n 5 /etc/passwd
root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
bin:x:2:2:bin:/bin:/usr/sbin/nologin
sys:x:3:3:sys:/dev:/usr/sbin/nologin
sync:x:4:65534:sync:/bin:/bin/sync

# Exibindo as cinco primeiras linhas do passwd.

linux:~/$ head -5 /etc/passwd
root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
bin:x:2:2:bin:/bin:/usr/sbin/nologin
sys:x:3:3:sys:/dev:/usr/sbin/nologin
sync:x:4:65534:sync:/bin:/bin/sync

# É possível omitir o '-n' passando direto o menos mais o número.
```



## HEXDUMP

Exibe o conteúdo de arquivos binários.

**Opções:**

```
-c	Exibe em hexadecimal com uma coluna ASCII.
```
Primeiro, digite `which sed` para sabermos onde se encontra o binário do *sed*.

```bash
linux:~/$ which sed
/bin/sed
```



**Exibindo o conteúdo do arquivo *SED*:**

```bash
linux:~/$ hexdump /bin/sed | head 
0000000 457f 464c 0102 0001 0000 0000 0000 0000
0000010 0002 003e 0001 0000 2760 0040 0000 0000
0000020 0040 0000 0000 0000 17d0 0001 0000 0000
0000030 0000 0000 0040 0038 0009 0040 001c 001b
0000040 0006 0000 0005 0000 0040 0000 0000 0000
0000050 0040 0040 0000 0000 0040 0040 0000 0000
0000060 01f8 0000 0000 0000 01f8 0000 0000 0000
0000070 0008 0000 0000 0000 0003 0000 0004 0000
0000080 0238 0000 0000 0000 0238 0040 0000 0000
0000090 0238 0040 0000 0000 001c 0000 0000 0000

# Usei o 'head' para exibir apenas as dez primeiras linhas por ser muito grande.
```



## JOIN - <span style="color:red">Desclassificado</span>

Faz a junção de dois arquivos por meio de um campo em comum.

**Opções:**
```
-1 campo -2 campo	Seleciona colunas em comum dos dois arquivos;
-j N				Coluna em comum;
-o '1.x 2.y'		Exibe a coluna X do 1° arquivo e a coluna Y do 2° arquivo.
```



**Crie os arquivos abaixo:**

```
--- Conteúdo do arquivo arq1
1 USER
2 rodrigo
3 gustavo

--- Conteúdo do arquivo arq2
1 age
2 25 anos
3 43 anos
```



**Selecionando coluna em comum:**

```bash
linux:~/$ join -j 1 arq1 arq2
1 USER age
2 rodrigo 25 anos
3 gustavo 43 anos

# Seleciona a primeira coluna em comum em ambos os arquivos.
```



**Crie os arquivos abaixo:**

```
--- Conteúdo do arquivo arq1
1 a
2 b
3 c

--- Conteúdo do arquivo arq2
1 a
2 b
3 c
```



**Entrlaçando colunas:**

```bash
linux:~/$ join -1 2 -2 2 arq1 arq2
a 1 1
b 2 2
c 3 3

# Junta a 2° coluna do 1° arquivo com a 2° coluna do 2° arquivo.
```



**Crie os arquivos abaixo:**

```
--- Conteúdo do arquivo arq1
1 a
2 b
3 c

--- Conteúdo do arquivo arq2
1 d
2 e
3 f
```



**Entrlaçando colunas:**

```bash
linux:~/$ join -o '1.1 1.2 2.2' arq1 arq2
1 a d
2 b e
3 c f

# Ele pega a 1° coluna do 1°arquivo, depois pega a 2° coluna do primeiro 
# arquivo e coloca a 2° coluna do 2° arquivo.
```



## LESS

Faz paginação de texto, usado quando apenas se quer ler o texto, e apenas isso.
Geralmente recebe a saída de um comando, mas pode ser usado diretamente. O *less* é bem completo por fazer buscas no texto, poder voltar a medida que avança (o paginador *more* não volta) `O less tem modo navegação`.

**Funcionamento:**

Dentro do texto, aperte barra '/', depois digite o que se quer procurar e de enter.

```bash
linux:~/$ less arquivo_de_texto
```



## NL

Exibe o conteúdo de arquivos (igual o *cat*) mas com uma diferença, ele enumera as linhas.

**Opções:**
```
-ba    Enumera todas as linhas;
-bt    Enumera as linhas (exceto as em branco).
```



**Exemplo:**

```bash
linux:~/$ nl arquivo.txt
```



## OD

Converte para formato Octal (Converte para outros formatos também).

**Opções:**
```
-t tipo		Escolhe o formato para converter;
-a			Caracteres nomeados. Idem `-t a`;
-o			Formato Octal, opção default;
-c			Formato ASCII, idem `-t c`;
-x			Formato hexadecimal, idem `-t x2`.
```


**Exemplos:**

```bash
# Formato OCTAL.
linux:~/$ od arq1 
0000000 020061 005141 020062 005142 020063 005143
0000014

# Formato ASCII.
linux:~/$ od -c arq1 
0000000   1       a  \n   2       b  \n   3       c  \n
0000014

# Formato Hexadecimal.
linux:~/$ od -x arq1 
0000000 2031 0a61 2032 0a62 2033 0a63
0000014
```



## PASTE

Exibe o conteúdo dos arquivos em colunas verticais delimitadas por TAB.

**Opções:**
```
-d    Define um delimitador;
-s    Exibe em linhas, ao invés de colunas.
```



**Crie os arquivos abaixo:**

```
--- Conteúdo do arquivo arq1
1
2
3

--- Conteúdo do arquivo arq2
4
5
6
```

**Exemplos:**

```bash
# Funcionamento normal
linux:~/$ paste arq1 arq2
1    4
2    5
3    6

# Exibe em linhas
linux:~/$ paste -s arq1 arq2
1    2    3
4    5    6
```



## PR - <span style="color:red">Desclassificado</span>

Converte os arquivos para impressão, separando por páginas com data e número de páginas. Por padrão é usado 66 linhas com 72 caracteres cada.

**Opções:**
```
-num		Divide por N colunas;
-d			Espaço duplo entre linhas;
-h nome		Escolhe um novo nome;
-l N   	    Seleciona número de linhas por página;
-o N 	    Margem a esquerda com N espaços em branco;
-w CA       Seleciona a quantidade de caracteres por linha, default 72.
```


## SORT

Ordena alfabeticamente.

**Opções:**
```
-t			Escolhe um delimitador;
-n			Ordena numericamente;
-k Coluna	Realiza a ordenação por meio da coluna selecionada;
-f			Ignora case sensitive;
-b			Ignora linhas em branco;
```



**Ordenando TTY's:**

```bash
linux:~/$ ls /dev/ttyS* | sort -n -t 'S' -k 2
/dev/ttyS0
/dev/ttyS1
/dev/ttyS2
/dev/ttyS3
/dev/ttyS4
/dev/ttyS5
/dev/ttyS6
/dev/ttyS7
/dev/ttyS8
/dev/ttyS9

# Como possuí muitas linhas, suprimi para até 10 linhas.
```

**Entendendo o motivo de ser campo 2**

```bash
# Vou exemplificar de uma forma bem simples,
# o comando abaixo vai cortar, para isso ele vai usar a
# letra S como delimitador e vai pegar o 1° campo:
linux:~/$ ls /dev/ttyS* | sort -n -t 'S' -k 2 | cut -d S -f 1
/dev/tty
/dev/tty
/dev/tty
/dev/tty
/dev/tty
/dev/tty
/dev/tty
/dev/tty
/dev/tty

# Antes do delimitador conta como campo 1.
# Mas depois já soma +1 no campo:
linux:~/$ ls /dev/ttyS* | sort -n -t 'S' -k 2 | cut -d S -f 2
0
1
2
3
4
5
6
7
8
9
10
11
12
```



## SPLIT

Divide um arquivo em partes (default dividir a cada mil linhas). Mas pode ser usado para dividir arquivos que não sejam arquivos de texto.

**Opções:**
```
-b T    Divide por tamanho;
-c N    Separa cada linha por uma quantidade de Bytes;
-d      Arquivos divididos serão nomeados com sufixo numérico;
-l L    Divide por número de linhas;
-n Q	Divide por quantidade.
```


## TAC

Semelhante ao *CAT*, mas exibe o conteúdo de trás para frente (inverso).



## TAIL

Mostra o final dos arquivos, por padrão exibe as dez últimas linhas de um arquivo.

**Opções:**
```text
-c N	Exibe N bytes;
-f      Exibe em tempo real;
-n N    Escolhe a quantidade de linhas que serão exibidas no final.
```

**Antes de inciarmos os exemplos:**

```bash
# Primeiro, vamos ver quantas linhas o arquivo passwd possui:
linux:~/$ wc -l /etc/passwd
46 /etc/passwd

# Vou usar o comando 'cat' com a opção '-n' para enumerar as linhas,
# assim posso mostrar o funcionamento do comando tail, já sabemos que o arquivo
# /etc/passwd possui 46 linhas.
```



**Exemplos de utilização do TAIL:**

```bash
# Uso do comando tail sem opções:
linux:~/$ cat -n /etc/passwd | tail
 37 uuidd:x:110:118::/run/uuidd:/usr/sbin/nologin
 38 avahi:x:118:126:Avahi mDNS daemon,,,:/var/run/avahi-daemon:/usr/sbin/nologin
 39 saned:x:119:127::/var/lib/saned:/usr/sbin/nologin
 40 lightdm:x:120:128:Light Display Manager:/var/lib/lightdm:/bin/false
 41 colord:x:121:130:colord colour management daemon,,,:/var/lib/colord:/usr/sbin/nologin
 42 pulse:x:122:131:PulseAudio daemon,,,:/var/run/pulse:/usr/sbin/nologin
 43 speech-dispatcher:x:123:29:Speech Dispatcher,,,:/run/speech-dispatcher:/bin/false
 44 hplip:x:125:7:HPLIP system user,,,:/run/hplip:/bin/false
 45 user:x:1000:1000:user,,,:/home/user:/bin/bash
 46 sshd:x:126:65534::/run/sshd:/usr/sbin/nologin
 
# Escolhendo a quantidade de linhas
linux:~/$ cat -n /etc/passwd | tail -15
 32 geoclue:x:113:122::/var/lib/geoclue:/usr/sbin/nologin
 33 proxy:x:13:13:proxy:/bin:/usr/sbin/nologin
 34 usbmux:x:115:46:usbmux daemon,,,:/var/lib/usbmux:/usr/sbin/nologin
 35 dnsmasq:x:116:65534:dnsmasq,,,:/var/lib/misc:/usr/sbin/nologin
 36 sync:x:4:65534:sync:/bin:/bin/sync
 37 avahi:x:118:126:Avahi mDNS daemon,,,:/var/run/avahi-daemon:/usr/sbin/nologin
 38 saned:x:119:127::/var/lib/saned:/usr/sbin/nologin
 39 lightdm:x:120:128:Light Display Manager:/var/lib/lightdm:/bin/false
 40 colord:x:121:130:colord colour management daemon,,,:/var/lib/colord:/usr/sbin/nologin
 41 pulse:x:122:131:PulseAudio daemon,,,:/var/run/pulse:/usr/sbin/nologin
 42 speech-dispatcher:x:123:29:Speech Dispatcher,,,:/run/speech-dispatcher:/bin/false
 43 root:x:0:0:root:/root:/bin/bash
 44 hplip:x:125:7:HPLIP system user,,,:/run/hplip:/bin/false
 45 user:x:1000:1000:user,,,:/home/user:/bin/bash
 46 sshd:x:126:65534::/run/sshd:/usr/sbin/nologin
 
 # O comando 'tail -n -15' e 'tail -15' exibem o mesmo resultado.
 # A opção '-N' pode ser omitida.
```



Diferença entre número negativo `-9` e positivo `+9`, ao colocar o número negativo, o comando `tail` vai funcionar como deve (começar exibindo do final do arquivo).

O número positivo faz com que o comando `tail` comece a exibir a partir da linha escolhida (mostrando também a linha escolhida).

**Exemplos:**

```bash
# Começando a exibir da linha 40
linux:~/$ cat -n /etc/passwd | tail +40
 40 colord:x:121:130:colord colour management daemon,,,:/var/lib/colord:/usr/sbin/nologin
 41 pulse:x:122:131:PulseAudio daemon,,,:/var/run/pulse:/usr/sbin/nologin
 42 speech-dispatcher:x:123:29:Speech Dispatcher,,,:/run/speech-dispatcher:/bin/false
 43 root:x:0:0:root:/root:/bin/bash
 44 hplip:x:125:7:HPLIP system user,,,:/run/hplip:/bin/false
 45 user:x:1000:1000:user,,,:/home/user:/bin/bash
 46 sshd:x:126:65534::/run/sshd:/usr/sbin/nologin

# Começando a exibir da linha 43
linux:~/$ cat -n /etc/passwd | tail +43
 43 root:x:0:0:root:/root:/bin/bash
 44 hplip:x:125:7:HPLIP system user,,,:/run/hplip:/bin/false
 45 user:x:1000:1000:user,,,:/home/user:/bin/bash
 46 sshd:x:126:65534::/run/sshd:/usr/sbin/nologin
```



## TR

Substitui, deleta ou agrupa caracteres originados da entrada padrão (stdin).

**Opções: **
```
-c C1 C2	Manter caractere 1 e troca tudo por caractere 2;
-d C1       Deleta caractere 1;
-s C1       Não exibe ocorrências repetitivas de um caractere.
```



**Exemplos de utilização do TR:**

```bash
# Convertendo caracteres para minúsculo
linux:~/$ echo "OLA" | tr 'A-Z' 'a-z'
ola

# Deletando a letra 'A'
linux:~/$ echo "OLA" | tr -d A
OL

# Não exibe ocorrências repetitivas de A até Z
linux:~/$ echo "OLAAAAAA" | tr -s 'A-Z'
OLA

# Mantém o primeiro caractere 'e' e substitui todo o 
# resto pelo segundo caractere 'a'
linux:~/$ echo "Teste de substituicao" | tr -c e a
aeaaeaaeaaaaaaaaaaaaaa
```



## UNIQ

Remove linhas duplicadas de um arquivo (Somente se estiver ordenado)

**Opções:**
```
-c  Exibe as linhas com a quantidade de vezes que ela foi duplicada;
-d	Exibe as linhas duplicadas (uma linha por grupo);
-D  Exibe todas as linhas duplicadas;
-i  Ignora case sensitive;
-u  Exibe apenas as linhas únicas.
```



**Crie o arquivo abaixo:**

```
--- Conteúdo do arquivo nomes
USER
USER
rodrigo
janeir
janeir
joao
joao
marcia
```



**Exemplos de uso do comando UNIQ**

```bash
# Exibindo o resultado sem argumentos, ele apenas elimina as linhas
# repetidas, mas mantém 1 linha, das linhas repetidas:
linux:~/$ uniq arquivo 
USER
rodrigo
janeir
joao
marcia

# Exibe apenas as duplicatas das linhas:
linux:~/$ uniq -D nomes
USER
USER
janeir
janeir
joao
joao

# Exibe duplicatas das linhas, uma por grupo (só as
# que se repetem)
linux:~/$ uniq -d nomes
USER
janeir
joao

# Exibe apenas as linhas que aparecem uma única vez
linux:~/$ uniq -u nomes
rodrigo
marcia

# Exibe a quantidade de vezes que a linha foi duplicada
linux:~/$ uniq -c nomes
2 USER
1 rodrigo
2 janeir
2 joao
1 marcia
```



## WC

Exibe o número de linhas, palavras, caracteres e bytes.

**Opções:**
```
-c	Número de bytes;
-l	Número de linhas;
-L	Número de caracteres da maior linha;
-m  Número de caracteres;
-w	Número de palavras.
```



A execução do wc sem argumento retorna:

​		Número de linhas;

​		Número de palavras;

​		Número/Tamanho de bytes.



**Exemplo de uma saída do _wc_:**

```bash
linux:~/$ wc nomes
8  8 49 nomes
```



## CHECKSUMS

É uma técnica usada para verificação da integridade de dados transmitidos através de um canal não confiável e com ruídos, onde, esses dados podem sofrer com algum tipo de degradação, seja no momento da transferência, seja por problemas no Hardware que armazene esses dados, entre outros tipos de problemas.

Segue os algoritmos usados atualmente, lembrando que não será explicado como funciona o algoritmo, mas será apresentado como usá-lo.
```
MD5SUM 
	Função criada por Ron Rivest foi amplamente utilizada. Gera um hash de 16 bytes. Foi totalmente quebrada em 2005 (colisões foram encontradas). Portanto, não deve ser usada.


SHA1SUM
	A função SHA-1 é atualmente a mais utilizada. Gera um hash de 20 bytes (mais forte que 	o MD5). Foi encontrada uma colisão em fevereiro de 2017, por esse motivo, deve ser substituída pela sha256 ou sha3.


SHA256SUM e SHA512SUM
	Da mesma família que SHA-1, ainda não foram encontradas falhas nesses dois algoritmos.


SHA3SUM
	Substitui o algoritmo SHA-1, é o mais recente e atual, deve ser usado.
```



**Como Funciona:**

​	Na prática, é gerado um hash do dado e após a transferência dele, por um canal com ruídos, e gerado novamente um hash. Se o hash novo for igual ao antigo, então não houve degradação do dado durante a transferência, mas se o hash novo for diferente do antigo, isso significa que houve degradação e o dado transferido não é mais o mesmo que foi enviado.

   Pode ser gerado um hash de palavras ou arquivos, para exemplificar o que foi dito, **crie um arquivo de texto** chamado 'arquivo' escrito 'banana'.

```bash
linux:~/$ sha256sum arquivo
5a81483d96b0bc15ad19af7f5a662e14b275729fbc05579b18513e7f550016b1

# Gerando um hash de 256 bytes a partir de um arquivo. 
```


Agora apague o último '**a**' do banana e gere um novo hash, você verá que o código é totalmente diferente.

O seu resultado deve ser igual ao meu, qualquer bit de diferença no arquivo irá gerar um hash diferente, portanto, se o valor for diferente não se espante, muitas coisas podem fazer com que isso aconteça, por exemplo, o modo de criação de um arquivo de texto no Windows é diferente do Linux, se tiver um espaço a mais, um enter a mais, tudo isso fará com que o hash seja diferente.

Para usar outros algoritmos, basta trocar o nomes, pelos nomes informados acima.

Outra forma de usar é usando o comando '**echo**':

```bash
linux:~/$ echo "teste" | sha256sum
06394af259db19355ccf0b668c77ec0fdd9b8d5aa388d848e1bb5959e80e2a24 - 
    
# Gerando um hash de 256 bytes a partir de uma palavra. 
```
E como saber se os códigos são iguais?

Pode ser feita a verificação a olho, o que é muito sujeito a erro e muito cansativo, podemos usar um '**if**' no Shell para comparar os valores ou podemos usar uma opção '-c' que tem em todos os algoritmos informados, ele pega o Hash de um arquivo e verifica se está correto.

**Pegue 'arquivo' e escreva 'banana' de novo.**

**Gere uma hash e salve num arquivo, exemplo abaixo:**

```bash
linux:~/$ sha256sum arquivo > checksumarquivo

# Gerando um hash a partir de um arquivo e 
# enviando o resultado para um outro arquivo.
```
Rode o comando de verificação (exemplo abaixo). O arquivo contendo o hash deve estar no mesmo diretório que o arquivo.
```bash
linux:~/$ sha256sum -c checksumarquivo
arquivo: SUCESSO

# Verificando hash's de um arquivo.
```
Dessa forma é possível testar vários hashs de uma única vez. Se algo der errado, ele irá informar o erro e o arquivo que deu erro.



## XZCAT, BZCAT e ZCAT

O comando `cat` server para exibir o conteúdo de um arquivo, mas não é possível exibir o conteudo de um arquivo de texto compactado, por isso, foi criado esses três comandos.

```tex
# XZCAT
	Exibe o conteúdo de arquivos compactados com o XZ;

# BZCAT
	Exibe o conteúdo de arquivos compactados com o BZ ou BZ2 (Ambos são bzip2);

# ZCAT
	Exibe o conteúdo de arquivos compactados com o GZ (gzip);
```

**Criei 3 arquivos, cada um compactado com um algoritmo mostrado acima**

**Exemplos:**

```bash
# Mostrando o conteúdo do arquivo compactado com xz
linux:~/$ xzcat arquivo_xz.xz 
Arquivo compactado com xz.

# Mostrando o conteúdo do arquivo compactado com bzip2
linux:~/$ bzcat arquivo_bz.bz2 
Arquivo compactado com bzip2.

# Mostrando o conteúdo do arquivo compactado com gzip
linux:~/$ zcat arquivo_gz.gz 
Arquivo compactado com gzip.
```






# <span style="color:#d86c00">**103.3 - Realizar o Gerenciamento Básico de Arquivos**</span>

Esse tópico aborda os comandos básicos para um bom gerenciamento dos arquivos do linux.



## PWD

Informa em qual diretório o shell se encontra.

```bash
linux:~/$ pwd
/tmp/
```



## CD

Altera o diretório atual, mudando de diretório (entra nas pastas).

Para entrar numa pasta, basta digita `cd DIRETORIO`, onde diretório é a pasta que você quer acessar.

**Exemplos:**

```bash
# Entrando na pasta tmp:
linux:~/$ cd /tmp/

# Entrando na pasta documentos:
linux:~/$ cd /home/user/Documentos/
```



**Caminho absoluto:** Começa sempre com a barra no início, 
começa sempre do diretório de topo (a raiz, ou seja, a `/`).

**Caminho relativo:** Omiti o diretório de topo, começa sempre de um diretório dentro dele.

```bash
# Entrando na HOME usando caminho absoluto
linux:~/$ cd /home/user/

# Entrando na HOME usando caminho relativo
linux:~/$ cd ~

# Entrando na pasta documentos usando o caminho relativo:
linux:~/$ cd ~/Documentos/
```



## FILE

Ele  reconhece o tipo de dado contido num objeto. Como no Unix não se usa  muito a extensão do arquivo, então, para saber do que se trata esse  arquivo, usa-se o comando file, que analisa o objeto e informa que tipo  de objeto ele é.

Exemplo:

```bash
linux:~/$ file nag2.pdf
nag2.pdf: PDF document, version 1.2
```


Ah, mas esse arquivo é um pdf porque têm .pdf nele. Então analisemos o exemplo abaixo:

```bash
linux:~/$ file nag2
nag2: PDF document, version 1.2
```

Foi removido a extensão do arquivo e mesmo assim, o comando **file** analisa o objeto e informa do que se trata.



## LS


Lista os arquivos dentro de um diretório.

**Opções:**
```
-a        Exibe também os arquivos ocultos;
-d        Lista somente o diretório em sí (pasta);
-h        Exibe o tamanho do diretório/arquivo de forma legível;
-i        Exibe o número inode;
-l        Exibe as propriedade;
-r        Exibe por ordem alfabética inversa;
-t        Ordena por data de modificação, sendo mais recente os primeiros;
-F        Exibe o tipo de arquivo;
-R        Exibe recursivamente;
--color   Exibe tudo de forma colorida.
```

***Propriedades***

Ao rodar o comando '**ls -lh**', será apresentado uma série de informações sobre cada arquivo, segue abaixo dois exemplos:

```
-rw-rw-r-- 1 USER USER 49 jul 22 21:05 join.txt
drwxrwxr-x 2 USER USER 4,0K jul 22 21:02 rename/
```



**Arquivo join.txt:**

```
-rw-r--r-- = (Tipo de arquivo / Tipo de permissão;)

1 = Quantidade de Hardlinks (Quantos inodes iguais existem);

USER USER = Dono e Grupo ao qual o arquivo/pasta pertence;

49 = Tamanho do arquivo em bytes.

jul 22 21:05 = Data da última modificação;

join.txt = Nome do arquivo/Diretório.
```



**Diretório rename:**

```
drwxrwxr-x = (Tipo de arquivo / Tipo de permissão;)

2 = Quantidade de Hardlinks (Quantos inodes iguais existem);

USER USER = Dono e Grupo ao qual o arquivo/pasta pertence;

4,0K = Tamanho do arquivo em Kilobyte.

jul 22 21:02 = Data da última modificação;

rename = Nome do arquivo/Diretório.
```

Perceba que arquivos comum só possuem 1 Hardlink, enquanto qualquer pasta possui 2 Hardlinks.

**Porque pastas possuem 2 Hardlinks ao criar a pasta?**

Quando criamos um arquivo/pasta no linux, o filesystem da a eles um número de inode, esse número vai representar esse arquivo/pasta nesse filesystem ("não terá outro igual no mesmo filesystem").

***Vamos ver isso na seção do comando `ln`***.

Mas quando criamos um hardlink (link físico ou atalho físico), na prática, o sistema de arquivo cria outro arquivo com o mesmo número de inode, esse é um dos motivos que não podemos criar hardlinks de diretórios. Para mais detalhe consulta a seção do comando `ln`.

LINK_PARA_LN

**Explicando os diretórios ocultos em pastas**

Conteúdos ocultos no linux são pastas ou arquivos, que começam com `.` (ponto) antes do nome do arquivo/pasta.
Ao executar o '**ls -a**' em um diretório, é exibido 2 diretórios no topo da lista, são eles:

```bash
# Listando o conteúdo oculto da pasta /tmp:
linux:/tmp\$ ls -lha
drwxrwxrwt 19 root  root  4,0K jul 22 21:03 .
drwxr-xr-x 23 root  root  4,0K jul 19 14:39 ..
```

**.** - É o diretório atual;
**..** - É o diretório anterior.



**Demonstrando o que foi dito acima**:

```bash
# Crie uma pasta chamada pasta1 no /tmp
linux:/tmp\$ mkdir pasta1

# Liste o inode da pasta1 e do /tmp
linux:/tmp\$ ls -id pasta1 /tmp/
8657494 pasta1  7864321 /tmp/
# O inode da pasta1 é 8657494, e do /tmp é 7864321.

# Agora liste o inode do diretório oculto chamado '..' (ponto ponto) 
# dentro de pasta1:
linux:/tmp\$ ls -id pasta1/..
7864321 pasta1/..
# Perceba que o inode do diretório oculto chamado '..' (ponto ponto)
# é o mesmo do /tmp, isso porque o '..' sempre vai referenciar o 
# diretório anterior.

# Exemplo do uso do diretório chamado '..':
linux:/tmp\$ cd ..
# Volta um diretório (mesmo inode do '..' e do diretório anterior)
```

Agora exemplificando o diretório `.` (ponto):

```bash
# Agora liste o inode do diretório oculto chamado '.' (ponto) 
# dentro de pasta1:
linux:/tmp\$ ls -id pasta1/.
8657494 pasta1/.

# Perceba que ele tem o mesmo inode da pasta1, assim, em comandos como 'cp'
# podemos simplesmente coloca o '.' no final, que representa nosso diretório
# atual.

# Entre na pasta1
linux:/tmp\$ cd pasta1

# agora copie qualquer coisa do /tmp para pasta1:
linux:/tmp/pasta1\$ cp /tmp/arq.gz .

# liste o conteúdo de pasta1:
linux:/tmp/pasta1\$ ls -lha
drwxrwxr-x  2 USER USER 4,0K jul 22 21:36 .
drwxrwxrwt 20 root  root  4,0K jul 22 21:25 ..
-rw-rw-r--  1 USER USER  202 jul 22 21:36 arq.gz

# perceba que a cópia funcionou graças ao inode que foi explicado acima.
```



## CP

Faz uma cópia de arquivos ou diretórios.

**Opções:**
```
-i  pergunta antes de sobrescrever;
-v  Exibe tudo que está sendo copiado;
-r  Recursivo, faz cópia de diretório;
-p	Preserva as características, como dono, grupo e última atualização.
```

**Exemplos**

```bash
# Copia o arquivo1 para outra pasta e renomeia ele no processo;
linux:~/$ cp arquivo1 aula/teste1

# Copia o arquivo1 para outra pasta e mantém seu nome;
linux:~/$ cp arquivo1 aula/

# Copia a pasta aula e cola no /tmp.
linux:~/$ cp -r aula/ /tmp/

# Copia a pasta aula preservando suas características e cola no /tmp.
linux:~/$ cp -rp aula/ /tmp/
```



## MV

Faz a movimentação de objetos ou renomeia objetos.

Para  mudar o nome do objeto não deve ser especificado nenhum diretório, caso  seja especificado ele irá mover ao invés de renomear.

```bash
# Renomeia um arquivo, o objeto nag2 será renomeado para linux_admin.pdf
linux:~/$ mv nag2 linux_admin.pdf

# Estou movendo o arquivo para o /tmp/ e mantendo seu nome.
linux:~/$ mv linux_admin.pdf /tmp/

# Estou movendo o arquivo para o /tmp/ e mantendo seu nome 
# (igual a opção acima)
linux:~/$ mv linux_admin.pdf /tmp/linux_admin.pdf

# Estou movendo o arquivo para o /tmp/ e mudando seu nome durante o ato de mover.
linux:~/$ mv linux_admin.pdf /tmp/nag2.pdf
```



## TOUCH

Sua  principal função é mudar o timestamp de um objeto, mas sua função mais  usada é criar um arquivo em branco. Isso acontece porque, ao mudar o  timestamp de um arquivo, se esse arquivo não existir, ele cria o arquivo  (cria o arquivo vazio).

**Opções:**
```
-m	Altera a data de modificação;
-a	Altera a data de acesso;
-t 	Usa o formato timestamp (ANODIAMÊSHORA = 201912061425 = 2019/12/06 às 14:25);
-d	Usa formato humano.
```

**Exemplos:**

```bash
# Altera a data de modificação, acesso e alteração:
linux:~/$ touch nomes.txt

# Altera a data de acesso e modificação para o
# ano de 2018, mês 08, dia 31 às 14:23:23
linux:~/$ touch -t "201808311423.23" nomes.txt

# Tanto a opção '-a' como a opção '-m', não recebem nenhum argumento.

# Altera a data/hora de acesso para data/hora atual do sistema 
# (altera a data de alteração também):
linux:~/$ touch -a nomes.txt

# Altera apenas a data/hora de acesso para o
# ano de 2018, mês 12, dia 31 às 14:23:23
linux:~/$ touch -a -t "201812311423.23" nomes.txt

# Altera a data/hora de modificação e alteração 
# para data/hora atual do sistema
linux:~/$ touch -m nomes.txt

# Altera apenas a data/hora de modificação para o ano 
# de 2019, mês 05, dia 15 às 15:00:00
linux:~/$ touch -m -t "201905151500.00" nomes.txt

# Altera a data/hora de modificação e acesso para
# o ano atual, mudando apenas o mês e hora:
linux:~/$ touch -d "February 1 01:09"  nomes.txt
```



## RM

É usado para remoção de arquivos e diretórios.

**Opções:**
```
-i	(interativo ou interactive): Pergunta se quer remover;
-v	Verbose, mostra enquanto excluí;
-r	Recursivo, usado para remoção de diretório, excluí tudo dentro do diretório.
-f	Força a exclusão;
-d	Remove diretório vazio (rmdir).
```


**Exemplos:**

```bash
# Remove o arquivo sem perguntar;
linux:~/$ rm nome_do_arquivo 

# Pergunta antes de remover;
linux:~/$ rm -i nome_do_arquivo

# Mostra todos os arquivos que estão sendo excluídos;
linux:~/$ rm -v nome_do_arquivo(s)
```


O **rm** é  usado para remoção de arquivos e diretórios, mas para que seja possível  remover um diretório, deve ser usado a opção '**-r**' (Recursive), isso  acontece porque na maioria das vezes os diretórios possuem arquivos, por  isso a opção recursivo, o que será feito com o diretório, será aplicado  (repetido) com o que estiver dentro dele.

Mesmo estando vazio você deve usar o '**-r**', isso porque, se estiver vazio, em teoria, deve ser usado o '**rmdir**' ou **rm -d**'.

```bash
linux:~/$ rm -r diretório

# Excluir um diretório.
```



## RMDIR

Usado para excluir diretórios vazios e apenas diretórios vazios.

```bash
linux:~/$ rmdir diretório

# Excluir diretório vazio
```



## MKDIR

Usado para criação de diretórios no Linux

**Opção:**
```
-p	Cria uma árvore de diretório.
```



**Exemplo:**

```bash
linux:~/$ mkdir nome_do_diretório

# Criando apenas um diretório
```



Por padrão, o mkdir só  cria um diretório, caso você queira criar mais de um diretório ou se  for preciso criar um diretório dentro de outro diretório que não exista  ainda, é usado a opção -p (parents).

```bash
linux:~/$ mkdir -p dir1/dir2

# Criando mais de um diretório simultaneamente.
# Dessa forma, será criado dir1 e dentro dele será criado dir2.
```



## FIND

Usado  para fazer buscas no Linux, dessa forma, é possível fazer buscas em  arquivos/diretórios usando uma gama de possibilidades bem vasta.

**Opções:**

```tex
-i			Ignora case sensitive;
-name		Nome do que será procurado (objeto);
-user		Pesquisa por usuário (objeto tem como dono esse usuário);
-group		Pesquisa por grupo (objeto tem como grupo o grupo especificado);
-maxdepth	Até quantos subdiretórios ele vai buscar;
-mindepth	Começa a busca depois de N diretórios;
-empty		Procura por arquivos e diretórios vazios;
-perm		Procura por permissão;
-cmin		Pesquisa por arquivos com status alterados usando a opção de minutos;
-ctime		Pesquisa por arquivos com status alterados usando a opção de dias;
-amin		Pesquisa por arquivos acessados usando a opção de minutos;
-atime		Pesquisa por arquivos acessados usando a opção de dias;
-mmin		Pesquisa por arquivos modificados usando a opção de minutos;
-mtime		Pesquisa por arquivos modificados usando a opção de dias;
-type		Pesquisa por tipo de objeto.

	Tipos de objetos:

		b	- Objeto do tipo bloco;
		c	- Objeto do tipo caractere;
		d	- Objeto do tipo diretório;
		f	- Objeto do tipo arquivo comum;
		l	- Objeto do tipo link simbólico.
```




**Alguns modelos de busca com FIND:**

```bash
# Faz uma busca na sua HOME, buscando tudo que tenha '.txt'.
linux:~/$ find /home/$USER/ -name "*.txt"

# Procura e exclui imediatamente. 
# Faz um busca por '.txt' na HOME e exclui os arquivos.
linux:~/$ find /home/$USER/ -name "*.txt" -exec rm -i {} \;

# Procura arquivos e diretórios vazios na sua HOME.
linux:~/$ find /home/$USER/ -empty

# Pesquisa por arquivos com mais de 1 giga, 
# iniciando a pesquisa no diretório corrente.
linux:~/$ find . -type f -size +1G

# Pesquisa por arquivos menores que 1 Giga
linux:~/$ find . -type f -size -1G

# Pesquisa por arquivos com exatamente 1 Giga
linux:~/$ find . -type f -size 1G

# Pesquisa por arquivos de texto, e exibe apenas os arquivos
# que tem teste escrito no '.txt'.
linux:~/$ find /home/$USER/ -iname "*.txt" -exec grep -Hin "teste" {} \;
```



**Pesquisando e executando um comando nos arquivos encontrador**

```bash
linux:~/$ find /home/$USER/ -type f -name "*.txt" -exec echo "Achado: " {} \;
```

Pesquisa feita na HOME `find /home/$USER/`, procurando arquivos que tenham '.txt' `-name "*.txt"`, sendo arquivos do tipo comum (Arquivos de texto (`-type f`)), após isso executa um '*echo*' para cada linha encontrada `-exec echo "Achado: " {} \;`.



***Opções do grep que foram usadas acima:***

```
-H       Exibe o nome do arquivo com o caminho;
-i       Ignora case sensitive;
-n       Exibe o número da linha que teve a ocorrência.
```
Algumas opções do find como: `amin, atime, cmin` dentre  outras, são passadas com o uso de um número, esse número representa os  dias ou os minutos (falaremos da representação por dia), por exemplo: 1 é  igual a 24 horas, ou seja, o uso dessas opções com o número 1 significa  um dia, isso porque um dia tem 24 horas. 

Então, se você quiser pesquisar  por 3 dias, você usa o número 3, porque cada dia tem 24 horas, se for  por cinco dias, usa o número 5 e por aí vai.

***Variação decorrente de cada dia:***

​        Como  foi explicado acima, você pode usar esses dias com o símbolo de adição  `+` ou com o símbolo de subtração `-`, se for usado o símbolo de adição,  isso significa que o tempo (em dias ou em minutos) é maior do que o  especificado e se for usado o símbolo de subtração, o tempo é menor do  que o especificado, exemplos:

```
+5	= Mais de cinco dias;
-10	= Menos de dez dias;
7	= Exatamente 7 dias.
```


Isso  serve para minutos também, segue a mesma linha de raciocínio, somente  mudam as opções que buscam por dia `ctime, atime e mtime` ou por minuto `cmim, amim e mmim`. Dessa maneira, podemos  criar métodos de busca mais completos.

```bash
# Pesquisa por arquivos acessados a menos de 5 minutos
linux:~/$ find . -type f -amin -5

# Pesquisa por arquivos acessados a menos de 1 dia
linux:~/$ find . -type f -atime -1

# Pesquisa por arquivos com o status alterado a menos de 1 dia
linux:~/$ find . -type f -ctime -1
```



**Crie alguns dados, usando os comandos abaixo:**

```bash
linux:~/$ mkdir -p pasta1/pasta2/pasta3/pasta4/pasta5/pasta6/pasta7

linux:~/$ touch pasta1/pasta2/pasta3/achei 

linux:~/$ touch pasta1/pasta2/pasta3/pasta4/pasta5/pasta6/pasta7/achei
```



O **MAXDEPTH** dita até quantos subdiretórios a busca será feita, vale ressaltar que, quando o ***find*** vai  fazer a busca, ele primeiro olha o diretório como um objeto e depois  ele entra no diretório para buscar objetos dentro dele, exemplos:

```bash
# Esse comando não vai retornar nada, porque o find
# só olhou o terceiro diretório como objeto e não como pasta.
linux:~/$ find . -maxdepth 3 -iname "achei" 2>/dev/null

# Para  simplificar isso, digite o comando abaixo, 
# usaremos a opção '-ls' no  início do comando 
# (Há diferença se você coloca essa opção no começo ou  no final),
# dessa forma, podemos exibir o que o find analisou.

linux:~/$ find . -ls -maxdepth 3 -name "achei" 2>/dev/null
21501536  4 drwxrwxr-x   3 USER  USER         4096 Jun 30 07:24 . 
21501851  4 drwxrwxr-x   3 USER  USER         4096 Jun 30 07:47 ./pasta1 
21501852  4 drwxrwxr-x   3 USER  USER         4096 Jun 30 07:47 /pasta1/pasta2
21501854  4 drwxrwxr-x   3 USER  USER         4096 Jun 30 07:25 ./pasta1/pasta2/pasta3

# Com isso, podemos ver que o find,  olha a pasta3, 
# mas ele não entra dentro da pasta3, ele a vê apenas como  um arquivo,
# vendo se ela não é o que estamos procurando. 
# Agora, se  adicionarmos mais um subdiretório para pesquisa, 
# aí sim o find irá “entrar” dentro de pasta3 para fazer uma busca.

linux:~/$ find . -maxdepth 4 -name "achei" 2>/dev/null
./pasta1/pasta2/pasta3/achei

# Rode o mesmo comando com '-ls' para ver que ele entra na pasta3.

# Fazendo uma busca mais profunda em pastas
linux:~/$ find . -maxdepth 8 -name "achei" 2>/dev/null
./pasta1/pasta2/pasta3/pasta4/pasta5/pasta6/pasta7/achei
./pasta1/pasta2/pasta3/achei
```

​        

## STAT

Exibe  informações sobre arquivos e diretórios, as informações exibidas mais  usadas são: *data de acesso*, *de modificação*, *alteração*, *número inode*  entre outras informações.

- **Data de Acesso**
  A  data de acesso é alterada quando o conteúdo do arquivo for acessado,  por exemplo, por meio de editores de texto. Também pode ser feito usando  o comando '**touch**'.

- **Data de Modificação**
  A  data de modificação é alterada quando se modifica o arquivo, por  exemplo, mudando alguma coisa dentro dele. Também pode ser feito usando o  comando '**touch**'.

- **Data de Alteração**
  Essa  data, representa o status do arquivo/diretório, ela muda quando é  alterado a data de modificação, mudando as permissões e até mesmo dono e  grupo dono do objeto.



**Como ver a data de criação?**

O comando ***stat*** não  exibe a data de criação, essa opção fica em branco, mas podemos saber a data em que foi criado o objeto, ao rodar o comando ***stat***, observe o campo '***inode***', pegue o número que estiver nele:

```bash
linux:~/$ stat teste.txt
Arquivo: 'teste.txt'
Tamanho: 		0            		Blocos: 0       	Bloco IO: 4096 arquivo comum vazio
Dispositivo:	801h/2049d			Inode: 21501537		Links: 1
Acesso: 		(0664/-rw-rw-r--)	Uid: ( 1006/USER)	Gid: ( 1006/   USER)
Acessar: 		2018-08-31 14:23:23.000000000 -0300
Modificar: 		2018-08-31 14:23:23.000000000 -0300
Alterar: 		2019-06-20 07:12:24.471085247 -0300
Nascimento: -
```



Agora vamos usar o comando `debugfs -R 'stat <NUMERO>' SISTEMA_de_ARQUIVOS`, exemplo:

```bash
linux:~/$ sudo debugfs -R 'stat <21501537>' /dev/sda1
Inode: 		21501537   	Type: regular       Mode:  0664	Flags: 0x80000
Generation:	2578143794	Version: 0x00000000:00000001
User:  		1006   		Group:  1006   		Size: 0
File ACL: 	0       	Directory ACL: 0
Links: 		1   		Blockcount: 0
Fragment:				Address: 0			Number: 0	Size: 0
ctime: 		0x5d0b5c08:7050c2fc -- Thu Jun 20 07:12:24 2019
atime: 		0x5b89798b:00000000 -- Fri Aug 31 14:23:23 2018
mtime: 		0x5b89798b:00000000 -- Fri Aug 31 14:23:23 2018
crtime: 	0x5d0b5b75:17cc6834 -- Thu Jun 20 07:09:57 2019
Size of extra inode fields: 32
EXTENTS:
```

Observe a opção `crtime`, que significa 'create time', ela mostra a data de criação do objeto.

É explicado um pouco sobre 

## CARACTERES CURINGA

Muitas  vezes é necessário fazer uma busca, uma listagem ou até mesmo criações  de vários objetos, isso se torna maçante se tiver que ser feito mais de  uma vez, por isso, para esses tipos de trabalho, usamos os caracteres  coringa.

OBS.: Para  uso de tais caracteres, os objetos a serem listados, devem ter algo em  comum, uma espécie de prefixo, que possa abranger a todos que o tenha.

Ex:  Vários arquivos num sistema que tem o nome começando por 'projeto-1',  todos os arquivos que tiverem esse nome, possuem um prefixo `projeto-1` e  logo são do mesmo projeto, sendo assim, podemos usar os caracteres  coringa neles.

**Os caracteres coringa são:**
```
Caractere	Nome
{}          Chaves
[]          Lista
?           Interrogação
*           Asterisco
```



### CHAVES {}

As  chaves possuem um funcionamento parecido com a lista, mas seu maior  destaque é durante a criação de pastas e arquivos, por exemplo, crie  vários arquivo com nome projeto-1 até projeto-20.

Para criar, poderíamos fazer isso um a um, mas com as chaves, podemos criar todos de uma só vez.

```bash
linux:~/$ touch projeto-{1..20}

# Criando projeto-1 até projeto-20.
```

O ponto ponto `..`, funciona como um "até", partindo de um ponto e indo até o outro ponto.



As chaves funcionam também na listagem, por exemplo:

```bash
linux:~/$ ls projeto-{7..15}
projeto-10  projeto-11  projeto-12  projeto-13  projeto-14  projeto-15  projeto-7  projeto-8  projeto-9
```

Perceba que funcionou, o seu resultado pode estar desordenado, mas funcionou.



As  chaves, assim como a lista, também aceitam caracteres individuais para  pesquisa, mas para pesquisar nas chaves, você deve separar tudo que for  um caractere individual por uma vírgula, exemplo:

```bash
linux:~/$ ls projeto-{1,9}
projeto-1  projeto-9

# Esse comando, pesquisa projeto-1 e projeto-9, 
# porque o 1 e 9 estão separados por vírgula.
```



Você pode também ir adicionando outras chaves, por exemplo:

```bash
linux:~/$ ls projeto-{1..3}-Part_{0..2}
ls: não foi possível acessar 'projeto-1-Part_0': Arquivo ou diretório inexistente
ls: não foi possível acessar 'projeto-1-Part_1': Arquivo ou diretório inexistente
ls: não foi possível acessar 'projeto-1-Part_2': Arquivo ou diretório inexistente
ls: não foi possível acessar 'projeto-2-Part_0': Arquivo ou diretório inexistente
ls: não foi possível acessar 'projeto-2-Part_1': Arquivo ou diretório inexistente
ls: não foi possível acessar 'projeto-2-Part_2': Arquivo ou diretório inexistente
ls: não foi possível acessar 'projeto-3-Part_0': Arquivo ou diretório inexistente
ls: não foi possível acessar 'projeto-3-Part_1': Arquivo ou diretório inexistente
ls: não foi possível acessar 'projeto-3-Part_2': Arquivo ou diretório inexistente

# Em vermelho no resultado acima, está os nomes de     
# arquivos que o 'ls' tentou procurar.
```

Como  esses arquivos não existem, o comando deu erro, mas serve para vermos o  que ele iria pesquisar.



### LISTA [ ]

A  lista é muito seletiva, ela só dá *match* com o que estiver dentro dela e com uma certa limitação, diferente das chaves, a lista não serve para ser usada durante a criação de um objeto, apenas para listagem.

**Crie alguns arquivos**

\- - - Criação dos arquivos pata, pate, pati, pato, patu
```bash
# Criando vários arquivos com um prefixo e variando as vogais.
linux:~/$ touch pat{a,e,i,o,u}

linux:~/$ ls
pata  pate  pati  pato  patu
```
Perceba  que se fosse usado as chaves da forma '`pat{a..u}`', seria usado o alfabeto para criar os arquivos, dessa forma teríamos arquivos como os listados abaixo e não como foram criados:
```
pata
patb
patc
patd
pate
patf
patg
path
pati
patj
patk
patl
patm
patn
pato
patp
patq
patr
pats
patt
patu
```
**Agora vamos listar os arquivo que terminam com E,I e U:**
```bash
# listagem de alguns arquivos que terminam com vogal:
linux:~/$ ls pat[eiu]
pate  pati  patu

# Perceba que cada arquivo dentro da lista, é considerado um caractere individual.
```
Outra  forma de listagem, é a listagem negada, como assim? tudo o que estiver dentro da lista não será listado, para isso, usamos o sinal de exclamação ou Acento circunflexo, como é usado nas Expressões Regulares.
```bash
# Listagem negando os arquivos, usando Sinal de Exclamação.
linux:~/$ ls pat[!eiu]
pata  pato

# Listagem negando os arquivos, usando Acento circunflexo.
linux:~/$ ls pat[^eiu]
pata  pato
```
Tanto  o sinal de exclamação como o acento circunflexo, representam a exclusão  de caracteres dentro da lista. 

Como usar um "até" na lista? ao invés  de usar o ponto ponto como o das chaves, na lista usa-se o traço`-`.

```bash
# Listagem usando um “até”
linux:~/$ ls pat[a-u]
```
Perceba que será usado todas as letras do alfabeto para pesquisa, como só temos as vogais, somente elas serão pesquisadas.

Outra  diferença entre a lista e as chaves, é que se for usado as chaves para  listagem, e alguns ou todos os arquivos procurados não existirem, o  comando '***LS***' retorna  um erro, já a lista não, se houver apenas 1 match, ela não exibe erro,  agora se não houver match nenhum, ai o '***LS***' exibe erro.



**Podemos usar alguns truques ao utilizar a lista, por exemplo:**


Ao  invés de criar uma lista contendo todos os números (de 0 até 9) podemos  informar que é de 0 até 9 de uma forma diferente, como:
```
Forma feia de fazer: 	[0123456789]
Forma bonita de fazer:	[0-9]
```

Essas duas formas abrangem números de 0 até 9, isso porque  traço funciona dizendo que "vai até" algum caractere.

**Abranger um alfabeto (como fazer):**
```
Maiúscula e Minúscula: [A-Za-z]
Minúscula: [a-z]
Maiúscula: [A-Z]
```



### ASTERISCO \*

O  asterisco da match com muitos caracteres e em grande quantidade, não  somente isso, ele é um coringa muito usado, isso acontece por causa da  sua nobre função. Por exemplo, se você quiser apagar todos os arquivos  de um diretório, mas não quer apagar o diretório, você usa o comando '***rm***' com auxílio do asterisco, como abaixo:
```bash
# Apagando todos os arquivos e diretórios dentro do diretório atual.
linux:~/$ rm -r *
```


**Fazendo listagem usando asterisco** 

Por dar match com tudo, é uma ótima ferramenta para listagem, mas tem que saber usar.
```bash
# Listando tudo que termina com '-release' dentro de /etc/
linux:~/$ ls /etc/*-release
/etc/lsb-release  /etc/os-release
```
Com um comando, foi possível descobrir todos os arquivos que terminam com '**-release**' que estão dentro de '**/etc**'.

Más, e se você quiser exibir o conteúdo desses dois arquivos? 
basta dar um '***cat***' usando o asterisco.

```bash
# Exibindo o conteúdo de tudo que termine com '-release' dentro de '/etc/'
linux:~/$ cat /etc/*-release
DISTRIB_ID=Ubuntu
DISTRIB_RELEASE=16.04
DISTRIB_CODENAME=xenial
DISTRIB_DESCRIPTION="Ubuntu 16.04.6 LTS"
NAME="Ubuntu"
VERSION="16.04.6 LTS (Xenial Xerus)"
ID=ubuntu
ID_LIKE=debian
PRETTY_NAME="Ubuntu 16.04.6 LTS"
VERSION_ID="16.04"
HOME_URL="http://www.ubuntu.com/"
SUPPORT_URL="http://help.ubuntu.com/"
BUG_REPORT_URL="http://bugs.launchpad.net/ubuntu/"
VERSION_CODENAME=xenial
UBUNTU_CODENAME=xenial

# Listando tudo que termine com 'ta':
linux:~/$ ls *ta
pata
```
Como  pode ver, não importa a quantidade de caracteres que tenha e nem qual seja o caractere, o asterisco `*` vai dar match (apenas se o prefixo estiver correto e se der para dar match)

Não  adianta listar tudo que comece com 'pa' sendo que não exista nada que  comece com 'pa', vai retornar erro, por isso, o prefixo que une um ou vários objetos tem que existir.



### INTERROGAÇÃO ?

O  sinal de interrogação é bem simples e só casa somente com um único  caractere, ele é muito utilizado quando se sabe o que pesquisar, porém,  pode ocorrer variações no que está procurando, por exemplo: 



**Crie os arquivos usando o comando abaixo:**

Comandos (Comandos encadeados criando arquivos):
```bash
# Comandos encadeados para criar alguns arquivos.
linux:~/$ touch bra{s,z}il && touch g{ar,}ota && touch pa{i,í}s
```
Agora vamos aos testes para que você possa ver como funciona o sinal de interrogação como um curinga.
```bash
linux:~/$ ls pa?s
pais  país
```
Viu como ele age, você deve saber o que está procurando, porém, pode ter alguma variação em sua sintaxe.



Você se esqueceu em que língua criou seu arquivo chamado 'brasil' (se foi criado em português brasileiro ou em Inglês).
```bash
linux:~/$ ls bra?il
brasil  brazil

# Dessa forma fica fácil de saber o que está procurando.
```
Vale  ressaltar que, como o interrogação só funciona para um único caractere,  você pode inserir mais interrogações que irão substituir mais  caracteres.
```bash
linux:~/$ ls g??ota
garota
```
Perceba  que, novamente, cada interrogação substitui apenas um e somente um  único caractere, então você deve saber todas as letras e suas variações  da string que deseja procurar, caso contrário, no menor descuido, vai  dar erro na listagem.



## TAR

O tar é  um empacotador de arquivos, contendo diversos arquivos num arquivo  final, ai você me pergunta, o que seria um empacotador de arquivos?

Imaginamos  que uma loja fez uma venda pela Internet, nessa venda, o cliente  comprou algumas coisas como camiseta, calça, meia, e alguns objetos a  mais, a loja precisa empacotar esses produtos para ser entregue ao  cliente, então um funcionário da loja vai e empacota tudo numa única  caixa de papelão (supondo que caiba tudo lá dentro) e então manda para a  transportadora, isso é uma analogia com o empacotador de arquivos, ele  agrupa tudo num único objeto, e qualquer coisa que seja feito com esse  objeto, ele irá carregar o que estiver dentro dele.

Perceba  que é diferente de compactador, um empacotador apenas agrupa tudo num  único objeto, já o compactador, também o faz, porém, ele comprime o  arquivo final, deixa ele menor, menos pesado, seria como pegar essa  caixa com os produtos do cliente e amassar ela, para deixá-la menor  (talvez não seja uma boa ideia).

O  bom de usar o tar é que ele usa compactadores, não precisando usar  outro comando para compactar e descompactar, para isso, usamos opções.

**Opções:**

```
--delete	Deleta um arquivo que esteja dentro de um arquivo empacotado;
-c          Cria um novo arquivo (empacota, e pode ser compactado);
-x          Extrai os arquivos;
-t          Lista os arquivos dentro do tar;
-v          Modo verbose;
-p          Preserva as permissões;
-f          Define o nome do arquivo;
-r          Adiciona arquivos a um arquivo .tar já criado;
-j          Compacta usando o algoritmo bzip2 (bz2);
-J          Compacta usando o algoritmo xz;
-z          Compacta usando o algoritmo gzip (gz);
```



**Crie alguns arquivos com o comando baixo:**
```bash
# Criando arquivos
linux:~/$ touch redacao_part{1..6}
```



**Agora vamos agrupar os arquivos:**

```bash
# Agrupando os arquivos da redação
linux:~/$ tar -cvf redacao.tar redacao_part{1..5}
```


**Agora vamos listar o conteúdo do arquivo tar:**

```bash
linux:~/$ tar -tf redacao.tar
redacao_part1
redacao_part2
redacao_part3
redacao_part4
redacao_part5
```


Agora  vamos adicionar mais um arquivo dentro do 'redacao.tar', lembrando que  isso só é possível porque o arquivos está empacotado e não compactado,  se estiver compactado não é possível adicionar mais arquivos.

```bash
linux:~/$ tar -rf redacao.tar redacao_part6
```

Agora liste novamente e verá que a parte 6 está dentro do arquivo 'redacao.tar'.



**Remover um arquivo que esteja dentro de um arquivo.tar.**

```bash
linux:~/$ tar --delete redacao_part{1..5} -f redacao.tar
```

Ao listar novamente, verá que só ficou a parte 6 da redação.



**Até agora só agrupamos os arquivos, vamos compacta-los.**

Primeiro, remova o arquivo existente:

```bash
linux:~/$ rm redacao.tar
```



Temos duas formas de compactar o arquivo, a primeira é criar o arquivo tar e  depois compactar (menos usual) e a segunda é criar o arquivo tar  compactando ele (mais usual).
```bash
# Criando um arquivo '.tar', compactado com gzip.
linux:~/$ tar -cvzf redacao.tar.gz redacao_part{1..5}

# Criando um arquivo '.tar', compactado com xz.
linux:~/$ tar -cvJf redacao.tar.xz redacao_part{1..5}

# Criando um arquivo '.tar', compactado com bzip2.
linux:~/$ tar -cvjf redacao.tar.bz2 redacao_part{1..5}

## Descompactando

# Descompactando um arquivo '.tar', compactado com gzip.
linux:~/$ tar -xvzf redacao.tar.gz redacao

# Descompactando um arquivo '.tar', compactado com xz.
linux:~/$ tar -xvJf redacao.tar.xz redacao

# Descompactando um arquivo '.tar', compactado com bzip2.
linux:~/$ tar -xvjf redacao.tar.bz2 redacao
```



## CPIO

Serve para agrupar (igual o tar), porém, seu funcionamento é bem diferente, ele só agrupa através de redirecionamento de saída padrão, ou seja, pega a saída de outro comando para agrupar.

**Opções:**
```
-o	Cria o arquivo;
-i	Extrai os arquivo;
-d	Cria os diretórios principais (caso eles não existam);
-A	Anexa arquivos ao fim de um arquivo existente;
-F	Informa o arquivo;
-t	Lista o conteúdo do arquivo.cpio.
```
Uma forma mais simplista de usar o CPIO é fazendo com que ele busque os arquivos a partir de um arquivo.txt, dentro de um arquivo de texto coloque as linhas abaixo:

**--- Dados do arquivo de texto**

```
/etc/passwd
/etc/group
```



**Agora rode o comando abaixo para agrupar:**

```bash
linux:~/$ cpio -o < arquivo.txt > backup.cpio

# Dessa forma, todos os caminhos de arquivos que estiverem dentro
# do arquivo de texto serão agrupados.
```

Você pode usar um '`cpio -t < backup.cpio`' para listar seu conteúdo.

```bash
linux:~/$ cpio -i < bakcup.cpio

# Desagrupando
```



**Adicionando mais arquivos dentro de um arquivo '.cpio':**

Para isso, apague o conteúdo de 'arquivo.txt' e adicione `/etc/shadow`.

```bash
# Adicionando /etc/shadow no backup.
linux:~/$ sudo cpio -Ao < arquivo.txt -F backup.cpio

# Listando seu conteúdo:
linux:~/$ cpio -t < backup.cpio
/etc/passwd
/etc/group
/etc/shadow
```



**Compactando um arquivo '.cpio'**

Para o *cpio*, temos duas formas de compactar (igual ao *tar*), porém, nenhum delas é através de parâmetros.



**Compatando usando compactadores**

Para isso, utilizamos os algoritmos de compactação separadamentevamos, sem envolver o *cpio* como um comando.

**Compactando usando compactadores e cpio juntos**

No momento em que formos agrupar, redirecionamos a saída do *cpio* (usando *pipe*) para algum dos compactadores, dessa forma ele fará a compactação do pacote '.cpio' (essa forma não será apresentada).

Todos os algoritmos apresentados, seguem a mesma forma para compactar e descompactar, são eles: `GZIP, BZIP2, XZ`.



**Compatando usando compactadores:**

```bash
# Compactando usando o gzip.
linux:~/$ gzip backup.cpio

# Compactando usando o bzip2
linux:~/$ bzip2 backup.cpio

# Compactando usando o xz.
linux:~/$ xz backup.cpio 

### Listando ###

# Arquivo gzip:
linux:~/$ ls
arquivo.txt  backup.cpio.gzip

# Arquivo bzip2:
linux:~/$ ls
arquivo.txt  backup1.cpio.bz2  

# Arquivo xz:
linux:~/$ ls
arquivo.txt  backup2.cpio.xz 
```

Perceba que ao compactar, o arquivo empacotado sumiu, para manter o arquivo empacotado, precisamos usar o argumento `-k`.

**Mantendo o arquivo agrupado:**

```bash
# Compactando usando o gzip.
linux:~/$ gzip -k backup.cpio

# Compactando usando o bzip2
linux:~/$ bzip2 -k backup.cpio

# Compactando usando o xz.
linux:~/$ xz -k backup.cpio 

### Listando ###

# Arquivo gzip:
linux:~/$ ls
arquivo.txt  backup.cpio.gzip  backup.cpio

# Arquivo bzip2:
linux:~/$ ls
arquivo.txt  backup1.cpio.bz2  backup.cpio

# Arquivo xz:
linux:~/$ ls
arquivo.txt  backup2.cpio.xz  backup.cpio
```



**Descompactando:**

Temos duas formas de descompactar esses algoritmos, uma das formas é usando um outro comando, que é uma variação do comando de compactação e a outra forma é através de parâmetros.

```bash
### Usando parâmetros:

# Descompactando usando o gzip.
linux:~/$ gzip -d backup.cpio

# Descompactando usando o bzip2
linux:~/$ bzip2 -d backup.cpio

# Descompactando usando o xz.
linux:~/$ xz -d backup.cpio

### Usando outro comando

# Descompactando usando o gzip.
linux:~/$ gunzip -k backup.cpio

# Descompactando usando o bzip2
linux:~/$ bunzip -k backup.cpio

# Descompactando usando o xz.
linux:~/$ unxz -k backup.cpio
```

O uso do `-k` faz com que o algoritmo de compactação mantenha o arquivo original após a compactação/descompactação.



## DD

O comando DD realizar uma cópia bit a bit, pode ser usado para copiar uma partição inteira, arquivos e etc.

**Opções:**
```
if=origem			É a origem;
of=destino			É o destino;
bs=N				Tamanho dos blocos;
count=N				Quantas vezes o bloco será copiado;
status=progress		Exibe o progresso.
```

***Sintaxe simples:***
sudo if=Local_origem of=Local_Destino

```bash
# Copiando uma ISO para uma partição USB
linux:~/$ sudo dd if=/home/USER/Downloads/linuxmint-19.1-cinnamon-64bit.iso of=/dev/sdb1 bs=4M status=progress

# Copiando um CD/DVD para uma ISO
linux:~/$ sudo dd if=/dev/sr0 of=img.iso bs=2048 status=progress
```


**Criar um "HD" virtual:**

```bash
# Cria um dispositivo de bloco com tamanho de 1G chamado part
linux:~/$ sudo dd if=/dev/zero of=/home/USER/part bs=1M count=1000
```
Isso é usado para criar uma “partição” dentro do disco,  onde você pode criptografar essa “partição”, você pode usar o `/dev/zero` para formatar discos inteiros também.



**Formatar um disco**

```bash
linux:~/$ sudo dd if=/dev/zero of=of=/dev/sbd1
```



# 103.4 Usar Streams, pipes e redirecionadores

Redirecionar fluxos de texto e conectá-los a fim de eficientemente  processar os dados. As tarefas incluem redirecionamento da entrada  padrão, da saída padrão e dos erros padrão, canalização (piping) da  saída de um comando à entrada de outro comando, usar a saída de um  comando como argumento para outro comando e enviar a saída de um comando simultaneamente para a saída padrão e um arquivo.

## Redirecionamento de Entrada, Saída e erro padrão

### Entrada Padrão (stdin - 0)

A entrada padrão é denominada ***stdin***, é  tudo que envia dados para máquina, por padrão o que envia esses dados é  o teclado, mas você pode mudar isso, mudando a entrada padrão, por  exemplo, a entrada poderia ser um arquivo, como usamos no ***CPIO***.

Para  mudar a entrada padrão você deve usar o símbolo de menor `<`, o uso  de um único símbolo de menor informa que a entrada não é mais padrão, e  que a entrada vem de um arquivo, como no ***CPIO***, isso simplesmente é uma mudança de entrada padrão.

**Crie um arquivo chamado teste.txt**

--- Conteúdo de teste.txt

```
OLA
```


**Mudando a entrada padrão**

```bash
linux:~/$ tr 'A-Z' 'a-z' < teste.txt
ola
```

O comando ***tr*** só  funciona por meio de redirecionamento de entrada, recebendo a entrada de alguma  forma, por isso, mudamos a entrada padrão (A entrada padrão nesse caso é  um arquivo).



#### Here document


Um  “Documento aqui” traduzido ao pé da letra, é quando utilizamos dois símbolos de menor `<<`, esse é um assunto um pouco avançado, mas  vou tentar exemplificar para ficar mais fácil de entender. 

Uma forma que deve ser conhecida pela maioria é o uso do EOF com o comando `cat`, EOF significa '***end-of-file***' (fim de arquivo), é usada para expressar que chegou ao fim do arquivo e a partir dali, não pode ser mais lido nenhum dado.

```bash
linux:~/$ cat << EOF
> Ola
> Tudo Bem?
EOF
```

Vale ressaltar que o EOF é  um delimitador, podendo ser usado qualquer outro delimitador. Dessa  forma, podemos criar “documentos” na entrada padrão, com espaços, quebra  de linha, tab, entre outros, sem a necessidade de ter isso num arquivo.  Para jogar o que for digitado num arquivo, usa-se um redirecionador de  saída padrão `cat << EOF > texte.txt`.



#### Here strings

Uma  “cadeia de caracteres”, é quando passamos uma cadeia de caracteres como  entrada para um comando, isso é feito porque no redirecionamento de  entrada, no modo simples (usando apenas um símbolo de menor) ele irá  procurar um arquivo, usando Here Strings, você passa o valor diretamente, sem ter que ter esse valor num arquivo. 

Exemplo,  você quer converter o dado de uma variável para qualquer outro formato,  no exemplo abaixo, vamos converter os dados para maiúsculo.

**Convertendo  dados de uma variável:***

```bash
# Convertendo os dados da variável para maiúsculo, 
# a variável 'nome' tem como valor o nome 'joao'

linux:~/$ tr 'a-z' 'A-Z' <<< "$nome"
JOAO
```



**Convertendo  dados de uma String:**

```bash
# Convertendo 'user' para maiúsculo.

linux:~/$ tr 'a-z' 'A-Z' <<< "user"
USER
```
Perceba  que esse comando converte um dado passado diretamente a ele, enquanto o  modelo acima pega o dado da variável, então, pode-se usar as duas  formas, isso não se limita a apenas o uso com tr, e sim, a qualquer coisa que precise ter o redirecionamento de entrada trocado.

**Resumo:**

```
<	- Redireciona a entrada padrão para um arquivo;
<<	- Redireciona a entrada padrão para uma entrada de fluxo;
<<<	- Redireciona a entrada padrão para Strings;
```



#### XARGS

Construir e executar linhas de comando a partir da entrada padrão.

O comando `xargs` pega a saída de um comando e executa outro comando, executando linha por linha, é muito usado com o comando `find`, mas pode ser usado com qualquer comando.

**Opções:**

```tex
-0, --null 
         Os itens são separados por nulo, sem espaço em branco; desabilita processamento de aspas e barra invertida e de EOF lógicos.

-a, --arg-file=ARQUIVO
         Lê argumentos de ARQUIVO, não da entrada padrão.
                             
-p, --interactive
         Solicita confirmação antes de executar os comandos.

-n, --max-args=MÁX-ARGS
         Usa no máximo MÁX-ARGS argumentos por linha de comando.
```



**Exemplos:**

```bash
# Procurando por arquivos de texto no /tmp, redirecionando a saída de erro
# para /dev/null e jogando os arquivos encontrados para o XARGS,
# ele vai rodar um comando 'echo "Achados: "' para cada linha que o find
# passar para ele:
linux:/tmp\$ find /tmp/ -type f -name "*.txt" 2> /dev/null | xargs -n1 echo "Achado: "
Achado:  /tmp/join.txt
Achado:  /tmp/teste_arquivo1_gz.txt
Achado:  /tmp/kernel-versão.txt
Achado:  /tmp/copy_join.txt
Achado:  /tmp/oi.txt

## A opção '-n 1' do xargs só permite 1 argumento por linha.

# Exemplo sem a opção de '-n 1' argumento por linha:
linux:/tmp\$ find /tmp/ -type f -name "*.txt" 2> /dev/null | xargs echo "Achado: "
Achado:  /tmp/join.txt /tmp/teste_arquivo1_gz.txt /tmp/kernel-versão.txt /tmp/copy_join.txt /tmp/oi.txt

## Todos os arquivos encontrados são exibidos na mesma linha (como vários
## argumentos).
```

A opção `-p` vai perguntar se você deseja executar a linha em que for exibida.

```bash
# Uso da opção -p:
linux:/tmp\$ find /tmp/ -type f -name "*.txt" 2> /dev/null | xargs -n1 -p echo "Achado: "
echo 'Achado: ' /tmp/join.txt ?...y
Achado:  /tmp/join.txt
echo 'Achado: ' /tmp/teste_arquivo1_gz.txt ?...n
echo 'Achado: ' /tmp/kernel-versão.txt ?...

# Se você digitar 'y' ele executa, se digitar 'n' ele nao irá executar.
```

A opção `-a` muda a entrada padrão para um arquivo e não vinda de um comando:

```bash
# Jogando os arquivos encontrados no arquivo 'arquis':
linux:/tmp\$ find /tmp/ -type f -name "*.txt" 2> /dev/null > arquis

# Fazendo o xargs lêr do arquivos 'arquis':
linux:/tmp\$ xargs -n1 -a arquis echo "Achado: "
Achado:  /tmp/join.txt
Achado:  /tmp/teste_arquivo1_gz.txt
Achado:  /tmp/kernel-versão.txt
Achado:  /tmp/copy_join.txt
Achado:  /tmp/oi.txt

# Fazendo o xargs apagar todos os arquivos de texto 
# encontrado pelo comando find:
linux:/tmp\$ find /tmp/ -type f -name "*.txt" 2> /dev/null | xargs -n1 -p rm
rm /tmp/join.txt ?...y
rm /tmp/teste_arquivo1_gz.txt ?...y
rm /tmp/kernel-versão.txt ?...y
rm /tmp/copy_join.txt ?...y
rm /tmp/oi.txt ?...y

# Agora rode o comando find de novo para ver se ele encontra, 
# caso não retorne nada, é porque não encontrou:
linux:/tmp\$ find /tmp/ -type f -name "*.txt" 2> /dev/null

```

Usando a opção `-0` ou `--null`:

```bash
linux:/tmp\$ find /tmp/ -type f -name "*.txt" 2> /dev/null
/tmp/Kenerl 2.0 linux.txt
/tmp/download_isos.txt
/tmp/linux mint 20.txt

# Criei 3 arquivos, 2 com espaçõs no nome e 1 sem espaço no nome.
# o find mostra eles como devem ser. 

# Olha o que acontece quando rodo com o xargs:
linux:/tmp\$ find /tmp/ -type f -name "*.txt" 2> /dev/null | xargs -n1 echo "achado: "
achado:  /tmp/Kenerl
achado:  2.0
achado:  linux.txt
achado:  /tmp/download_isos.txt
achado:  /tmp/linux
achado:  mint
achado:  20.txt

## O xargs acha que cada espaço é um nome diferente.
# Para que isso não aconteça, usamos a opção -0, para que ele
# só interprete caractere nulo (^@), quem faz essa troca para 
# caractere nulo é o find, usando a opção '-print0':
linux:/tmp\$ find /tmp/ -type f -name "*.txt" 2> /dev/null -print0 | xargs -0 -n1 echo "Achados: "
Achados:  /tmp/join.txt
Achados:  /tmp/Kenerl 2.0 linux.txt
Achados:  /tmp/download_isos.txt
Achados:  /tmp/linux mint 20.txt
```

Nem todos os comando tem a opção `-print0`, nesse caso vou usar um exemplo do comando `fdupes`, ele encontra arquivos duplicados no sistema e não possui essa opção.

Outro exemplo do xargs, usando o [fdupes](https://www.100security.com.br/fdupes):

```bash
### Opções do comando FDUPES:
# -r = Recursivo
# -n = Exclui arquivo vazios da busca.
# -A = Exclui arquivos ocultos da busca.
linux:/tmp\$ fdupes -rnA /tmp/ 2>/dev/null | sed '/^$/d; s/^/"/; s/$/"/'
"/tmp/ola.txt"
"/tmp/join.txt"
"/tmp/teste arquivo gz"
"/tmp/arquivo_gz"
"/tmp/arq.gz"
"/tmp/pasta1/arq.gz"

# Nesse caso eu encapsulei a saída para delimitar os nomes dos arquivos.
# O xargs vai interpretar cada nome como deveria.
```

<span style="color:red">**Para relembrar a explicação acima, consulte a seção**</span> [quoting](#quoting).

- **Explicação**
  - **sed** 
    Editor de fluxo para filtrar e transformar textos;
  - **/^$/d**
    Apagar as linhas em branco;
  - **s/^/"/**
    Adiciona aspas (`"`) no começo da linha;
  - **s/$/"/**
    Adiciona aspas (`"`) no fim da linha;



**Apagando os arquivos duplicados**:

```bash
# Agora vamos apagar os arquvios duplicados:
linux:/tmp\$ fdupes -rnA /tmp/ 2>/dev/null | sed '/^$/d; s/^/"/; s/$/"/' | xargs -n1 -p rm
rm /tmp/ola.txt ?...y
rm /tmp/join.txt ?...y
rm '/tmp/teste arquivo gz' ?...y
rm /tmp/arquivo_gz ?...y
rm /tmp/arq.gz ?...y
rm /tmp/pasta1/arq.gz ?...y
```



### Saída Padrão (stdout - 1)

Uma  saída padrão, conhecido como stdout, é tudo o que é enviado para tela  (monitor), o monitor é o padrão da saída, para redirecionar a saída  usamos o símbolo de maior `>`.
```bash
# Mudando a saída padrão do echo
linux:~/$ echo "oi" > teste.txt
```
Dessa forma, ao invés do echo manda  para saída padrão (monitor), ele envia essa saída para um arquivo.  Perceba que o uso de apenas um símbolo de maior faz a sobrescrita do  arquivo, e caso ele já exista e possua dados, esses dados serão  sobrescrevidos.

**Append**

Muitas  vezes, ao mudar a saída padrão, não queremos sobrescrever o conteúdo do  arquivo final, para isso acrescentamos o novo conteúdo, há um já  existente. O arquivo teste.txt já possui a palavra 'oi', agora vamos  adicionar dados a esse arquivo, sem ter que apagar o que já existe, para  isso usamos dois símbolos de maior `>>`.

```bash
# Adicionando dados:
linux:~/$ echo "Tudo bem?" >> teste.txt

# listando o conteúdo de teste.txt:
linux:~/$ cat teste.txt
oi
Tudo Bem?
```

Perceba que o que já existia no arquivo não foi apagado.




### PIPE

O  pipe é considerado um redirecionador de saída, mas eu gosto de julgá-lo  como um redirecionador misto, isso porque, o pipe pega a saída de um  comando e redireciona como entrada de outro comando, ele não manipula  apenas a saída mas a entrada também.

```bash
# Redirecionando a entrada e saída padrão:
linux:~/$ echo "oi" | tr 'a-z' 'A-Z'
OI

# Redirecionando a entrada e saída padrão:
linux:~/$ echo "oi, tudo bem com voce" | egrep "o"
oi, tudo bem com voce
```

No segundo exemplo, quando feito o teste no terminal, você verá que as letras `o` ficarão vermelhas (<span style="color:#FF0000">o</span>).



### TEE

Lê da entrada padrão e grava na saída padrão e no arquivo escolhido.

O comando `tee` vai pegar a saída de um comando, exibir essa saída na tela e então vai ao mesmo tempo gravar ela em um arquivo.



**Opções:**

```tex
-a, --append
	Anexa os arquivos (incrementa).
```



**Exemplos: **

```bash
# Exibindo a versão do kernel e escrevendo num arquivo: 
linux:/tmp\$ echo "Kernel: $(uname -r)" | tee kernel-versão.txt
Kernel: 5.4.0-42-generic

# Exiba o conteúdo de kernel-versão.txt:
linux:/tmp\$ cat kernel-versão.txt
Kernel: 5.4.0-42-generic
```

Vale lembrar que dessa forma o comando `tee` está apagando e escrevendo de novo no arquivo, para que ele possa incrementar, usaremos a opção `-a`:

```bash
# Exibindo o nome da máquina e escrevendo num arquivo: 
linux:/tmp\$ echo "Hostname: $(hostname)" | tee -a kernel-versão.txt
Hostname: chiredean

# Exiba o conteúdo de kernel-versão.txt:
linux:/tmp\$ cat kernel-versão.txt
Kernel: 5.4.0-42-generic
Hostname: chiredean
# Perceba que o conteúdo sofreu um incremento (não foi substituido).
```




### Erro Padrão (stderr - 2)

O erro padrão, ou saída padrão de erros é a saída padrão, ou seja, é o monitor, a menos que isso seja alterado.

Diferente do `stdin` e `stdout` que possuem redirecionados sendo so símbolos de menor `<` e maior `>`, *stderr* usa o mesmo símbolo do redirecionamento de saída (*stdout* `>`), seguido do número que você deseja (0 = sdtin, 1 = sdtout e 2 = sdterr).

**OBS.**: Como *sdtin* e *stdout* possuem símbolos próprios (menor `<` e maior `>`), podemos omitir o número 0 e 1, pois cada símbolo já tem uma definição.

```bash
# Jogando a saída de erro para o arquivo erros_sdterr:
linux:/tmp\$ ls arq.gz 2> erros_stderr

## Como o arquivo arq.gz não existe, o comando ls vai retornar
## um erro na saída padrão:
linux:/tmp\$ ls arq.gz
ls: não foi possível acessar 'arq.gz': Arquivo ou diretório inexistente
## O '2>' informa que vamos redirecionar a saída de erro padrão 
## para o arquivo erros_stderr.

# Podemos usar o modo append, usando o símbolo de maior maior '>>',
# assim vamos ter um incremento no arquivo de erros e ele não será
# substituido:
linux:/tmp\$ ls arq1.gz 2>> erros_stderr

# Exibindo o conteúdo de erros_stderr:
linux:/tmp\$ cat erros_stderr 
ls: não foi possível acessar 'arq.gz': Arquivo ou diretório inexistente
ls: não foi possível acessar 'arq1.gz': Arquivo ou diretório inexistente
```



## Meios de redirecionar

Como já foi explicado boa parte de como funciona o sistema de redirecionamento, vou passar alguns exemplos que podem ser úteis:

**Exemplos:**

```bash
## Redirecionando a entrada padrão para um arquivo:
linux:/tmp\$ tr "a-z" "A-Z" < arquis 
/TMP/JOIN.TXT
/TMP/TESTE_ARQUIVO1_GZ.TXT
/TMP/KERNEL-VERSãO.TXT
/TMP/COPY_JOIN.TXT
/TMP/OI.TXT

## Redirecionando a entrada padrão para uma string:
linux:/tmp\$ tr "a-z" "A-Z" <<< "kernel"
KERNEL

## Redirecionando a entrada padrão para uma variável:
linux:/tmp\$ tr "a-z" "A-Z" <<< "$USER"
LINUX

## Redirecionando a entrada padrão para um fluxo de dados:
linux:/tmp\$ cat << "FIM"
> $USER
> $SHELL
> FIM
$USER
$SHELL

## Redirecionando a saída padrão para um arquivo:
linux:/tmp\$ echo "kernel" > arq_sdtout

## Redirecionando a saída padrão para um arquivo com append:
linux:/tmp\$ echo "kernel 2" >> arq_sdtout

## Redirecionando a saída de erro padrão para um arquivo:
linux:/tmp\$ ls kernel 2> arq_stderr

## Redirecionando a saída de erro padrão para um arquivo com append:
linux:/tmp\$ ls kernel2 2>> arq_stderr

## Conecta a saída de erro com a saída padrão.
# Trancar stderr ao sdtout:
linux:/tmp\$ ls -lh arq_stderr arq2_stderr > arq_saida 2>&1

# Exibindo o conteúdo de arq_saida:
linux:/tmp\$ cat arq_saida 
ls: não foi possível acessar 'arq2_stderr': Arquivo ou diretório inexistente
-rw-rw-r-- 1 linux linux 76 jul 23 14:05 arq_stderr

# O '2>&1' informa que a saída de erro será a mesma de stdout,
# no caso, vai para o mesmo arquivo.

# O '>&2' conecta a saída padrão na saída de erro:
linux:/tmp\$ ls -lh arq_stderr arq2_stderr 2> arq_saida2 >&2

# Exibindo o conteúdo de arq_saida2:
linux:/tmp\$ cat arq_saida2 
ls: não foi possível acessar 'arq2_stderr': Arquivo ou diretório inexistente
-rw-rw-r-- 1 linux linux 76 jul 23 14:05 arq_stderr

# O símbolo '2>&-' fecha a saída de erro (não existe saída de erro).
linux:/tmp\$ ls -lh arq2_stderr 2>&-
## O erro não será exibido, é o mesmo que '2> /dev/null'.
```



# 103.5 Criar, monitorar e eliminar processos

Executar o gerenciamento básico de processos.

Os processos trabalham com diferentes tipos de IDs, sendo eles:

| Nomeclatura | Descrição                                                |
| ----------- | -------------------------------------------------------- |
| PID         | Process ID (ID do processo)                              |
| PPID        | Parent Process ID (ID do processo pai do nosso processo) |
| UID         | User Identifield (ID do usuário)                         |
| GID         | Group Identifield (ID do grupo)                          |
| EUID        | Effective User ID                                        |
| EGID        | Effective Group IP                                       |

Quando um arquivo, um processo, dentre outras coisas são criadas no linux, elas são atribuidas a um usuário dono e um grupo dono, normalmente <span style="color:#F8F8FF">**euid**</span> e <span style="color:#F8F8FF">**egid**</span> são iguais a <span style="color:#F8F8FF">**uid**</span> e <span style="color:#F8F8FF">**gid**</span>. Mas quando usamos comando como `sudo` que invoca as permissões de outro usuário (executa com o outro usuário), ou quando possuem bits especiais setados, <span style="color:#F8F8FF">**euid**</span> e <span style="color:#F8F8FF">**egid**</span> são criados com o <span style="color:#F8F8FF">**uid**</span> e <span style="color:#F8F8FF">**gid**</span> do usuário que realmente efetuou a ação.

**Exemplo:**

```bash
# Crie um arquivo e veja quem é o usuario e dono dele:
linux:/tmp\$ touch arquivo1

linux:/tmp\$ stat arquivo1 | grep id
Acesso: (0664/-rw-rw-r--)  Uid: ( 1001/   linux)   Gid: ( 1001/   linux)

## Perceba que o usuário efeteivo era o linux (usuário linux) 

# Agora crie um novo arquivo usando o sudo:
linux:/tmp\$ sudo touch arquivo2

linux:/tmp\$ stat arquivo2 | grep id
Acesso: (0644/-rw-r--r--)  Uid: (    0/    root)   Gid: (    0/    root)

# Perceba que agora foi atribuido o ID 0 (id do root), porque quem criou
# o arquivo foi o root e não meu usuário linux.

# Essa é a diferença entre UID, GID e EUID e EGID.
```



## PS

O comando `ps` exibe os processos ativos no momento de sua execução. Se executado sem argumentos, vai exibir os processos apenas do usuário atual.

**Opções:**

```tex
-u	- Todos os processos por usuário;
-x	- Todos os processos que não tenham um tty atrelado;
-a	- Todos os processos de todos os usuários;
-f	- Mostra os processos em forma de árvore (exibe parentesco);
-l	- Mostra em forma de lista 
-C <string>	- Busca por tudo da string.
-A - Seleciona todos os processos de todos os usuários.
-U <user> - Pesquisa por usuário.

-o <formato> - Seleciona um formato para stdout.
state   = Estado do processo;
tty     = tty do processo;
user    = Usuário do processo;
command = Comando usado para criar o processo;
comm    = Nome do processo;
fname   = 8 bytes do nome do processo (não pega o nome todo);
rss     = Memória usada pelo processo;
ni      = É o valor de espaço do usuário, podemos usar para controlar o 
          valor da prioridade de um processo;
pri     = É a prioridade real do processo que é usada pelo kernel do Linux 
          para agendar uma tarefa. Maior número significa menor prioridade;
state   = Estado do processo em formato Linux moderno.
s       = Estado do processo em formato Linux moderno.
stat    = Estado do processo em formato BSD.

wchan   = Endereço da função do kernel em que o processo está em suspensão 
          (use wchan se desejar o nome da função do kernel).
          As tarefas em execução exibirão um traço ('-') nesta coluna.
```

**Informações fornecidas pelo PS**

```
USER - Nome de usuário efetivo.

PID - O número que representa o ID do processo.

%CPU - Utilização da CPU pelo processo no formato "##.#".
Atualmente, é o tempo de CPU usado dividido pelo tempo em que o
processo está em execução (proporção cputime / realtime), expresso
em porcentagem.

%MEM - Proporção do tamanho do conjunto residente do processo para a
memória física na máquina, expressa como uma porcentagem. 

VSZ - Tamanho da memória virtual do processo em KiB (unidades de
1024 bytes) que o processo está consumindo.

RSS - Tamanho do conjunto residente, a memória física não trocada
que uma tarefa usou (em kilobytes).

TTY - São consoles virtuais. Eles são terminais virtuais (emuladores
de terminal) implementados pelo kernel e independentes do ambiente
gráfico.

START - Hora em que o comando foi iniciado.
Se o processo foi iniciado há menos de 24 horas, o formato de saída
é "HH:MM:SS"; caso contrário, é "Mmm dd" (onde Mmm é um nome de mês
com três letras).

TIME - Tempo acumulado da CPU, formato "[DD-] HH:MM:SS". 
(também conhec ido por cputime).

COMMAND - Comando usado para criar o processo.
```



**Exemplos:**

```bash
# Exibindo os processos do usuário atual:
linux:~/$ ps
    PID TTY          TIME CMD
  21362 pts/0    00:00:00 bash
  21556 pts/0    00:00:00 ps
  
# Exibindo todos os processos de todos os usuários:
linux:/tmp\$ ps -a
    PID TTY          TIME CMD
  21361 pts/0    00:00:00 su
  21362 pts/0    00:00:00 bash
  21548 pts/1    00:00:00 sudo
  21549 pts/1    00:00:00 vim
  21585 pts/0    00:00:00 ps
  
## Exibindo os processos que não tem uma tty atrelado.
# Para isso vamos ter que exibir outros usuário também:
linux:/tmp\$ ps -uxa
root       19146  0.0  0.0      0     0 ?        I    13:40   0:00 [kworker/0:3-events]
root       19229  0.0  0.0      0     0 ?        I    13:41   0:00 [kworker/3:1-events]
root       19278  0.6  0.0      0     0 ?        I    13:45   0:13 [kworker/2:1-events]
root       20894  0.1  0.0      0     0 ?        I    14:01   0:01 [kworker/u8:3-i915]
# Suprimi a saída...

# Mostrando em modo de árvore:
linux:/tmp\$ ps -uaf
USER         PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
linux      21377  0.0  0.0  11048  5332 pts/1    Ss   14:12   0:00 bash
root       21548  0.0  0.0  14048  5284 pts/1    S+   14:14   0:00  \_ sudo vim /usr/bin/ps1
root       21549  0.0  0.1  25104 10236 pts/1    S+   14:14   0:00      \_ vim /usr/bin/ps1
linux       8028  0.0  0.0  11176  5404 pts/0    Ss   11:02   0:00 bash
root       21361  0.0  0.0  13708  4948 pts/0    S    14:11   0:00  \_ su linux
linux      21362  0.0  0.0   9996  4184 pts/0    S    14:11   0:00      \_ bash
linux      21652  0.0  0.0  11912  3736 pts/0    R+   14:21   0:00          \_ ps -uaf

# Em modo de lista:
linux:/tmp\$ ps -la
F S   UID     PID    PPID  C PRI  NI ADDR SZ WCHAN  TTY          TIME CMD
4 S     0   21361    8028  0  80   0 -  3427 -      pts/0    00:00:00 su
4 S  1001   21362   21361  0  80   0 -  2499 do_wai pts/0    00:00:00 bash
4 S     0   21548   21377  0  80   0 -  3512 -      pts/1    00:00:00 sudo
4 S     0   21549   21548  0  80   0 -  6276 -      pts/1    00:00:00 vim
0 R  1001   21672   21362  0  80   0 -  2873 -      pts/0    00:00:00 ps

# Pesquisando os PIDs de um processo:
linux:/tmp\$ ps -C chrome
    PID TTY          TIME CMD
  17419 ?        00:01:02 chrome
  17430 ?        00:00:00 chrome
  17431 ?        00:00:00 chrome
  17436 ?        00:00:00 chrome
  17454 ?        00:21:10 chrome
  17460 ?        00:00:06 chrome
  17495 ?        00:00:04 chrome
  17795 ?        00:00:09 chrome
  18084 ?        00:00:03 chrome
  18337 ?        00:00:00 chrome
  18365 ?        00:00:03 chrome
  18603 ?        00:28:37 chrome
  18623 ?        00:05:15 chrome
  18636 ?        00:30:35 chrome
  18661 ?        00:00:00 chrome

# Ótimo comando, cria uma saída personalizada:
ps -U USUARIO -o user,tt,state,pid,ppid,command,comm,rss,ni,pri,lstart

# Mesma coisa acima, mas para todos os usuários:
ps -Ao user,tt,state,pid,ppid,command,comm,rss,ni,pri,lstart
```



<span style="color:red">**Observação:**</span>

```
Quando você notar que o campo PRI (PRIORITY) possui um traço (-),
significa que o processo está rodando em tempo real.

O valor do PRI é estranho e não segue as normas de atribuição do valor,
o PRI do comando TOP, parece ser mais correto e segue as normas de atribuição.
```



**Entendendo as flags de State, Stat e S**:

```
Aqui estão os diferentes valores que os especificadores de saída s, stat e state 
exibirão para descrever o estado de um processo:

D = sono ininterrupto (geralmente IO)
I = thread ocioso do kernel
R = em execução ou executável (na fila de execução)
S = suspensão interrompível (aguardando a conclusão de um evento)
T = parado pelo sinal de controle do trabalho
t = parado pelo depurador durante o rastreamento
W = paginação (não é válida desde o kernel 2.6.xx)
X = morto (nunca deve ser visto)
Z = processo extinto ("zumbi"), finalizado mas não colhido pelo pai

Para formatos BSD e quando a palavra-chave stat é usada, caracteres adicionais podem ser exibidos:

<= alta prioridade (não é agradável para outros usuários)
N = baixa prioridade (agradável para outros usuários)
L = tem páginas bloqueadas na memória (para E / S personalizadas e em tempo real)
s = é um líder de sessão
l = é multiencadeado (usando CLONE_THREAD, como fazem os pthreads NPTL)
+ = está no grupo de processos em primeiro plano
```



## PSTREE

Exibe uma árvore dos processos.



**Opções**

```
-p = Mostra o PID.
```

**Exemplos:**

```bash
# Mostrando sem argumentos:
linux:/tmp\$ pstree
systemd─┬─ModemManager───2*[{ModemManager}]
        ├─NetworkManager───2*[{NetworkManager}]
        ├─accounts-daemon───2*[{accounts-daemon}]
        ├─acpid
        ├─3*[agetty]
        ├─avahi-daemon───avahi-daemon
        ├─blueberry-tray─┬─python3───rfkill
        │                └─4*[{blueberry-tray}]
        ├─bluetoothd
        ├─colord───2*[{colord}]
        ├─cron
        ├─csd-printer───2*[{csd-printer}]
        ├─cups-browsed───2*[{cups-browsed}]
        ├─cupsd
        ├─dbus-daemon
        ├─firefox─┬─Privileged Cont───25*[{Privileged Cont}]
        │         ├─RDD Process───3*[{RDD Process}]
        │         ├─Web Content───36*[{Web Content}]
        │         ├─Web Content───29*[{Web Content}]
        │         ├─Web Content───25*[{Web Content}]
        │         ├─Web Content───19*[{Web Content}]
        │         ├─WebExtensions───23*[{WebExtensions}]
        │         └─81*[{firefox}]

# Exibindo o PID:
linux:/tmp\$ pstree -p
systemd(1)─┬─ModemManager(1030)─┬─{ModemManager}(1053)
           │                    └─{ModemManager}(1055)
           ├─NetworkManager(820)─┬─{NetworkManager}(995)
           │                     └─{NetworkManager}(1000)
           ├─accounts-daemon(808)─┬─{accounts-daemon}(892)
           │                      └─{accounts-daemon}(998)
           ├─acpid(813)
           ├─agetty(1056)
           ├─agetty(18531)
           ├─agetty(18545)
           ├─avahi-daemon(815)───avahi-daemon(875)
           ├─blueberry-tray(1749)─┬─python3(1769)───rfkill(1772)
           │                      ├─{blueberry-tray}(1764)
           │                      ├─{blueberry-tray}(1765)
           │                      ├─{blueberry-tray}(1767)
           │                      └─{blueberry-tray}(1768)
           ├─bluetoothd(904)
           ├─colord(1591)─┬─{colord}(1592)
           │              └─{colord}(1594)
           ├─cron(817)
           ├─csd-printer(1597)─┬─{csd-printer}(1598)
           │                   └─{csd-printer}(1599)
           ├─cups-browsed(999)─┬─{cups-browsed}(1036)
           │                   └─{cups-browsed}(1037)
           ├─cupsd(818)
           ├─dbus-daemon(819)
           ├─firefox(2066)─┬─Privileged Cont(2241)─┬─{Privileged Cont}(2244)
           │               │                       ├─{Privileged Cont}(2247)
           │               │                       ├─{Privileged Cont}(2248)
           │               │                       ├─{Privileged Cont}(2252)
           │               │                       ├─{Privileged Cont}(2254)
           │               │                       ├─{Privileged Cont}(2255)
           │               │                       ├─{Privileged Cont}(2256)
           │               │                       ├─{Privileged Cont}(2257)
           │               │                       ├─{Privileged Cont}(2258)
           │               │                       ├─{Privileged Cont}(2259)
           │               │                       ├─{Privileged Cont}(2262)
           │               │                       ├─{Privileged Cont}(2263)
           │               │                       ├─{Privileged Cont}(2264)
           │               │                       ├─{Privileged Cont}(2269)
           │               │                       ├─{Privileged Cont}(2270)
           │               │                       ├─{Privileged Cont}(2285)
           │               │                       ├─{Privileged Cont}(2287)
           │               │                       ├─{Privileged Cont}(2295)
           │               │                       ├─{Privileged Cont}(2297)
           │               │                       ├─{Privileged Cont}(2299)
           │               │                       ├─{Privileged Cont}(2309)
           │               │                       ├─{Privileged Cont}(2310)
           │               │                       ├─{Privileged Cont}(2311)
           │               │                       ├─{Privileged Cont}(2327)
           │               │                       └─{Privileged Cont}(23960)
```



## PGREP

Exibe apenas o PPID do nome do processo.



**Opções:**

```
-u <usuário> = Filtra por usuário.
```



**Exemplos:**

```bash
# Exibindo o PPID do firefox:
linux:~/$ pgrep firefox
2066

# Exibindo o PPID do bash:
linux:~/$ pgrep bash
8028
21362
23261
23280

# Exibindo o PPID do bash mas para o usuário linux:
linux:~/$ pgrep -u linux bash
21362
```



## TOP

Exibe os processos em tempo real.



**Opções:**

```
-- Opções da linha de comando

-u = Faz buscas por um usuário
-b = Joga a informação na tela (Fica atualizando).
-d <N> = Número de segundos entre atualizações.
-n <N> = Número de interações até encerrar.

-- Opções do dashboard do TOP
k = Mata um processo.
u = Filtra por usuário.
L = Filtra por string.
```



**Exemplos:**

```bash
# Jogando a informação na tela, atualizando a cada 2 segundos e com apenas 2 interações,
# Apenas para o usuário linux:
linux:~/$ top -b -d 2 -n 2 -u linux
top - 16:24:19 up  6:37,  1 user,  load average: 0,98, 0,83, 0,85
Tarefas: 266 total,   1 em exec., 264 dormindo,   0 parado,   1 zumbi
%CPU(s):  8,8 us,  4,4 sis,  0,0 ni, 85,3 oc,  0,0 ag,  0,0 ih,  1,5 is  0,0 tr
MB mem :   7873,5 total,    486,4 livre,   3106,8 usados,   4280,3 buff/cache
MB swap:   2048,0 total,   2029,0 livre,     19,0 usados,   4096,0 mem dispon.

    PID USUARIO   PR  NI    VIRT    RES    SHR S  %CPU  %MEM    TEMPO+ COMANDO
  25512 linux     20   0   12092   3796   3188 R   6,2   0,0   0:00.02 top
  21362 linux     20   0   10128   4312   3560 S   0,0   0,1   0:00.27 bash
  25461 linux     20   0    9996   4164   3572 S   0,0   0,1   0:00.00 bash

top - 16:24:21 up  6:37,  1 user,  load average: 0,99, 0,83, 0,85
Tarefas: 266 total,   1 em exec., 264 dormindo,   0 parado,   1 zumbi
%CPU(s): 10,1 us,  5,1 sis,  0,0 ni, 84,3 oc,  0,1 ag,  0,0 ih,  0,4 is  0,0 tr
MB mem :   7873,5 total,    486,4 livre,   3106,8 usados,   4280,3 buff/cache
MB swap:   2048,0 total,   2029,0 livre,     19,0 usados,   4096,0 mem dispon.

    PID USUARIO   PR  NI    VIRT    RES    SHR S  %CPU  %MEM    TEMPO+ COMANDO
  25512 linux     20   0   12092   3796   3188 R   0,5   0,0   0:00.03 top
  21362 linux     20   0   10128   4312   3560 S   0,0   0,1   0:00.27 bash
  25461 linux     20   0    9996   4164   3572 S   0,0   0,1   0:00.00 bash
```



## KILL, KILLALL, PKILL

Comandos usados para matar processos.



### KILL

Mata processo por processo.



**Opções:**

```
-l = Lista todos os sinais do kill;
```



**Exemplos:**

```bash
# Criei vários processos no tmux (vários terminais):
linux:~/$ ps -u
USER         PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
linux      25720  0.0  0.0   9748  3628 pts/1    S+   16:39   0:00 tmux
linux      25723  0.0  0.0  10800  4912 pts/2    Ss+  16:39   0:00 -bash
linux      25730  0.0  0.0  10800  5000 pts/3    Ss+  16:39   0:00 -bash
linux      25737  0.0  0.0  10800  4900 pts/4    Ss+  16:39   0:00 -bash
linux      25746  0.0  0.0  10800  4932 pts/5    Ss+  16:39   0:00 -bash
linux      25867  0.2  0.0  10800  4900 pts/6    Ss+  16:44   0:00 -bash

# Vou matar uma sessão em específica do Tmux:
linux:~/$ kill -15 25746

# Listando de novo:
linux:~/$ ps -u
USER         PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
linux      25720  0.0  0.0   9748  3628 pts/1    S+   16:39   0:00 tmux
linux      25723  0.0  0.0  10800  4912 pts/2    Ss+  16:39   0:00 -bash
linux      25730  0.0  0.0  10800  5000 pts/3    Ss+  16:39   0:00 -bash
linux      25737  0.0  0.0  10800  4900 pts/4    Ss+  16:39   0:00 -bash
linux      25867  0.0  0.0  10800  4900 pts/6    Ss+  16:44   0:00 -bash

# Matando mais de 1 processo por vez:
linux:~/$ kill -9 25867 25737

# Matando o PPID (Processo pai do Tmux client):
linux:~/$ kill -15 25720
```



### KILLALL

Mata todos os processos a partir do nome do processo.

**Opções:**

```
-u <USER> = Mata todos os processos do usuário.
```



**Exemplos:**

```bash
# Matando o processo cliente do tmux,
# isso só vai "deslogar" o usuário:
linux:~/$ killall "tmux: client"

# Matando realmente o tmux (elimina todos os processos relacionados ao tmux):
linux:~/$ killall "tmux: server"

# Matando o processo top:
linux:~/$ killall -9 top

# Para ver o nome de um comando:
linux:~/$ ps -Ao user,pid,tt,comm | grep ^linux
linux      21362 pts/0    bash
linux      25461 pts/1    bash
linux      26184 ?        tmux: server
linux      26185 pts/2    bash
linux      26192 pts/3    bash
linux      26199 pts/4    bash
linux      26206 pts/5    bash
linux      26340 pts/1    tmux: client
linux      26341 pts/0    ps
linux      26342 pts/0    grep

## Matando um usuário.
# Exibindo usuários logados:
root:~\$ who
linux    tty1         2020-07-29 14:56
root     tty7         2020-07-28 09:49 (:0)

# Matando o usuário linux:
root:~\$ killall -u linux

# Exibindo os usuários logados:
root:~\$ who
root     tty7         2020-07-28 09:49 (:0)
```





