---
tags:
creation date: 2021-08-17 10:09
modification date: Tuesday 17th August 2021 10:09:01
---

[[00.00 Welcome]] 

# Infant FreeSurfer

[Source](https://surfer.nmr.mgh.harvard.edu/ftp/dist/freesurfer/infant/)

### Installation

1. Download from source
2. Unzip the .tar.gz
3. The infant version of `recon-all`, called `infant_recon_all`, is located in `infant-freesurfer/bin/infant_recon_all`
4. ??? Source the [[FreeSurfer]] environment: `source $FREESURFER_HOME/FreeSurferEnv.sh`
5. ??? Set up the directory for the subjects: `export SUBJECTS_DIR=/your/data/path`
6. ??? Run the Infant version of `recon-all`

### Output of `infant_recon_all --help`

```
REQUIRED FLAGGED ARGUMENTS

--s subjid
--subject subjid

FreeSurfer subject name as found in $SUBJECTS_DIR. This identifies the subject that is to be processed. The input file, unless indicated otherwise, should be located in $SUBJECTS_DIR/$s/mprage.nii.gz before processing is started.


--age age_in_months

Age of the FreeSurfer subject that is to be processed. 

  

OPTIONAL FLAGGED ARGUMENTS

--kneigh num

Number of training subjects to be used to acquire prior information about segmentation. Default is set to 4. 


--outdir directory

Name of the output directory where all the reconall results are written. The default is $SUBJECTS_DIR/$s/, where $s is the subject id of the subject to be reconned.
```

### Usage (infant_recon_all with more explanation than current documentation)

Before running `infant_recon_all`:
- Make sure `$FREESURFER_HOME` is set to the FS 7.1.1 directory AND exported
- Add `$FREESURFER_HOME` to `$PATH` and export it 
- Set the `$INFANT_FREESURFER` directory environment variable
- Run `bash $INFANT_FREESURFER/SetUpFreeSurfer.sh`
- Run `bash $INFANT_FREESURFER/FreeSurferEnv.sh`
- Set the `$SUBJECTS_DIR`

The directory structure IFS expects is very different than the structure FS uses. It expects

```
/path/to/subjects/
|- subject-01
|  |- mprage.nii.gz
```

where `$SUBJECTS_DIR` is `/path/to/subjects` and `subject-01` is passed to `infant_recon_all` as the subject id (SUBJECT_ID passed to `--s`). The command will look for `$SUBJECTS_DIR/SUBJECT_ID/mprage.nii.gz` as the input to process.

Once the proper environment variables are loaded and the directory structure is in place, you can run:

`infant_recon_all --s SUBJECT_ID --age AGE_IN_MONTHS --outdir OUTPUT_DIRECTORY`

Arguments:
- `--s SUBJECT_ID`: the name of the directory within `$SUBJECTS_DIR` containing the `mprage.nii.gz` to process. Context: `$SUBJECTS_DIR/SUBJECT_ID/mprage.nii.gz` is the input image
- `--age AGE_IN_MONTHS`: the age of the patient at the time of the scan in whole months. (Jenna tried using floats such as 10.2 as the argument here and got an error about a malformed number)
- `--outdir OUTPUT_DIRECTORY`: OPTIONAL ARGUMENT, used to specify an output directory. Defaults to `$SUBJECTS_DIR/SUBJECT_ID/` if not otherwise specified. 

Note from Jenna: I've been setting up my jobs on CUBIC by making the directory I want as the output, taking the directory of the desired output as the $SUBJECTS_DIR, taking the basename of the desired output as the SUBJECT_ID, and copying the input file to `/path/desired_output_directory/mprage.nii.gz`. After the job is run, I remove the mprage.nii.gz file from the desired output directory



### Where in the world are my statistics of interest?

IFS does some weird stuff with the statistics that normally live in aseg.stats. Most of the outputs live in `brainvol.stats`, but only most. For example, the number of holes (aka Euler number) can be found in separate files fro the left and right hemispheres under the `surf/` directory in `surf/lh.orig.euler.txt` and `surf/rh.orig.euler.txt`. The estimated total intracranial volume (eTIV) can be found in the `stats/lh.aparc.stats` or the `stats/rh.aparc.stats` files.

##### Example files

**stats/brainvol.stats**

```
# Measure BrainSeg, BrainSegVol, Brain Segmentation Volume, 912625.000000000000, mm^3
# Measure BrainSegNotVent, BrainSegVolNotVent, Brain Segmentation Volume Without Ventricles, 891945.000000000000, mm^3
# Measure SupraTentorial, SupraTentorialVol, Supratentorial volume, 793250.103639168665, mm^3
# Measure SupraTentorialNotVent, SupraTentorialVolNotVent, Supratentorial volume, 774647.103639168665, mm^3
# Measure SubCortGray, SubCortGrayVol, Subcortical gray matter volume, 43311.000000000000, mm^3
# Measure lhCortex, lhCortexVol, Left hemisphere cortical gray matter volume, 237669.267453678971, mm^3
# Measure rhCortex, rhCortexVol, Right hemisphere cortical gray matter volume, 236409.158171598508, mm^3
# Measure Cortex, CortexVol, Total cortical gray matter volume, 474078.425625277450, mm^3
# Measure TotalGray, TotalGrayVol, Total gray matter volume, 599496.425625277450, mm^3
# Measure lhCerebralWhiteMatter, lhCerebralWhiteMatterVol, Left hemisphere cerebral white matter volume, 128759.763500198227, mm^3
# Measure rhCerebralWhiteMatter, rhCerebralWhiteMatterVol, Right hemisphere cerebral white matter volume, 128216.914513693016, mm^3
# Measure CerebralWhiteMatter, CerebralWhiteMatterVol, Total cerebral white matter volume, 256976.678013891244, mm^3
# Measure Mask, MaskVol, Mask Volume, 1085015.000000000000, mm^3
# Measure SupraTentorialNotVentVox, SupraTentorialVolNotVentVox, Supratentorial volume voxel count, 774956.000000000000, mm^3
# Measure BrainSegNotVentSurf, BrainSegVolNotVentSurf, Brain Segmentation Volume Without Ventricles from Surf, 872937.103639168665, mm^3
# Measure VentricleChoroidVol, VentricleChoroidVol, Volume of ventricles and choroid plexus, 18603.000000000000, mm^3
```

For any measure in this file
- Read the file
- Find the first matching line containing the string representing the measure
- Split the line on "," and take the second to last element from that list.

**stats/lh.aparc.stats**

```
# Table of FreeSurfer cortical parcellation anatomical statistics 
# 
# CreationTime 2021/10/23-07:18:25-GMT
# generating_program mris_anatomical_stats
# cvs_version 7.1.1
# mrisurf.c-cvs_version 7.1.1
# cmdline mris_anatomical_stats -th3 -mgz -f /cbica/projects/bgdimagecentral/Data/clip-controls/derivatives/mpr_ifs_reconall_7.1.1//sub-HM9ZQ82X7/ses-19 00age00608/anat/sub-HM9ZQ82X7_ses-1900age00608_acq-MprPrimary3p0UnknownContrastFromScanner35008_run-01_T1w/work//sub-HM9ZQ82X7_ses-1900age00608_acq-MprP rimary3p0UnknownContrastFromScanner35008_run-01_T1w/stats/lh.aparc.stats -b -a aparc.annot -c aparc.annot.ctab sub-HM9ZQ82X7_ses-1900age00608_acq-MprPri mary3p0UnknownContrastFromScanner35008_run-01_T1w lh 
# sysname Linux
# hostname 2119fmn015
# machine x86_64
# user bgdimagecentral
# 
# SUBJECTS_DIR /cbica/projects/bgdimagecentral/Data/clip-controls/derivatives/mpr_ifs_reconall_7.1.1//sub-HM9ZQ82X7/ses-1900age00608/anat/sub-HM9ZQ82X7_ ses-1900age00608_acq-MprPrimary3p0UnknownContrastFromScanner35008_run-01_T1w/work
# anatomy_type surface
# subjectname sub-HM9ZQ82X7_ses-1900age00608_acq-MprPrimary3p0UnknownContrastFromScanner35008_run-01_T1w
# hemi lh
# AnnotationFile aparc.annot
# AnnotationFileTimeStamp 2021/10/23 02:32:56
# Measure Cortex, NumVert, Number of Vertices, 93620, unitless
# Measure Cortex, WhiteSurfArea, White Surface Total Area, 56631.5, mm^2
# BrainVolStatsFixed-NotNeeded because voxelvolume=1mm3
# Measure BrainSeg, BrainSegVol, Brain Segmentation Volume, 912625.000000, mm^3
# Measure BrainSegNotVent, BrainSegVolNotVent, Brain Segmentation Volume Without Ventricles, 891945.000000, mm^3
# Measure BrainSegNotVentSurf, BrainSegVolNotVentSurf, Brain Segmentation Volume Without Ventricles from Surf, 872937.103639, mm^3
# Measure Cortex, CortexVol, Total cortical gray matter volume, 474078.425625, mm^3
# Measure SupraTentorial, SupraTentorialVol, Supratentorial volume, 793250.103639, mm^3
# Measure SupraTentorialNotVent, SupraTentorialVolNotVent, Supratentorial volume, 774647.103639, mm^3
# Measure EstimatedTotalIntraCranialVol, eTIV, Estimated Total Intracranial Volume, 1034309.949257, mm^3
# NTableCols 10
# TableCol 1 ColHeader StructName
# TableCol 1 FieldName Structure Name
# TableCol 1 Units NA
# TableCol 2 ColHeader NumVert
# TableCol 2 FieldName Number of Vertices
# TableCol 2 Units unitless
# TableCol 3 ColHeader SurfArea
# TableCol 3 FieldName Surface Area
# TableCol 3 Units mm^2
# TableCol 4 ColHeader GrayVol
# TableCol 4 FieldName Gray Matter Volume
# TableCol 4 Units mm^3
# TableCol 5 ColHeader ThickAvg 
# TableCol 5 FieldName Average Thickness
# TableCol 5 Units mm
# TableCol 6 ColHeader ThickStd
# TableCol 6 FieldName Thickness StdDev
# TableCol 6 Units mm 
# TableCol 7 ColHeader MeanCurv
# TableCol 7 FieldName Integrated Rectified Mean Curvature
# TableCol 7 Units mm^-1
# TableCol 8 ColHeader GausCurv 
# TableCol 8 FieldName Integrated Rectified Gaussian Curvature
# TableCol 8 Units mm^-2
# TableCol 9 ColHeader FoldInd
# TableCol 9 FieldName Folding Index 
# TableCol 9 Units unitless 
# TableCol 10 ColHeader CurvInd
# TableCol 10 FieldName Intrinsic Curvature Index
# TableCol 10 Units unitless
# ColHeaders StructName NumVert SurfArea GrayVol ThickAvg ThickStd MeanCurv GausCurv FoldInd CurvInd
bankssts 686 458 1385 3.277 0.730 0.094 0.018 4 0.5
caudalanteriorcingulate 552 400 1277 2.871 0.983 0.120 0.022 5 0.5
caudalmiddlefrontal 2191 1346 5474 3.515 0.651 0.114 0.032 24 2.6
cuneus 1518 770 3582 3.367 0.752 0.126 0.047 24 2.9
entorhinal 183 117 422 4.636 0.411 0.082 0.014 1 0.1
fusiform 3327 2013 9587 3.763 0.805 0.124 0.036 51 4.7
inferiorparietal 4462 2737 12572 3.549 0.757 0.111 0.028 60 4.6
inferiortemporal 3565 2108 10786 3.677 0.828 0.114 0.030 41 4.2
isthmuscingulate 1451 939 3413 3.131 1.427 0.107 0.029 11 1.5
lateraloccipital 6373 3722 14392 3.210 0.772 0.121 0.038 84 9.4
lateralorbitofrontal 2538 1586 7385 3.685 0.727 0.107 0.028 22 2.8
lingual 3447 1956 8544 3.356 0.905 0.119 0.040 54 5.1
medialorbitofrontal 1518 914 5974 4.528 0.657 0.106 0.026 18 1.5
middletemporal 3463 2118 11552 3.701 0.709 0.114 0.031 43 3.9
parahippocampal 913 561 3114 3.770 0.820 0.116 0.034 13 1.2
paracentral 1493 941 3691 3.432 0.791 0.105 0.024 13 1.4     
parsopercularis 1526 933 4291 3.545 0.728 0.106 0.030 19 1.7
parsorbitalis 676 415 2328 3.594 0.814 0.129 0.043 11 1.1
parstriangularis 1334 834 3759 3.727 0.703 0.112 0.028 14 1.5
pericalcarine 1528 959 2512 2.501 0.851 0.105 0.027 13 1.6
postcentral 4796 2814 10035 2.944 0.911 0.114 0.033 62 6.1
posteriorcingulate 1322 864 3111 3.252 1.403 0.104 0.027 10 1.3
precentral 5381 3392 12179 3.127 0.773 0.115 0.028 60 5.7
precuneus 4638 2789 12578 3.526 0.891 0.120 0.033 74 5.7
rostralanteriorcingulate 610 391 1651 3.576 1.182 0.072 0.015 2 0.4
rostralmiddlefrontal 5684 3300 16181 3.557 0.733 0.125 0.039 75 8.7
superiorfrontal 6545 3870 20500 3.967 0.719 0.116 0.031 73 8.3
superiorparietal 6389 3959 15020 3.270 0.667 0.118 0.032 79 7.7
superiortemporal 3015 1809 7384 3.284 0.740 0.096 0.025 38 3.0
supramarginal 4000 2360 10897 3.576 0.729 0.118 0.033 59 5.3
frontalpole 231 123 1160 4.750 0.322 0.129 0.049 3 0.4
temporalpole 466 235 2922 4.538 0.604 0.105 0.038 7 0.6
transversetemporal 393 231 910 3.003 0.705 0.106 0.026 4 0.4
insula 2372 1604 7112 3.922 0.854 0.079 0.017 8 1.4
```

To get the EstimatedTotalIntraCranialVol:
- Read the file
- Find the first matching line containing the string "EstimatedTotalIntraCranialVol"
- Split the line by "," and take the second to last element from that list

**surf/lh.orig.euler.txt**

```
WARNING: the source volume info is not consistent between the info contained
WARNING: in the surface data (2.556458,3.742447,-18.412346) and that of the transform (2.556458,3.742462,-18.412346).
euler # = v-e+f = 2g-2: 93096 - 279336 + 186224 = -16 --> 9 holes
      F =2V-4:          186224 != 186192-4 (-36)
      2E=3F:            558672 = 558672 (0)

total defect index = 18
```

To get the number of holes, 
- Read the file
- Find the first matching line containing the string "holes"
- Get the characters between "-->" and "holes" and strip the whitespace off of it


[[Infant FreeSurfer]]