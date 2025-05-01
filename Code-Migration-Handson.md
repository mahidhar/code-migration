### Command Set #1 ###
<br>
Lets start with using module commands to search for the MATLAB software available. We can then check which environment variables are changed when one of these modules is loaded.

```
module spider matlab
module show matlab/2022b
```

<br>
Next we look for the ADIOS2 IO library and check for the dependencies that are needed to be loaded and the check the module dependencies that are automatically loaded.

```
module spider adios2
module reset
module load cpu/0.17.3b  gcc/10.2.0/npcyll4  openmpi/4.1.3/oq3qvsv
module load adios2/2.7.1
module list
module show adios2/2.7.1
```

### Command Set #2 ###

cp -r /cm/shared/examples/sdsc/pytorch .
cd pytorch
sbatch --res=complecs_day2 -A gue998 run-pytorch-cpu-shared.sh

check the output file!

### Command Set #3 ###

module reset

srun --pty --partition=shared --nodes=1 --ntasks-per-node=1 --cpus-per-task=8 --mem=16G -A gue998 --reservation=complecs_day2 -t 00:30:00 --wait 0 /bin/bash

export TMPDIR=/scratch/$USER/job_$SLURM_JOBID

cd /scratch/$USER/job_$SLURM_JOBID

module load singularitypro
singularity build excerpt.sif docker://rkitchen/excerpt

cp -r /cm/shared/examples/sdsc/excerpt/input .
mkdir output

singularity run --bind ./input:/exceRptInput --bind ./output:/exceRptOutput --bind /expanse/projects/qstore/data/excerpt/hg38:/exceRpt_DB/hg38 /cm/shared/apps/containers/singularity/excerpt/excerpt.sif INPUT_FILE_PATH=/exceRptInput/SRR026761.sra


### Command Set #4 ###

Set up our MPI compiler environment:

module reset
module load gcc/10.2.0
module load openmpi/4.1.3

Clone the code repository

git clone --branch v8.2.13 https://github.com/stamatak/standard-RAxML.git
cd standard-RAxML/

Inspect the Makefiles. In particular Makefile.AVX2.MPI.gcc

Then build!

make -f Makefile.AVX2.MPI.gcc
