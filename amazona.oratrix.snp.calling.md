#### Reference genome downloaded from [ncbi](https://ftp.ncbi.nlm.nih.gov/genomes/all/GCA/039/720/435/GCA_039720435.1_bAmaOch1.hap1/GCA_039720435.1_bAmaOch1.hap1_genomic.fna.gz)
#### Amazona ochrocephala (yellow-crowned parrot) assemblies - details [here](https://www.ncbi.nlm.nih.gov/datasets/genome/GCA_039720435.1/)

#### Download the reference genome directly from ncbi
- Rename to `ycpa.ref.fna.gz`
```
wget https://ftp.ncbi.nlm.nih.gov/genomes/all/GCA/039/720/435/GCA_039720435.1_bAmaOch1.hap1/GCA_039720435.1_bAmaOch1.hap1_genomic.fna.gz
mv GCA_039720435.1_bAmaOch1.hap1_genomic.fna.gz ycpa.ref.fna.gz
```

### Activate conda environment
```
source activate uce_env
```

### Indexing the reference genome
- `index.refgenome.sh`
```
#!/bin/sh
#
#SBATCH --job-name=index.refgenome              # Job Name
#SBATCH --partition=mlz-unlimited         # Name of the Slurm partition used
#SBATCH --nodelist=n029

#gunzip reference genome
gunzip ycpa.ref.fna.gz

#index for bwa
bwa index ycpa.ref.fna

#index for picard (v2.18.29)
picard CreateSequenceDictionary R=ycpa.ref.fna O=ycpa.ref.fna.dict

#index for samtools
samtools faidx ycpa.ref.fna
```

### Run fastp to trim sequence data
#### Create output folder
```
mkdir fastp.out
```

### Renaming raw read names
#### Modern RAPiD genomics samples
- Changing sequence names to desired sequence names from `/oratrix_nov2024/new_samples/OCO_184801_SampleSheet.csv`
### Read 1
```
mv combined-RAPiD-Genomics_F432_OCO_184801_P005_WA01_i5-505_i7-59_L001-4_R1_001.fastq.gz ao_MLZ_70063_R1.fastq.gz
mv combined-RAPiD-Genomics_F432_OCO_184801_P005_WA02_i5-505_i7-27_L001-4_R1_001.fastq.gz ao_MLZ_70074_R1.fastq.gz
mv combined-RAPiD-Genomics_F432_OCO_184801_P005_WA03_i5-505_i7-82_L001-4_R1_001.fastq.gz ao_MLZ_1105_R1.fastq.gz
mv combined-RAPiD-Genomics_F432_OCO_184801_P006_WA01_i5-534_i7-97_L001-4_R1_001.fastq.gz ao_UMMZ_103984_R1.fastq.gz
mv combined-RAPiD-Genomics_F432_OCO_184801_P006_WA02_i5-534_i7-98_L001-4_R1_001.fastq.gz ao_UMMZ_95618_R1.fastq.gz
mv combined-RAPiD-Genomics_F432_OCO_184801_P006_WA03_i5-534_i7-99_L001-4_R1_001.fastq.gz ao_UMMZ_95619_R1.fastq.gz
mv combined-RAPiD-Genomics_F432_OCO_184801_P007_WA01_i5-534_i7-59_L001-4_R1_001.fastq.gz ao_ANSP_90568_R1.fastq.gz
mv combined-RAPiD-Genomics_F432_OCO_184801_P007_WA02_i5-534_i7-27_L001-4_R1_001.fastq.gz ao_LSUMZ_23890_R1.fastq.gz
mv combined-RAPiD-Genomics_F432_OCO_184801_P007_WA03_i5-534_i7-82_L001-4_R1_001.fastq.gz ao_LSUMZ_33050_R1.fastq.gz
mv combined-RAPiD-Genomics_F432_OCO_184801_P007_WA04_i5-534_i7-7_L001-4_R1_001.fastq.gz ao_LSUMZ_39731_R1.fastq.gz
mv combined-RAPiD-Genomics_F432_OCO_184801_P007_WA05_i5-534_i7-38_L001-4_R1_001.fastq.gz ao_LSUMZ_43831_R1.fastq.gz
mv combined-RAPiD-Genomics_F432_OCO_184801_P007_WA06_i5-534_i7-74_L001-4_R1_001.fastq.gz ao_LSUMZ_43832_R1.fastq.gz
mv combined-RAPiD-Genomics_F432_OCO_184801_P007_WA07_i5-534_i7-77_L001-4_R1_001.fastq.gz ao_UMMZ_130517_R1.fastq.gz
```

