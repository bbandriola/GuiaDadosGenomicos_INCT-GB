# Controle de qualidade do sequenciamento

O primeiro passo após o recebimento dos dados de sequenciamento é avaliar a qualidade das sequências geradas.

## Avaliação de dados de sequenciamento Illumina 
Para avaliar a qualidade das sequências obtidas por sequenciamento Illumina, você pode utilizar o programa [FastQC](https://www.bioinformatics.babraham.ac.uk/projects/fastqc/). O FastQC processa dados de sequências em formato BAM, SAM ou FastQ. Neste tutorial, nosso objetivo é avaliar arquivos de Fastq para identificar qualquer problema antes de realizarmos o mapeamento das amostras.

A primeira etapa é abrir o terminal. Com o terminal aberto, podemos executar o programa. O FastQC é uma aplicação desenvolvida em Java e, portanto, requer um ambiente Java configurado para sua execução. Há diversas formas de configurar esse ambiente. Neste tutorial, utilizaremos o [Conda](https://docs.conda.io/en/latest/), um gerenciador de pacotes que permite instalar e gerenciar programas por meio da criação e ativação de ambientes específicos, configurados de acordo com as necessidades de cada análise.
  - Caso você ainda não tenha o **conda** instalado em seu sistema, converse com os mantenedores dos servidores para solicitar a implementação. 

Vamos criar um ambiente **conda** com o Java instalado para poder executar o FastQC: 
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

Ao executar essa linha de comando, o **conda** irá listar os pacotes e as dependências que serão instalados e solicitará uma confirmação para prosseguir com a instalação. Basta digitar ```y``` e precionar **Enter**. 

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

Após a criação e configuração do ambiente, podemos baixar o FastQC e executá-lo. Para isso, precisamos acessar o site de distribuição do programa: [FastQC](https://www.bioinformatics.babraham.ac.uk/projects/fastqc/). No site, clique em **Download Now**. Você será redirecionado para uma nova página, onde deverá localizar o arquivo **FastQC v0.12.1 (Win/Linux zip file)**. Clique com o botão direito do mouse sobre o arquivo e copie o endereço do link. Você deve copiar algo como: ```https://www.bioinformatics.babraham.ac.uk/projects/fastqc/fastqc_v0.12.1.zip```. Com o endereço copiado, fazeremos o download do programa para o servidor utilizando o comando ```wget```. Em seguida, descompactaremos o programa e ele estará pronto para uso. 

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

A execução do comando gera dois arquivos para cada arquivo de entrada, utilizando como base o prefixo do arquivo ```.fastq```.
* Arquivo ```seq1.html``` é um relatório contendo estatísticas básicas identificadas pelo programa, incluindo qualidade das bases e das sequências, distribuição e tamanho do conteúdo das sequências, informações sobre duplicatas e a presença de adaptadores.
  * [Exemplo de um sequenciamento excelente qualidade](https://www.bioinformatics.babraham.ac.uk/projects/fastqc/good_sequence_short_fastqc.html)
  * [Exemplo de um sequenciamento com problemas de qualidade](https://www.bioinformatics.babraham.ac.uk/projects/fastqc/bad_sequence_fastqc.html)
* Arquivo compactado ```seq1_fastqc.zip``` contém as mesmas informações do relatório em HTML dispostas de forma independente, em formato de png e svg, além de arquivos de texto contendo os dados utilizados para plotar os gráficos. 

Após essa etapa de checagem, você poderá tomar decisões importantes sobre os próximos passos do mapeamento, como a necessidade de realizar filtragem ou remoção de contaminantes, ajustar o tamanho das sequências utilizadas nas análises, filtros para reduzir o excesso de duplicatas ou avaliar a super-representação de determinadas sequências.

Concluída essa etapa, podemos seguir para o [Mapeamento](./Mapeamento) 


