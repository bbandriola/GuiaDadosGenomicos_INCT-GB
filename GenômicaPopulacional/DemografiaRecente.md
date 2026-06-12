## Estimativa de demografia recente 

Estimar a demográfica de populações é uma tarefa corriqueira em projetos que investigam as dinâmicas históricas que influenciaram na atual situação genética de espécies e populações. Dentro dos programas que estimam a história demográfica recente podemos citar o [stairway-plot](https://github.com/xiaoming-liu/stairway-plot-v2/tree/master). O [stairway-plot](https://github.com/xiaoming-liu/stairway-plot-v2/tree/master) possibilita a investigação da demografia de populações através da análise das frequências alélias, esse arquivo é denominado espectro de frequência alélica (do inglês, site frequency spectrum - SFS). O SFS pode ser gerado de duas formas: 
- Unfolded: quando temos a informação do alelo ancestral.
- Folded: quando não sabemos o estado ancestral dos sítios.

### Vamos abordar primeiro a geração do **SFS unfolded**: 
Supondo que você sabe a o estado ancestral dos seus alelos. Vamos trabalhar com o programa [est-sfs](https://nf-co.re/modules/estsfs) e para isso, precisamos gerar o arquivo de entrada para o programa. Nessa etapa precisamos dos seguintes arquivos: 
* ```arquivodevariantes.vcf.gz```: o arquivo VCF pode estar em nível de *scaffolds* ou cromossomos. Em ambos os caso, é necessário gerar um arquivo VCF para cada um dos seus cromossomos ou *scaffolds*. Para fazer isso você pode utilizar o comando:

   ```linux
    while read chr; do
    echo "Processamento de $chr"
    vcftools --gzvcf arquivo.vcf.gz --chr "$chr" --recode --stdout | gzip > "${chr}.vcf.gz"
    done < cromossomos.txt
    ```
    O arquivo ```cromossomos.txt``` contém o código para cada uma das regiões cromossômicas no VCF de entrada.

* ```Generate_input_est-sfs.py```: arquivo para gerar o input para o programa est-sfs.
* ```groupfile.txt```: arquivo para definir os indivíduos da mesma população e o indivíduo (ou os indivíduos) que apresentam o estado ancestral.

<details>  
  <summary> Arquivo Generate_input_est-sfs.py </summary>


```linux
# Usage: python script.py input.vcf(.gz) groups.txt
# groups.txt
# INGROUP: sample1 sample2 sample3
# OUTGROUP1: sample11
# OUTGROUP2: sample12 sample13

import sys
import re
import gzip


def open_vcf(filename):
    if filename.endswith(".gz"):
        return gzip.open(filename, "rt")
    return open(filename)


def parse_group_file(group_file):
    groups = {}
    with open(group_file) as f:
        for line in f:
            line = line.strip()
            if not line or line.startswith("#"):
                continue
            name, samples = line.split(":")
            groups[name.strip()] = samples.strip().split()
    return groups


def get_sample_indices(vcf_header, groups):
    header_fields = vcf_header.strip().split("\t")
    sample_names = header_fields[9:]

    indices = {}
    for group, samples in groups.items():
        indices[group] = []
        for s in samples:
            if s not in sample_names:
                raise ValueError(f"Sample {s} not found in VCF")
            indices[group].append(sample_names.index(s))
    return indices


def extract_genotypes(fields):
    format_fields = fields[8].split(":")
    if "GT" not in format_fields:
        return None

    gt_index = format_fields.index("GT")
    sample_data = fields[9:]

    genotypes = []
    for s in sample_data:
        parts = s.split(":")
        if len(parts) <= gt_index:
            genotypes.append("./.")
        else:
            genotypes.append(parts[gt_index])

    return genotypes


def count_nucleotides(genotypes, ref, alt):
    counts = {"A": 0, "C": 0, "G": 0, "T": 0}
    alleles = [ref] + alt.split(",")

    for gt in genotypes:
        if gt in ("./.", ".|.", "."):
            continue  # just skip missing, don't kill site

        gt = re.split("[/|]", gt)

        for allele in gt:
            if allele == ".":
                continue
            allele = int(allele)
            if allele < len(alleles):
                base = alleles[allele]
                if base in counts:
                    counts[base] += 1

    return counts


def format_counts(counts):
    return f"{counts['A']},{counts['C']},{counts['G']},{counts['T']}"


def main(vcf_file, group_file):
    groups = parse_group_file(group_file)

    with open_vcf(vcf_file) as f:
        for line in f:
            if line.startswith("#CHROM"):
                indices = get_sample_indices(line, groups)
                print("\t".join(groups.keys()))
                continue

            if line.startswith("#"):
                continue

            fields = line.strip().split("\t")
            ref = fields[3]
            alt = fields[4]

            # keep SNP filter (you can remove if needed)
            if len(ref) != 1 or any(len(a) != 1 for a in alt.split(",")):
                continue

            genotypes = extract_genotypes(fields)
            if genotypes is None:
                continue

            output = []

            for group in groups:
                group_gts = [genotypes[i] for i in indices[group]]
                counts = count_nucleotides(group_gts, ref, alt)
                output.append(format_counts(counts))

            print("\t".join(output))


if __name__ == "__main__":
    if len(sys.argv) != 3:
        print("Usage: python script.py input.vcf(.gz) groups.txt")
        sys.exit(1)

    main(sys.argv[1], sys.argv[2])
```
</details>

<details>  
  <summary> Arquivo groupfile.txt </summary>

Syntax do arquivo groupfile.txt:
  - O arquivo deve ter as amostras separadas por espaço
  - Utilizar apenas uma amostra para cada um dos grupos externos (os que irão fornecer a informação sobre o alelo ancestral). 
```linux
INGROUP: sample1 sample2 sample3
OUTGROUP1: sample11
OUTGROUP2: sample12 
```
</details>

Uma vez que você possui todos os arquivos e eles estão prontos para serem utilizados, precisamos executar o seguinte comando para gerar o arquivo de entrada para o programa:
```linux
for i in chr*vcf.gz; do python Generate_input_est-sfs.py $i groupfile_ANCAllele.txt > input_est-sfs_$i.allele; done
```
Esse comando irá gerar um arquivo de entrada para cada cromossomo como o seguinte: 

```linux
INGROUP	OUTGROUP1	OUTGROUP2
0,17,0,5	0,0,0,2	0,2,0,0
0,19,5,0	0,0,2,0	0,0,2,0
18,0,2,0	0,0,2,0	0,0,2,0
12,0,10,0	2,0,0,0	2,0,0,0
0,9,0,15	0,0,0,2	0,0,0,2
```
Esse é o arquivo de entrada do est-sfs. Contudo, ainda é necessário fazer um último ajuste antes de estar pronto para ser utilizado no est-sfs. Como pode-se perceber, no arquivo há três colunas: 

INGROUP - a frequência das bases A,C,G,T para cada alelos para cada indivíduo. Lembre-se que são alelos, ou seja, para cada um indivíduo teremos a contagem de dois sítios.
OUGROUP1 e OUTGROUP2 - a frequência dos alelos para cada grupo externo.

Apesar da contagem de alelos estar correta quando as espécies são diploides, o est-sfs não aceita que os grupos externos possuam mais de um alelo. Portanto, iremos executar o comando abaixo para retirar o cabeçalho e modificar as colunas dos grupos externos para conterem apenas uma alelo:
```linux
for i in inputest-sfs_*.allele; do tail -n +2 $i | awk -F'\t' 'BEGIN{OFS="\t"} {gsub(/2/,"1",$2); gsub(/2/,"1",$3); print}' > $i.txt; done
```
O resultado desse comando será:
```linux
0,17,0,5	0,0,0,1	0,1,0,0
0,19,5,0	0,0,1,0	0,0,1,0
18,0,2,0	0,0,1,0	0,0,1,0
12,0,10,0	1,0,0,0	1,0,0,0
0,9,0,15	0,0,0,1	0,0,0,1
```

Com essa etapa concluida, partimos para a execução da estimativa do alelo ancestral com o est-sfs. Para executar o programa será necessário criar um arquivo de configuração ```config.txt``` contendo as seguintes informações:

```linux
$ nano config.txt

n_outgroup 2
model 1
nrandom 5
```
<details>  
  <summary> Arquivo config.txt </summary>
  
Primeiro, vamos utilizar o software [est-sfs](https://nf-co.re/modules/estsfs). 




Para estimar o tamanho populacional atual, podemos utilizar o [currentNe](), [NeEstimatior]() e 
