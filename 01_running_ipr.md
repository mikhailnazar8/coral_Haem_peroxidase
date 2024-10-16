Running interproscan
================
Mikhail Nazareth

To annotate the protein files from all the genomes (look at
Supplementary Table 1) with the domains present in each of the protein
sequences ̛

``` bash
 interproscan.sh -i ${species}_proteins.fasta  --disable-precalc -f tsv -goterms -cpu 12
```

after running the whole genomes(protein files) through interproscan, we
first create subset of all the proteins that have the Animal Haem
Peroxidase domain signature(PF03098) and their additional domain
information

``` bash
grep "PF03098" ${species}_interproscan.tsv | cat > ${species}_hp.tsv

cut -f1 ${species}_hp.tsv | sort | uniq > ${species}_prot_id.txt

sed -i 's/[[:space:]]//g' "${species}_prot_id.txt"

while read -r seq; do
  grep "$seq"  ${species}_interproscan.tsv | cat >> "${species}_ipr.tsv"
done < "${species}_prot_id.txt"
```

we also used a python script intergrated with SAMTOOLS to extract the
haem peroxidaase domains from the protein sequences (look at
ipr2fasta.py for more details)

``` bash
python3 ipr2fasta.py ${species}_interproscan.tsv ${species}_prot.fasta
```
