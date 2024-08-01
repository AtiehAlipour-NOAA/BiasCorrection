# Cloud-AWS

Cloud-AWS is run by NOAA.

**User Guide: [Cloud Computing User Information](https://clouddocs.rdhpcs.noaa.gov/wiki/index.php?title=Cloud_Computing_User_Information)**

NOAA Cloud Computing uses the Parallel Works software application, in a version customized for NOAA RDHPCS.

Parallel Works User Guide is linked [here](https://parallelworks.com/docs).

**Follow these steps to request access to Cloud-AWS and log in to the Parallel Works for the first time.**

1- **Request Access to a Project:**

  - Contact your project lead to identify the specific project that is located on Cloud-AWS and you need to access.
  - Go to the [AIM website](https://aim.rdhpcs.noaa.gov/) and click on `Request new access to a project.`
  - Click on `Click here to Request Access To a Project.`
  - Select the project and fill in the Justification box with a short description of the work you intend to do for that project.
  - Click the `Submit Request` button.
  - Once the approval process is complete, you will receive an email confirming your addition to the project on Cloud-AWS as requested.

2- **Access to Cloud** 

 - Log in to Parallel Works [here](https://noaa.parallel.works/login).
 - Use your case-sensitive username (First.Last) and password (PIN + NOAA-RDHPCS RSA token).
 - Explore available clusters under your account.


![image](https://github.com/noaa-ocs-modeling/SurgeTeamCoordination/assets/148251584/cc97c1bd-f515-41df-84ab-b2c8681a4cb4)

 - When a cluster is active, the switch button is green.
 - **Note:** If you push a green button to turn the cluster off mistakenly, it will terminate all active sessions for other users on that cluster.

3- **Access a Cluster**

  - Send a message to the cluster owner to request permission.
  - **Note:** The Owner of that cluster needs to restart the cluster after giving permission.
  - After gaining permission, copy your username with the headnode IP.

![image](https://github.com/noaa-ocs-modeling/SurgeTeamCoordination/assets/148251584/9b65d360-6c03-4bc9-abd5-3096825fbe48)


  - Click the IDE icon, select `New Terminal` from the `Terminal` tab.
  - From `Terminal` select `New Terminal`.
  - A terminal window will open at the bottom of the page.
  
  
![image](https://user-images.githubusercontent.com/148251584/285081301-2b475342-8869-4a1e-95a7-64f01d12bc45.png)

 
 - In the terminal, run `ssh <First.Last>@<cluster_ip_address>` that you copied to connect to the cluster.
 
4- **Access Jupyter Notebook**

 To initiate a Jupyter session on Parallel Works, follow the steps below:
   
  - First, create a virtual environment that includes your required libraries. 
  
    - Access the cluster using the Terminal tab as explained in section 3.
      
    - Contact the cluster owner to identify the Lustre folder where you should create your Conda environment.
      
    - Create a folder with your name under the Lustre folder.
      
    - Inside your directory, download Miniconda3: `wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh`
      
    - Add execute permissions using: `chmod 700 Miniconda3-latest-Linux-x86_64.sh`
      
    - Install Conda in your directory: `./Miniconda3-latest-Linux-x86_64.sh`
      
    **Ensure you install Conda environment in your Lustre directory.**
        
    - Add Conda to the bash file: `source ~/.bashrc`
      
    - Create your virtual environment with your list of libraries. For example:
      
          `mamba create -n <yourenvirmentment-name> -cconda-forge python=3.10 geos gdal proj hdf5 netcdf4`
         
    - Activate the virtual environment:  `conda activate <yourenvirmentment-name>`
      
    - Install JupyterLab: `mamba install jupyterlab`
      
    **For more information on installing and activating a virtual environment, please refer [here](https://github.com/noaa-ocs-modeling/SurgeTeamCoordination/blob/main/guides/Create_Virtual_Environment_RDHPCS.md).**
      
  - Next, connect to Parallel Works using port forwarding.
   
    - Open WSL or Windows PowerShell.
      
    - Generate an SSH key by running: `ssh-keygen` This will generate an id_rsa.pub file in your ~/.ssh folder.
      
    - In the Parallel Works dashboard, go to your account under Authentication and SSH key.
      
        ![image](https://github.com/noaa-ocs-modeling/SurgeTeamCoordination/assets/148251584/45e750a7-0595-49b9-960b-98732d02e703)
        ![image](https://github.com/noaa-ocs-modeling/SurgeTeamCoordination/assets/148251584/58308a1a-e79e-4d9a-bdc0-41a58412cbcc)
        
     - Select "Add Key".
      
     - Add the key name and paste the text from ~/.ssh/id_rsa.pub file, then select "Add Key".
      
     - Now, using Windows PowerShell or WSL, log in to Parallel Works and establish your tunnel using a local port number within the 8800-8900 range (e.g., 8857): 
      
        `ssh -i ~/.ssh/id_rsa -L<local_port>:localhost:<local_port> <First.Last>@<cluster_ip_address>`

         for example: ssh -i ~/.ssh/id_rsa -L8857:localhost:8857 Atieh.Alipour@22.33.234.11
    
   - **Jupyter session on the Headnode:** Inside PowerShell and WSL, when logged in to Parallel Works with a local port number, to start your Jupyter session on the Headnode:
   
      - Activate your virtual environment: `conda activate <env_name>` or `source /PathToMiniconda3/bin/activate <env_name>` 
      
      - Navigate to your directory where you want to open your Jupyter session.  
      
      - Start your Jupyter session with a chosen port number within the 8800-8900 range (e.g., 8857):
   
        `jupyter lab --no-browser --port=8857`
        
      - Open a local browser on your laptop/desktop and enter the provided URL. This should bring up the Jupyter interface (IDE).
      
   - **Jupyter session on Compute nodes:** Inside PowerShell and WSL, when logged in to Parallel Works with a local port number, to start your Jupyter session on Compute nodes:
   
      - First, submit a job to get access to a compute node for a specified time frame: `srun -t 4:00:00 --pty bash`
      
      - Generate a new pair of public and private keys on the head node (similar to what you did on your local machine) and add the public key to your ~/.ssh/authorized_keys.
      
        - Generate an SSH key by running `ssh-keygen`
        
      - In the Parallel Works dashboard, go to your account under Authentication and SSH key.
        
        ![image](https://github.com/noaa-ocs-modeling/SurgeTeamCoordination/assets/148251584/45e750a7-0595-49b9-960b-98732d02e703)
        ![image](https://github.com/noaa-ocs-modeling/SurgeTeamCoordination/assets/148251584/58308a1a-e79e-4d9a-bdc0-41a58412cbcc)
        
      - Select "Add Key".
        
      - Add the key name and paste the text from the id_rsa.pub file, then select "Add Key" (use for example vi /home/Atieh.Alipour/.ssh/id_rsa to view and copy your key).
        
      - Now, from the head node, establish your tunnel by adding the head node hostname (to see the head node hostname, run `hostname` in PW terminal):
        
        `ssh -i /home/<First.Last>/.ssh/id_rsa -Nf -R<local_port>:localhost:<local_port> <First.Last>@<cluster_ip_address>`
        
         for example:   ssh -i /home/Atieh.Alipour/.ssh/id_rsa -Nf -R8857:localhost:8857 mgmt-sorooshmani-nhccolab2-00017
        
      - Activate your virtual environment: `conda activate <env_name>` or `source /pathtominiconda3/bin/activate <env_name>`
      - Navigate to your directory where you want to open your Jupyter session. 
      - Start your Jupyter session with a chosen port number within the 8800-8900 range (e.g., 8857):
   
        `jupyter lab --no-browser --port=8857`
        
      - Open a local browser on your laptop/desktop and enter the provided URL. This should bring up the Jupyter interface (IDE).
      
