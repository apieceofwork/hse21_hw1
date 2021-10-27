# hse21_hw1
Home assignment bioinformatics
#команды, используемые для домашнего задания (взяты из семинара 4)

создание символических ссылок в папке для файлов

ln -s /usr/share/data-minor-bioinf/assembly/oil_R1.fastq

ln -s /usr/share/data-minor-bioinf/assembly/oil_R2.fastq

ln -s /usr/share/data-minor-bioinf/assembly/oilMP_S4_L001_R1_001.fastq

ln -s /usr/share/data-minor-bioinf/assembly/oilMP_S4_L001_R2_001.fastq

отбор миллионов чтений (типа paired-end и 1.5 миллиона чтений типа mate-pairs)

seqtk sample -s3108 oil_R1.fastq 5000000 > sub1.fq

seqtk sample -s3108 oil_R2.fastq 5000000 > sub2.fq

seqtk sample -s3108 oilMP_S4_L001_R1_001.fastq 1500000 > sub3.fq

seqtk sample -s3108 oilMP_S4_L001_R2_001.fastq 1500000 > sub4.fq

работа с программами fastQC и multiQC

htop

mkdir fastqc

ls sub* | xargs -P 4 -tI{} fastqc -o fastqc {}

mkdir multifastqc 

multiqc -o multiqc fastqc

platanus_trim sub1.fq sub2.fq

platanus_internal_trim sub3.fq sub4.fq

rm oilMP_S4_L001_R1_001.fastq

rm oilMP_S4_L001_R2_001.fastq

rm oil_R2.fastq

rm oil_R1.fastq


mkdir fastqc2

ls *.fq.* | xargs -P 4 -tI{} fastqc -o fastqc2 {}

multiqc -o multiqc fastqc2

mkdir trimmed_fq

mv -v *.fq.* *trimmed trimmed_fq/


time platanus assemble -o Poil -t 1 -f trimmed_fq/sub1.fq.trimmed trimmed_fq/sub2.fq.trimmed


time platanus scaffold -o Poil -t 1 -c Poil_contig.fa -IP1 trimmed_fq/sub1.fq.trimmed trimmed_fq/sub2.fq.trimmed -OP2 trimmed_fq/sub3.fq.int_trimmed trimmed_fq/sub4.fq.int_trimmed 2> scaffold.log

time platanus gap_close -o Poil -t 1 -c Poil_scaffold.fa -IP1 trimmed_fq/sub1.fq.trimmed trimmed_fq/sub2.fq.trimmed -OP2 trimmed_fq/sub3.fq.int_trimmed trimmed_fq/sub4.fq.int_trimmed 2>gapclose.log
