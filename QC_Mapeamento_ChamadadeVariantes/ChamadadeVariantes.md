## Chamada de Variantes

O processo de chamada de variantes possui como intuito reduzir a matrix do dados para aqueles sítios que possuem alguma variação em relação a referência, seja ela estrutural (ex: Indels, Inversões) ou variações nucleotídicas, tanto de nucleotídicos únicos (do inglês, Single Nucleotide Polymorphism - SNPs), ou múltiplos (do inglês, Muti-nculeotide Polymorphisms - MNPs). Este procedimento irá gerar um arquivo final chamado [VCF](https://samtools.github.io/hts-specs/VCFv4.2.pdf). Este arquivo possui diversas colunas e informações importantes que devem ser levadas em consideração quando montando o seu arquivo. Ao longo do tutorial falaremos mais sobre eles. 

No processo de chamada de variantes, inicialmente, incluímos todos os sítios variantes, sem nenhum filtro. Em seguida, para gerar uma matrix de variantes confiável, aplicamos filtros, como: 
- Profundidade
- Qualidade da base
- Número de alelos

Um importante conceito que você irá se deparar ao ler os artigos é a filtragem definida como *hard-filtering*. O [*hard-filtering*](https://gatk.broadinstitute.org/hc/en-us/articles/360035531112--How-to-Filter-variants-either-with-VQSR-or-by-hard-filtering#2), ou em português filtragem pesada, é uma série de passos determinados pelo GATK que garante uma qualidade adequada e acurácea para as bases que serão chamadas. Esses filtros removem variantes com qualidade de mapeamento da base < 40, qualidade da base < 30, profundidade < 2 (aqui sugerimos que a profundidade seja de acordo com a média de cada indivíduo, sendo a profundidade miníma 1/3 da média e a máxima 3* a média), número de variantes em cada fitas >60, entre outros filtros que podem ser aplicados. Apesar de ter sido uma prática retirada do GATK, o *hard-filtering* pode e deve ser feito com qualquer um dos programas escolhidos para realizar esta etapa.

Os dois programas mais comuns em estudos genômicos com animais não-modelos são o [GATK HaplotypeCaller](https://gatk.broadinstitute.org/hc/en-us/categories/360002302312) e o [BCFtools](https://samtools.github.io/bcftools/bcftools.html). Devido essa alta aplicação, diversos estudos comparam os dois e discutem seus desempenhos. No trabalho de [Lefouili e colaboradores (2022)](https://doi.org/10.1038/s41598-022-15563-2) os autores constataram maior taxa de recuperação de sítios variantes com o BCFtools mpileup e maior presença de falsos positivos quando utilizaram o GATK HaplotypeCaller, geralmente relacionadas as regiões repetitivas, com sequências de boa qualidade. Eles ainda sugerem o uso do BCFtools como uma ferramenta com maior acurácea e, quando o/a pesquisador/a decidir pela utilização do GATK, em estudos populacionais a referência deve ser a mais próxima possível da espécies alvo. 

Mas além desses dois programas, ainda há diversas outras ferramentas que podem ser utilizadas com a mesma finalidade. As mais comuns são: 
- [VCFtools](https://vcftools.github.io/man_latest.html): mais utilizado no processo e filtragem de VCFs já prontos.
- [ANGSD](https://www.popgen.dk/angsd/index.php/ANGSD): mais especificamente para chamada de variantes com dados de baixa cobertura.
- [DeepVariant](https://github.com/google/deepvariant): necessita de núcleos de GPU para atingir um maior desempenho.
- [strobealign](https://github.com/ksahlin/strobealign)

Cada um dos programas citados acima possuem suas vantagens, desvantagens e requerimentos computacionais para execução. Para mais informações, sempre consulto o manual do programa desejado.

Abaixo serão exemplificados duas formas de gerar o VCF, utilizando o BCFtools e o GATK. 

### Geração de um VCF com o BCFtools
Em construção :technologist:

Utilizando o programa [BCFtools](https://samtools.github.io/bcftools/bcftools.html):
```linux
bcftools mpileup -b bamlist -C50 -f referência.fasta -d 100 -Q30 -q30 --threads 7 -a FORMAT/AD,FORMAT/DP,INFO/AD -I | bcftools call -o sítiosvariantes_amostras.vcf.gz -Oz -f GQ,GP -v -V indels -m --threads 7
```
<details>
   <summary> :writing_hand: Explicação do comando </summary>

A primeira coisa que precisamos observar aqui são os comandos *mpileup* (sumariza a informação das sequências em cada posição) e *call* (chamada de variantes) que serão os modulos para chamar as variantes.   
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
- ```-V```: não incluir Indels. Caso queira incluir, não usar este comando.
- ```-v```: chama apenas sítios varientes.

</details>

Um detalhe para levar em consideração é a chamada dos tipos de variantes ou sítios que você gostaria de incluir em seu arquivo VCF. Algumas análises, como π e *dxy*, necessitam de todos os sítios para possibilitar a estimativa correta. Caso você tenha a intenção de realizar essas análises há uma pequena mudança no comando acima: 

```linux
bcftools mpileup -b bamlist -C50 -f referência.fasta -d 100 -Q30 -q30 --threads 7 -a FORMAT/AD,FORMAT/DP,INFO/AD -I | bcftools call -o amostras.vcf.gz -Oz -f GQ,GP -m --threads 7
```

Nota-se que a única mudança foi a retirada do parâmetro ```-v``` e ```-V```. Outra mudança é o tamanho do arquivo. Como ele contêm todos os sítios, o arquivo terá um tamanho muito maior. Garanta que você possui espaço de armazenamento o suficiente para gerar o arquivo. 

### Geração de um VCF com o GATK

Com o programa [GATK]() a chamada de variantes é um processo que exige mais comandos. Por isso, acesse o tutorial [Chamada de Variantes com o GATK](./ChamadaDeVariantesGATK.md)


Utilizando o programa [deepVariants]():
```linux
# em breve
```

Após a geração do arquivo de variantes, podemos checar algumas informações importantes para compreender os nossos dados e avaliar se estão ok para serem utilizados nas análises subsequêntes. Uma das primeiras etapas para compreender a distribuição dos nossos dados, pode ser averiguada realizando uma PCA. Assim, podemos compreender se há um desvio do esperado. Ademais, podemos realizar checagens para filtrar nossos dados da melhor forma.  

### Estatísticas básicas para posterior filtragem 
Com o VCF gerado, vamos calcular as estatísticas básicas para compreendermos sobre a qualidade das variantes que foram chamadas. Vamos começar calculando a Qualidade Phread da base (QUAL), o Número de Alelos por sítio (AN) e a Profundidade Absoluta da sítio (DP).
```Linux
# Calcular Profundidade, Qualidade das bases, Número de alelos
bcftools query -f'%CHROM\t%POS\t%REF\t%ALT\t%QUAL\t%AN\t%DP\n' seuarquivo.vcf.gz | gzip -c > report_de_qualidade.tab.gz
```
O código acima resultará no seguinte arquivo: 
| Região | Posição | Alelo da referência | Alelo alternativo | Qualidade Phread da base | Número de alelos | Profundidade do sítio entre todos os ind. | 
| --- | --- | --- | --- | --- | --- | --- |
| chr.1 | 122 | A | G | 4990.21 | 116 | 819 |
| chr.1 | 143 | A | C | 6191.18 | 116 | 834 |
| chr.1 | 155 | T | G | 6892.26 | 116 | 954 |
| chr.1 | 177 | A | G | 4166.75 | 106 | 822 |
| chr.1 | 224 | C | T | 6130.23 | 116 | 959 |
| chr.1 | 326 | A | G | 3119 | 106 | 684 |
| chr.1 | 492 | G | A | 2758.26 | 116 | 533 |
| chr.1 | 522 | C | T | 1943.52 | 106 | 415 |

Vamos calcular também a profundidade média por indivíduo:
```Linux
# Calcular a profundidade média por indivíduo
vcftools --gzvcf seuarquivo.vcf.gz --depth --out report_de_profundidade_por_ind
# o resultado dessa linha é: report_de_profundidade_por_ind.idepth
```

Vamos fazer o mesmo para a profundidade média por sítio:
```Linux
# Calcular a profundidade média por indivíduo
vcftools --gzvcf seuarquivo.vcf.gz --site-mean-depth --out report_de_profundidade_por_sitio
# o resultado dessa linha é: report_de_profundidade_por_sitio.ldepth.mean
```

### Profundidade
A profundida aqui é definida como o **número de sequencias que foram identificadas para aquela região**. 
Vamos supor que após o mapeamento, a média de profundidade ficou em cerca de 30x. Aqui podemos fazer um filtro de média de cobertura miníma e máxima com base nessa informação. Podemos plotar a média, mediana e quartils de distribuição da profundidade de cada sítio: 

```R
Profundidade_sitios <- read_delim("report_de_profundidade_por_sitio.ldepth.mean", delim = "\t", col_names = c("chr", "pos", "media_profundidade", "variancao_profundidade"))
Profundidade_sitios <- Profundidade_sitios %>% 
  mutate(pos = as.numeric(pos),
  media_profundidade = as.numeric(media_profundidade),
  variancao_profundidade = as.numeric(variancao_profundidade))
summary(Profundidade_sitios$media_profundidade)
```
O resultado deve ser: 
| Min. | 1st Qu. | Median | Mean | 3rd Qu. | Max. | NA's |
| ---- | ---- | ---- | ---- | ---- | ---- | ---- | 
| 1.926 | 15.887 | 16.981 | 17.280 | 17.982 | 3333.070 | 1 |

Após analisar esses valores, podemos escolher a filtragem por profundidade de acordo com: 
- Filtro de profundidade miníma: 1/3 da profundidade média por indivíduo. [^1]
- Filtro de profundidade máxima: 2x ou 3x a profundidade média por indivíduo [^1].

## Filtragem de um VCF 

A filtragem de um arquivo VCF pode ser realizada em diversas etapas e composta por diferentes combinações de filtros. Abaixo serão exemplificados algumas opções recorrentes nesse processo: 

### Quantidade máxima de indivíduos faltantes por sítio
```linux
bcftools view -i 'F_MISSING <= 0.2' -m2 -M2 -v snps -Oz -o arquivo_de_saída.vcf.gz arquivo_de_entrada.vcf.gz
```

### Frequência alélica miníma
```linux
bcftools view -q 0.05:minor -Oz -o arquivo_de_saída.vcf.gz arquivo_de_entrada.vcf.gz
```

### Exclusão de regiões 
```linux
vcftools --gzvcf arquivo_de_entrada.vcf.gz --not-chr mtCHR --recode | gzip > arquivo_de_saída.vcf.gz
```
- ```--not-chr```: especificar o nome da região a ser excluída do VCF. 

### Obtenção de sítios independentes 
```linux
# em breve
```

Muitos desses comandos podem ser utilizados em conjunto para gerar um VCF final filtrado. Explore as possibilidades e se enfrentar qualquer problema, compartilhe na [aba de discussões](https://github.com/bbandriola/GuiaDadosGenomicos_INCT-GB/discussions) deste repositório. 

Com o VCF devidamente filtrato e pronto para ser utilizado, retorne a [página inicial do repositório](https://github.com/bbandriola/GuiaDadosGenomicos_INCT-GB/tree/main) para explorar as análises que podem ser realizada com o seu arquivo :dragon:

[^1]: Whole genome sequences of 297 Duolang sheep for litter size. DOI: 10.1038/s41597-025-05448-0
[^2]: Reference genome choice compromises population genetic analyses. DOI: 10.1016/j.cell.2025.08.034 