### Read 2
```
mv combined-RAPiD-Genomics_F432_OCO_184801_P005_WA01_i5-505_i7-59_L001-4_R2_001.fastq.gz ao_MLZ_70063_R2.fastq.gz
mv combined-RAPiD-Genomics_F432_OCO_184801_P005_WA02_i5-505_i7-27_L001-4_R2_001.fastq.gz ao_MLZ_70074_R2.fastq.gz
mv combined-RAPiD-Genomics_F432_OCO_184801_P005_WA03_i5-505_i7-82_L001-4_R2_001.fastq.gz ao_MLZ_1105_R2.fastq.gz
mv combined-RAPiD-Genomics_F432_OCO_184801_P006_WA01_i5-534_i7-97_L001-4_R2_001.fastq.gz ao_UMMZ_103984_R2.fastq.gz
mv combined-RAPiD-Genomics_F432_OCO_184801_P006_WA02_i5-534_i7-98_L001-4_R2_001.fastq.gz ao_UMMZ_95618_R2.fastq.gz
mv combined-RAPiD-Genomics_F432_OCO_184801_P006_WA03_i5-534_i7-99_L001-4_R2_001.fastq.gz ao_UMMZ_95619_R2.fastq.gz
mv combined-RAPiD-Genomics_F432_OCO_184801_P007_WA01_i5-534_i7-59_L001-4_R2_001.fastq.gz ao_ANSP_90568_R2.fastq.gz
mv combined-RAPiD-Genomics_F432_OCO_184801_P007_WA02_i5-534_i7-27_L001-4_R2_001.fastq.gz ao_LSUMZ_23890_R2.fastq.gz
mv combined-RAPiD-Genomics_F432_OCO_184801_P007_WA03_i5-534_i7-82_L001-4_R2_001.fastq.gz ao_LSUMZ_33050_R2.fastq.gz
mv combined-RAPiD-Genomics_F432_OCO_184801_P007_WA04_i5-534_i7-7_L001-4_R2_001.fastq.gz ao_LSUMZ_39731_R2.fastq.gz
mv combined-RAPiD-Genomics_F432_OCO_184801_P007_WA05_i5-534_i7-38_L001-4_R2_001.fastq.gz ao_LSUMZ_43831_R2.fastq.gz
mv combined-RAPiD-Genomics_F432_OCO_184801_P007_WA06_i5-534_i7-74_L001-4_R2_001.fastq.gz ao_LSUMZ_43832_R2.fastq.gz
mv combined-RAPiD-Genomics_F432_OCO_184801_P007_WA07_i5-534_i7-77_L001-4_R2_001.fastq.gz ao_UMMZ_130517_R2.fastq.gz
```

### Trimming the sequence data
- `fastp.sh`
```
#!/bin/sh
#
#SBATCH --job-name=fastp              # Job Name
#SBATCH --partition=mlz-unlimited         # Name of the Slurm partition used
#SBATCH --nodelist=n029

#-f 5 #trim front 5 #currently removed
#-t 5 #trim tail 5 #currently removed
#-q 20 #phred score 20 drop 
#-u 50 #unqualified 50% or over drop
#-h #output html file
#-c #perform overlap correction

#define files variable
files="ao_MLZ_70063
ao_MLZ_70074
ao_MLZ_1105
ao_UMMZ_103984
ao_UMMZ_95618
ao_UMMZ_95619
ao_ANSP_90568
ao_LSUMZ_23890
ao_LSUMZ_33050
ao_LSUMZ_39731
ao_LSUMZ_43831
ao_LSUMZ_43832
ao_UMMZ_130517"

#loop the paired end samples:
for sample in $files
do
	fastp -i /home/ramirezb/amazona_oratrix/yhpa_may2024_samples/renamed_raw_reads/${sample}_R1.fastq.gz -I /home/ramirezb/amazona_oratrix/yhpa_may2024_samples/renamed_raw_reads/${sample}_R2.fastq.gz -o fastp.out/${sample}-READ1.fastq.gz -O fastp.out/${sample}-READ2.fastq.gz --unpaired1 fastp.out/${sample}-READ-singleton.fastq.gz --unpaired2 fastp.out/${sample}-READ-singleton.fastq.gz -f 5 --cut_right --cut_right_window_size 5 --cut_right_mean_quality 30 -c -h fastp.out/${sample}.html
done
```

