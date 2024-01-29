# Workshop - OTU pipeline - January 2024
This workshop will be focused on the workflow described [here](https://github.com/Ralpina/fungalOTUpipeline).

## 1. Setting up your KewHPC personal directory
   If you work on a PC, download MobaXterm and connect to the [KewHPC cluster](https://rbg-kew-bioinformatics-utils.readthedocs.io/en/latest/KewHPC/Overview/), as explained below. 
   Open MobaXterm and click on "Session":  
   ![image](https://github.com/Ralpina/Workshop-OTU-pipeline/assets/48416466/5a473963-f66b-45cc-8ea1-b7f5b0f66ab4)  
   
   Then click on "SSH" and fill the empty fields as shown below, with your Kew account username:  
   ![image](https://github.com/Ralpina/Workshop-OTU-pipeline/assets/48416466/a0eb7233-d02d-457e-9c3c-cc9aee469e5d)  
   
   This will prompt you to insert your Kew account password; after that, you should see the below:  
   ![image](https://github.com/Ralpina/Workshop-OTU-pipeline/assets/48416466/bc3b4d86-bf49-444f-bdc1-7b7dc4082f81)

   Click again on "Session" and then on "SFTP"; fill the empty fields as done for the "SSH" tab above:
   ![image](https://github.com/Ralpina/Workshop-OTU-pipeline/assets/48416466/6eee9547-90b9-4e95-b6f0-56004a19db31)  

   This will allow you to directly upload the files provided to your own KewHPC directory, as explained in the next step. Alternatively, follow the instructions [here](https://rbg-kew-bioinformatics-utils.readthedocs.io/en/latest/KewHPC/Transfer_data_via_sftp/#from-macos-linux) to transfer files using your local terminal to your personal KewHPC directory, if you are a Mac user.  

## 2. Uploading data, running scripts (and basic Unix commands)
   In the terminal tab (SSH) opened earlier, we can start familiarising with basic UNIX commands. We first create a few directories, where we will eventually upload the files provided. Type the following, then press enter:  
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
   
   ```cp``` to copy a file, or ```cp -r``` to copy a non-empty folder   
   ```cat``` to print the content of a file  
   ```more``` to print the content of a file while scrolling  
   ```pwd``` if you are not sure where you are and you want to see your current working directory  
   
   </details>

   Once the directories have been created, we can upload the sequences files .ab1 (to folder "seqs"), the script files (to folder "scripts"), and the UNITE database (to folder "database"), using the SFTP connection set up earlier to navigate to the proper directory.  
   
   
   In this workflow, we will find three types of scripts (see "scripts" folder):  
   - slurm scripts. They can have any name and be run from any directory, provided that the right working directory path is specified within the instructions. To prepare a slurm script, have a look at the [KewHPC guidelines](https://rbg-kew-bioinformatics-utils.readthedocs.io/en/latest/software/slurm/). The instructions in the first part of the script will have to include the directory you want to run the analyses in, and the files you want to print updates about the status of your analyses and any error messages. For instance, for one of our slurm scripts called "2_phred.slurm", these will be:  
   ```#SBATCH --chdir=/home/yourusername/myco/```    
   ```#SBATCH --error=/home/yourusername/myco/errors/errorphred.txt```    
   ```#SBATCH --output=/home/yourusername/myco/errors/out.txt```  

"yourusername" needs to be replace by your actual username for all the slurm scripts (you will recognise them by the .slurm extension). You can do that in several ways, but the easiest one is using a text editor, e.g. GNU nano. Type the following:
 ```nano scripts/2_phred.slurm```        
then manually replace the path with your username at the lines 3,5,6. Do the same for the following files:

XXXXXXXadd scripts.

  You will be able to run slurm scripts by typing ```sbatch```, following by the name of your script, as in:
   
   ```sh
   sbatch myscript.slurm
   ```
 
- non-slurm scripts (e.g. python or shell) have the specific extensions .py and .sh. Shell scripts need to be made executable as follows:  
```sh 
chmod +x myscript.sh
# and then run using:
./myscript.sh
# python scripts can be run as follows:
module load python/2.7.18
python myscript.py
```

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

Sequences with the extension `ab1` should be then uploaded to the newly created directoy `seqs`. The number of sequences uploaded can be checked using `ls seqs | wc -l`.  
A local database can also be downloaded from the UNITE website, for example: [UNITE v.9 database](https://doi.plutof.ut.ee/doi/10.15156/BIO/2938068), or check for new releases [here](https://unite.ut.ee/repository.php).  
  
    
   
   
   

   



