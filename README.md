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
   In the terminal tab (SSH) opened earlier, we can start familiarising with basic UNIX commands. We first create a few directories, where we will eventually upload the files provided. Type the following:  
   ```mkdir seqs```  
   ```mkdir scripts```  
   The command ```mkdir``` creates a new empty directory (or folder). We can check whether the directories above have been created by using the command ```ls```, which will list all the files in your current directory. To "go" to a specific directory, we can type the change directory command ```cd``` followed by the path and the name of the directory. For example:  
   ```cd seqs```  
   To go back to the upper directory, we can type:  
   ```cd ../```  
   Other important commands are the following:  
   ```cp``` to copy a file or ```cp -r``` to copy a non-empty folder   
   ```cat
    
   
   
   

   



