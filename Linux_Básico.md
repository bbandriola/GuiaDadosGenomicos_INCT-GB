# Comandos básicos de Linux

O principal sistema operacional (SO) utilizado na bioinformática é o Linux. Esse SO é de código aberto e gratuito. A maioria das tarefas em bioinformárica a serem utilizadas não possuem uma interface gráfica e são realizadas a partir de uma *Shell* ou Terminal. 

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
