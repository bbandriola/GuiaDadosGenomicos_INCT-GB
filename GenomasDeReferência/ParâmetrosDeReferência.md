# Genoma de Referência
Um genoma de referência é uma montagem genômica que atende a padrões específicos de qualidade. Esses, são utilizados como base para o mapeamento de sequências, anotação genômica e análises comparativas, auxiliando na compreensão de características biológicas dos organismos. Esses genomas devem ser incorporados a bancos de dados e servir como recurso confiável, atendendo a critérios mínimos de qualidade e completude, disponibilizando uma representação acurada do genoma de uma dada espécie. Por essa razão, diversos consórcios de genômica (e.g. [EBP](https://www.earthbiogenome.org/), [VGP](https://vertebrategenomesproject.org/), [B10K](https://b10k.com/), [10KP](https://db.cngb.org/10kp/), [AGC](https://mvs.unimelb.edu.au/research/partnerships/amphibian-genomics-consortium)) dedicados à produção de genomas de referência, estabeleceram padrões de qualidade que garantem a utilização dessas montagens a longo prazo. Esses padrões buscam assegurar que o genoma represente fielmente a sequência de DNA e a estrutura cromossômica da espécie-alvo, minimizando a presença de lacunas e erros de montagem.

Um genoma de referência pode ser construído a partir de diferentes tipos de dados de sequenciamento, mas algumas características gerais são esperadas:

- Minimização dos gaps entre sequências ao longo dos cromossômos. 
- Sequências longas com alta acurácia de base.
- Proporcionalidade na representação das sequências.

Para atingir esses quesitos é necessário a utilização de certas tecnologias de sequenciamento de sequências longas e quantidade de DNA adequado. As sequências longas são necessárias para a produção de um genoma *de novo*, realizar o sequenciamento do transcriptoma completo, resolver variações estruturais, entre outras aplicações. As sequências longas, geralmente, apresentam um tamanho >10kb, podendo ser de leitura única (ONT) ou circulares (PacBio). 

- **Tecnologia [PacBio](https://www.pacb.com/technology/hifi-sequencing/)**:

  PRÓS: produz sequências longas com uma alta qualidade de acurácia (do Inglês, high-fidelity -  HiFi). Taxa de erro <0,5%. Sequências de tamanho >10 kb.

  CONTRAS: os sequenciadores são sensíveis a qualidade do DNA de entrada (Revio) ou possuem um custo elevado (Onso).
  
- **Tecnologia da [Oxford Nanopore Technologies (ONT)](https://nanoporetech.com/)**:

  PRÓS: produz sequências longas e sequências ultralongas. As sequências longas possuem alta acurácia e uma taxa de erro <0,5%. As ONT ultralongas podem chegar a um comprimento >100kb. A tecnologia ONT duplex apresenta uma acurácia similar ao PacBio HiFi, com sequências mais longas.

  CONTRAS: possui limitações na utilização de programas para o processamento dos dados e um Q-score menor que as do PacBio. As sequências ultralongas possuem uma taxa de erro <10%, são mais custosas e requerem um volume alto de DNA.

Além das sequências longas, para genomas diploides <1Gb, reconstruir as sequências cromossômicas corretamente requer a utilização de técnicas de montagem de cromossomos (*Long-range data*), como Hi-C e Pore-C. Essa técnica detecta a interação física entre dois fragmentos de DNA quando associados, fornecendo informações sobre quais sequências estão localizadas perto uma da outra. 

Definida as tecnologias de sequenciamento a serem utilizadas, partimos para a montagem do genoma (no Inglês, *genome assembly*). Para uma montagem *de novo* apresentar alto desempenho, são reportados quatro cruciais passos:
1. Correção nos erros de acurácia das sequências longas.
2. Montagem do genoma com as sequências já curadas.
3. Simplificação do gráfico da montagem com as sequências ultralongas.
4. Faseamento e montagem dos cromossomos com dados de conformação de cromatina.

Para encontrar os comandos de montagem de genomas *de novo*, acesse o tutorial de [montagem de genomas *de novo*](./MontagemdeGenomas.md).

Ao gerar um genoma de referência, nosso objetivo é que os dados sejam de boa qualidade. Uma das métricas lançadas inicialmente pelo [VGP]() e, posteriormente, simplificada pelo [EBP](), define o padrão de qualidade das bases: 

### <p align="center"> 6.C.Q40 </p>

* 6: representa a **continuidade de Contig**. Outros possíveis valores representam:
  * C: NG50 = Chr N50.
  * 7: NG50 >10 Mpb.
  * 6: NG50 >1 Mpb.
  * 4: NG50 >10 kpb.
* C: representa a estrutura da montagem (scaffolding). Outros possíveis valores representam:
  * C: scaffolds em nível cromossômico.
  * 7: NG50 dos scaffolds >10 Mpb.
  * 5: NG50 dos scaffolds >100 kpb 
* Q40: representa a acurácia de base, determinada pelo valor de Phread que reflete a taxa de erro em X pb. Um Q40 significa uma taxa de erro de base inferior a 1 base em 10.000 nucleotídeos, ou precisão de 99,99%.   

Abaixo são citados demais parâmetros de qualidade com base na documentação do [EBP](https://static1.squarespace.com/static/5a5e6c9518b27d27bddaf20f/t/69a8a3cda0a3f86095cb01df/1772659661389/EBP-SequencingAssemblyStandard-V7.pdf) :technologist::

| Parâmetros | Representação de um genoma completo | Eucariotos com boa disponibilidade de DNA +  maior completude | Eucariotos com boa disponibilidade de DNA +  menor completude | Eucariotos com pouca disponibilidade de DNA | 
| :--- | :---: | :---: | :---: | :---: |
| Quantidade de DNA | - | >100 ng DNA por Gigabase | >100 ng DNA por Gigabase | <100 ng DNA por Gigabase | 
| Tipo de sequenciamento | Sequências longas/ultralongas + RNA-seq + Hi-C | Sequências longas/ultralongas + RNA-seq + Hi-C | Sequências longas + RNA-seq + Hi-C | Sequências longas + RNA-seq | 
| Padrão de qualidade | C.C.Q60 | min. 7.C.Q50 | min. 6.7.Q40 | min. 4.5.Q30 |
| NG50 Contigs** | = NG50 do cromossomo | >10 Mpb | >1 Mpb | >10 kpb | 
| NG50 Scaffold** | = NG50 do cromossomo | = NG50 do cromossomo | >10 Mpb | >100 kbp | 
| Gaps | Sem gaps | <200 | <1.000 | <10.000 |
| Qualidade da base | >60 | >50 | >40 | >30 | 
| Falsas duplicatas | 0% | <1% | <5% | <10% | 
| Curadoria | Todos conflitos resolvidos | Automatizada + Manual | Automatizada | Não necessária | 
| Completude de Kmers | 100% | >95% | >90% | >80% | 
| Faseamento (Fase do bloco do NG50, quando possível) | = NG50 do cromossomo | >1 Mpb | >100 kpb | Não requerido | 
| Presença de genes conservados de cópia única (*) | >98% | >95% | >90% | >80% | 
| Transcritos mapeáveis | 98% | >90% | >80% | >70% |  
| Designação cromossômica (Hi-C) | 98% | >90% | >80% | Não necessário |
| Cromossomo sexual | Ordem correta; sem gaps | Pares homologos localizados | Pelo menos um (e.g. X ou Z) | Fragmentado |
| Organelas | Um alelo completo | Um alelo completo | Fragmentada | Não necessário |

** Por que utilizar o NG50 e não o N50? N50 é a métrica que reflete o tamanho total da montagem do genoma, enquanto o NG50 é o tamanho esperado do genoma (estimado a partir de um genoma próximo ou citometria de fluxo, por exemplo).

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

### Diretrizes de citação de recursos do INCT-GB

Sempre que seus dados forem gerados com recursos do INCT-GB se faz necessário o reconhecimento do apoio cedido através da citação do número do processo CNPq em publicações e demais documentos. Você pode checar as diretrizes de citação [aqui](https://docs.google.com/document/d/18nuMD2QsBxuAgXP-oRV9r--0ewpXZdoSpFfdF2ey0xo/edit?tab=t.0).

## REFERÊNCIAS

Lawniczak, M. K. N., et al. 2022. Standards recommendations for the Earth BioGenome Project. [DOI:10.1073/pnas.2115639118](
https://doi.org/10.1073/pnas.2115639118). </p>
Park, S., et al. 2023. Benchmark study for evaluating the quality of reference genomes and gene annotations in 114 species. [DOI:10.3389/fvets.2023.1128570](https://doi.org/10/fvets.2023.1128570).</p>
Durbin, H., & Li, R. 2024. Genome assembly in the telomere-to-telomere era. [DOI:10.1038/s41576-024-00718-w](https://doi.org/10.1038/s41576-024-00718-w).</p>
Blaxter, M., et al. 2025. The Earth BioGenome Project Phase II: illuminating the eukaryotic tree of life. [DOI:10.3389/fsci.2025.1514835](https://doi.org/10.3389/fsci.2025.1514835).
