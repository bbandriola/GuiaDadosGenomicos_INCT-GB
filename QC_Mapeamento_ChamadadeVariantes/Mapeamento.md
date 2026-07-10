# Mapeamento de genomas

O processo de mapeamento genômico refere-se aos procedimentos que permitem organizar as sequências brutas obtidas do sequenciamento em relação a uma sequência guia, um genoma de referência. O genoma de referência pode ter sido montado pelo seu próprio grupo de pesquisa, utilizando técnicas discutidas em [outros tutoriais desse repositório](../GenomasDeReferência), ou pode ter sido gerado por outros grupos e disponibilizado em bancos de dados públicos, como o [NCBI](https://www.ncbi.nlm.nih.gov/datasets/genome/).

De modo geral, dados provenientes de diferentes tecnologias de sequenciamento podem ser utilizados para o mapeamento. Entretanto, essa abordagem é mais frequentemente aplicada quando sequenciamos sequências curtas. Para a obtenção de um bom resultado, dois fatores são importantes: a qualidade do genoma de referência escolhido (proximidade filogenética e qualidade da montagem) e a cobertura das sequências brutas geradas (5×, 10×, 20×...). Esses fatores influenciam diretamente a qualidade do mapeamento, sua contiguidade, a confiabilidade das bases identificadas e, consequentemente, influenciará as análises posteriores. 

Os processos que envolvem o mapeamento são: 
1. Checagem de qualidade das [sequências brutas](./QC.md)
2. Filtragem pré-mapeamento
   - Remoção dos adaptadores
   - Filtragem de sequências de baixa qualidade
3. Mapeamento das sequências contra o genoma de referência
4. Marcação das sequências de duplicata
5. [Geração da sequência consenso](./SequênciaConsenso.md)

## Mapeamento de sequências Illumina 

Vamos supor que possuímos dois arquivos de sequenciamento Illumina pareados codificados no formato 1.9+: um arquivo *foward* (leituras da fita na direção 5' -> 3') e um arquivo *reverse* (leituras da fita na direção 3' -> 5'), chamadas de ```seq1_1.fastq.gz``` e ```seq1_2.fastq.gz```, respectivamente. A primeira etapa é a remoção de sequências de adaptadores e filtragem de sequências indesejadas, como aquelas de baixa qualidade. Essas sequência trazem ruído na análise e compromete a eficiência do mapeamento. Para fazer isso, utilizaremos o programa [fastp](https://github.com/OpenGene/fastp).  

Um comando geral do [fastp](https://github.com/OpenGene/fastp) é o seguinte: 

```linux
fastp -i seq1_1.fastq.gz -I seq1_2.fastq.gz -o seq1_1_trimmed.fq.gz -O seq1_2_trimmed.fq.gz --qualified_quality_phred 15 --unqualified_percent_limit 40 --length_required 0  -h seq1_fastp.html -w 4
```

<details>  
  <summary> :writing_hand: Explicação do comando</summary>


  - ```fastp```: chamar o comando.
  - ```-i``` : sequência *foward*.
  - ```-I``` : sequência *reverse*.
  - ```-o``` : resultado do arquivo filtrado da sequência *foward*.
  - ```-O``` : resultado do arquivo filtrado da sequência *reverse*.
  - ```-h```: relatório da qualidade dos dados em formato HTML.
  - ```-w```: número de núcleos a serem utilizados na análise.
</details>

No comando acima os adaptadores serão automaticamente reconhecidos e removidos. Caso você tenha alguma sequência de adaptores especifícas, você pode utilizar o parâmetro ```--adapter_sequence``` para as sequências do arquivo de leitura *foward* e o ```--adapter_sequence_r2``` para as sequências do arquivo da leitura *reverse*. Em relação aos demais parâmetros de filtragem, os citados acima são os valores padrões da análise, ou seja, **devem** ser revisados e escolhidos de acordo com os seus dados. Por exemplo, no filtro ```--qualified_quality_phred``` um valor de 15 é um valor razoavelmente baixo. Neste caso, valores acima de 30 darão mais confiabilidade a base. Em relação ao filtro de ```--length_required``` é possível definir um tamanho minímo para suas sequências. Além disso, o github do [fastq](https://github.com/OpenGene/fastp) apresenta outras opções de filtragem. Explore!!! 

O comando executado produzirá dois arquivos de saída importantes: ```seq1_1_trimmed.fq.gz``` e ```seq1_2_trimmed.fq.gz```. Ambos serão utilizados na próxima etapa do processamento. Você pode checar o número de sequências mantidas em cada um dos arquivos com o comando: 

```linux
zcat seq1_1_trimmed.fq.gz | wc -l
zcat seq1_2_trimmed.fq.gz | wc -l
```

O [fastp](https://github.com/OpenGene/fastp) é apenas uma das opções para realizar a etapa de filtragem pré-mapeamento. Existem outras ferramentas disponíveis, como [Trimmomatic](http://www.usadellab.org/cms/?page=trimmomatic), [adapterremoval](https://github.com/MikkelSchubert/adapterremoval) e [Trim Galore!](https://www.bioinformatics.babraham.ac.uk/projects/trim_galore/). 

Uma questão importante é se atentar ao tipo de tecnologia utilizada para o sequenciamento. Se você estiver utilizando sequências provenientes de outras tecnologias de sequenciamente (exemplo: Oxford Nanopore) outros programas devem ser utilizadas devido a arquitetura dos arquivos brutos provenientes dessas tecnologias. O pré-processamento dessas amostras não serão abordadas nesse tutorial, mas podem ser realizadas por outras ferramentas como [fastplong](https://github.com/OpenGene/fastplong). 

Seguimos... :otter:

Após a execução do comando do ```fastp```, partimos para o mapeamento das sequências brutas contra nosso genoma de referência. Nessa etapa é importante que tenhamos o genoma de referência baixado em algum diretório do servidor e os arquivos resultantes do comando anterior. 

Caso você não tenha baixado o genoma de referência de interesse, faça isso :mag_right:

<details>  
  <summary> :pushpin: Como baixar um genoma de referência do NCBI </summary>

  A primeira etapa é encontrar o genoma de referência de interesse. Para isso, explore o banco do [NCBI](https://www.ncbi.nlm.nih.gov/datasets/genome/). Depois dessa escolha, precisamos localizar o endereço FTP onde os arquivos estão armazenados. Como exemplo, utilizaremos o genoma de referência da espécie [*Lepidochelys kempii*](https://www.ncbi.nlm.nih.gov/datasets/genome/GCF_965140285.1/). Para acessar o caminho do FTP, clique na opção **FTP**, localizada acima das informações gerais da montagem. Ao clicar nessa opção, você será redirecionado para uma página contendo todos os arquivos associados ao genoma de referência de [*Lepidochelys kempii*](https://ftp.ncbi.nlm.nih.gov/genomes/all/GCF/965/140/285/GCF_965140285.1_rLepKem1.hap1/). Para fazer o download desse genoma, abra o terminal, navegue até o diretório onde os arquivos serão armazenar e execute o comando abaixo.
  :warning: Certifique-se que há espaço suficiente na partição que deseja fazer o download desse arquivo. Você pode checar isso com ```df -h```. 
  Comando para dowload do genoma de referência do NCBI:
  ```linux
wget --recursive --no-host-directories https://www.ncbi.nlm.nih.gov/datasets/genome/GCF_965140285.1/ -P ./
```
- ```wget```: comando que recupera arquivos disponíveis na web. Suporta HTTP, HTTPS e FTP.
- ```--recursive```: indica ao wget para fazer o download do diretório completo.
- ```--no-host-directories```: impede que o wget crie uma pasta com o nome do domínio ou do nome do host do site ao baixar arquivos.
- ```-P```: indica o diretório de saída. 
Também há a possibilidade de fazer o download de apenas um arquivo. Na linha de comando apenas o arquivo ```GCF_965140285.1_rLepKem1.hap1_genomic.fna.gz``` irá ser baixado:
 ```linux
wget --recursive --no-host-directories https://www.ncbi.nlm.nih.gov/datasets/genome/GCF_965140285.1/GCF_965140285.1_rLepKem1.hap1_genomic.fna.gz -P ./
```

Por fim, vamos descompactar o genoma de referência: 
```linux
gunzip GCF_965140285.1_rLepKem1.hap1_genomic.fna.gz
```
</details>

Uma vez que temos o genoma de referência de interesse, precisamos realizar a indexação desse arquivo. A indexação permite a ordenação do genoma de referência em porções, indicando a ordem das sequências ao longo do arquivo. A escolha de como indexar seu genoma de referência, dependerá do programa de alinhamento que será utilizados. Para mapear com o BWA, utilizaremos do comando *bwa index*. 

```linux
bwa index genoma_de_referencia.fna
```
<details>
   <summary> :writing_hand: Arquivos de saída </summary>
  
   - ``` genoma_de_referencia.fna.amb```
   - ``` genoma_de_referencia.fna.ann```
   - ``` genoma_de_referencia.fna.bwt```
   - ``` genoma_de_referencia.fna.pac```
   - ``` genoma_de_referencia.fna.sa```

</details>

Caso você decida mapear com o [minimap2](https://github.com/lh3/minimap2), é necessário utilizar o seguinte comando:
```linux
minimap2 -d genoma_de_referencia.mmi genoma_de_referencia.fasta
```

<details>
   <summary> :writing_hand: Arquivos de saída </summary>

O comando acima gera uma única saída. Esse arquivo pode ser utilizado no lugar da referência na linha para mapear. Existem outras formar de gerar um index no minimap2. Cheque qual a melhor no seu caso. 
   - ```genoma_de_referencia.mmi```

</details>

Após a indexação, passamos para o mapeamento. 

Para mapear as sequências já filtradas ao genoma de referência, podemos utilizar o programa [BWA](https://github.com/lh3/BWA) ou [minimap2](https://github.com/lh3/minimap2), mas há diversos outros, como [strobealign](https://github.com/ksahlin/strobealign).

O [BWA](https://github.com/lh3/BWA) é um programa muito popular para o mapeamento de sequências Illumina. Nesse caso, utilizaremos o comando *bwa-mem*, ele ultiliza de um algoritmo de alinhamento local, eficiente e acurado para mapear dados provenientes de sequenciamento Illumina com tamanhos de sequência que variam de 70bp a maiores. Além disso, vamos usar o programa *SAMtools* para gerar o arquivo final em formato BAM, uma versão binária do arquivo SAM. 
:warning: Atenção: essa é uma etapa computacionalmente custosa. É normal ela levar um tempo considerável para ser finalizada. O importante é garantir memória suficiente para o processo ser finalizado sem nenhum problema. 

O comando do **bwa mem** é extremamente simples: 
```linux
bwa mem reference_genomic.fna seq1_1_trimmed.fq.gz seq1_2_trimmed.fq.gz | samtools sort -O bam -o seq1_ref.bam
```

O [minimap2](https://github.com/lh3/minimap2) é um programa versátil que lida com diferentes tipos de sequenciamento (PacBio, ONP, Illumina) descrito como 3x mais rápido que o BWA-MEM. A linha de comando para realizar o mapeamento pode ser:
```linux
minimap2 -ax sr reference_genomic.fna seq1_1_trimmed.fq.gz seq1_2_trimmed.fq.gz | samtools sort -O bam -o seq1_ref.bam
```

<details>
   <summary> :writing_hand: Explicação do comando </summary>
   
   - ```-ax```: parâmetro para especificar sequências curtas provenientes de sequenciamento Illumina.
   - ```reference_genomic.fna```: nesse caso, utilizamos apenas o fasta da referência e o minimap irá gerar a indexação padrão para mapear em conjunto com a linha de mapeamento. Caso você já tenha indexado, pode utilizar o arquivo gerado no lugar dp arquivo FASTA da referência. 

</details>

Com as sequências brutas mapeadas, obtemos nosso segundo tipo de arquivo: o arquivo BAM. O arquivo BAM é a versão binário de um SAM e contêm todas as informações do mapeamento: local em que cada sequência mapeaou, a profundidade de cada região do genoma e contiguidade. Em ambos casos, o resultado do **BWA** e do **minimap2** produzem um arquivo SAM, mas para economizar espaço e desempenho do processamento dos dados, utilizamos o **samtools** para gerar o arquivo final sem precisar escrever o SAM, pulando direto para um arquivo BAM. 

A próxima etapa do processamento dos dados é marcar as sequências de duplicas. As sequências de duplicatas são resultados de cópias idênticas de fragmentos do DNA, podem ter origem biológica ou serem artefatos do sequênciamento (artefatos de PCR). É importante marcarmos essas regiões para posterior filtragem. A marcação das sequências de duplicata pode ser feita com o programa [sambamba markup](https://lomereiter.github.io/sambamba/docs/sambamba-markdup.html). Por fim, vamos indexar nosso arquivo BAM.
```linux
sambamba markdup seq1_ref.bam | samtools index > dupmark_seq1_ref.bam 
```
Se você quiser não apenas marcar as duplicatas de PCR, mas também exclui-las, adicione o parâmetro ```-r``` na linha acima. 

Antes de avaliarmos nosso mapeamento, precisamos indexar nosso arquivo BAM para ser utilizado em análises posteriores. Para isso, utilizamos a função [*index*](https://www.htslib.org/doc/samtools-index.html) do [samtools](https://www.htslib.org/doc/samtools.html): 
```linux
samtools index dupmark_seq1_ref.bam 
```

Por fim, para avaliar o resultado do nosso mapeamento, utilizaremos o [samtools](https://www.htslib.org/doc/samtools.html) com o módulo [*flagstat*](https://www.htslib.org/doc/samtools-flagstat.html): 
```linux
samtools flagstat dupmark_seq1_ref.bam
```
O resultado do comando acima é um relatório de qualidade do mapeamento. Para uma explicação completa do resultado visite o site do [flagstat](https://www.htslib.org/doc/samtools-flagstat.html). 

Após a finalização dessa etapa, é possível utilizar o arquivo BAM para gerar um [SFS](), estimar [regiões de homozigose](), [heterozigosidade ao longo do genoma](), entre outras análises. Além disso, o arquivo BAM é o primeiro passo para gerarmos o [consenso da sequência das amostras](./SequênciaConsenso.md) e realizarmos a [chamada de variantes](https://github.com/bbandriola/GuiaDadosGenomico_INCT-GB/blob/472b9dd920d725e7a5c8f18b5e606e321dc7b01a/An%C3%A1lises_Pr%C3%A9-filtros/ChamadadeVariantes.md). A partir daqui, siga o tutorial que melhor convém a você. 

Caso queria olhar as demais opções, retorne a [página inicial do repositório](https://github.com/bbandriola/GuiaDadosGenomico_INCT-GB.git).

## Pipelines de mapeamento 

Os comandos apresentados anteriormente fornecem uma visão geral de como os dados devem ser processados para obter conjuntos de dados confiáveis para as análises subsequentes. Entretanto, os avanços nas tecnologias de sequenciamento permitiram a geração de grandes volumes de dados a custos cada vez mais acessíveis, tornando comum o sequenciamento de centenas indivíduos em um mesmo projeto. Logo, torna-se necessário adaptar e automatizar as etapas de processamento desses dados. Para otimizar o desempenho e o tempo no processamento inicial, diversos *pipelines* foram desenvolvidos com o objetivo de integrar todas as etapas descritas anteriormente em um único fluxo de trabalho, permitindo a execução de todos esses passos por meio de uma única linha de comando (acompanhada de diversos arquivos de configuração para a definição das parâmetros do processamento :sweat_smile:). 

Sugerimos dois principais *pipelines*: 
- **SNPArcher** [Repositório](./SNPArcher) [Artigo](https://doi.org/10.1093/molbev/msad270)
- **Paleomix** [Repositório](./Paleomix) [Artigo](https://doi.org/10.1038/nprot.2014.063)



