  _____ ____    _  _____ _____  
 |_   _/ ___|  / \|_   _|_   _| 
   | || |     / _ \ | |   | |   
   | || |___ / ___ \| |   | |   
   |_| \____/_/   \_\_|   |_| 

If using this template for publication, please cite:

Archer, D.B., Coombes, S.A., McFarland, N.R., DeKosky, S.T., Vaillancourt, D.E. Development of a Transcallosal Tractography Template and its Application to Dementia. NeuroImage, 2019.

####################
###TCATT Template###
####################
This folder contains a total of 32 NIfTI files.  The below 32 files are binarized files which contain a single tract. Voxels included in each tract are represented with a value of 1. 

Angular_Gyrus_Final.nii.gz
Anterior_Orbital_Gyrus_Final.nii.gz
Calcarine_Sulcus_Final.nii.gz
Cuneus_Final.nii.gz
Gyrus_Rectus_Final.nii.gz
Inferior_Frontal_Gyrus_Pars_Opercularis_Final.nii.gz
Inferior_Frontal_Gyrus_Pars_Orbitalis_Final.nii.gz
Inferior_Frontal_Gyrus_Pars_Triangularis_Final.nii.gz
Inferior_Occipital_Final.nii.gz
Inferior_Parietal_Lobule_Final.nii.gz
Inferior_Temporal_Gyrus_Final.nii.gz
Lateral_Orbital_Gyrus_Final.nii.gz
Lingual_Gyrus_Final.nii.gz
M1_Final.nii.gz
Medial_Frontal_Gyrus_Final.nii.gz
Medial_Orbital_Gyrus_Final.nii.gz
Medial_Orbitofrontal_Gyrus_Final.nii.gz
Middle_Frontal_Gyrus_Final.nii.gz
Middle_Occipital_Final.nii.gz
Middle_Temporal_Gyrus_Final.nii.gz
Olfactory_Cortex_Final.nii.gz
Paracentral_Final.nii.gz
PMd_Final.nii.gz
PMv_Final.nii.gz
preSMA_Final.nii.gz
S1_Final.nii.gz
SMA_Final.nii.gz
Superior_Frontal_Gyrus_Final.nii.gz
Superior_Occipital_Final.nii.gz
Superior_Parietal_Lobule_Final.nii.gz
Superior_Temporal_Gyrus_Final.nii.gz
Supramarginal_Gyrus_Final.nii.gz

###To perform a slice-level analysis on an image in the MNI space, the below code structure could be used using the SMA_Final.nii.gz tract (for example). 

MNI image: MNI_FA.nii.gz
tract: SMA_Final.nii.gz

# Split 3D images in the z direction
fslsplit SMA_Final.nii.gz tract -z
fslsplit MNI_FA.nii.gz map -z

# Create a list of the 2D files created
ls tract* > tract_list.txt
ls map* > map_list.txt

# Combine lists to create a single list with two columns
paste map_list.txt tract_list.txt > analysis_list.txt

# For each slice
while read -r imgVar maskVar ; do
    # Calculate average value within mask
    FA=`3dROIstats -quiet -nzmean -mask $maskVar $imgVar | awk '{print $2}'`
    # Display results to screen
    echo "$maskVar - $FA"
done < analysis_list.txt
