# getFast

在使用bedtools作为序列读取工具的时候，需要注意工具是否根据了strand来读取。因为正反链是具有差异的。

https://bedtools.readthedocs.io/en/latest/content/tools/getfasta.html

![50](https://github.com/JoneSu1/getFast/assets/103999272/a83adbc8-36b7-492c-b496-832e2b615606)

```
$ cat test.fa
>chr1
AAAAAAAACCCCCCCCCCCCCGCTACTGGGGGGGGGGGGGGGGGG

$ cat test.bed
chr1 20 25 forward 1 +
chr1 20 25 reverse 1 -

$ bedtools getfasta -fi test.fa -bed test.bed -s -name
>forward
CGCTA
>reverse
TAGCG
```

下面是如何调用bedtools getfasta去获得序列的代码
```
$ cat genes.bed12
chr1  164404  173864  ENST00000466557.1       0       -       173864  173864  0       6       387,59,66,216,132,112,  0,1479,3695,4644,8152,9348,
chr1  235855  267253  ENST00000424587.1       0       -       267253  267253  0       4       2100,150,105,158,       0,2562,23161,31240,
chr1  317810  328455  ENST00000426316.1       0       +       328455  328455  0       2       323,145,        0,10500,

$ bedtools getfasta -fi chr1.fa -bed genes.bed12 -split -name
>ENST00000466557.1
gaggcgggaagatcacttgatatcaggagtcgaggcgggaagatcacttgacgtcaggagttcgagactggcccggccaacatggtgaaaccgcatctccactaaaaatacaaaaattagcctggtatggtggtgggcacctgtaatcccagtgacttgggaggctaaggcaggagaatttcttgaacccaggaggcagaggttgcagtgaccagcaaggttgcgccattgcaccccagcctgggcgataagagtgaaaactccatctcaaaaaaaaaaaaaaaaaaaaaaTTCCTTTGGGAAGGCCTTCTACATAAAAATCTTCAACATGAGACTGGAAAAAAGGGTATGGGATCATCACCGGACCTTTGGCTTTTACAGCTCGAGCTGACAAAGTTGATTTATCAAGTTGTAAATCTTCACCTGTTGAATTCATAAGTTCATGTCATATTTTCTTTCAGACAATTCTTCAGTTTGTTTACGTAGATCAGCGATACGATGATTCCATTTCTtcggatccttgtaagagcagagcaggtgatggagagggtgggaggtgtagtgacagaagcaggaaactccagtcattcgagacgggcagcacaagctgcggagtgcaggccacctctacggccaggaaacggattctcccgcagagcctcggaagctaccgaccctgctcccaccttgactcagtaggacttactgtagaattctggccttcagacCTGAGCCTGGCAGCTCTCTCCAACTTTGGAAGCCCAGGGGCATGGCCCCTGTCCACAGATGCACCTGGCATGAGGCGTGCCCAGAGGGACAGAGGCAGATGAGTttcgtctcctccactggattgtgagggcCAGAGTTGAACTCCCTCATTTTCCGTTCCCCAGCATTGGCAGGTTCTGGGACTGGTGGCTGTGGTGGCTCGTTGGTCTTTGTCTCTTAGAAGGTGGGGAATAATCATCATCT
>ENST00000424587.1
ccaggaagtgaaaatgacactttactgttttaatttgcatttctctgcttacaagtggattacacacattttcgtgtgctgttggctacttatTCATTCAGAAAACATACTAAGTGCTGGCTCTTTTTCATGTCCTTTATCAAGTTTGGATCATGTCATTTGCTATTTTCTTTCTGATGTAAACTCTCAAAGTCTGAAGTGTATTGTCTTTTCCTGACACATATGTTGTAAATAATTTTCTGGCTTACATTTTGACTTTTAATTTCATTCACGATGTTTTTAATGAATAATTTTAATTTTTATGAATGCAAGTTAAAATAATTCTTTCATTGTGGTCTCTGACATGTCATGCCAATAAGGGTCTTCTCCTCCAAGAGCACAGAAATATTTGCCAATACTGTCCTTAAAATCGGTCACAGTTTCATTTTTTATATATGCATTTTACTTCAATTGGGGCTTCATTTTACTGAATGCCCTATTTGAAGCAAGTTTCTCAGTTAATTCTTTTCTCAAAGGGCTAAGTATGGTAGATTGCAAACATAAGTGGCCACATAATGCTCTCACCTCctttgcctcctctcccaggaggagatagcgtccatctttccactccttaatctgggcttggccgtgtgacttgcactggccaatgggatattaacaagtctgatgtgcacagaggctgtagaatgtgcacgggggcttggtctctcttgctgccctggagaccagctgccCCACGAAGGAACCAGAGCCAACCTGCTGCTTCCTGGAGGAAGACAGTCCCTCTGTCCCTCTGTCTCTGCCAACCAGTTAACCTGCTGCTTCCTGGAGGGAGACAGTCCCTCAGTCCCTCTGTCTCTGCCAACCAGTTAACCTGCTGCTTCCTGGAGGAAGACAGTCACTCTGTCTCTGccaacccagttgaccgcagacatgcaggtctgctcaggtaagaccagcacagtccctgccctgtgagccaaaccaaatggtccagccacagaatcgtgagcaaataagtgatgcttaagtcactaagatttgggCAAAAGCTGAGCATTTATCCCAATCCCAATACTGTTTGTCCTTCTGTTTATCTGTCTGTCCTTCCCTGCTCATTTAAAATGCCCCCACTGCATCTAGTACATTTTTATAGGATCAGGGATCTGCTCTTGGATTAATGTTGTGTTCCCACCTCGAGGCAGCTTTGTAAGCTTCTGAGCACTTCCCAATTCCGGGTGACTTCAGGCACTGGGAGGCCTGTGCATCAGCTGCTGCTGTCTGTAGCTGACTTCCTTCACCCCTCTGCTGTCCTCAGCTCCTTCACCCCTGGGCCTCAGGAAATCAATGTCATGCTGACATCACTCTAGATCTAAAAGTTGGGTTCTTGgaccaggcgtggtggctcacacctgtaatcccagcactttgggaggccgaggcgggtggatcacaaggtcaggagatcaagacgattctggctaacacggtgaaaccccgtctctactaaaaatacaaaaaaattagccgggtgtggtggcaggtgcctgtagccccagctacttgggaggctgaggcaggagaatggcttgaacctgggaggtggagcttgcagtgagccaagatcacgccactgcactccagaatgggagagagagcgagactttctcaaaaaaaaaaaaaaaaCTTAGGTTCTTGGATGTTCGGGAAAGGGGGTTATTATCTAGGATCCTTGAAGCACCCCCAAGGGCATCTTCTCAAAGTTGGATGTGTGCATTTTCCTGAGAGGAAAGCTTTCCCACATTATACAGCTTCTGAAAGGGTTGCTTGACCCACAGATGTGAAGCTGAGGCTGAAGGAGACTGATGTGGTTTCTCCTCAGTTTCTCTGTGCAGCACCAGGTGGCAGCAGAGGTCAGCAAGGCAAACCCGAGCCCGGGGATGCGGAGTGGGGGCAGCTACGTCCTCTCTTGAGCTACAGCAGATTCACTCTGTTCTGTTTCATTGTTGTTTAGTTTGCGTTGTGTTTCTCCAACTTTGTGCCTCATCAGGAAAAGCTTTGGATCACAATTCCCAGtgctgaagaaaaggccaaactcttggttgtgttctttgattAGTgcctgtgacgcagcttcaggaggtcctgagaacgtgtgcacagtttagtcggcagaaacttagggaaatgtaagaccaccatcagcacataggagttctgcattggtttggtctgcattggtttggtCTTTTCCTGGATACAGGTCTTGATAGGTCTCTTGATGTCATTTCACTTCAGATTCTTCTTTAGAAAACTTGGACAATAGCATTTGCTGTCTTGTCCAAATTGTTACTTCAAGTTTGCTCTTAGCAAGTAATTGTTTCAGTATCTATATCAAAAATGGCTTAAGCCTGCAACATGTTTCTGAATGATTAACAAGGTGATAGTCAGTTCTTCATTGAATCCTGGATGCTTTATTTTTCTTAATAAGAGGAATTCATATGGATCAG
>ENST00000426316.1
AATGATCAAATTATGTTTCCCATGCATCAGGTGCAATGGGAAGCTCTTctggagagtgagagaagcttccagttaaggtgacattgaagccaagtcctgaaagatgaggaagagttgtatgagagtggggagggaagggggaggtggagggaTGGGGAATGGGCCGGGATGGGATAGCGCAAACTGCCCGGGAAGGGAAACCAGCACTGTACAGACCTGAACAACGAAGATGGCATATTTTGTTCAGGGAATGGTGAATTAAGTGTGGCAGGAATGCTTTGTAGACACAGTAATTTGCTTGTATGGAATTTTGCCTGAGAGACCTCATTCCTCACGTCGGCCATTCCAGGCCCCGTTTTTCCCTTCCGGCAGCCTCTTGGCCTCTAATTTGTTTATCTTTTGTGTATAAATCCCAAAATATTGAATTTTGGAATATTTCCACCATTATGTAAATATTTTGATAGGTAA

# use the UNIX fold command to wrap the FASTA sequence such that each line
```
如何批量使用
```
bedtools getfasta -fi /path/to/reference_genome.fasta -bed /path/to/your_gene_positions.bed -fo /path/to/output_sequences.fasta -s
```
-fi后面跟的是参考基因组的路径。

-bed后面跟的是你的BED文件的路径。

-fo后面跟的是输出文件的路径，提取的序列会保存在这个文件中。

-s是一个重要的选项，表示要根据strand信息提取序列。如果你的基因或特征位于负链上，它会自动提取互补序列


**In the qualitative reseach method to get sequcence data by bedtools**

```
bedtools getfasta -fi /home/junhua/Documents/deepEpromote/DeepEpromoter/ref_genome/hg38.fa -bed /home/junhua/Documents/deepEpromote/DeepEpromoter/Epromoter_list/EpromoterVScontrol_500BP.bed -fo /home/junhua/Documents/deepEpromote/DeepEpromoter/Epromoter_list/bedtools/output_sequences.fasta -s
```

```
The output:

>chr1:68555-69055()
CACTTTTGAATTCCCTATTCTTTTATCCTCTGTTAATTTTTAAGTATTATATTTGTGATATTATTTTTTCTTTTTTTCTATTTTTTATCTTTCATTTCATTTTGGCCTATTTTTTTCTCTTAAGAACTTTAATATCACCAAATAACATGTGTGCTACAAACTGTTTTGTAGTTCAAAGAAAAAGGAGATAAACATAGAGTTATGGCATAGACTTAATCTGGCAGAGAGACAAGCATAAATAATGGTATTTTATATTAGGAATAAACCTAACATTAATGGAGACACTGAGAAGCCGAGATAACTGAATTATAAGGCATAGCCAGGGAAGTAGTGCGAGATAGAATTATGATCTTGTTGAATTCTGAATGTCTTTAAGTAATAGATTATAGAAAGTCACTGTAAGAGTGAGCAGAATGATATAAAATGAGGCTTTGAATTTGAATATAATAATTCTGACTTCCTTCTCCTTCTCTTCTTCAAGGTAACTGCAGAGGCTATTT
>chr1:925231-925731()
ACGGGGACTCGAGAGAGCGGGCAGGAGGCGGGTTGGGAGGGCGCGGAGCCCCGGGTTCGGGGGAGACTGGAGGGGCGCACGTGCGGCCGGGTGCGAGCGCGCGGCGGGGGAGGCTGCGGGGCGGCGCGGGGGCGCGCGCGGAGCCCGAGCGGCGGCGCCAGGTCACACAACCTGTTTTGGCGCCTGCGGGCGCCTGGGCCCAAGGGTGCGACGCGGGGGCGCCTGAGCCGGGACACAGGGGGTGCGGTGAGCGCCAGGCGCCGCGGGGAGTTAAAAAGTTCGGGACCTGAGCGGTGCGTGGTTCCGCGGTGGCCGCCTCTTCCTGCCGCGCAGGCCGAGGGTCCCGACGGCGCCGCTCACCGCTCCGGGACTCAGCCTTTCTGGGCCCGGCCTGCGGTTCCCTCGGGGCCGGGGAGAGGGTGGAGCGCGGGAGGAGGGGCGCCGGGTGGGGACGCCCAGGCCCTTCGTCGGGGGAGGGCGCTCCACCCGGGCTGGAGTTG
>chr1:938775-939275()
```

**If we want to remain another lable in data**
We need use python to annotate the output_sequence file.
```
# Paths to your BED and FASTA files
bed_file_path = '/home/junhua/Documents/deepEpromote/DeepEpromoter/Epromoter_list/EpromoterVScontrol_500BP.bed'
output_fasta_path = '/home/junhua/Documents/deepEpromote/DeepEpromoter/Epromoter_list/bedtools/annotated_sequences.fasta'
# Dictionary to store annotations, with the key being a tuple of (chr, start, end)
annotations = {}
with open(bed_file_path, 'r') as bed_file:
    for line in bed_file:
        parts = line.strip().split()
        # Assuming the 1st, 2nd, and 3rd columns are chr, start, and end, and the 4th is strand
        key = (parts[0], parts[1], parts[2])
        # Store additional annotations, here assuming everything from the 4th column onwards
        annotations[key] = parts[3:]

# Reading the FASTA file and appending annotations
with open(fasta_file_path, 'r') as fasta_file, open('/home/junhua/Documents/deepEpromote/DeepEpromoter/Epromoter_list/bedtools/annotated_sequences.fasta', 'w') as out_file:
    for line in fasta_file:
        if line.startswith('>'):
            # Extracting chr, start, and end from the FASTA header
            header_parts = line[1:].strip().split(':')
            chr_start_end = header_parts[1].split('-')
            chr = header_parts[0]
            start = chr_start_end[0]
            end = chr_start_end[1].split('(')[0]

            key = (chr, start, end)
            annotation = annotations.get(key, ["Unknown strand", "No annotation"])
            # Correcting the output format to include the strand in the header
            out_line = f'>{chr}:{start}-{end}({annotation[0]}) {annotation[1]}\n'
        else:
            out_line = line
        out_file.write(out_line)

print("Annotated sequences have been saved.")


```

**In the next step I want to compare the new output_sequence data with old sequence data in python**

