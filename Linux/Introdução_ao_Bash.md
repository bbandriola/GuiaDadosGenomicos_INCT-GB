# Introdução a scripts e comandos em Bash
## Uso de loops 
```for```
```while```
```if```

### Exemplos: 
1. Concatenar o conteúdo de diversos arquivos, colocando o nome do arquivo antes do conteúdo, em apenas um:
```bash
for f in *.txt; do
    echo "===== $f =====" >> combined.txt
    cat "$f" >> combined.txt
done
```
O mesmo comando pode ser escrito em uma única linha:
```bash
for i in *fasta; do echo "===== $f =====" >> combined.txt | cat "$f" >> combinedfastas.fasta; done
```
