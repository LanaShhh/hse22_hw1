# hse22_hw1

## Исполняемые команды

### 1. Создание ссылок
```
ln -s /usr/share/data-minor-bioinf/assembly/oil_R1.fastq
ln -s /usr/share/data-minor-bioinf/assembly/oil_R2.fastq
ln -s /usr/share/data-minor-bioinf/assembly/oilMP_S4_L001_R1_001.fastq
ln -s /usr/share/data-minor-bioinf/assembly/oilMP_S4_L001_R2_001.fastq
```

### 2. Выбираем случайные чтения (seed 226)
```
seqtk sample -s226 oil_R1.fastq 5000000 > sub1.fastq
seqtk sample -s226 oil_R2.fastq 5000000 > sub2.fastq
seqtk sample -s226 oilMP_S4_L001_R1_001.fastq 1500000 > sub_mp1.fastq
seqtk sample -s226 oilMP_S4_L001_R2_001.fastq 1500000 > sub_mp2.fastq
```

### 3. Оценка чтений через fastQC
```
mkdir fastqc
ls sub* | xargs -tI{} fastqc -o fastqc {}
```

### 4. Оценка чтений через multiqc
```
mkdir multiqc
multiqc -o multiqc fastqc
```

### 5. Обрезка чтений
```
platanus_trim sub1.fastq sub2.fastq
platanus_internal_trim sub_mp1.fastq sub_mp2.fastq
```
