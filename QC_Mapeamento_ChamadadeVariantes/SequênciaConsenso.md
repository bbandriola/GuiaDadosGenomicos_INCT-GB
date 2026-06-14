# Geração da Sequência Consenso

A sequência consenso é um importante arquivo obtido apartir do [mapeamento dos dados brutos](./Mapeamento.md). Esse processo resulta em um arquivo FASTA no qual cada cabeçalho, linhas que iniciam com o simbolo ```>```, indicam o código da sequência, seguido da sequência *consenso* daquela porção do genoma. A sequência consenso armazena a informação do nucleotídeo, ou aminoácido, mais comum ou mais conservado, em cada posição do genoma, sendo um arquivo extremamente útil em diversos casos: 

- Obtenção da sequência específica
- Geração de árvores independentes
- Estimativa de tempo de divergência
- Predição de função gênica

Entre outras análises... :smile_cat:

Exemplo de um arquivo FASTA com dois cromossomos: 
```linux
> chr 1
ACGTACGTACGTACGTACGTACGT...
> chr 2
TGCATGCATGCATGCATGCATGCA...
```

Para gerar a sequência consenso, precisamos do arquivo BAM (resultante do mapeamento) e o genoma de referência da espécie utilizada no mapeamento. Podemos gerar esse arquivo a partir de dois programas: o [**ANGSD**](https://www.popgen.dk/angsd/index.php/ANGSD), utilizado principalmente para sequenciamento de baixa e média cobertura; ou [**BCFtools**](https://samtools.github.io/bcftools/bcftools.html). De acordo com a sua escolha, explore a sugestão nas seções a seguir:

<details>
   <summary> :mag_right: com ANGSD </summary>

   1. Acesse o diretório que contém os arquivos BAM:
      ```unix
      cd diretório/dos/BAMs
      ```
   2. Geração das sequências consenso para cada arquivo BAM:
      ```linux
      for i in *bam; do angsd -i $i -doFasta 2 -doCount 1 -explode 1 -setMinDepth 5 -p 10 -minQ 30 -out $i; done
      ```
      O comando específica o arquivo de entrada com o parâmetro ```-i```, solicita a geração do arquivo fasta de acordo com a base mais comum da sequencia com ```-doFasta 2```, ```-explore 1``` gera a sequência para todos os cromossomos, ```-setMinDepth 5``` e ```-minQ30``` específica que é necessário pelo menos 5 sequências e uma pontuação de Phred de, pelo menos, 30 em todos os sítios para determinar qual base será definida (caso contrário, um N é colocado no lugar). Por fim, ```-o``` determina a saída do comando. Atualmente, o ANGSD está desenvolvendo uma opção para gerar a sequência consenso utilizando o código [IUPAC](https://www.bioinformatics.org/sms/iupac.html).

</details>


<details>
   <summary> :mag_right: com BCFtools </summary>

  Para gerar uma sequência consenso com o BCFtools, é necessário já obter um arquivo de variants (em VCF). Nesse tutorial, será abordada a geração do arquivo de variantes seguido da geração do arquivo FASTA. Caso queira mais explicações sobre o arquivo VCF, visite o tutorial de [chamada de variantes](./ChamadadeVariantes.md).
 
   1. Realize a chamada de variantes para obtenção do arquivo VCF excluindo Indels:
      ```linux
      bcftools mpileup -b bamlist -C50 -f referência.fasta -d 100 -Q30 -q30 --threads 7 -a FORMAT/AD,FORMAT/DP,INFO/AD -I | bcftools call -o sítiosvariantes_amostras.vcf.gz -Oz -f GQ,GP -V indels -m --threads 7
      ```
      - ```-b```: arquivo com o caminho de todos os arquivos BAM a serem incluídos na geração do VCF.
      - ```-C```: coeficiente para a redução da qualidade do mapeamento em sequência com um número excessivo de discrepâncias.
      - ```-f```: arquivo FASTA da referência.
      - ```-d```: máximo de sequências a serem consideradas por posição.
      - ```-Q```: qualidade miníma da base para ser chamada.
      - ```-q```: qualidade miníma do mapeamento para a base ser chamada.
      - ```-a```: lista de tags para serem adicionadas nas columas FORMAT e INFO.
      - ```-I```: não incluir Indels.
      - ```-o```: arquivo de saída.
      - ```-Oz```: formato do arquivo de saída.
      - ```-f```: campos da coluna FORMAT que serão incluídos em cada amostra. 
      - ```-V```: não incluir Indels (caso algum tenha passado no mpileup).
   
   2. Obteção do nome de cada amostra e indexação do VCF:
      ```linux
      bcftools query -l sítiosvariantes_amostras.vcf.gz > amostras.txt
      bcftools index sítiosvariantes_amostras.vcf.gz
      ```
   
   3. Geração do arquivo consenso de cada amostra a partir de um VCF contendo <ins>apenas</ins> sítios variantes:
      ```linux
      while read amostra; do echo "Processando $amostra"; bcftools consensus -f referência.fasta -M N -I -s "$amostra" input.vcf.gz > "${amostra}.consenso.fa"; done < amostras.txt
      ```

</details>

Para mais detalhes de possíveis parâmetros para cada um dos programas e etapas, visite os manuais. 

Manuais: 
:books: [**ANGSD**](https://www.popgen.dk/angsd/index.php/ANGSD)
:books: [**BCFtools**](https://samtools.github.io/bcftools/bcftools.html)