### Mapping samples
#### Create a folder to hold the mapped samples
```
mkdir mapped
```

- `map.template.sh`
#### This file does not need to be submitted, just created in Bletchley as a reference
```
#!/bin/sh
#
#SBATCH --job-name=amazona.bwa               # Job Name
#SBATCH --partition=medium         # Name of the Slurm partition used
#SBATCH --nodelist=n017

#map a given sample (paired-end) against the reference genome using the bwa (v0.7.17) 'mem' command and write the results to an output sam file.
bwa mem -t 2 ycpa.ref.fna fastp.out/INDIV-READ1.fastq.gz fastp.out/INDIV-READ2.fastq.gz > mapped/INDIV.sam
#convert sam to bam using samtools (v1.9)
samtools view --threads 2 -bS mapped/INDIV.sam > mapped/INDIV.bam
#sort the bam
samtools sort --threads 2 mapped/INDIV.bam -o mapped/INDIV.sorted.bam

#remove large extraneous intermediary files
#rm mapped/INDIV.sam
#rm mapped/INDIV.bam

#Run qualimap (v2.2.2a)
qualimap bamqc -nt 2 -bam mapped/INDIV.sorted.bam -outfile mapped/INDIV.sorted.pdf

#Add read groups, mark duplicates, and fix mates, all using picard (v2.18.29)
picard AddOrReplaceReadGroups INPUT=mapped/INDIV.sorted.bam OUTPUT=mapped/INDIV.sorted.rg.bam RGID=1 RGLB=kibrary1 RGPL=illumina RGPU=R1 RGSM=INDIV.sorted
picard MarkDuplicates INPUT=mapped/INDIV.sorted.rg.bam OUTPUT=mapped/INDIV.sorted.mark.bam METRICS_FILE=mapped/INDIV.sorted.metrics.txt MAX_FILE_HANDLES_FOR_READ_ENDS_MAP=1000

#remove large, extraneous intermediary files
#rm mapped/INDIV.sorted.bam
#rm mapped/INDIV.sorted.rg.bam
```

- Submit directly into terminal:
```
#define the variable 'array'
array=(ao_MLZ_70063
ao_MLZ_70074
ao_MLZ_1105
ao_UMMZ_103984
ao_UMMZ_95618
ao_UMMZ_95619
ao_ANSP_90568
ao_LSUMZ_23890
ao_LSUMZ_33050
ao_LSUMZ_39731
ao_LSUMZ_43831
ao_LSUMZ_43832
ao_UMMZ_130517)

#this code produces a job file for each sample 
for i in "${array[@]}"; do awk '{gsub("INDIV","'$i'",$0); print($0);}' map.template.sh > "$i".map.sh; done

#this code submits each file as a unique job
for i in "${array[@]}"; do sbatch "$i".map.sh; done
```

