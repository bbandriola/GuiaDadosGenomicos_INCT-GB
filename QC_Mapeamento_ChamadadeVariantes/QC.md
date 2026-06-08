## Controle de qualidade do sequenciamento

O primeiro passo após o recebimento dos dados do sequenciamento é analisar a qualidade das sequências geradas. 

## Avaliação de dados de sequenciamento Illumina 
Para avaliar a qualidade das sequências provenientes do sequenciamento Illumina, você pode utilizar o programa [FastQC](https://www.bioinformatics.babraham.ac.uk/projects/fastqc/). 

A primeira coisa a se fazer é abrir o nosso terminal. Com o terminal aberto, queremos executar o programa. O FastQC é um aplicação em java, ou seja, requer um ambiente de java para ser executado. Há diversas formas se configurar esse sistema. Aqui, utilizaremos o [Conda](https://docs.conda.io/en/latest/), um gerenciador de pacotes que funciona através de ativação de ambientes especifícos de acordo com as configurações iniciais. 
  - Caso você ainda não tenha o Conda instalado em seu sistema, converse com os mantenedores dos servidores para a implementação. 

Vamos criar um ambiente Conda com o Java instalado para poder executar o FastQC: 
```linux
conda create -n java conda-forge::openjdk
```
<details>  
  <summary> Explicação comando</summary>
  
  - ```conda``` : para chamar o programa.
  
  - ```create``` : para criar novos ambientes.
  
  - ```-n``` : parâmetro que define o nome do ambiente.
  
  - ```conda-forge``` : canal de disponibilidade do pacote do Java.
  
  - ```::openjdk``` : especifica o programa e versão a ser baixado. Nesse caso, a versão é a mais nova.
</details>

Ao executar a linha de comando, o Conda irá listar os pacotes e dependências que serão instaladas e pedirá uma confirmação para prosseguir com a instalação. Apenas digite ```y``` e precione enter. 

Após a instalação, precisamos ativar o ambiente. Para isso utilizaremos o comando: 

``` linux
conda activate java
```
<details>  
  <summary> Explicação comando</summary>
  
  - ```conda``` : para chamar o programa.
  
  - ```activate``` : para ativar um ambiente.
  
  - ```java``` : nome do ambiente que deseja ativar. Aqui, queremos ativar o ambiente do Java.

</details>

Após a criação e ajuste do ambiente, podemos baixar o FastQC e executa-lo. Para isso precisamos visitar o website de distribuição do programa: [FastQC](https://www.bioinformatics.babraham.ac.uk/projects/fastqc/). No site, clique **Download Now**. Você será redirecionado para uma nova aba onde deve localizar o arquivo **FastQC v0.12.1 (Win/Linux zip file)**. Clique com o botão direito do mouse em cima desse arquivo e copie o caminho do arquivo. Você deve copiar algo como: ```https://www.bioinformatics.babraham.ac.uk/projects/fastqc/fastqc_v0.12.1.zip```. Uma vez que copiamos esse arquivo, vamos fazer o download para dentro do nosso servidor utilizando o comando ```wget```, descompactar o programa e ele estará pronto para ser utilizado. 

```linux
# download do pacote
wget https://www.bioinformatics.babraham.ac.uk/projects/fastqc/fastqc_v0.12.1.zip
# descompactar
unzip fastqc_v0.12.1.zip
# acesse a pastq que foi gerada
cd FastQC/
# execute o FastQC e exiba os parâmetros que podem ser utilizados
./fastqc -h
```

Agora que temos tudo pronto, podemos executar o programa para avaliar a qualidade das nossas sequências. No terminal, navegue até o diretório onde suas sequências brutas estão depositadas. 

```linux
cd [caminho/para/o/diretório/das/suas/sequências/brutas]
```

Uma vez no diretório, execute o comando do FastQC que gerará um arquivo HTML para cada uma das suas sequências. 

```bash
./fastqc -o outfastqc seq1.fastq seq2.fastq seq3.fastq ... seqN.fastq
```
<details>  
  <summary> Explicação comando</summary>
  
  - ```./fastqc``` : para chamar o programa.
  
  - ```-o outfastqc``` : definição do diretório de saída. O padrão é o diretório atual ```.```.
  
  - ```seqN.fastq``` : arquivos fastq que você deseja analisar.

</details>

O resultado do comando é a produção de dois arquivos para cada arquivo de entrada que retoma o prefixo dos arquivos ```.fastq```.
* arquivo ```seq1.html``` relatório contendo estatísticas básicas identificadas pelo programa, qualidade das bases e das sequências, distribuição e tamanho do conteúdo das sequências, informações de duplicatas e adaptadores.
  * [Exemplo de um sequenciamento excelente](https://www.bioinformatics.babraham.ac.uk/projects/fastqc/good_sequence_short_fastqc.html)
  * [Exemplo de um sequenciamento com problema](https://www.bioinformatics.babraham.ac.uk/projects/fastqc/bad_sequence_fastqc.html)
* arquivo compactado ```seq1_fastqc.zip``` com os mesmos gráficos do HTML, mas disposto de forma independente e em formato de png e svg, além de arquivos de texto com o dado utilizado para plotar os gráficos. 

Após essa checagem você pode decidir questões importantes sobre os próximos passos do mapeamento, como o tamanho das suas sequências (caso, haja alguma contaminação ou a necessidade de utilizar tamanhos menores do que os estabelecidos previamente), excesso de duplicatas ou super representação de algumas sequências. Feito esse passo, podemos seguir para o [Mapeamento](./Mapeamento). 


