---
title: Running on tiger
---

-   Build the environment on tigercpu:

    ``` {.example}
    module load anaconda3/2020.7
    wget https://github.com/CLIMADA-project/climada_python/archive/v2.0-beta.tar.gz
    tar xf v2.0-beta.tar.gz
    conda env create -f climada_python-2.0-beta/requirements/env_climada.yml climada_env
    conda activate climada_env
    conda install -c conda-forge notebook
    ```

-   Prepare a notebook as an example. I used this example

    ``` {.example}
    climada_python-2.0-beta/doc/tutorial/1_main_climada.ipynb
    ```

    but I had to comment out 2 cells because they were downloading data
    from the internet but the compute nodes don\'t have access to the
    internet. If you search for the string you will find them in the
    notebook:

    ``` {.python}
    # %matplotlib inline
    # ent_fl.exposures.plot_basemap(buffer=50000.0); # exposures in Florida
    ```

    and

    ``` {.python}
    # imp_fl.plot_basemap_eai_exposure(buffer=50000.0); # average annual impact at each exposure
    ```

-   Create a slurm script. This is an example for running for 10 min:

    ``` {.bash org-language="sh"}
    #! /bin/bash
    #SBATCH -N 1
    #SBATCH -n 1
    #SBATCH -t 00:10:00

    module load anaconda3
    conda activate climada_env

    jupyter nbconvert --to notebook --execute 1_main_climada.ipynb
    ```

    I called this script `slurm.sh`, you can call it whatever you want.

-   Put the script in the queue, at the terminal, enter:

    ``` {.example}
    sbatch slurm.sh
    ```

-   The job is sent to the queue. It probably won\'t execute right away.
    You can check its status with:

    ``` {.example}
    squeue -u netid
    ```

    where you have to replace `netid` with you actual netif (i.e user
    name on tigercpu).

-   when the job is done, it creates a new notebook called
    `1_main_climada.nbconvert.ipynb`. It contains the initial notebook
    along with the output cells.
