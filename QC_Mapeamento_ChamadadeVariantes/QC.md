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
  
  - ```conda``` : para chamar o prograna.
  
  - ```create``` : para criar novos ambientes.
  
  - ```-n``` : parâmetro que define o nome do ambiente.
  
  - ```conda-forge``` : canal de disponibilidade do pacote do Java.
  
  - ```::openjdk``` : especifica o programa e versão a ser baixado. Nesse caso, a versão é a mais nova.

</details>

Ao executar a linha de comando, o Conda irá listar os pacotes e dependências que serão instaladas e pedirá uma confirmação, se você estiver de acordo. Apenas digite ```y``` e precione enter, caso queira seguir com a instalação, Nesse caso, nós queremos. Após a instalação, precisamos ativar o ambiente. Para isso utilizaremos o comando: 
``` linux
conda activate java
```

<details>  
  <summary> Explicação comando</summary>
  
  - ```conda``` : para chamar o prograna.
  
  - ```activate``` : para ativar um ambiente.
  
  - ```java``` : nome do ambiente que deseja ativar. Aqui, queremos ativar o ambiente do Java.

</details>

Após a criação e ajuste do ambiente, podemos baixar o FastQC e executa-lo. Para isso precisamos visitar o website de distribuição do programa: [FastQC](https://www.bioinformatics.babraham.ac.uk/projects/fastqc/). 

Abra o terminal e navegue até o diretório onde suas sequências brutas estão depositadas. 
```linux
cd [caminho/para/o/diretório/das/suas/sequências/brutas]
```
Uma vez nesse diretório, execute o comando do FastQC que gerará um arquivo HTML para cada uma das suas sequências. 
```bash
fastqc 