### Variant refinement
- `variant.refinement.template.sh`
#### This file does not need to be submitted, just created in Bletchley as a reference
```
#!/bin/sh
#
#SBATCH --job-name=INDIV.var.refine              # Job Name
#SBATCH --partition=short         # Name of the Slurm partition used
#SBATCH --nodelist=n[023-024]

######################
#### Variant Refinement ###
######################

##may need to add -Xmx flag (e.g java -Xmx48g -jar) if you run out of memory on the cluster during these steps
##this code points to the executable for picard v.2.18.29

### Index bam, identify problematic regions, realign, fix mates
samtools index mapped/INDIV.sorted.mark.bam
java -jar /home/ramirezb/GenomeAnalysisTK-3.8-1-0-gf15c1c3ef/GenomeAnalysisTK.jar -T RealignerTargetCreator -R ycpa.ref.fna -I mapped/INDIV.sorted.mark.bam -o mapped/INDIV.sorted.realign.intervals

#run IndelRealigner
java -jar -Xmx10g /home/ramirezb/GenomeAnalysisTK-3.8-1-0-gf15c1c3ef/GenomeAnalysisTK.jar -T IndelRealigner -R ycpa.ref.fna -targetIntervals mapped/INDIV.sorted.realign.intervals -I mapped/INDIV.sorted.mark.bam -o mapped/INDIV.sorted.marked.realigned.bam
picard FixMateInformation INPUT=mapped/INDIV.sorted.marked.realigned.bam OUTPUT=mapped/INDIV.sorted.marked.realigned.fixmate.bam SO=coordinate CREATE_INDEX=true

#remove extraneous files
#rm mapped/INDIV.sorted.mark.bam
#rm mapped/INDIV.sorted.realign.intervals
#rm mapped/INDIV.sorted.marked.realigned.bam
```

#### After the previous set of jobs finishes, we are going to submit another set of batch jobs for variant refinement

- Submit directly into terminal:
```
#define array variable
array=(ao_MLZ_70063
ao_MLZ_70074
ao_MLZ_1105
ao_UMMZ_103984
ao_UMMZ_95618
ao_UMMZ_95619
ao_ANSP_90568
ao_LSUMZ_23890
ao_LSUMZ_33050
ao_LSUMZ_39731
ao_LSUMZ_43831
ao_LSUMZ_43832
ao_UMMZ_130517)

#this code produces a job file for each sample
for i in "${array[@]}"; do awk '{gsub("INDIV","'$i'",$0); print($0);}' variant.refinement.template.sh > var.refine."$i".sh; done

#this code submits files, 
for i in "${array[@]}"; do sbatch var.refine."$i".sh; done
```


#### Combining final files from previous run (updated_snp_calling) with this one
```
cp /home/ramirezb/amazona_oratrix/updated_snp_calling/mapped/*.fixmate.bam /home/ramirezb/amazona_oratrix/yhpa_may2024_samples/all_mapped
cp /home/ramirezb/amazona_oratrix/updated_snp_calling/mapped/*.fixmate.bai /home/ramirezb/amazona_oratrix/yhpa_may2024_samples/all_mapped

cp /home/ramirezb/amazona_oratrix/yhpa_may2024_samples/mapped/*.fixmate.bam /home/ramirezb/amazona_oratrix/yhpa_may2024_samples/all_mapped
cp /home/ramirezb/amazona_oratrix/yhpa_may2024_samples/mapped/*.fixmate.bai /home/ramirezb/amazona_oratrix/yhpa_may2024_samples/all_mapped
```

### Variant calling
- run directly in terminal:
```
samtools quickcheck -v all_mapped/*sorted.marked.realigned.fixmate.bam > bad_bams.fofn   && echo 'all ok' || echo 'some files failed check, see bad_bams.fofn'

ls -lh mapped/*sorted.marked.realigned.fixmate.bam
```

