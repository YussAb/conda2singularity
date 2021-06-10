# conda2singularity
brief description of a method to simply build a singularity image from a conda environment.

## step 1

First, you'll want to get the environment YML for your particular conda environment.

```bash
conda activate your_env
conda env export > environment.yml
```

## step 2 

You create a definition file that will build a singularity image starting from environment.yml, using a miniconda's docker image.

(you can find this example as conda.def in this repository)

```
Bootstrap: docker

From: continuumio/miniconda3

%files
    environment.yml

%post
    /opt/conda/bin/conda env create -f environment.yml

%runscript
   echo ". /opt/conda/etc/profile.d/conda.sh" >> $SINGULARITY_ENVIRONMENT
   echo "conda activate $(head -n 1 environment.yml | cut -f 2 -d ' ')" >> $SINGULARITY_ENVIRONMENT
```

## step 3 

You build your singularity definition file to a singularity image

```bash
sudo singularity build conda.sif conda.def
```

## step 4

run command

```bash
singularity run conda.sif [command]
```

## References:

* https://stackoverflow.com/questions/54678805/containerize-a-conda-environment-in-a-singularity-container

* https://stackoverflow.com/questions/56446195/activate-conda-environment-on-execution-of-singularity-container-in-nextflow

* https://hub.docker.com/r/continuumio/miniconda3
