# MCFEP
Here are two example runs to get familiar with MCFEP, using the Schrodinger Suite, one for a small molecule, one for protein-protein. The essential idea is using pulling restraints to morph the initial lambda0 input into a target conformation, and replace by default the second half of the FEP windows with the pre-built target conformer. Due to the usage of symbolic links and Github not able to maintain them as-is, the runs are stored in mcfepExampleRuns.zip.

### Small molecule MCFEP, calculating relative binding energy of PDB:5J64 ligand to PDB: 5J9X ligand, experimental ddG=-1.5

To begin, your protein and ligands need to be prepared. In this example run, they have already been prepared with Maestro.

The next thing to do is using FEP+ in Maestro to setup the fep_plus input and save an .fmp file. In this example run, your input (5j64to5j9x.fmp) is provided.

1. Force Field Builder should be run to generate ligand dihedral parameters not already available. When you use FEP+ in Maestro to prepare your FEP input, you will see a warning if there are such missing ligand parameters. An ffbuilder stage can be added to the fep_plus workflow, however for a better workflow and file consistency, we separately run ffbuilder and directly use the output .opls file when launching your multisim jobs. In Maestro, open the .fmp file with FEP+, select the two ligands and launch Force Field Builder. Add the two ligands, and run this job in Maestro. You may also write the job and launch the job from the command line. The ffbuilder job should take about 2 hours on a single CPU. Once completed, under the \*oplsdir directory you will find a file named custom_202*.opls which is the parameter file to specify when launching fep_plus. Move this file to the launch directory. In this example run, a custom_2022_3_5j9x.opls file is provided so you won't need to run ffbuilder first, though you may need to change the metafile.json file in it to match your Schrodinger version. 

2. Generate the multisim restraints file for morphing your lambda0 complex into the lambda1 target, based on 5j9xTarget.pdb using command

        python3 common/gen_restraints.py 5j9xTarget.pdb  
   This will generate a restraints.tmpl file with a default force constant of 5 for further use.

3. Start the FEP run by running the fep.sh script
   
        ./fep.sh seed=2007 gpu=2
   The seed and gpu keywords are optional. Follow the printed out instruction messages, and also read the in-script comments for more information.

4. Additionally, in the solvLeg directory, run fepSolvLegOnly.sh to obtain the solvent leg energy. When using "assign" charges, the default option for ligand partial charges in the multisim workflow, the solvent leg must be using the same input fmp file, and therefore the identical input conformation as the complex leg ligands.




### Protein MCFEP, calculating relative binding energy of RBD/ACE2 complex Q498R mutation on RBD, experimental ddG=0.2

To begin, your proteins need to be prepared. In this example run, they have already been prepared with Maestro.

1. Generate the multisim restraints file for morphing your lambda0 complex into the lambda1 target, based on stateBtarget_alignedtorezeroed.pdb using command

        python3 common/gen_restraints.py stateBtarget_alignedtorezeroed.pdb
   This will generate a restraints.tmpl file with a default force constant of 5 for further use.

2. Start the FEP run by running the fep.sh script. Follow the instruction messages, and also read the in-script comments for more information.

3. Additionally, in the solvLeg directory, run fepSolvLegOnly.sh to obtain the solvent leg energy. 

### Notes 

The coordinates in the target structure file should be pre-aligned to the starting structure, otherwise the pulling process will have huge translational movements which should be avoided. However, if the starting structure is rezeroed during the workflow, its coordinates may change significantly and invalidate the pre-alignment. To acheive the effect of zeroing (default option) and avoid the coordinates change, the starting structures in these examples have been prezeroed, and have rezero turned off in the workflow. To prezero, in Maestro write a system builder job with the input starting structure, and without running the system builder job, the saved input mae file is already rezeroed. Then, align the target structure to the prezeroed file. Prezeroing is not mandatory, however, but smaller coordinates may help your simulation performance.


### For running a new system
The RMSD measurements criteria is system-specific and manually defined in 1.view.sh. This should be changed for a new system, if RMSD measurements (with cpptraj) are being used. For small molecule ligands, residues near it can be auto-detected once the ligand number and ligand residue name is provided, and autoregion=1 is set, in 1.view.sh.
Additionally, routine features like the settings of the solvent leg asl in protein FEP should also be changed as needed. The example runs try to be an inclusive template, but will not automatically include all run cases. Regular experience with Desmond simulations or FEP+ should be sufficient to facilitate the process. For any questions, you may send an email to liaojunzhuo AT gmail.com

### Citation and contact info
If you find our method helpful, please cite : J. Chem. Theory Comput. 2024, 20, 19, 8609–8623, https://doi.org/10.1021/acs.jctc.4c00954. For any assistance, please contact liaojunzhuo AT gmail.com