#### Error when ran with -Xmx10g: ERROR MESSAGE: An error occurred because you did not provide enough memory to run this program. You can use the -Xmx argument (before the -jar argument) to adjust the maximum heap size provided to Java. Note that this is a JVM argument, not a GATK argument.
- `snp.calling.sh`
```
#!/bin/sh
#
#SBATCH --job-name=amazona.snp.calling              # Job Name
#SBATCH --partition=mlz-unlimited         # Name of the Slurm partition used
#SBATCH --ntasks-per-node=5
#SBATCH --nodelist=n029

# check bam files are all okay
#samtools quickcheck -v *sorted.marked.realigned.fixmate.bam > bad_bams.fofn   && echo 'all ok' || echo 'some files failed check, see bad_bams.fofn'

###############################################
### VARIANT CALLING: GATK UNIFIED GENOTYPER ###
###############################################

#emit snps only vcf for downstream filtering and popgen analyses
java -jar -Xmx12g /home/ramirezb/GenomeAnalysisTK-3.8-1-0-gf15c1c3ef/GenomeAnalysisTK.jar -T UnifiedGenotyper -R ycpa.ref.fna --num_threads 4 \
-I all_mapped/ao_BC_107.sorted.marked.realigned.fixmate.bam \
-I all_mapped/ao_BC_108.sorted.marked.realigned.fixmate.bam \
-I all_mapped/ao_BC_109.sorted.marked.realigned.fixmate.bam \
-I all_mapped/ao_BC_A112.sorted.marked.realigned.fixmate.bam \
-I all_mapped/ao_BC_A113.sorted.marked.realigned.fixmate.bam \
-I all_mapped/ao_BC_A114.sorted.marked.realigned.fixmate.bam \
-I all_mapped/ao_BC_A115.sorted.marked.realigned.fixmate.bam \
-I all_mapped/ao_BC_A116.sorted.marked.realigned.fixmate.bam \
-I all_mapped/ao_BC_A117.sorted.marked.realigned.fixmate.bam \
-I all_mapped/ao_BC_A118.sorted.marked.realigned.fixmate.bam \
-I all_mapped/ao_SP_1.sorted.marked.realigned.fixmate.bam \
-I all_mapped/ao_SP_2.sorted.marked.realigned.fixmate.bam \
-I all_mapped/ao_SP_3.sorted.marked.realigned.fixmate.bam \
-I all_mapped/ao_SP_4.sorted.marked.realigned.fixmate.bam \
-I all_mapped/ao_SP_5.sorted.marked.realigned.fixmate.bam \
-I all_mapped/ao_SP_6.sorted.marked.realigned.fixmate.bam \
-I all_mapped/ao_SP_7.sorted.marked.realigned.fixmate.bam \
-I all_mapped/ao_SP_8.sorted.marked.realigned.fixmate.bam \
-I all_mapped/ao_SP_831.sorted.marked.realigned.fixmate.bam \
-I all_mapped/ao_SP_832.sorted.marked.realigned.fixmate.bam \
-I all_mapped/ao_SP_833.sorted.marked.realigned.fixmate.bam \
-I all_mapped/ao_SP_834.sorted.marked.realigned.fixmate.bam \
-I all_mapped/ao_SP_835.sorted.marked.realigned.fixmate.bam \
-I all_mapped/ao_SP_836.sorted.marked.realigned.fixmate.bam \
-I all_mapped/ao_SP_837.sorted.marked.realigned.fixmate.bam \
-I all_mapped/ao_SP_838.sorted.marked.realigned.fixmate.bam \
-I all_mapped/ao_SP_839.sorted.marked.realigned.fixmate.bam \
-I all_mapped/ao_SP_840.sorted.marked.realigned.fixmate.bam \
-I all_mapped/ao_SP_MA.sorted.marked.realigned.fixmate.bam \
-I all_mapped/ao_B_818.sorted.marked.realigned.fixmate.bam \
-I all_mapped/ao_B_719.sorted.marked.realigned.fixmate.bam \
-I all_mapped/ao_MLZ_50773.sorted.marked.realigned.fixmate.bam \
-I all_mapped/ao_MLZ_50774.sorted.marked.realigned.fixmate.bam \
-I all_mapped/ao_MLZ_50775.sorted.marked.realigned.fixmate.bam \
-I all_mapped/ao_MLZ_35920.sorted.marked.realigned.fixmate.bam \
-I all_mapped/ao_MLZ_59507.sorted.marked.realigned.fixmate.bam \
-I all_mapped/ao_MLZ_45517.sorted.marked.realigned.fixmate.bam \
-I all_mapped/ao_MLZ_32244.sorted.marked.realigned.fixmate.bam \
-I all_mapped/ao_MLZ_39530.sorted.marked.realigned.fixmate.bam \
-I all_mapped/ao_MLZ_40633.sorted.marked.realigned.fixmate.bam \
-I all_mapped/ao_MLZ_40634.sorted.marked.realigned.fixmate.bam \
-I all_mapped/ao_MLZ_40635.sorted.marked.realigned.fixmate.bam \
-I all_mapped/ao_MLZ_41497.sorted.marked.realigned.fixmate.bam \
-I all_mapped/ao_MLZ_48333.sorted.marked.realigned.fixmate.bam \
-I all_mapped/ao_MLZ_70063.sorted.marked.realigned.fixmate.bam \
-I all_mapped/ao_MLZ_70074.sorted.marked.realigned.fixmate.bam \
-I all_mapped/ao_MLZ_1105.sorted.marked.realigned.fixmate.bam \
-I all_mapped/ao_UMMZ_103984.sorted.marked.realigned.fixmate.bam \
-I all_mapped/ao_UMMZ_95618.sorted.marked.realigned.fixmate.bam \
-I all_mapped/ao_UMMZ_95619.sorted.marked.realigned.fixmate.bam \
-I all_mapped/ao_ANSP_90568.sorted.marked.realigned.fixmate.bam \
-I all_mapped/ao_LSUMZ_23890.sorted.marked.realigned.fixmate.bam \
-I all_mapped/ao_LSUMZ_33050.sorted.marked.realigned.fixmate.bam \
-I all_mapped/ao_LSUMZ_39731.sorted.marked.realigned.fixmate.bam \
-I all_mapped/ao_LSUMZ_43831.sorted.marked.realigned.fixmate.bam \
-I all_mapped/ao_LSUMZ_43832.sorted.marked.realigned.fixmate.bam \
-I all_mapped/ao_UMMZ_130517.sorted.marked.realigned.fixmate.bam \
--sample_ploidy 2 --max_alternate_alleles 3 --output_mode EMIT_VARIANTS_ONLY -o all.amazona.oratrix.snps.unfiltered.vcf
```


