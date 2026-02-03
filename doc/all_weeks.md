# Laboratory Notebook

Week: 1  
From: 19 January 2026  
To: 25 January 2026

---

## Monday

### Completed

* Literature review: Martin et al. 2025
* ViruAnnot GitHub repository:
  * Forked to my personal repository
  * Cloned locally on my machine

---

## Tuesday

### Completed

* RStudio installation
* Laboratory notebook template
* ViruAnnot analysis:
  * Documentation of my pipeline analysis methodology
  * Creation of a local conda environment for MetaProkka

### In progress

* Analysis and testing of Prokka (although I believe it may not be necessary)
* 

### Discussion

* 

### To do

* Read the ViruAnnot scripts
* Reproduce the pipeline on a small dataset in interactive mode and analyze the results
* Run the automated pipeline on a small dataset
* Flash presentation on the internship topic for Thursday, 05 February 2026
* Write 100 words per day in the report
* Read 2 new articles per week

---

## Wednesday

### Completed

* Creation of a personal GitHub repository

### In progress

* DRAM-v analaysis
* ViruAnnot analysis

### Discussion

* The Prokka-gv analysis is indeed not possible with the mock dataset that was provided to me, but this is not an issue

### To do

* 

---

## Thursday

### Completed

* 

### In progress

* DRAM-v analaysis 

### Discussion

* a v2 of DRAM is in developpement (beta)
* Some operations ensured by DRAM-v seems to be already or at least partially ensured by ViruAnnot i have to check the dissimilarities   

### To do

* 

---

## Friday

### Completed

* 

### In progress

* 

### Discussion

* 

### To do

* 

---

## Weekly summary

### Completed

* 

### In progress

* 

### Discussion

* 

### To do

* 

--- 



## Week   
Week: 2 
From:  26 january 2026
To:  01 february 2026

---

### Monday

#### Completed

* ViruAnnot:
  * Creating a conda env for the step 02_annotate_virus.py
```shell

conda env create -n 02_annot_viral_env
conda activate 02_annot_viral_env
conda install pip
pip install pandas
pip install PyYAML

```
  * Decompressing the files in "~/Documents/annotation/annotation_fonctionnelle/mollusca_metagenome/split_faa/"
  * Running the step 02_annotate_virus.py
```shell
# viruannot repo
cd ~/Documents/viruAnnot/
# path variables
annofonc="/home/fouchard/Documents/annotation/annotation_fonctionnelle/mollusca_metagenome/split_faa/mollusca_metagenome_vOTU.part_001_"

sufannofonc=".norm.domtblout"

# create a results directory and .gitignore it
mkdir results
echo "results/" >> .gitignore

# Run 02_annotate_virus.py
python3 scripts/02_annotate_virus.py \
--config config.yaml \
--phrogs "${annofonc}PHROGS${sufannofonc}" \
--vogdb  "${annofonc}VOGDB${sufannofonc}" \
--viphogs "${annofonc}viPHOG${sufannofonc}" \
--output results/annotation.tsv

```
  * it produces table "annotation.tsv" with this format : 
```shell

gene_id	hit_id	evalue	bitscore	coverage	db
LDFBEDJL_00007	VOG22799	9e-36	109.6	0.8755020080321285	VOGDB
LDFBEDJL_00012	VOG06822	1.6e-76	243.5	0.806282722513089	VOGDB
LDFBEDJL_00015	VOG14008	1.1e-22	67.1	0.6565349544072948	VOGDB
LDFBEDJL_00032	VOG01242	2.3e-21	62.5	0.7575757575757576	VOGDB
LDFBEDJL_00040	VOG51642	2.8e-34	104.7	0.7785016286644951	VOGDB
LDFBEDJL_00049	ViPhOG210	1.4e-22	66.1	0.5052447552447552	ViPhOGs

``` 

gene_id     = identifier of the gene identified from the contigs of the experiment 
hit_id      = identifier of the corresponding hit 
bitscore    = bitscore, correspond to a log-odds score, expressed in bits, measuring how much better the alignment fits the HMM profile than a random sequence model. 
coverage    = coverage, correspond to target protein coverage on query protein 
db          = DataBase form which the hit were obtainned 

#### In progress

* Running the ViruAnnot pipeline on mocking data to better understand it 

#### Discussion

*  Concerning viruannot : 
   * There isnt options to read compressed files (.gz for exemple) which is very limiting regarding how much heavy uncompressed files can be. This feature could be implemented later. However it concerns intermediary files though 

#### To do

* Running the step 03_annotate_specialized.py

---

### Tuesday

#### Completed

* Creation of conda environment for the step 03_annotate_specialized
```shell

conda create -n 03_annotate_specialized
conda activate 03_annotate_specialized
conda install pip
pip install pandas
pip install PyYAML

```
* Create a symbolic link to the mocking data
```shell

ln -s ~/Documents/annotation/annotation_fonctionnelle/mollusca_metagenome/split_faa/ anno_fonc

```

* Running the step 03_annotate_specialized.py

```shell 

python3 scripts/03_annotate_specialized.py \
--config config.yaml \
--resfams anno_fonc/mollusca_metagenome_vOTU.part_001_Resfams.norm.domtblout \
--vfdb anno_fonc/mollusca_metagenome_vOTU.part_001_VFDB.tsv \
--output results/annotation_specialized.tsv

```
#### In progress

