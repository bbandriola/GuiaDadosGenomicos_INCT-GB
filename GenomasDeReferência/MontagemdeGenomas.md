### EM CONSTRUÇÃO... :technologist: 

# Montagem de genomas de novo 

A montagem de genomas *de novo* é aquela que não utiliza de um genoma previamente montado para guiar a organização das sequências provenientes da tecnologia de sequenciamento escolhida. Os programas a serem utilizados para a montagem dependem do tipo de tecnologia de sequenciamento. 

### Programas e suas aplicações

| Tipo de dado | Avaliação de qualidade | Pré-processamento | Mapeamento | Scaffolding | Avaliação da montagem |
|:---|:---:|:---:|:---:|:---:|:---:|
| PacBio HiFi | FastQC (HiFi), Nanoplot (HiFi) | CutAdapt | [hifiasm (HiFi)]() | [Bionano’s Solve]() | Merqury, gfastats, compleasm |
| PacBio HiFi + ONT ultralongas | FastQC (HiFi), Nanoplot (HiFi) | CutAdapt | [HiCanu](), [hifiasm (HiFi)](), [Verkko](), [LJA]() | [Bionano’s Solve]() | Merqury, gfastats, compleasm |
| PacBio HiFi + Hi-C | [FastQC (HiFi)](), [Nanoplot (HiFi)]() | CutAdapt | [YaHS]() | [Bionano’s Solve](), [SALSA2(Hi-C)](), [PretextView (Curadoria Manual, Hi-C)]() | Merqury, gfastats, compleasm |
| ONT + ONT ultralongas |  |  |  |  |  |
| ONT + Hi-C |  | | [YaHS]() |  |  |
| PacBio HiFi + ONT ultralongas + Hi-C |  | | [YaHS]() |  |  |

Para a montagem do genoma mitocondrial, sugere-se o uso do [MitoHifi pipeline](https://github.com/marcelauliano/MitoHiFi).

* Esses softwares são apenas sugestões. Sinta-se à vontade para usar o que for mais adequado para você. 