##########################
July 16th - Recalling SNPs without samples that got sequenced twice (removing ao_B_818, ao_B_719, and ao_SP_MA)

- `updated.snp.calling_071625.sh`
```
#!/bin/sh
#
#SBATCH --job-name=amazona.snp.calling              # Job Name
#SBATCH --partition=mlz-unlimited         # Name of the Slurm partition used
#SBATCH --ntasks-per-node=5
#SBATCH --nodelist=n029

# check bam files are all okay
#samtools quickcheck -v *sorted.marked.realigned.fixmate.bam > bad_bams.fofn   && echo 'all ok' || echo 'some files failed check, see bad_bams.fofn'

###############################################
### VARIANT CALLING: GATK UNIFIED GENOTYPER ###
###############################################

#emit snps only vcf for downstream filtering and popgen analyses
java -jar -Xmx12g /home/ramirezb/GenomeAnalysisTK-3.8-1-0-gf15c1c3ef/GenomeAnalysisTK.jar -T UnifiedGenotyper -R ycpa.ref.fna --num_threads 4 \
-I all_mapped/ao_BC_107.sorted.marked.realigned.fixmate.bam \
-I all_mapped/ao_BC_108.sorted.marked.realigned.fixmate.bam \
-I all_mapped/ao_BC_109.sorted.marked.realigned.fixmate.bam \
-I all_mapped/ao_BC_A112.sorted.marked.realigned.fixmate.bam \
-I all_mapped/ao_BC_A113.sorted.marked.realigned.fixmate.bam \
-I all_mapped/ao_BC_A114.sorted.marked.realigned.fixmate.bam \
-I all_mapped/ao_BC_A115.sorted.marked.realigned.fixmate.bam \
-I all_mapped/ao_BC_A116.sorted.marked.realigned.fixmate.bam \
-I all_mapped/ao_BC_A117.sorted.marked.realigned.fixmate.bam \
-I all_mapped/ao_BC_A118.sorted.marked.realigned.fixmate.bam \
-I all_mapped/ao_SP_1.sorted.marked.realigned.fixmate.bam \
-I all_mapped/ao_SP_2.sorted.marked.realigned.fixmate.bam \
-I all_mapped/ao_SP_3.sorted.marked.realigned.fixmate.bam \
-I all_mapped/ao_SP_4.sorted.marked.realigned.fixmate.bam \
-I all_mapped/ao_SP_5.sorted.marked.realigned.fixmate.bam \
-I all_mapped/ao_SP_6.sorted.marked.realigned.fixmate.bam \
-I all_mapped/ao_SP_7.sorted.marked.realigned.fixmate.bam \
-I all_mapped/ao_SP_8.sorted.marked.realigned.fixmate.bam \
-I all_mapped/ao_SP_831.sorted.marked.realigned.fixmate.bam \
-I all_mapped/ao_SP_832.sorted.marked.realigned.fixmate.bam \
-I all_mapped/ao_SP_833.sorted.marked.realigned.fixmate.bam \
-I all_mapped/ao_SP_834.sorted.marked.realigned.fixmate.bam \
-I all_mapped/ao_SP_835.sorted.marked.realigned.fixmate.bam \
-I all_mapped/ao_SP_836.sorted.marked.realigned.fixmate.bam \
-I all_mapped/ao_SP_837.sorted.marked.realigned.fixmate.bam \
-I all_mapped/ao_SP_838.sorted.marked.realigned.fixmate.bam \
-I all_mapped/ao_SP_839.sorted.marked.realigned.fixmate.bam \
-I all_mapped/ao_SP_840.sorted.marked.realigned.fixmate.bam \
-I all_mapped/ao_MLZ_50773.sorted.marked.realigned.fixmate.bam \
-I all_mapped/ao_MLZ_50774.sorted.marked.realigned.fixmate.bam \
-I all_mapped/ao_MLZ_50775.sorted.marked.realigned.fixmate.bam \
-I all_mapped/ao_MLZ_35920.sorted.marked.realigned.fixmate.bam \
-I all_mapped/ao_MLZ_59507.sorted.marked.realigned.fixmate.bam \
-I all_mapped/ao_MLZ_45517.sorted.marked.realigned.fixmate.bam \
-I all_mapped/ao_MLZ_32244.sorted.marked.realigned.fixmate.bam \
-I all_mapped/ao_MLZ_39530.sorted.marked.realigned.fixmate.bam \
-I all_mapped/ao_MLZ_40633.sorted.marked.realigned.fixmate.bam \
-I all_mapped/ao_MLZ_40634.sorted.marked.realigned.fixmate.bam \
-I all_mapped/ao_MLZ_40635.sorted.marked.realigned.fixmate.bam \
-I all_mapped/ao_MLZ_41497.sorted.marked.realigned.fixmate.bam \
-I all_mapped/ao_MLZ_48333.sorted.marked.realigned.fixmate.bam \
-I all_mapped/ao_MLZ_70063.sorted.marked.realigned.fixmate.bam \
-I all_mapped/ao_MLZ_70074.sorted.marked.realigned.fixmate.bam \
-I all_mapped/ao_MLZ_1105.sorted.marked.realigned.fixmate.bam \
-I all_mapped/ao_UMMZ_103984.sorted.marked.realigned.fixmate.bam \
-I all_mapped/ao_UMMZ_95618.sorted.marked.realigned.fixmate.bam \
-I all_mapped/ao_UMMZ_95619.sorted.marked.realigned.fixmate.bam \
-I all_mapped/ao_ANSP_90568.sorted.marked.realigned.fixmate.bam \
-I all_mapped/ao_LSUMZ_23890.sorted.marked.realigned.fixmate.bam \
-I all_mapped/ao_LSUMZ_33050.sorted.marked.realigned.fixmate.bam \
-I all_mapped/ao_LSUMZ_39731.sorted.marked.realigned.fixmate.bam \
-I all_mapped/ao_LSUMZ_43831.sorted.marked.realigned.fixmate.bam \
-I all_mapped/ao_LSUMZ_43832.sorted.marked.realigned.fixmate.bam \
-I all_mapped/ao_UMMZ_130517.sorted.marked.realigned.fixmate.bam \
--sample_ploidy 2 --max_alternate_alleles 3 --output_mode EMIT_VARIANTS_ONLY -o updated.54.amazona.oratrix.snps.unfiltered.vcf

#removed:
#-I all_mapped/ao_SP_MA.sorted.marked.realigned.fixmate.bam \
#-I all_mapped/ao_B_818.sorted.marked.realigned.fixmate.bam \
#-I all_mapped/ao_B_719.sorted.marked.realigned.fixmate.bam \
```
- `slurm-32530.out`