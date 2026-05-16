Essa seção está dividida em duas partes: 
1. [Introdução a Linux](./Linux_Básico.md)
2. [Introdução a scripts/comandos em Bash](./Introdução_ao_Bash.md)

Sinta-se a vontade para acessar na ordem desejada e de acordo com o tópico de interesse. 

# Introdução a Linux :desktop_computer:

Linux é um sistema operacional (SO), disponível em distribuições como o Ubuntu, amplamente utilizado na bioinformática. Esse sistema é gratuito e de código aberto, favorecendo a reprodutibilidade e a eficiência no processamento de grandes volumes de dados, como é o caso de [análises de dados genômicos](https://www.notion.so/3626457f0f6d806a85b5ca14bc6f123e?v=3626457f0f6d8012aec0000cfedda990&source=copy_link).

Na bioinformática, muitas tarefas são realizadas por meio de um terminal (basta digitar "terminal" no macOS ou "cmd" no Windows para abrir), já que diversas ferramentas não possuem interface gráfica. Através do terminal, é possível acessar programas essenciais para o processamento de dados genômicos e automatizar tarefas repetitivas, como a execução de *loops* e *pipelines*. Por esse motivo, o conhecimento de **comandos** do Linux, **expressões regulares (regex)** e **manipuladores de texto** é fundamental para atividades rotineiras, como navegar entre diretórios, organizar arquivos e processar dados.

Para começar a entender o sistema do Linux, precisamos entender que a organização de diretórios é defida de forma hierárquica, na qual, as pastas são delimitadas por um separador ```/```. Se quisermos saber em qual diretório estamos, podemos utilizar o comando ```pdw``` no nosso terminal.

O retorno desse comando será algo como: /GuiaDadosGenomicos_INCT-GB/Linux/Linux_Básico.md. Ou seja, o arquivo <ins>Linux_Básico.md</ins> está dentro do diretóro <ins>Linux</ins>, dentro do diretório <ins>GuiaDadosGenomicos_INCT-GB</ins>. Mas onde está o diretório **GuiaDadosGenomicos_INCT-GB**? Ele pode estar em qualquer lugar do computador, mas geralmente estará no nosso usuário. A pasta de cada usuário pode ser acessada com o comando: ```$ cd ~```.

Além dessa organização básica, de cada usuário, o computador possui uma organização própria. Geralmente, essa organização se localiza na _root_ (raiz) do computador. Para acessar esse local podemos utilizar o comando: ```$ cd /```. 

O ```/``` nesse caso, redireciona para a raiz do computador. Lá são encontrados documentos básicos de funcionamento do servidor e aplicações compartilhadas entre todos os usuários da máquina. Uma dessas pastas é:

- &nbsp; /bin: local onde estão instalado os comandos comportilhados por todos os usuários.

Para navegarmos pelos diretórios sem precisar escrever o caminho completo, utilizamos _caminhos relativos_, como a utilização do ponto ```.```. 

```.``` representa o diretório atual. 
```..``` representa o diretório anterior.

:books: Exercício: Imagine que queremos descobrir em qual pasta do computador estamos. Qual o comando podemos utilizar para realizar essa tarefa?
<p>
    <input type="checkbox" id="q1">
    <label for="q1"><b> ```cd```
    <label for="q1"><b> ```pwd```
</p>
<details>
  
  <summary> Clique aqui para ver a resposta</summary>
  
```
$ pwd
/GuiaDadosGenomicos_INCT-GB/Linux/PraticaLinux
```

</details>

:books: Exercício: Agora que sabemos que estamos nao diretório <ins>Linux</ins>, nosso objetivo é chegar no diretório anterior, <ins>GuiaDadosGenomicos_INCT-GB</ins>. Para isso, precisamos utilizar uma combinação do que já vimos até o momento. Qual é a opção correta? 

- [ ] ```cd ../../```
- [ ] ```cd ../```
- [ ] ```pwd ../```

<details>
  
  <summary> Clique aqui para ver a resposta</summary>
  
```
$ cd ../
$ pwd 
/GuiaDadosGenomicos_INCT-GB/Linux
```

</details>

Um último truque que podemos utilizar é a tecla *Tab*. A tecla *Tab* pode ser utilizada: 
- &nbsp; 1x: Autocompletar um caminho ou nome de arquivo.
- &nbsp; 2x: exibir todas as opções de autocompletar.  

Abaixo estão apresentados alguns comandos básicos utilizados com frequência no dia a dia de quem trabalha com Linux. 

### Comandos básicos 
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
| `gzip/zip [opções] [arquivo]` | Compactar arquivos com final `.gz/.zip` |
| `gunzip/unzip [opções] [arquivo]` | Descompactar arquivos |
| `cat [opções] [arquivo]` | Exibir conteúdo de um arquivo |
| `grep [opções] [expressão a ser procurada] [arquivo de entrada]` | Procurar por padrões em um arquivo |
| `sed [opções] 's/padrão/novo padrão/' [arquivo de entrada] > [arquivo de saída]` | Substituir ou modificar padrões de texto |
| `awk [opções] 'padrão {ação a ser executada}' [arquivo]` | Processar e analisar textos |
| `head [arquivo]` | Mostrar as 10 primeiras linhas de um arquivo |
| `tail [arquivo]` | Mostrar as 10 últimas linhas de um arquivo |
| `sort [opções] [arquivo]` | Ordenar conteúdo de um arquivo |
| `[comando] --help` | Explicação dos comandos |
| `echo [opções] "texto a ser imprimido"` | Imprimir conteúdo na tela |
| `ln -s /diretório/do/arquivo /destino/do/link` | Criar link simbólico |
| `wc [opções]` | Quantificador do conteúdo |
| `diff [opções]` | Checar a diferença entre dois arquivos |

Quer saber mais sobre cada um desses programas? Você pode utilizar o comando ```man [comando]``` e será exibida todas as "flags" possíveis com aquela função.

Além dos comandos, também vamos abordar os caracteres curingas (no inglês eles são chamados de *wildcard*).

```?``` : Representa qualquer caractere individual. Ou seja, qualquer letra de a-z ou número de 0-9.

```*``` : Representa múltiplos caracteres, incluindo espaço.


Na programação encontramos o que chamamos de Expressões Regulares (*Regex*). Essas servem para especificar e manipular padrões ou definir ações:
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

Quando utilizando regex para manipular ou especificar padrões, utilizados a expressão entre aspas simples. 
Por exemplo, o arquivo abaixo é o **diálogo.txt** e contem com o seguinte conteúdo:
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

Nosso objetivo é achar todas as linhas que começem com a palavra **Bom** dentro do arquivo. Para isso, quais comandos podemos utilizar?

Opção 1: ```$ grep '^Bom' diálogo.txt```

Opção 2: ```$ cat '^Bom' diálogo.txt```

Opção 3: ```$ awk '^Bom' diálogo.txt```

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


Além dos comandos, básicos e dos regex, também há comandos de **redirecionamento e condutores**. Os principais são: 

```>``` : Criar ou sobrescrever um arquivo.

```>>``` : Adiciona a saída ao final de um arquivo existente.

```|``` ou pipe : quando queremos utilizar mais de um comando em uma mesma linha. Utilizado para redirecionar uma saída para outra entrada.

Podemos juntar esses redirecionadores com números para especificar saídas especificas.

```1 > saída.log``` : Redirecionando a saída do programa para um arquivo log. 

```2 > erro.log``` : Redireciona todos os erros de um determinado comando para um arquivo log.

```> saídaeerro.log 2>&1``` : Redirecionar a saída padrão e os erros para o mesmo arquivo.

```> saída.log 2> erro.log``` : Redirecionar a saída padrão para um arquivo e os erros para outro.

Agora que já aprendemos boa parte da syntax, podemos aprofundar nosso conhecimento e entrar na construção de [_scripts_ ou linhas de comando em bash](./Introdução_ao_Bash.md).
 
Esse tutorial foi construido a partir das seguintes referências: 
> https://sites.google.com/view/intro-to-unix-2026/ &nbsp; https://www.ibm.com/br-pt/think/topics/linux &nbsp; https://reqchecker.eu/manual/extract_syntax.html


