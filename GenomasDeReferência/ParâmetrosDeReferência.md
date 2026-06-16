### ESTAMOS EM CONSTRUÇÃO... :technologist: 

# Genoma de Referência
Um genoma de referência é um genoma que atinger determinados padrões para ser utilizado como uma referência para mapear, base para anotação genômica e análises de comparação. O genoma de refere3ncia de uma espécie precisa atender a certos critêrios para ser incluído em bases de dados como genoma de referência, além de ser útil para trabalhos que desejam utiliza-lo. O objetivo na geração de um genoma de referência é que esse seja empregado em diversos trabalhos de genômica, podendo garantir refinamento suficiente em análises que exigem a existência de um genoma de referência para atingir seus objetivos. 

Por isso, diversos consórcios genômicos, que possuem como objetivo a geração de genomas de referência, padronizaram a qualidade para a geração de um genoma de referência que esteja alinhado com o uso desses a longo prazo, que representre de forma fiel as sequências de DNA e estrutura cromossômica da espécie alvo, minimazando ao máximo as lacunas dentro de cada cromossômo. 

Um genoma de referência pode ser construído a partir de diferentes conjuntos de dados. Abaixo são citados algumas características gerais de um genoma de referência: 

- Minimização dos gaps entre sequências ao longo dos cromossômos. 
- Sequências longas com um alto grau de cobertura.
- Tecnologias de ligação das sequências, como Hi-C 

Os parâmetros de padronização da qualidade de geração dos genomas de referência citados abaixo foram retirados do documento gerado pelo [EGP](https://static1.squarespace.com/static/5a5e6c9518b27d27bddaf20f/t/69a8a3cda0a3f86095cb01df/1772659661389/EBP-SequencingAssemblyStandard-V7.pdf).

Padrões de montagem :technologist:

| Parâmetros | Eucariotos com boa disponibilidade de DNA | Eucariotos com pouca disponibilidade de DNA | 
| --- | --- | --- |
| Quantidade de DNA | >100 ng DNA por Gigabase | <100 ng DNA por Gigabase | 
| Tipo de sequenciamento | Sequências longas + RNA-seq + Hi-C | Sequências longas + RNA-seq + Hi-C | 
| Padrão de qualidade | 6.C.Q40 | 4.5.Q30 |
| Contiguidade do genoma (NG50) | > 1 Mpb | >100kb | 
| Contiguidade dos cromossomos (NG50) | > 1 Mpb scaffolding |  | 
| Taxa de erro | 1/10.000 pb | 1/10.000 pb | 
| Falsas duplicatas | < 5% | < 5% | 
| Completude de Kmers | > 90% | > 90% | 
| Designação cromossômica (Hi-C) | > 90% | > 90% | 
| Presença de genes conservados de cópia única (*) | > 90% | > 90% | 
| Transcritos mapeáveis | > 90% | > 90% |  

* Mbp: milhão de pares de bases.

Quando possível, também é encorajado a produção de genomas de referência de acordo com os haplótipos de cada cromossomo, associação do número de cromossomos e cariótipo estimado (quando disponível), além da identificação dos genomas das organelas. Também é importante a identificação de cromossomos sexuais. 

### Nomenclatura cromossômica 

Caso já haja uma anotação e designação de orientação a uma determinada espécie, se indica a utilização dessa. Caso não haja, recomenda-se nomear os cromossomos por tamanho, levando em consideração *scaffolds* que foram reconhecidos como parte de um determinado cromossomo, mas não puderam ser localizados corretamente. Nesse caso, há uma indicação na análise de sintenia de que determinado *scaffold* pertence a determinado cromossomo, porém não foi possível determinar a posição exata. 

O método de escolha da nomeação deve ser reportada nos metadados da submissão.   

## Submissão do genoma de referência 

### Projeto guarda-chuva

~~Assim como em grandes consórcios, é desejado que todos os genomas produzidos como parte do INCT-GB estejam vinculados ao BioProject do instituto. Para vincular seu genoma de referência ao projeto guarda-chuva do INCT-GB, utilize o código XXXXXXXXX.~~ 

### Identificador da amostra

Ao submeter seu genoma de referência, um código de *Sample name* é solicitado. Recomenda-se o uso de identificadores do [Tree of Life ID](https://id.tol.sanger.ac.uk/). O formato desse código deve seguir a seguinte ordem: 

```<primeira letra do nível taxonômico superior e primeira letra do clado, ambas minúscula><uma letra maiúscula e duas minúsculas para o gênero><uma letra maiúscula e três minúsculas do epíteto específico>.<um número identificador do indivíduo>```

Para vertebrados, se recomenda a utilização de apenas uma das letras da taxonômia superior. 

Exemplo: Aves Scytalopus speluncae 1 -> aScySpel.1

## Referências
Lawniczak et al., 2022. Standards recommendations for the Earth BioGenome Project. [DOI:10.1073/pnas.2115639118](
https://doi.org/10.1073/pnas.2115639118).

Park et al., 2023. Benchmark study for evaluating the quality of reference genomes and gene annotations in 114 species. [DOI:10.3389/fvets.2023.1128570](https://doi.org/10.3389/fvets.2023.1128570).

[Base de dados RefSeq - NCBI]( https://www.ncbi.nlm.nih.gov/refseq/about/).

https://github.com/diegomics/GEP2
https://github.com/diegomics/GAME 

