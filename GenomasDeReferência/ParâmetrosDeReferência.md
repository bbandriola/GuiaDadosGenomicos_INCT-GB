### ESTAMOS EM CONSTRUÇÃO... :technologist: 

# Genoma de Referência
Um genoma de referência é uma montagem genômica que atende a padrões específicos de qualidade. Esses, são utilizados como base para o mapeamento de sequências, anotação genômica e análises comparativas para auxiliar na compreensão de características biológicas dos organismos. Esses genomas devem ser incorporados a bancos de dados e servir como recurso confiável, atendendo a critérios mínimos de qualidade e completude, disponibilizando uma representação acurada do genoma de uma dada espécie. Por essa razão, diversos consórcios de genômica (e.g. [EBP](https://www.earthbiogenome.org/), [VGP](https://vertebrategenomesproject.org/), [B10K](https://b10k.com/), [10KP](https://db.cngb.org/10kp/), [AGC](https://mvs.unimelb.edu.au/research/partnerships/amphibian-genomics-consortium)) dedicados à produção de genomas de referência, estabeleceram padrões de qualidade que garantem a utilização dessas montagens a longo prazo. Esses padrões buscam assegurar que o genoma represente fielmente a sequência de DNA e a estrutura cromossômica da espécie-alvo, minimizando a presença de lacunas e erros de montagem.

Um genoma de referência pode ser construído a partir de diferentes tipos de dados de sequenciamento, mas algumas características gerais são esperadas:

- Minimização dos gaps entre sequências ao longo dos cromossômos. 
- Sequências longas com um alto grau de cobertura.
- Tecnologias de ligação das sequências, como Hi-C.

Para atingir esses quesitos é necessário a utilização de certas tecnologias de sequenciamento de sequências longas e quantidade de DNA adequado. As sequências longas são necessárias para a produção de um genoma *de novo*, realizar o sequenciamento do transcriptoma completo, resolver variações estruturais, entre outras aplicações. As determinadas sequências longas possuem tamanho maior que 200 pares de bases (pb) até milhões de pb. Elas podem ser de leitura única (ONT) ou circulares (PacBio). A tecnologia [PacBio](https://www.pacb.com/technology/hifi-sequencing/) produz sequências longas com uma alta qualidade de acurácia (do Inglês, high-fidelity -  HiFi). Contudo, alguns dos sequenciadores são sensíveis a qualidade do DNA de entrada (Revio) ou possuem um custo elevado (Onso). A tecnologia da [Oxford Nanopore Technologies (ONT)](https://nanoporetech.com/) produz sequências longas, tendo como uma das principais vantagens a produção de sequências ultra-longas. Contudo, possui limitações na utilização de progranas para o processamento dos dados e um Q-score menor que o do PacBio.  

Para gerar um genoma de referência de boa qualidade que possa ser útil em diversos estudos, citamos abaixo os parâmetros de padronização da qualidade com base na documentação do [EBP](https://static1.squarespace.com/static/5a5e6c9518b27d27bddaf20f/t/69a8a3cda0a3f86095cb01df/1772659661389/EBP-SequencingAssemblyStandard-V7.pdf):

Padrões de montagem :technologist:

| Parâmetros | Eucariotos com boa disponibilidade de DNA +  maior completude | Eucariotos com boa disponibilidade de DNA +  menor completude | Eucariotos com pouca disponibilidade de DNA | 
| :--- | :---: | :---: | :---: |
| Quantidade de DNA | >100 ng DNA por Gigabase | >100 ng DNA por Gigabase | <100 ng DNA por Gigabase | 
| Tipo de sequenciamento | Sequências longas + RNA-seq + Hi-C | Sequências longas + RNA-seq + Hi-C | Sequências longas + RNA-seq | 
| Padrão de qualidade | min. 7.C.Q50 | min. 6.7.Q40 | min. 4.5.Q30 |
| Contigs NG50 | >10 Mpb | >1 Mpb | >10 kpb | 
| Scaffold NG50 | = NG50 do cromossomo | >10 Mpb | >100 kbp | 
| Gaps | <200 | <1.000 | <10.000 |
| Qualidade da base | >50 | >40 | >30 |0 pb | 
| Falsas duplicatas | <1% | <5% | <10% | 
| Curadoria | Automatizada + Manual | Automatizada | Não necessária | 
| Completude de Kmers | >95% | >90% | >80% | 
| Faseamento (Fase do bloco do NG50) | >1 Mpb | >100 kpb | Não requerido | 
| Presença de genes conservados de cópia única (*) | >95% completo | >80% | >70% | 
| Transcritos mapeáveis | >90% | >80% | >70% |  
| Designação cromossômica (Hi-C) | >90% | >80% | Não necessário |
| Cromossomo sexual | Pares homologos localizados | Pelo menos um (e.g. X, Z, Y, ou W) | Fragmentado |
| Organelas | Um alelo completo | Fragmentada | Não necessário |

* Mbp: milhão de pares de bases.
* Por que utilizar o NG50 e não o N50? N50 é a métrica que reflete o tamanho total da montagem do genoma, enquanto o NG50 é o tamanho esperado do genoma (estimado a partir de um genoma próximo ou citometria de fluxo, por exemplo).

Quando possível, também é encorajado a produção de genomas de referência de acordo com os haplótipos de cada cromossomo, associação do número de cromossomos e cariótipo estimado (quando disponível), além da identificação dos genomas das organelas. Também é importante a identificação de cromossomos sexuais. 

### Nomenclatura cromossômica 

Caso já haja uma anotação e designação de orientação a uma determinada espécie, se indica a utilização dessa. Caso não haja, recomenda-se nomear os cromossomos por tamanho, levando em consideração *scaffolds* que foram reconhecidos como parte de um determinado cromossomo, mas não puderam ser localizados corretamente. Nesse caso, há uma indicação na análise de sintenia de que determinado *scaffold* pertence a determinado cromossomo, porém não foi possível determinar a posição exata. 

O método de escolha da nomeação deve ser reportada nos metadados da submissão.   

## Submissão do genoma de referência 

### Projeto guarda-chuva

Assim como em grandes consórcios, é desejado que todos os genomas produzidos como parte do INCT-GB estejam vinculados ao BioProject do instituto. Para vincular seu genoma de referência ao projeto guarda-chuva do INCT-GB, utilize o código XXXXXXXXX.

### Identificador da amostra

Ao submeter seu genoma de referência, um código de *Sample name* é solicitado. Recomenda-se o uso de identificadores do [Tree of Life ID](https://id.tol.sanger.ac.uk/). O formato desse código deve seguir a seguinte ordem: 

```<primeira letra do nível taxonômico superior e primeira letra do clado, ambas minúscula><uma letra maiúscula e duas minúsculas para o gênero><uma letra maiúscula e três minúsculas do epíteto específico>.<um número identificador do indivíduo>```

Para vertebrados, se recomenda a utilização de apenas uma das letras da taxonômia superior. 

Exemplo: Bird Scytalopus speluncae 1 -> bScySpel.1

## Referências
Lawniczak et al., 2022. Standards recommendations for the Earth BioGenome Project. [DOI:10.1073/pnas.2115639118](
https://doi.org/10.1073/pnas.2115639118).

Park et al., 2023. Benchmark study for evaluating the quality of reference genomes and gene annotations in 114 species. [DOI:10.3389/fvets.2023.1128570](https://doi.org/10.3389/fvets.2023.1128570).

[Base de dados RefSeq - NCBI]( https://www.ncbi.nlm.nih.gov/refseq/about/).

https://github.com/diegomics/GEP2
https://github.com/diegomics/GAME 

