# Advanced Data Analysis - Introduction to NGS data analysis
## Department of Animal and Plant Sciences, University of Sheffield
#### Nicola Nadeau, Alison Wright, Victor Soria-Carrasco

The aim of this course is to give an introduction to handling NGS sequence data on the UoS HPC cluster (ShARC) and to some of the analyses you might want to do including investigating gene expression and nucleotide variation (SNPs).


## Schedule 2018/2019

| Content | Date | Session | Venue | Lead |
| ------- | ---- | ------- | ----- | ---- |
| [Introduction to the HPC and NGS data](https://github.com/visoca/MolEcolStats-introNGSdata/blob/master/test.md) | Wed 20/02/2019 | Morning (9-12) | Hicks G25 | Nicola Nadeau |
| [Sequence data formats and assessing sequence quality](https://github.com/visoca/MolEcolStats-introNGSdata/blob/master/test.md) | Wed 20/02/2019 | Afternoon (2-5pm) | Geography B4 | Nicola Nadeau |
| [Aligning Illumina RNA-seq data](https://github.com/alielw/APS-NGS-day2-AM/blob/master/README.md) | Tue 21/02/2019  | Morning (9-12) | ADB - A04 (Perak) | Alison Wright |
| [Differential gene expression analyses](https://github.com/alielw/APS-NGS-day2-PM/blob/master/README.md) | Tue 21/02/2019  | Afternoon (2-5pm) | Diamond 2 | Alison Wright |
| [SNP and genotype calling](https://github.com/visoca/variant_calling) | Fri 22/02/2019 | Morning (9-12) | ADB - A04 (Perak) | Victor Soria-Carrasco |


### Before you start
Here are some websites that it is useful to have on hand (you might want to bookmark these so you can easily go back to them)

Linux and Shell cheatsheet (it's not cheating!): http://rcg.group.shef.ac.uk/courses/linux/shell-cheatsheet.html

CiCS page on using the ShARC cluster: https://www.sheffield.ac.uk/cics/research/hpc/sharc

CiCS page on interactive useage of the cluster: https://www.sheffield.ac.uk/cics/research/hpc/using/interactive

CiCS page on submitting jobs to the cluster (more on his later): https://www.sheffield.ac.uk/cics/research/hpc/sharc/batch

Documentation on file storage on ShARC http://docs.iceberg.shef.ac.uk/en/latest/hpc/filestore.html

The genomics software repository: http://soria-carrasco.staff.shef.ac.uk/softrepo/

#### Logging in and getting started
We will be working on windows machines, which means that you need to use a program (ssh client) to access the cluster. We will be using MobXterm. Start by opening the program, if you have used it before to connect to sharc you may find "sharc.sheff.ac.uk" under "User sessions", in which case you can just double click on this to launch an ssh session on sharc. If not, click on "Session">"SSH" and enter
```
sharc.sheffield.ac.uk
```
in the "Remote host" box and specify your username (port should always be 22).

Request an interactive session:
```bash
qrsh
```
You should always start by doing this. No work should ever be done on the head node! If you are on a head node you will see someting like this in your command line prompt:
```
[bo1nn@sharc-login1 ~]$
```
This node is just a gateway to the worker nodes. If you are on a worker node you will see the name of the node, eg.
```
[bo1nn@sharc-node004 ~]$
```
#### Important note
***
This tutorial relies on having access to a number of programs. The easiest way is to have your account configured to use the Genomics Software Repository. If that is the case you should see the following message when you get an interactive session with ```qrsh```:
```
  Your account is set up to use the Genomics Software Repository
     More info: http://soria-carrasco.staff.shef.ac.uk/softrepo
```
If you don't get that message, follow the instructions [here](http://soria-carrasco.staff.shef.ac.uk/softrepo/) to set up your account.

In addition, if you want to configure the ```nano``` text editor to have syntax highlighting and line numbering, you can configure it this way:
```bash
cat /usr/local/extras/Genomics/workshops/January2018/.nanorc >> /home/$USER/.nanorc
```
***

#### Note on transferring output files to your local computer for visualization
***
You probably will want to transfer files to your own computer for visualization (especially the images). In Linux and Mac, you can do that using rsync on the terminal. For example, to transfer one of the pdf files or all the results that are generated in this practical, the command would be: 
```bash
# transfer pdf file
rsync myuser@iceberg.sheffield.ac.uk:/data/myuser/gwas_gemma/output/hyperparameters.pdf ./
# transfer all results
rsync -av myuser@iceberg.sheffield.ac.uk:/data/myuser/gwas_gemma/output ./
```
Another possibility is to email the files, for example:
```bash
echo "Text body" | mail -s "Subject: gemma - hyperparameter plot" -a /data/myuser/gwas_gemma/output/hyperparameters.pdf your@email
```
If you are working on a windows machine and using MobaXterm then the easiest option is to use the graphical sftp panel on the left, using the icons or dragging and dropping from your computer. Other graphical alternatives are [WinSCP](http://dsavas.staff.shef.ac.uk/software/xconnect/winscp.html), [Filezilla](https://filezilla-project.org/) or [Cyberduck](http://www.macupdate.com/app/mac/8392/cyberduck). You can find more detailed information [here](https://www.sheffield.ac.uk/wrgrid/using/access).

***

