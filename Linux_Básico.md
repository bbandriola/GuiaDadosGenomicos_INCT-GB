# Comandos básicos de Linux

O principal sistema operacional (SO) utilizado na bioinformática é o Linux. Esse SO é o mais escolhido, pois o código aberto e gratuito, tornando o processo de processamento de grandes volumes de dados reprodutível e eficiente. Na bioinformática utilizamos o SO Linux devido a rapídez do sistema e sua fácil utilização. Além disso, a maioria das tarefas em bioinformárica não possuem uma interface gráfica e são realizadas a partir de uma *Shell* ou Terminal, o qual possibilita acesso a ferramentas indispensáveis para o processamento dos dados genômicos e a realização de tarefas repetitivas (como os *loops*). Por esse motivo, saber comandos básicos, expressões regulares (**regex**) e manipuladores, é importante para conseguir realizar tarefas rotineiras, como acessar pastas e manipular arquivos.

A organização de diretórios em Linux é defida de forma hierárquica, na qual, as pastas são delimitadas por um separador /. Se quisermos saber onde estamos, podemos utilizar o comando ```pdw``` no nosso terminal de Linux.

O retorno desse comando será algo como: /GuiaDadosGenomicos_INCT-GB/Linux/PraticaLinux. Ou seja, o diretório <ins>PraticaLinux</ins> está dentro do diretóro <ins>Linux</ins>, dentro do diretório <ins>GuiaDadosGenomicos_INCT-GB</ins>. Mas onde está o diretório **GuiaDadosGenomicos_INCT-GB**? Ele pode estar em qualquer lugar do computador, mas geralmente estará no nosso usuário. A pasta de cada usuário pode ser acessada com o comando: ```$ cd ~```

Além dessa organização básica a partir de cada usuário do computador, ainda há uma organização da própria máquina. Geralmente, essa organização se localiza na _root_ (raiz) do computador. Para acessar esse local podemos utilizar o comando: ```$ cd /```.

O ```/``` nesse caso, redireciona para a raiz do computador. Lá são encontrados documentos básicos de funcionamento do servidor e aplicações compartilhadas entre todos os usuários da máquina. Uma dessas pastas é:

- &nbsp; /bin: local onde estão instalado os comandos que podem ser utilizados por todos os usuários.

Para navegarmos pelos diretórios sem precisar escrever o caminho completo, utilizamos _caminhos relativos_, como a utilização do ponto ```.```. 
```.``` representa o diretório atual. 
```..``` representa o diretório anterior.

Imaginamos que queremos desconbrir em qual pasta do computador estamos. Qual o comando podemos utilizar para descobrir isso?
Opção 1: ```cd```
Opção 2: ```pwd```

<details>
  <summary> Click to check the answer</summary>
  
```
$ pwd
/GuiaDadosGenomicos_INCT-GB/Linux/PraticaLinux
```

</details>

Agora que sabemos que estamos na página <ins>PraticaLinux</ins>, nosso objetivo é chegar no diretório anterior, chamado de <ins>Linux</ins>. Para isso, precisamos utilizar uma combinação do que já vimos até o momento. Qual é a opção correta? 
Opção 1: ```cd ../../```
Opção 2: ```cd ../```
Opção 3: ```pwd ../```

<details>
  <summary> Click to check the answer</summary>
  
```
$ cd ../
$ pwd 
/GuiaDadosGenomicos_INCT-GB/Linux
```

</details>

Abaixo estão apresentados alguns comandos básicos utilizados com frequência no dia a dia de quem trabalha com Linux. 

### Comandos básicos 
| Função| Comando | 
| --- | --- |
| Listar conteúdo | ls [opções] [caminho ou diretório] |
| Mostrar diretório atual | pwd [opções] |
| Acessar um diretório | cd [caminho ou diretório] |
| Criar um diretório | mkdir [opções] [nome do diretório] |
| Remover um arquivo | rm [opções] [arquivo] |
| Remover uma pasta | rm [opções] [pasta] |
| Copiar arquivos e diretórios | cp [opções] [arquivo a ser copiado] [destino do arquivo copiado + nome] |
| Mover arquivos de local | mv [arquivo a ser movido] [destino do arquivo] |
| Criar/editar arquivos | nano [nome do arquivo] |
| Compactar arquivos com final .gz/zip | gzip/zip [opções] [arquivo] |
| Descompactar arquivos | gunzip/unzip [opções] [arquivo] |
| Exibidir conteúdo de um arquivo | cat [opções] [arquivo] |
| Procurar por padrões em um arquivo | grep [opções] [expressão a ser procurada] [arquivo de entrada] |
| Substituir ou modificar padrões de texto | sed [opções] 's/padrão/novo padrão/' [arquivo de entrada] > [arquivo de saída]* |
| Processar e analisar textos | awk [opções] 'padrão {ação a ser executada}' [arquivo] |
| Mostrar as 10 primeiras linhas de um arquivo | head [arquivo] |
| Mostrar as 10 últimas linhas de um arquivo | tail [arquivo] |
| Ordenar conteúdo de uma arquivo | sort [opções] [arquivo] |
| Explicação dos comandos | [comando] --help |
| Imprimir conteúdo na tela | echo [opções] "texto a ser imprimido" |

Além dos comandos, o Linux também apresenta Expressões Regulares (regex) que servem para especificar e manipular padrões ou definir ações:
| Expressões Regulares | Descrição | 
| --- | --- |
| ^ | Início da linha |
| $ | Final da linha |
|  |  |
|  |  |
|  |  |
|  |  |
|  |  |

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

Nosso objetivo é achar todas as linhas que começem com a palavra **Bom** dentro do arquivo. Para isso, podemos utilizar o comando grep:
```Linux
grep '^Bom' diálogo.txt
```
!(file:///Users/brunandriola/Desktop/Screen%20Shot%202026-05-13%20at%2010.33.41.png)

Além dos comandos, básicos e dos regex, também há comandos de **redirecionamento e condutores**. Os principais são: 
```>``` : Criar ou sobrescrever um arquivo.
```>>``` : Adiciona a saída ao final de um arquivo existente.
```|``` ou pipe : redirecionar uma saída para otra entrada.

Podemos juntar esses redirecionadores com números para especificar saídas especificas.
```1 > saída.log``` : Redirecionando a saída do programa para um arquivo log. 
```2 > erro.log``` : Redireciona todos os erros de um determinado comando para um arquivo log.
```> saídaeerro.log 2>&1``` : Redirecionar a saída padrão e os erros para o mesmo arquivo.
```> saída.log 2> erro.log``` : Redirecionar a saída padrão para um arquivo e os erros para outro.

Para finalizer, também vamos abordar os caracteres curingas (no inglês eles são chamados de *wildcard*).
```?``` : Representa qualquer caractere individual. Ou seja, qualquer letra de a-z e número de 0-9. Pode ser utilizado para localizar sequências onde se sabe que há uma variante em um região específica. Por exemplo:
```
Sequência: ACGTACAGTAC?
```
```*``` : Representa qualquer número de caracteres.
 
Esse tutorial foi construido a partir das seguintes referências: 


