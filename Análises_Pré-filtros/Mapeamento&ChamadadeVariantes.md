# Mapeamento e Chamada de Variantes

"Nesse tutorial falaremos sobre o procedimento de mapeamento e chamada de variantes. Daremos algumas opções de acordo com cada dado que você disponibiliza. Começaremos explorando tecnologias de sequenciamento e algoritmos de mapeamento, asssim como os programas utilizados para realizar tais procedimentos. 
Seguindo o processamento, partindo de um arquivo fastq, obtendo um arquivo bam, se necessário, você pode passar para a chamada de variantes."

## Introdução Mapeamento

Atualmente, há diversas tecnologias de sequenciamento que possuem diferentes objetivos: 
- Sequenciamento ultra-long reads (sequencias ultra-longas):
- Sequenciamento long reads (sequencias longas):
- Sequenciamento short reads (sequencias curtas): 

### Algoritmos de sequenciamento

Os algoritmos são: 
-: 


#### Task chunk

### Another task chunk

### Mapeamento

De acordo com cada tipo de mapeamento escolha o software que você gostaria de seguir:
- PacBio + Hi-C + Illumina:
- Hi-C + Illumina:
- Illumina:


Após terminar os tutoriais acima, volte a essa página para fazer a Chamada de variantes."

## Chamada de Variantes

O processo de chamada de variantes possui como intuito reduzir a matrix do dados para aqueles sítios que possuem alguma variação, seja ela estrutural (ex: Indels, Inversões) ou variações nucleotídicas (do inglês, Single Nucleotide Polymorphism - SNPs). Inicialmente, faremos uma chamada de variantes incluindo todos os sítios variantes, sem nenhum filtro. Após disso, para gerar uma matrix de variantes confiável, aplicamos filtros básicos: 
- Profundidade
- Qualidade da base
- Número de alelos #BCFtools filter command with the ‘-i’ flag to include variants which had more than 90% of the maximum AN value (2∗ number of samples)

### 1. Gerar um VCF incluindo apenas sítios variantes 


### 2. Calcular as estatísticas básicas para posterior filtragem 
```Linux
# Calcular Profundidade, Qualidade das bases, Número de alelos
bcftools query -f'%CHROM\t%POS\t%REF\t%ALT\t%QUAL\t%AN\t%DP\n' seuarquivo.vcf.gz | gzip -c > report_de_qualidade.tab.gz
```
O código acima resultará no seguinte arquivo: 
| Região | Posição | Alelo da referência | Alelo alternativo | Qualidade Phread da base | Número de alelos | Média da profundidade do sítio entre todos os ind. | 
| --- | --- | --- | --- | --- | --- | --- |
| chr.1 | 122 | A | G | 4990.21 | 116 | 819 |
| chr.1 | 143 | A | C | 6191.18 | 116 | 834 |
| chr.1 | 155 | T | G | 6892.26 | 116 | 954 |
| chr.1 | 177 | A | G | 4166.75 | 106 | 822 |
| chr.1 | 224 | C | T | 6130.23 | 116 | 959 |
| chr.1 | 326 | A | G | 3119 | 106 | 684 |
| chr.1 | 492 | G | A | 2758.26 | 116 | 533 |
| chr.1 | 522 | C | T | 1943.52 | 106 | 415 |

### Profundidade
A profundida aqui é definida como o **número de sequencias que foram identificadas para aquela região**. 
Vamos supor que após o mapeamento, a média de profundidade ficou em cerca de 30x. Aqui podemos fazer um filtro de média de cobertura miníma e máxima com base nessa informação. Podemos plotar a média, mediana e quartils de distribuição da profundidade de cada amostra: 

```python
# colocar aqui um plot de cobertura ao longo do genoma
```
Após analisar esses valores, podemos escolher a filtragem por profundidade de acordo com: 
- Filtro de profundidade miníma: menor do que 1,5x a média de profundidade [^1]
- Filtro de profundidade máxima: profundidade superior ao percentil de 25% [^1]

[^1]: Reference genome choice compromises population genetic analyses. DOI: 10.1016/j.cell.2025.08.034 



- [Article title](article-URL)
