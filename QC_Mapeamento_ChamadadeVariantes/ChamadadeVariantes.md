## Chamada de Variantes

O processo de chamada de variantes possui como intuito reduzir a matrix do dados para aqueles sítios que possuem alguma variação, seja ela estrutural (ex: Indels, Inversões) ou variações nucleotídicas (do inglês, Single Nucleotide Polymorphism - SNPs). Inicialmente, faremos uma chamada de variantes incluindo todos os sítios variantes, sem nenhum filtro. Após disso, para gerar uma matrix de variantes confiável, aplicamos filtros básicos: 
- Profundidade
- Qualidade da base
- Número de alelos #BCFtools filter command with the ‘-i’ flag to include variants which had more than 90% of the maximum AN value (2∗ number of samples)

São diversas as ferramentas para realizar a etapa de chamada de variantes. As mais comumente utilizadas são: 
- [GATK](https://gatk.broadinstitute.org/hc/en-us/categories/360002302312)
- [BCFtools](https://samtools.github.io/bcftools/bcftools.html)
- [VCFtools](https://vcftools.github.io/man_latest.html)

Além disso, outras ferramentas como [DeepVariant](https://github.com/google/deepvariant), programa baseado em redes neurais, e [strobealign](https://github.com/ksahlin/strobealign) são descritas como mais eficientes do que as citadas acima, mas não serão abordadas nesse tutorial. Para mais informações sobre esses programas, acesse seus manuais linkados aqui. 

### 1. Gerar um VCF incluindo apenas sítios variantes 
Em construção :technologist:

Utilizando o programa [BCFtools]():
```linux
bcftools mpileup -b bamlist -C50 -f referência.fasta -d 100 -Q30 -q30 --threads 7 -a FORMAT/AD,FORMAT/DP,INFO/AD -I | bcftools call -o sítiosvariantes_amostras.vcf.gz -Oz -f GQ,GP -V indels -m --threads 7
```
<details>
   <summary> :writing_hand: Explicação do comando </summary>
   
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

</details>

Utilizando o programa [GATK](): 
```linux
# em breve
```

Utilizando o programa [deepVariants]():
```linux
# em breve
```

### 2. Calcular as estatísticas básicas para posterior filtragem 
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
- Filtro de profundidade miníma: 1/3 da profundidade média por indivíduos. [^1]
- Filtro de profundidade máxima: 2x ou 3x a profundidade média por indivíduo [^1].

[^1]: Whole genome sequences of 297 Duolang sheep for litter size. DOI: 10.1038/s41597-025-05448-0
[^2]: Reference genome choice compromises population genetic analyses. DOI: 10.1016/j.cell.2025.08.034 

