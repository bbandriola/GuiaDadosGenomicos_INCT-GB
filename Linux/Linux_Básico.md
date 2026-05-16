Essa seção está dividida em duas partes: 
1. [Introdução a Linux](./Linux_Básico.md)
2. [Introdução a scripts/comandos em Bash](./Introdução_ao_Bash.md)

Sinta-se a vontade para acessar na ordem desejada e de acordo com o tópico de interesse. 

# Introdução a Linux :desktop_computer:

Linux é um sistema operacional (SO), disponível em distribuições como o Ubuntu, amplamente utilizado na bioinformática. Esse sistema é gratuito e de código aberto, favorecendo a reprodutibilidade e a eficiência no processamento de grandes volumes de dados, como é o caso de [análises de dados genômicos](https://www.notion.so/3626457f0f6d806a85b5ca14bc6f123e?v=3626457f0f6d8012aec0000cfedda990&source=copy_link).

Na bioinformática, muitas tarefas são realizadas por meio de um terminal (basta digitar "terminal" no macOS ou "cmd" no Windows para abrir), já que diversas ferramentas não possuem interface gráfica. Através do terminal, é possível acessar programas essenciais para o processamento de dados genômicos e automatizar tarefas repetitivas, como a execução de *loops* e *pipelines*. Por esse motivo, o conhecimento de **comandos** do Linux, **expressões regulares (regex)** e **manipuladores de texto** é fundamental para atividades rotineiras, como navegar entre diretórios, organizar arquivos e processar dados.

Para começar a entender o sistema do Linux, precisamos entender que a organização de diretórios é defida de forma hierárquica :card_index_dividers:, na qual, as pastas são delimitadas por um separador ```/```. Se quisermos saber em qual diretório estamos, podemos utilizar o comando ```pdw``` no nosso terminal.

O retorno desse comando será algo como: ```/GuiaDadosGenomicos_INCT-GB/Linux/Linux_Básico.md```. Ou seja, o arquivo <ins>Linux_Básico.md</ins> está dentro do diretóro <ins>Linux</ins>, dentro do diretório <ins>GuiaDadosGenomicos_INCT-GB</ins>. Mas onde está o diretório <ins>GuiaDadosGenomicos_INCT-GB</ins>? Ele pode estar em qualquer lugar do computador, mas geralmente estará no diretório padrão do seu usuário. A pasta de cada usuário pode ser acessada com o comando: ```$ cd ~```.

Além dessa organização de cada usuário, o computador possui uma organização própria localizada na _root_ (raiz) do computador. Para acessar esse local podemos utilizar o comando: ```$ cd /```. 

O ```/``` nesse caso, redireciona para a raiz do computador. Lá são encontrados documentos de funcionamento do servidor e aplicações compartilhadas entre todos os usuários da máquina. Uma dessas pastas é:

:file_folder: ```/bin```: local onde estão instalado os comandos compartilhados por todos os usuários.

Para navegarmos pelos diretórios sem precisar escrever o caminho completo, utilizamos **caminhos relativos**, como a utilização do ponto ```.```. O ```.``` pode significar dois estados:

```.``` representa o diretório atual :round_pushpin:

```..``` representa o diretório anterior :back:

### :elf_woman: Seção de prática 

:books: Exercício: Imagine que queremos descobrir em qual diretório do computador estamos. Qual o comando podemos utilizar para realizar essa tarefa?

- ```cd```
- ```pwd```

<details>  
  <summary> Clique aqui para ver a resposta</summary>
  
```
$ pwd
/GuiaDadosGenomicos_INCT-GB/Linux/PraticaLinux
```

</details>

:books: Exercício: Agora que sabemos que estamos no diretório <ins>Linux</ins>, nosso objetivo é chegar no diretório anterior, <ins>GuiaDadosGenomicos_INCT-GB</ins>. Para isso, precisamos utilizar uma combinação do que já vimos até o momento. Qual é a opção correta? 

- ```cd ../../```
- ```cd ../```
- ```pwd ../```

<details>
  
  <summary> Clique aqui para ver a resposta</summary>
  
```
$ cd ../
$ pwd 
/GuiaDadosGenomicos_INCT-GB/Linux
```

</details>

Um dos truques que podemos fazer é utilizar a tecla *Tab* para autocompletar nome de arquivos ou exibir as opções: 
- 1x: Autocompletar um caminho ou nome de arquivo.
- 2x: exibir todas as opções de autocompletar.

## Comandos do Linux

O Linux trabalha com uma gama de comandos que podem ser executados através do terminal. Abaixo estão citados alguns comandos utilizados com frequência no dia a dia de quem trabalha com o sistema :arrow_heading_down: 

| **Comando** | **Função** |
|---|---|
| `ls [opções] [caminho ou diretório]` | Listar conteúdo |
| `pwd [opções]` | Mostrar diretório atual |
| `cd [caminho ou diretório]` | Acessar um diretório |
| `mkdir [opções] [nome do diretório]` | Criar um diretório |
| `rm [opções] [arquivo]` | Remover um arquivo |
| `rm [opções] [pasta]` | Remover uma pasta |
| `cp [opções] [arquivo a ser copiado] [destino do arquivo copiado + nome]` | Copiar arquivos e diretórios |
| `mv [arquivo a ser movido] [destino do arquivo]` | Mover arquivos de local |
| `nano/vim/emacs [nome do arquivo]` | Criar/editar arquivos |
| `gzip/zip [opções] [arquivo]` | Compactar arquivos `.gz/.zip` |
| `gunzip/unzip [opções] [arquivo]` | Descompactar arquivos |
| `cat [opções] [arquivo]` | Exibir conteúdo de um arquivo |
| `grep [opções] [expressão a ser procurada] [arquivo de entrada]` | Procurar por padrões em um arquivo |
| `sed [opções] 's/padrão/novo padrão/' [arquivo de entrada] > [arquivo de saída]` | Substituir ou modificar padrões de texto |
| `awk [opções] 'padrão {ação a ser executada}' [arquivo]` | Processar e analisar textos |
| `head [arquivo]` | Mostrar as 10 primeiras linhas de um arquivo |
| `tail [arquivo]` | Mostrar as 10 últimas linhas de um arquivo |
| `sort [opções] [arquivo]` | Ordenar conteúdo de um arquivo |
| `[comando] --help` | Explicação das funções de um comando |
| `echo [opções] "texto a ser imprimido"` | Imprimir conteúdo na tela |
| `ln -s /diretório/do/arquivo /destino/do/link` | Criar link simbólico |
| `wc [opções]` | Quantificador do conteúdo |
| `diff [opções]` | Checar a diferença entre dois arquivos |

:thought_balloon: Quer saber mais sobre cada um desses comandos? Você pode utilizar o comando ```man [comando]``` e será exibido todas as "flags" possíveis de serem utilizadas com a função.

## Expressões Regulares (*Regex*)

As expressões regulares servem para especificar e manipular padrões ou definir ações. A tabela abaixo contém algumas dessas expressões :arrow_heading_down:

| Expressões Regulares | Descrição | 
| --- | --- |
| `^palavra` | Corresponde ao início da linha |
| `palavra$` | Corresponde ao final da linha |
| `[]` | Corresponde a qualquer uma das caracteres dentro dos []  |
| `[^]` | Corresponde a qualquer carcatere, exceto as de dentro dos []  |
| `\\` | Corresponder ao ```\``` |
| `X|Z` | Corresponde a **X** ou **Z** |
| `+` | Quantificador de uma ou mais ocorrências |
| `*` | Quantificador de zero ou mais ocorrências |
| `{n,m}` | Quantificador, corresponde a, no mínimo **n** ocorrências e no máximo m ocorrências da expressão que anterior|
| `\w` | Expressão que irá corresponder aos caractere de palavras |
| `\d` | Expressão que irá corresponder a digitos (0-9) |
| `()` | Agrupamento |

:warning: Quando utilizando uma regex para manipular ou especificar padrões, utilizados a expressão entre aspas simples.

### :elf_woman: Seção de prática

:books: Exercício: O arquivo  **diálogo.txt** e contém com o seguinte conteúdo:
```
Bom dia, pessoal! Como vocês estão hoje?
Bom te ver por aqui, Lucas!
Eu trouxe os documentos que vocês pediram.
A reunião vai começar em cinco minutos.
Bom lembrar que amanhã é o prazo final da inscrição.
Eu quase tinha esquecido disso.
Bom trabalho no relatório, ficou excelente.
Que alivio ouvir isso depois de tantas revisões!
Bom, então podemos encerrar por hoje?
Até amanhã!
```
Nosso objetivo é achar todas as linhas que começem com a palavra **Bom** dentro do arquivo. Para isso, qual comando podemos utilizar?

-  ```$ grep '^Bom' diálogo.txt```

-  ```$ cat '^Bom' diálogo.txt```

-  ```$ awk '^Bom' diálogo.txt```

<details>
  
  <summary> Clique aqui para ver a resposta</summary>
  
```Linux
$ grep '^Bom' diálogo.txt
Bom dia, pessoal! Como vocês estão hoje?
Bom te ver por aqui, Lucas!
Bom lembrar que amanhã é o prazo final da inscrição.
Bom trabalho no relatório, ficou excelente.
Bom, então podemos encerrar por hoje?
```

</details>

No Linux também trabalhamos com **Caracteres curingas (*wildcard*)**:

```?``` : Representa qualquer caractere individual. Ou seja, qualquer letra de a-z ou número de 0-9.

```*``` : Representa múltiplos caracteres, incluindo espaço.

## Redirecionamento e condutores

Os principais são: 

```>``` : Criar ou sobrescrever um arquivo.

```>>``` : Adiciona a saída ao final de um arquivo existente.

```|``` ou pipe : quando queremos utilizar mais de um comando em uma mesma linha. Utilizado para redirecionar uma saída para outra entrada.

Podemos juntar esses redirecionadores com números para especificar saídas especificas.

```1 > saída.log``` : Redirecionando a saída do programa para um arquivo log. 

```2 > erro.log``` : Redireciona todos os erros de um determinado comando para um arquivo log.

```> saídaeerro.log 2>&1``` : Redirecionar a saída padrão e os erros para o mesmo arquivo.

```> saída.log 2> erro.log``` : Redirecionar a saída padrão para um arquivo e os erros para outro.

Agora que já aprendemos boa parte da syntax, podemos aprofundar nosso conhecimento e entrar na construção de [_scripts_ ou linhas de comando em bash](./Introdução_ao_Bash.md).
 
Referências: 
<details>
  > https://sites.google.com/view/intro-to-unix-2026/ &nbsp; https://www.ibm.com/br-pt/think/topics/linux &nbsp; https://reqchecker.eu/manual/extract_syntax.html
</details>

