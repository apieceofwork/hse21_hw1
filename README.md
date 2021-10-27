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

Скрины и статистика

![image](https://user-images.githubusercontent.com/57996343/139093837-bd731fee-50df-4b91-9137-fa06798aa9b7.png)

![image](https://user-images.githubusercontent.com/57996343/139094099-9686e2b9-a6b9-4f17-b9bd-216fcbf38344.png)

![image](https://user-images.githubusercontent.com/57996343/139094158-484cffa0-a5cb-49c9-a647-77cbf849eb21.png)

![image](https://user-images.githubusercontent.com/57996343/139094208-fb340371-11fd-489d-b9be-b91b55dcf35e.png)

![image](https://user-images.githubusercontent.com/57996343/139094249-56af3fd9-f62b-4b25-aa95-5a7a484cace0.png)

![image](https://user-images.githubusercontent.com/57996343/139094332-ca055c59-480a-44f7-9665-58d10d808ef5.png)

![image](https://user-images.githubusercontent.com/57996343/139094399-c1f706f9-108f-4fca-ba43-ee060d4a7e58.png)

![image](https://user-images.githubusercontent.com/57996343/139094472-49ca9f18-612b-4932-bebc-b7d7dd5cc02a.png)

![image](https://user-images.githubusercontent.com/57996343/139094559-b31b2479-df90-43b2-acd7-375305905ff9.png)

![image](https://user-images.githubusercontent.com/57996343/139094615-975fac6e-177e-429a-a7a7-8f944185e1ab.png)
