# Workshop - OTU pipeline - January 2024
This workshop will be focused on the workflow described [here](https://github.com/Ralpina/fungalOTUpipeline).
Comments with the symbols :triangular_flag_on_post::computer: will mark the commands/instructions you will need to follow. Other comments are for explanation only.

## Setting up your KewHPC personal directory
   :triangular_flag_on_post::computer: If you work on a PC, download MobaXterm and connect to the [KewHPC cluster](https://rbg-kew-bioinformatics-utils.readthedocs.io/en/latest/KewHPC/Overview/), as explained below. 
   Open MobaXterm and click on "Session":  
   ![image](https://github.com/Ralpina/Workshop-OTU-pipeline/assets/48416466/5a473963-f66b-45cc-8ea1-b7f5b0f66ab4)  
   
   :triangular_flag_on_post::computer: Then click on "SSH" and fill the empty fields as shown below, with your Kew account username:  
   ![image](https://github.com/Ralpina/Workshop-OTU-pipeline/assets/48416466/a0eb7233-d02d-457e-9c3c-cc9aee469e5d)  
   
   :triangular_flag_on_post::computer: This will prompt you to insert your Kew account password; after that, you should see the below:  
   ![image](https://github.com/Ralpina/Workshop-OTU-pipeline/assets/48416466/bc3b4d86-bf49-444f-bdc1-7b7dc4082f81)

   :triangular_flag_on_post::computer: Click again on "Session" and then on "SFTP"; fill the empty fields as done for the "SSH" tab above:
   ![image](https://github.com/Ralpina/Workshop-OTU-pipeline/assets/48416466/6eee9547-90b9-4e95-b6f0-56004a19db31)  

   This will allow you to directly upload the files provided to your own KewHPC directory, as explained in the next step. Alternatively, follow the instructions [here](https://rbg-kew-bioinformatics-utils.readthedocs.io/en/latest/KewHPC/Transfer_data_via_sftp/#from-macos-linux) to transfer files using your local terminal to your personal KewHPC directory, if you are a Mac user.  

## Uploading data, running scripts (and basic Unix commands)
   In the terminal tab (SSH) opened earlier, we can start familiarising with basic UNIX commands. We first create a few directories, where we will eventually upload the files provided.  
   :triangular_flag_on_post::computer: Type the following, then press enter:  
   ```mkdir myco```  
   ```mkdir myco/seqs```  
   ```mkdir myco/scripts```  
   ```mkdir myco/database```  
   The command ```mkdir``` creates a new empty directory (or folder). We can check whether the directories above have been created by using the command ```ls```, which will list all the files in your current directory. To "go" to a specific directory, we can type the change directory command ```cd``` followed by the path and the name of the directory. For example:  
   ```cd myco/seqs```  
   To go back to the upper directory, we can type:  
   ```cd ../```  
   this will bring you to the ```myco``` directory. Typing:  
   ```cd ../../```   
   instead, will bring you back to your home directory.

   <details>
   <summary> Expand to see other important commands... </summary>  
   
   ```cp filename``` to copy a file, or ```cp -r ./directory``` to copy a non-empty directory   
   ```cat filename``` to print the content of a file  
   ```more filename``` to print the content of a file while scrolling  
   ```pwd``` if you are not sure where you are and you want to see your current working directory  
   You will have the opportunity to try them later on.
   
   </details>

   :triangular_flag_on_post::computer: Once the directories have been created, we can upload the sequence files .ab1 (to the folder "seqs"), the script files (to the folder "scripts"), and the UNITE database (to the folder "database"), using the SFTP connection set up earlier to navigate the proper directory. Notice that we are using the [UNITE v.9 database](https://doi.plutof.ut.ee/doi/10.15156/BIO/2938068), but you can look for new releases [here](https://unite.ut.ee/repository.php).  
   
   #### Scripts
   
   In this workflow, we will find three types of scripts (see "scripts" folder):  
   - slurm scripts. They can have any name and be run from any directory, provided that the right working directory path is specified within the script header. To prepare a slurm script, have a look at the [KewHPC guidelines](https://rbg-kew-bioinformatics-utils.readthedocs.io/en/latest/KewHPC/Running_Analysis_with_SLURM/), explaining all the arguments that can be included in the script header, for example the directory you want to run the analyses in, and the log files path. It's always useful to include a log file for error messages. For instance, one of our slurm scripts called "2_phred.slurm" includes the following arguments:  
   ```#SBATCH --chdir=/home/yourusername/myco/```    
   ```#SBATCH --error=/home/yourusername/myco/errors/errorphred.txt```    
   ```#SBATCH --output=/home/yourusername/myco/errors/out.txt```  

      "yourusername" needs to be replace by your actual Kew username for all the slurm scripts (you will recognise them by the .slurm extension). You can do that in several ways, but the easiest way is using a text editor, e.g. GNU nano.  

      :triangular_flag_on_post::computer: Type the following:
      ```sh
      cd myco     
      nano scripts/2_phred.slurm
      ```        
       :triangular_flag_on_post::computer: then manually replace "yourusername" with your Kew username (in the format xx00kg) at lines 3,5,6.  Once you've finished, use Ctrl+o to save your changes and Ctrl+x to exit the text editor.  
      :triangular_flag_on_post::computer: Do the same for the following scripts: 3_phd2fasta.slurm, 4_phrap_assembly.slurm, 6_trim_filter.slurm, 8_searchUnite.slurm, 9_hard_filt.slurm, 10_denovo_centroids.slurm, 12_centroids_blastn.slurm.  

     Eventually, you will run slurm scripts by typing ```sbatch```, followed by the name of your script, as in:    
      ```sh
      sbatch myscript.slurm
      ```
 
   - non-slurm scripts (e.g. python or shell) have the specific extensions .py and .sh.
     Shell scripts need to be made executable as follows:  
      ```sh 
      chmod +x myscript.sh
      # and then run using:
      ./myscript.sh
      ```  
      python scripts can be run by first "opening" python, as follows:  
      ```sh
      module load python/2.7.18  
      # and then run using:
      python myscript.py
      ```  
     :triangular_flag_on_post::computer: To be able to run .py and .sh, we need to copy them to the upper directory ```myco```, using the following commands:  
     ```sh
     # assuming you are still in the myco directory,  
     # the following command will allow you to copy each file to the myco directory:  
     cp ./scripts/1_create_dirs.sh ./
     cp ./scripts/5_fasta_to_fastq.py ./
     cp ./scripts/7_chimaera_filter.sh ./
     cp ./scripts/5_fasta_to_fastq.py ./
     cp ./scripts/8b_fasta_to_fastq.py ./
     cp ./scripts/11_match_unmatched.sh ./
     cp ./scripts/13_centroids_blast_summary.py ./
     cp ./scripts/14_searchUniteFree.sh ./
     cp ./scripts/15_funguild.sh ./
     ```
     :triangular_flag_on_post::computer: Then we need to make the .sh scripts executable, using the following commands:
     ```sh
     chmod +x 1_create_dirs.sh 
     chmod +x 7_chimaera_filter.sh
     chmod +x 11_match_unmatched.sh
     chmod +x 14_searchUniteFree.sh
     chmod +x 15_funguild.sh
     ``` 
   
     

 <details>
   <summary> Why are we using different script types? </summary>  
   
   Ideally, we would be able to run most of the commands with simple shell scripts. However, we are sharing memory resources on KewHPC, and therefore Slurm allows to equally allocate resources among users.
   In this workflow, the most time/memory consuming scripts are designed to be slurm scripts. This might not be evident when having a small number of sequences, but it becomes obvious when having thousands of sequences.
   
   </details>


## Running the pipeline
## 1
The first script of this workflow will simply create directories to organise the data at the different stages of the pipeline. (You can always change the directory names to what works best for you).
```sh
./1_create_dirs.sh

```
The script will create the following directories:  
`myco/phred_out`  
`myco/assembled`  
`myco/assembled_fastq`  
`myco/chimeras`  
`myco/results`  
`myco/sing`  
`myco/singR`  
`myco/sing_fastq`  
`myco/errors`  
You can check whether they have been created by typing ```ls``` in the myco folder.  

:question: How many sequences are we working on?  
:triangular_flag_on_post::computer: Type the following (then press enter):  
`ls seqs | wc -l`  
<details>
   <summary> Answer </summary>  
   
   204
   
   </details>  

:question: How many forward and how many reverse sequences?   
:triangular_flag_on_post::computer: Type:  
```sh 
ls seqs/ITS1F* | wc -l    
ls seqs/ITS4* | wc -l    
```
The wildcard (asterisk) will allow us to count all files starting by the string of characters provided, i.e. forward and reverse primer name, respectively.
<details>
   <summary> Answer </summary>  
   
   102 forward and 102 reverse sequences
   
   </details>  

## 2

Let's continue running the pipeline, with the second script "2_phred.slurm". This script will perform basecalling and the first quality screening, see the explanations [here](https://github.com/Ralpina/fungalOTUpipeline/tree/main?tab=readme-ov-file#basecalling-using-phred-and-first-quality-screening).  
:triangular_flag_on_post::computer: Type:  
```sh
sbatch scripts/2_phred.slurm
```
This command will send your job to the queueing system.  
:triangular_flag_on_post::computer: It should run very quickly (with only 200 sequences), but you'll be able to check the status of your job by typing:  
```squeue```  
and see something like this:  
![image](https://github.com/Ralpina/Workshop-OTU-pipeline/assets/48416466/7843c06a-20cb-4fd7-93d6-b3e06b5fba2e)  
The script will produce output files with extension ".phd.1" folder "phred_out". 
If you're interested, you can have a look at the information included in a .phd.1 file (including various quality information, bases called, etc.) by typing:  
```more phred_out/ITS1F_SV02S2D02.ab1.phd.1```  


:triangular_flag_on_post::computer: Some sanity checks after running this script might include having a look at:  
 - The number of phd.1 files generated. We would expect this to be smaller than the number of initial sequences, as an [initial quality screening](https://github.com/Ralpina/fungalOTUpipeline/blob/main/README.md#basecalling-using-phred-and-first-quality-screening) was also performed.     
   ```ls phred_out | wc -l```  
   <details>
   <summary> Answer... </summary>  
   
    155
   
   </details>  
 - The number of sequences removed by the initial quality screening:  
   ```wc -l waste.txt```
   <details>
   <summary> Answer... </summary>  
   
    49
   
   </details> 
 - ...and their identity, if you are interested:  
   ```cat waste.txt``` or ```more waste.txt```   
 - The sum of the two numbers above, which should correspond to the number of initial sequences in ```seqs```
    <details>
    <summary> Answer... </summary>  
   
    155 + 49 = 204
   
    </details> 
 - finally, at the error file, normally empty:  
    ```cat errors/errorphred.txt```


## 3
:triangular_flag_on_post::computer: Type:  
```sh
sbatch ./scripts/3_phd2fasta.slurm
```
This script will [prepare the sequences for the assembly](https://github.com/Ralpina/fungalOTUpipeline/blob/main/README.md#preparing-files-for-phrap-assembly-using-phd2fasta) (of forward and reverse sequences coming from the same DNA template). The output files, in fasta format, will be in the folder "assembled" and will include the actual sequence files (with no extension), and the quality files (with extension .qual).  


:triangular_flag_on_post::computer: Some sanity checks:  
 - We can have a look at the number of unique codes extracted from the file names, where by unique code we indicate the code associated to a DNA template:        
   ```wc -l codes.txt```  
   <details>
   <summary> Answer... </summary>  
   
    79
   
   </details>  
 - The number of fasta (sequences and quality) generated by phd2fasta, which should be twice the number of codes:  
   ```ls assembled | wc -l```
   <details>
   <summary> Answer... </summary>  
   
    158 (= 79x2)
   
   </details>  
 - We can have a look at the error file, normally empty:  
   ```cat errors/errorphd2fasta.txt```

## 4
:triangular_flag_on_post::computer: Type:  
```sh
sbatch scripts/4_phrap_assembly.slurm
```
This will essentially generate contigs assembled from forward and reverse primers from the same DNA template, when possible, with the benefit of having longer sequences for OTU identification. The script will perform several steps, and the [details are reported here](https://github.com/Ralpina/fungalOTUpipeline/blob/main/README.md#assembling-forward-and-reverse-sequences-using-phrap).   

:triangular_flag_on_post::computer:    
 - The most interesting thing to check is how many contigs have been generated, and from how many sequences they have been generated, for example if there were multiple forward and reverse sequences with the same code (i.e. from the same DNA template):  
   ```tail -n 17 ./errors/outphrap.txt```
   <details>
   <summary> Answer... </summary>  
   
    ![image](https://github.com/Ralpina/Workshop-OTU-pipeline/assets/48416466/d5a4eb72-105f-4bad-886b-d1e9886ff231)

   
   </details>  
 - The script will also have created a fasta and a quality file for all contigs in "assembled_fastq". To count the number of contigs:   
   ```grep -c "^>" ./assembled_fastq/contigs.fasta```  
   "grep" captures the ">" character at the beginning of the file (indicated by "^"). The "-c" flag tells grep to count the characters captured.  
   <details>
   <summary> Answer... </summary>  
   
    61
   
   </details>  
 - We can also check which codes have not been assembled into contigs, resulting in unidirectional sequences, here called "singlets" (but do not confuse these with "singletons", explained below). The number of codes for singlet sequences:  
   ```wc -l singlets.txt```  
    <details>
    <summary> Answer... </summary>  
   
     18
   
    </details>  
   and the number of singlets:  
   
   ```grep -c "^>" ./sing_fastq/singF.fasta```  
   
   ```grep -c "^>" ./sing_fastq/singR.fasta```  

   <details>
   <summary> Answer... </summary>  
   
     18  
     13 
   
    </details>  

   
   
   








 




  
    
   
   
   

   



