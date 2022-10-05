# hse22_hw1

## Описание файлов

- Ниже в README - исполняемые команды
- images - скриншоты статистики multiqc:
  - general_statistics.png, fastqc_per_sequence_quality_scores_plot.png - необрезанных чтений
  - general_statistics_trimmed.pngб fastqc_per_sequence_quality_scores_plot_trimmed.png - обрезанных
- data - .fa файлы и .ipynb файл с кодом
  - Poil_contig.fa - контиги
  - Poil_scaffold.fa - скаффолды
  - Poil_closed_gaps.fa - уменьшение гэпов
  - Contig_scaffold_analysis.ipynb - ноутбук с кодом
- reports - отчеты multiqc
  - multiqc_report - без обрезки
  - multiqc_report_trimmed - с обрезкой

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

### 4. Оценка чтений через multiQC
```
mkdir multiqc
multiqc -o multiqc fastqc
```

### 5. Обрезка чтений
```
platanus_trim sub1.fastq sub2.fastq
platanus_internal_trim sub_mp1.fastq sub_mp2.fastq
```

#### Удаляем исходные .fastq файлы
```
rm sub1.fastq sub2.fastq sub_mp1.fastq sub_mp2.fastq
```

### 6. Оценка обрезанных чтений через fastQC
```
mkdir fastqc_trimmed
ls sub* | xargs -tI{} fastqc -o fastqc_trimmed {}
```

### 7. Оценка обрезанных чтений через multiQC
```
mkdir multiqc_trimmed
multiqc -o multiqc_trimmed fastqc_trimmed
```

### 8. Сбор контигов
```
time platanus assemble -o Poil -f sub1.fastq.trimmed sub2.fastq.trimmed 2> platanus_assemble.log
```

### 9. Сбор скаффолдов
```
time platanus scaffold -o Poil -c Poil_contig.fa -IP1 sub1.fastq.trimmed sub2.fastq.trimmed -OP2 sub_mp1.fastq.int_trimmed sub_mp2.fastq.int_trimmed 2> scaffold.log
```

### 10. Уменьшение количества гэпов
```
time platanus gap_close -o Poil -c Poil_scaffold.fa -IP1 sub1.fastq.trimmed sub2.fastq.trimmed -OP2 sub_mp1.fastq.int_trimmed sub_mp2.fastq.int_trimmed 2> gapclose.log
```


## Отчеты
### Без обрезки
![](https://github.com/LanaShhh/hse22_hw1/blob/main/images/general_statistics.png)
![](https://github.com/LanaShhh/hse22_hw1/blob/main/images/fastqc_per_sequence_quality_scores_plot.png)

### С обрезкой
![](https://github.com/LanaShhh/hse22_hw1/blob/main/images/general_statistics_trimmed.png)
![](https://github.com/LanaShhh/hse22_hw1/blob/main/images/fastqc_per_sequence_quality_scores_plot_trimmed.png)
