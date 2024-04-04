# MCFEP

-1. To begin, your protein and ligands need to be prepared. In this example run, they have already been prepared with Maestro.

0. The next thing to do is using FEP+ in Maestro to setup the fep_plus input and save an .fmp file. In this example run, your input (5j64to5j9x.fmp) is provided.

1. Additionally, Force Field Builder should be run to generate ligand dihedral parameters not already available in OPLS4. When you use FEP+ in Maestro to prepare your FEP input, you will see a warning if there are such missing ligand parameters. An ffbuilder stage can be added to the fep_plus workflow, however for a better workflow and file consistency, we suggest to separately run ffbuilder and directly use the output .opls file when launching your multisim jobs. In Maestro, open the .fmp file with FEP+, select the two ligands and launch Force Field Builder. Add the two ligands, and run this job in Maestro. You may also write the job and launch the job from the command line. When doing so, you will see a folder with contents similar to that of in the ffb folder provided. The ffbuilder job should take about 2 hours on a single CPU.

2. Start the FEP run by running the fep.sh script and providing the 5j64to5j9x.fmp file. Follow the instruction messages, and also read the in-script comments for more information.
