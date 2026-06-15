# Submissão de dados brutos para o NCBI

Uma das formas de armazenar e disponibilizar os dados gerados em projetos de sequenciamento genômico é depositando os arquivos brutos em uma repositório online. Um dos repositórios mundialmente utilizados é o SRA - Sequence Read Archive, do NCBI. O SRA dispõe de um espaço digital para o deposito de sequências biológicas, por exemplo sequências de DNA, sequências de proteínas, arquivos de metagenômica e dados de sequênciamento de DNA ambiental.

Uma vez que seu artigo for aceito para a publicação, os dados brutos utilizados nas análises devem ser obrigatoriamente disponibilizados. Uma forma de disponibilizar esses arquivos é armazena-los no SRA. 

Neste tutorial vamos entender como é feito esse processo e os arquivos necessários para submeter uma sequência para o repositório do SRA.

A documentação base para realizar a submissão foi retirada do [site do NCBI](https://www.ncbi.nlm.nih.gov/sra/docs/submit/). 

:lizard: **Quais dados podem ser submetidos?**

FASTQ, BAM, CRAM, SFF, HDF5 (arquivos de sequenciamento PacBio e MinIon Oxford Nanopore) e FASTAs com arquivos de qualidade associados. Os arquivos submetidos precisam conter informações sobre a qualidade das bases. [+ informações](https://www.ncbi.nlm.nih.gov/sra/docs/submitformats/).

Os arquivos não dezem estar comprimidos no formato de *zip*, apenas em *gzip* ou *bzip2*. Além disso, os dados comprimidos não devem passar de 100GB. Caso o estudo tenha mais que 5TB de dados brutos, os dados devem ser submetidos em múltiplcas rodadas de submissão.  


:lizard: **O que é necessário para submeter uma sequência?**

Três informações: 

- BioSample: o nome da espécie das sequência que serão depositadas. 
- SRA: contendo os metadados da amostras (plataforma de sequenciamento e biblioteca) e as sequências brutas.
- BioProject: deve conter o objetivo do estudo, o escopo, os participantes e os financiamentos. Caso hoje possua mais de um BioSample e SRA, o mesmo BioProject deve estar associado.

:lizard: **Pode onde é feita a submissão?**

Pelo [SRA Submission Portal Wizard](https://submit.ncbi.nlm.nih.gov/subs/sra/).

:lizard: **Como é feita a submissão?**

No exemplo a seguir iremos simular a submissão de arquivos pair-end provenientes de sequenciamento Illumina. 

1. Faça o login no [SRA Portal Wizard](https://submit.ncbi.nlm.nih.gov/subs/sra/).
2. Crie um nova submissão em "New Submission". Você será redirecionado para a página de submissão.
3. Preencha os campos de **Submitter** de acordo com suas informações e clique em "Continue".
4. Na aba **General Info**, caso você esteja criando um BioProject e um BioSample junto com a submissão das sequências, selecione "No" em ambas as opções (indicando que não há um BioProject e BioSample para essa pesquisa ainda). Caso você já tenha essas informações, especifique-as.
5. Na porção **Release date**, o indicado é a submissão mediante a publicação do artigo. Selecione a opção desejada, mas converse com sua/seu PI para marcar a opção correta.

   5.1 Caso você ainda não tenha um BioProject e BioSample, duas novas abas irão se abrir. Preencha todas as abas de acordo com as informações solicitadas. Preencha as abas com as informações relevantes do projeto e das amostras.

6.    

:lizard: ****