* 

#### Discussion

* 

#### To do

* 

---

### Wednesday

#### Completed

* Running the step 
```shell 

python3 scripts/04_annotate_functional.py \
--config config.yaml \
--kofam anno_fonc/mollusca_metagenome_vOTU.part_001_KOFAM.norm.domtblout \
--pfam anno_fonc/mollusca_metagenome_vOTU.part_001_Pfam.norm.domtblout \
--output results/04_annotation_functionnal.tsv

```

#### In progress

* 

#### Discussion

* It wasn't advised to create an conda environment for each step since :
  * 02 to 05 require the same python dependancies 
  * Running the entire pipeline would require to use the same environment 
  * 

#### To do

* Correct the README.md in ViruAnnot/results : 
  * the inputs for 02 -> 04 step are wrong
* Add an environment.yaml file to make the environment settlement easier 
* correct the script setup_databases.sh
  * there are many bugs to correct 
* The step 06 require databases used for annotations but im currently not able to dowloand them locally

---

### Thursday

#### Completed

* 

#### In progress

* Running ViruAnnot on the soere meb server 

#### Discussion

* 

#### To do

* Obtain le the final gff with the test data

---

### Friday

#### Completed

* Added the scripts directory to the PATH:

  ```bash
  
  export PATH="$HOME/viruAnnot/scripts:$PATH"

  source ~/.bashrc

  ```
* I created `local_paths.sh` to define common environment variables (e.g. `BASE`) used across scripts.

An initial attempt to execute the file using:

```bash
bash local_paths.sh
```
* create a directory for descriptors files + downloading the VOGDB descriptor file : 
```shell
#VOGDB
wget https://fileshare.lisc.univie.ac.at/vog/vog232/vog.annotations.tsv.gz
#PFAM
wget https://ftp.ebi.ac.uk/pub/databases/Pfam/current_release/Pfam-A.clans.tsv.gz
#KO_fam
wget ftp://ftp.genome.jp/pub/db/kofam/ko_list.gz
#PHROGS et resfam sont sur le serveur de l'équipe 

# J'ai l'impression que je n'ai pas besoin de descripteur pour ViPhOGs

# Pour VFDB, j'ai téléchargé un un fichier .xls à partir de ce lien :
https://www.mgc.ac.cn/VFs/download.htm



```

#### In progress

* 

#### Discussion

* I cant find the descriptor files on the soere-meb serv
  * these files generally come with the corresponding DB to link annotations id with their fonction (.e.g K00001 > alcool deshydrogenase in KEGG)

#### To do

* 

---

### Weekly summary

#### Completed

* 

#### In progress

* 

#### Discussion

* 

#### To do

*  

---
# Laboratory Notebook

## Week   
Week: 3 
From:  02 february 2026
To:  08 february 2026

---

### Monday 26-02-02

#### Completed

* I've made functionning the viruannot pipeline on mocking data : 
```shell
source local_paths.sh

python3 functional_annotation.py --config config.yaml --gff $MPROK --phrogs $PHROGS --vogdb $VOGDB --viphogs $VIPHOGS --resfams $RESFAM --vfdb $VFDB --kofam $KOFAM --pfam $PFAM --output final_test3.gff --scripts-dir scripts

```
* I've made functionning the viruannot pipeline on air_metagenome data
```shell
# modifying 

```
#### In progress

* 

#### Discussion

* 

#### To do

* 

---

### Tuesday 26-02-03

#### Completed

* Openning a new tree in the serv to organize my project "viruAMG" which is also available following this link : 
https://github.com/NaFouchard/viruAMG.git

* I organized it with the following tree according to B.batut (2016)
```
.
├── bin
│   ├── functional_annotation.py
│   └── setup_databases.sh
├── conf
│   ├── config.yaml
│   └── local_path.sh
├── doc
│   └── README.md
├── exp
├── raw
│   └── descriptors
│       ├── ko_list.gz
│       ├── Pfam-A.clans.tsv.gz
│       └── vog.annotations.tsv.gz
├── results
└── src
    ├── 01_prokka-gv
    ├── 02_annotate_virus.py
    ├── 03_annotate_specialized.py
    ├── 04_annotate_functional.py
    ├── 05_merge_annotations_table.py
    ├── 06_generate_gff.py
    └── __pycache__
        └── annotate_virus.cpython-37.pyc

```
* I've also created a script(local_paths) to create variables in order to facilitate the execution of functional_annotation.py, it integrate a function to both inspect the first line of the refering file and declare if the vaiable doesnt refer to a file


#### In progress

* 

#### Discussion

* Local_paths.sh construction isn't ideal, the best would be to have 2 independant scripts, the first one declaring variables and the second one verifying if they are viable.
*  

#### To do

* Adjust the congi.yaml file to fit with my configuration (naotably for descriptor files)

---

### Wednesday

#### Completed

* 

#### In progress

* 

#### Discussion

* 

#### To do

* 

---

### Thursday

#### Completed

* 

#### In progress

* 

#### Discussion

* 

#### To do

* 

---

### Friday

#### Completed

* 

#### In progress

* 

#### Discussion

* 

#### To do

* 

---

### Weekly summary

#### Completed

* 

#### In progress

* 

#### Discussion

* 

#### To do

*  

---


