# **Phantoms-For-Nuclear-Medicine-Imaging-Application**

The different digitial phantoms for medical imaging application are developed and maintained by Auer Benjamin from the Brigham and Women's Hospital and Harvard Medical School, Boston, MA, USA, and Pells Sophia from the University of Massachussets Chan Medical School, Worcester, MA, USA.

**Contact:** Auer, Benjamin Ph.D <bauer@bwh.harvard.edu>

Table of contents:
```diff
- 1. Objective
- 2. Defrise Phantom
  - 2.1 Description
  - 2.2 Usage in GATE
- 3. Derenzo Phantom
  - 3.1 Description
  - 3.2 Usage in GATE
- 4. Jaszczak Phantom
  - 4.1 Description
  - 4.2 Usage in GATE
- 5. Jaszczak Phantom with fillable rods and spheres
  - 5.1 Description
  - 5.2 Usage in GATE
- 6. Brain Perfusion
  - 6.1 Description
  - 6.2 Usage in GATE  
- 7. Brain Dopamine DaT
  - 7.1 Description
  - 7.2 Usage in GATE 
- 8. Brain Tumor/GlioBlastoma
  - 8.1 Description
  - 8.2 Usage in GATE  
- 9. Lu-177 DOTATATE patient data
  - 9.1 Description
  - 9.2 Usage in GATE    
```

# 1. Objective

###### Objective
The digital phantoms described and distributed below can be incorporated in simulation software, such as [GATE](http://www.opengatecollaboration.org) for emulating nuclear medicine acquisition. We provide a Defrise, Derenzo, and Jaszczak phantoms used in quality control procedures. Additionaly, we provide digital phantoms emulating clinical brain perfusion, dopamine transporter, and glioblastoma radiotracer distribution. 

The open-source **mesh50_XCAT** phantom based on the 4D extended cardiac-torso digital anthropomorphic ([XCAT](https://otc.duke.edu/industry-investors/available-technologies/xcat/)) human phantom can be downloaded [here](https://github.com/BenAuer2021/Mesh-based-Human-Phantom-for-Simulation/edit/main/README.md).

# 2. Defrise Phantom

## 2.1 Description

We provide the Defrise attenuation and activity voxelized phantoms in interfile format (*16-bit unsigned integer, \*.i33 for raw data and \*.h33 for the header files*). The 20-cm diameter cylindrical Defrise phantom can be used to estimate axial sampling of nuclear medicine imaging systems. It is based on the Defrise Insert<sup>TM</sup> Model ECT/DEF/[I](https://www.spect.com/wp-content/uploads/2020/06/Micro-Defrise-Phantom.pdf) from Data Spectrum Corporation, Durham, NC, USA. 

<p align="center">
  <img width="1191" alt="Screen Shot 2023-06-06 at 3 42 03 PM" src="https://github.com/BenAuer2021/Phantoms-For-Nuclear-Medicine-Imaging-Simulation/assets/84809217/6d1ebc0e-0ae9-4ea6-915d-f469efc60d5a">
</p>

The 20-cm long Defrise phantom consists of a series of 13 hot disks (20-cm diameter, 8 mm in length) evenly spaced 8 mm apart along the axial direction. The Defrise activity phantom (**Defrise_200x200x200.i33**) can be simulated as being filled with uniform tracer activity throughout the volume of the hot disks. The integer values for the hot and cold disk are set to 10 and 0, respectively. It consists of 200x200x200 voxels of 1 mm<sup>3</sup> (size of 16 MB).
  
The voxelized attenuation phantom (**Defrise_Atn_200x200x200.i33**) can be used in attenuation correction for SPECT or PET reconstruction and/or as attenuation media for simulation. The integer values for the hot and cold disk are set to 1 and 0, respectively. Similarly to the activity phantom, it consists of 200x200x200 voxels of 1 mm<sup>3</sup> (size of 16 MB).

## 2.2 Usage in GATE

The voxelized phantoms (*interfile format*) can be loaded in GATE via the following command lines for a <sup>99m</sup>Tc source, where *'VoxSource'* is the source volume name,
```ruby
/gate/source/addSource VoxSource voxel
/gate/source/VoxSource/reader/insert image
/gate/source/VoxSource/imageReader/translator/insert linear
/gate/source/VoxSource/imageReader/linearTranslator/setScale 0.03 Bq
/gate/source/VoxSource/imageReader/readFile PATH_TO/Defrise_200x200x200.h33
/gate/source/VoxSource/imageReader/verbose 1
/gate/source/VoxSource/gps/particle gamma
/gate/source/VoxSource/gps/ang/type iso
/gate/source/VoxSource/gps/ang/mintheta 0.0 deg
/gate/source/VoxSource/gps/ang/maxtheta 180.0 deg
/gate/source/VoxSource/gps/ang/minphi 0.0  deg
/gate/source/VoxSource/gps/ang/maxphi 360.0 deg
/gate/source/VoxSource/gps/energytype Mono
/gate/source/VoxSource/gps/ene/mono 140.5 keV # For Tc-99m
/gate/source/VoxSource/setIntensity 1
/gate/source/VoxSource/setPosition -100.0 -100.0 -100.0 mm
/gate/source/VoxSource/dump 1
```
The default position of the voxelized source is in the 1<sup>st</sup> quarter, so the voxelized source has to be shifted over half its dimension in the negative direction on each axis. As the voxel size is 1 mm<sup>3</sup>, dimensions of the image (*provided in the file name*) need to be divided by two along X, Y, Z dimensions.

The voxelized attenuation phantom can be used as attenuation map in GATE via the following command lines. Note, the voxelized phantom should **A)** not collide with any other system components and **B)** be contained entirely within its *'mother'* volume (*e.g., world here*). 
```ruby
/gate/world/daughters/name VoxAttn
/gate/world/daughters/insert ImageNestedParametrisedVolume # Or ImageRegularParametrisedVolume
/gate/VoxAttn/geometry/setImage Defrise_Atn_200x200x200.h33
/gate/VoxAttn/geometry/setRangeToMaterialFile Attenuation_Defrise_Range.dat
/gate/VoxAttn/placement/setTranslation  0. 0. 0. cm
/gate/VoxAttn/attachPhantomSD
```

Indices in the voxelized attenuation image are translated into materials via the parameters defined in the *'Attenuation_Defrise_Range.dat'* file. For example, the following indices to materials conversion,
```ruby
2
0 0 Air
1 1 Water
```
# 3. Derenzo Phantom

## 3.1 Description

We provide the Derenzo attenuation and activity voxelized phantoms in interfile format (*16-bit unsigned integer, \*.i33 for raw data and \*.h33 for the header files*). The Derenzo phantom can be used to estimate the tomographic spatial resolution of nuclear medicine imaging systems.

<p align="center">
  <img width="857" alt="Screen Shot 2023-06-06 at 4 50 43 PM" src="https://github.com/BenAuer2021/Phantoms-For-Nuclear-Medicine-Imaging-Simulation/assets/84809217/62392420-cfc9-441e-9820-6e13a0bc3478">
</p>

This cylindrical phantom of 22 cm diameter by 16 cm in height, was adapted from the rod
region of the ultra-deluxe Jaszczak phantom<sup>TM</sup> from [Data Spectrum Corporation]
(http://www.spect.com/products-all.html). The phantom consists of hot rods and an active uniform background region with a contrast ratio of 10:1 (**Derenzo_220x220x161_with_bkg.i33**). We also provide the activity phantom without background activity (**Derenzo_203x191x161_without_bkg.i33**). The phantom consists of six sets of rod sources with diameters of 11.1, 9.5, 7.9, 6.4, 4.8, and 3.2 mm. The centre-to-centre distance between two adjacent rods was equal to two times the rod diameter.

The Derenzo activity phantom can be simulated as being filled with uniform tracer activity throughout the volume of the hot rods and with or without background activity. The integer values for the hot rods and cold regions are set to 10 and 0, respectively. The background value is set to 1. It consists of 203x191x161 voxels of 1 mm<sup>3</sup> (size of 12.5 MB) for the version without active background. It consists of 220x220x161 voxels of 1 mm<sup>3</sup> (size of 15.6 MB) for the version **with** active background.
  
The voxelized attenuation phantom (**Derenzo_222x222x161_Attn.i33**) can be used for attenuation correction for SPECT or PET reconstruction and/or as attenuation media for simulation. The integer values are set to 1 within the phantom (as entirely filled with water) and 0 outside, respectively. It consists of 222x222x161 voxels of 1 mm<sup>3</sup> (size of 15.9 MB).

## 3.2 Usage in GATE

The voxelized phantoms (*interfile format*) can be loaded in GATE via the following command lines for a <sup>99m</sup>Tc source, where *'VoxSource'* is the source volume name,
```ruby
/gate/source/addSource VoxSource voxel
/gate/source/VoxSource/reader/insert image
/gate/source/VoxSource/imageReader/translator/insert linear
/gate/source/VoxSource/imageReader/linearTranslator/setScale 0.03 Bq
/gate/source/VoxSource/imageReader/readFile PATH_TO/Derenzo_203x191x161_without_bkg.h33 #w/o bkg
#/gate/source/VoxSource/imageReader/readFile PATH_TO/Derenzo_220x220x161_with_bkg.h33 #w bkg
/gate/source/VoxSource/imageReader/verbose 1
/gate/source/VoxSource/gps/particle gamma
/gate/source/VoxSource/gps/ang/type iso
/gate/source/VoxSource/gps/ang/mintheta 0.0 deg
/gate/source/VoxSource/gps/ang/maxtheta 180.0 deg
/gate/source/VoxSource/gps/ang/minphi 0.0  deg
/gate/source/VoxSource/gps/ang/maxphi 360.0 deg
/gate/source/VoxSource/gps/energytype Mono
/gate/source/VoxSource/gps/ene/mono 140.5 keV # For Tc-99m
/gate/source/VoxSource/setIntensity 1
/gate/source/VoxSource/setPosition -101.5 -95.5 -80.5 mm #w/o bkg
#/gate/source/VoxSource/setPosition -110.0 -110.0 -80.5 mm #w bkg
/gate/source/VoxSource/dump 1
```
The default position of the voxelized source is in the 1<sup>st</sup> quarter, so the voxelized source has to be shifted over half its dimension in the negative direction on each axis. As the voxel size is 1 mm<sup>3</sup>, dimensions of the image (*provided in the file name*) need to be divided by two along X, Y, Z dimensions.

The voxelized attenuation phantom can be used as attenuation map in GATE via the following command lines. Note, the voxelized phantom should **A)** not collide with any other system components and **B)** be contained entirely within its *'mother'* volume (*e.g., world here*). 
```ruby
/gate/world/daughters/name VoxAttn
/gate/world/daughters/insert ImageNestedParametrisedVolume # Or ImageRegularParametrisedVolume
/gate/VoxAttn/geometry/setImage Derenzo_222x222x161_Attn.h33
/gate/VoxAttn/geometry/setRangeToMaterialFile Attenuation_Derenzo_Range.dat
/gate/VoxAttn/placement/setTranslation  0. 0. 0. cm
/gate/VoxAttn/attachPhantomSD
```

Indices in the voxelized attenuation image are translated into materials via the parameters defined in the *'Attenuation_Derenzo_Range.dat'* file. For example, the following indices to materials conversion,
```ruby
2
0 0 Air
1 1 Water
```

# 4. Jaszczak Phantom

## 4.1 Description

We provide the Jaszczak attenuation and activity voxelized phantoms in interfile format (*16-bit unsigned integer, \*.i33 for raw data and \*.h33 for the header files*). The Jaszczak phantom is used to estimate tomographic uniformity, contrast, and spatial resolution in SPECT quality control procedures.

<p align="center">
<img width="872" alt="Screen Shot 2023-06-06 at 5 50 07 PM" src="https://github.com/BenAuer2021/Phantoms-For-Nuclear-Medicine-Imaging-Simulation/assets/84809217/2bce2ebd-0263-4e17-9ea8-ee2a5bd34d03">
</p>

This cylindrical phantom of 22 cm diameter by 16.7 cm in height, was adapted from a CT acquisition of a deluxe Jaszczak phantom<sup>TM</sup> from [Data Spectrum Corporation](http://www.spect.com/products-all.html). The phantom consists of 3 sectors, one uniform, one with cold spheres, and another one with cold rods. The spheres are 9.5, 12.7, 15.9, 19.1, 25.4, and 31.8 mm in diameter. The rods are 4.8, 6.4, 7.9, 9.5, 11.1, and 12.7 mm in diameter. The centre-to-centre distance between two adjacent rods was equal to two times the rod diameter.

The Jaszczak activity phantom (**Jaszczak_238x237x134.i33**) can be simulated as being filled with uniform tracer activity. The integer values for the hot and cold regions are set to 10 and 0, respectively. It consists of 238x237x134 voxels of 0.939453x0.939453x1.25 mm<sup>3</sup> (size of 15.1 MB).
  
The voxelized attenuation phantom (**Jaszczak_238x237x134_Atn.i33**) can be used for attenuation correction for SPECT or PET reconstruction and/or as attenuation media for simulation. The integer values are set to 1 within the phantom (as entirely filled with water except the rod/sphere regions) and 0 outside, respectively. It consists of 238x237x134 voxels of 0.939453x0.939453x1.25 mm<sup>3</sup> (size of 15.1 MB). The attenuation phantom cannot make use of any interpolation counter to the activity phantom as it must strictly represent materials as binary values (0 for material 1, 1 for material 2, 3 for material 3,...), without the use of floating-point numbers. Thus, eventhough they show similar distribution on the figure above the same voxelized phantoms cannot be used for source and attenuation definition.

## 4.2 Usage in GATE

The voxelized phantoms (*interfile format*) can be loaded in GATE via the following command lines for a <sup>99m</sup>Tc source, where *'VoxSource'* is the source volume name,
```ruby
/gate/source/addSource VoxSource voxel
/gate/source/VoxSource/reader/insert image
/gate/source/VoxSource/imageReader/translator/insert linear
/gate/source/VoxSource/imageReader/linearTranslator/setScale 0.03 Bq
/gate/source/VoxSource/imageReader/readFile PATH_TO/Jaszczak_238x237x134.h33
/gate/source/VoxSource/imageReader/verbose 1
/gate/source/VoxSource/gps/particle gamma
/gate/source/VoxSource/gps/ang/type iso
/gate/source/VoxSource/gps/ang/mintheta 0.0 deg
/gate/source/VoxSource/gps/ang/maxtheta 180.0 deg
/gate/source/VoxSource/gps/ang/minphi 0.0  deg
/gate/source/VoxSource/gps/ang/maxphi 360.0 deg
/gate/source/VoxSource/gps/energytype Mono
/gate/source/VoxSource/gps/ene/mono 140.5 keV # For Tc-99m
/gate/source/VoxSource/setIntensity 1
/gate/source/VoxSource/setPosition -223.589814 -222.650361 -167.5 mm
/gate/source/VoxSource/dump 1
```
The default position of the voxelized source is in the 1<sup>st</sup> quarter, so the voxelized source has to be shifted over half its dimension in the negative direction on each axis. As the voxel size is 1 mm<sup>3</sup>, dimensions of the image (*provided in the file name*) need to be divided by two along X, Y, Z dimensions.

The voxelized attenuation phantom can be used as attenuation map in GATE via the following command lines. Note, the voxelized phantom should **A)** not collide with any other system components and **B)** be contained entirely within its *'mother'* volume (*e.g., world here*). 
```ruby
/gate/world/daughters/name VoxAttn
/gate/world/daughters/insert ImageNestedParametrisedVolume # Or ImageRegularParametrisedVolume
/gate/VoxAttn/geometry/setImage Jaszczak_238x237x134_Atn.h33
/gate/VoxAttn/geometry/setRangeToMaterialFile Attenuation_Jaszczak_Range.dat
/gate/VoxAttn/placement/setTranslation  0. 0. 0. cm
/gate/VoxAttn/attachPhantomSD
```

Indices in the voxelized attenuation image are translated into materials via the parameters defined in the *'Attenuation_Jaszczak_Range.dat'* file. For example, the following indices to materials conversion,
```ruby
2
0 0 Air
1 1 Water
```

# 5. Jaszczak Phantom with fillable rods and spheres

## 5.1 Description

We provide the fillable Jaszczak attenuation and activity voxelized phantoms in interfile format (*16-bit unsigned integer, \*.i33 for raw data and \*.h33 for the header files*). The fillable Jaszczak phantom can be used to estimate tomographic uniformity, contrast, and spatial resolution in SPECT and PET. Counter to the regular Jaszczak phantom described in the section 4 above, it simulates spheres and rods filled with activity (instead of cold).

<p align="center">
<img width="700" alt="Screen Shot 2023-06-06 at 5 49 48 PM" src="https://github.com/BenAuer2021/Phantoms-For-Nuclear-Medicine-Imaging-Simulation/assets/84809217/3b0bfd85-8cb8-4edd-90da-499c29247dd1">
</p>

This cylindrical phantom of 22 cm diameter by 16.7 cm in height, was adapted from a CT acquisition of a deluxe Jaszczak phantom<sup>TM</sup> from [Data Spectrum Corporation](http://www.spect.com/products-all.html). The phantom consists of 3 sectors, one uniform, one with hot spheres, and another **one** with **hot** rods. The spheres are 9.5, 12.7, 15.9, 19.1, 25.4, and 31.8 mm in diameter. The rods are 4.8, 6.4, 7.9, 9.5, 11.1, and 12.7 mm in diameter. The centre-to-centre distance between two adjacent rods was equal to two times the rod diameter.

The Jaszczak activity phantom can be simulated with (**Fillable_Jaszczak_238x237x134_with_bkg.i33**) and without (**Fillable_Jaszczak_238x237x134_without_bkg.i33**) background activity as being filled with uniform tracer activity in the rod, and sphere volumes. The integer values for the rods/spheres and cold regions are set to 10 and 0, respectively. The background value is 0 (no activity) or 1 (activity). It consists of 238x237x134 voxels of 0.939453x0.939453x1.25 mm<sup>3</sup> (size of 15.1 MB).
  
The voxelized attenuation phantom (**Fillable_Jaszczak_238x237x134_Atn.i33**) can be used for attenuation correction for SPECT or PET reconstruction and/or as attenuation media for simulation. The integer values are set to 1 within the phantom (as entirely filled with water) and 0 outside, respectively. It consists of 238x237x134 voxels of 0.939453x0.939453x1.25 mm<sup>3</sup> (size of 15.1 MB). The attenuation phantom cannot make use of any interpolation counter to the activity phantom as it must strictly represent materials as binary values (0 for material 1, 1 for material 2, 3 for material 3,...), without the use of floating-point numbers. Thus, eventhough they show similar distribution on the figure above the same voxelized phantoms cannot be used for source and attenuation definition.

## 5.2 Usage in GATE

The voxelized phantoms (*interfile format*) can be loaded in GATE via the following command lines for a <sup>99m</sup>Tc source, where *'VoxSource'* is the source volume name,
```ruby
/gate/source/addSource VoxSource voxel
/gate/source/VoxSource/reader/insert image
/gate/source/VoxSource/imageReader/translator/insert linear
/gate/source/VoxSource/imageReader/linearTranslator/setScale 0.03 Bq
/gate/source/VoxSource/imageReader/readFile PATH_TO/Fillable_Jaszczak_238x237x134_with_bkg.h33
#/gate/source/VoxSource/imageReader/readFile PATH_TO/Fillable_Jaszczak_238x237x134_without_bkg.h33 #w/o background
/gate/source/VoxSource/imageReader/verbose 1
/gate/source/VoxSource/gps/particle gamma
/gate/source/VoxSource/gps/ang/type iso
/gate/source/VoxSource/gps/ang/mintheta 0.0 deg
/gate/source/VoxSource/gps/ang/maxtheta 180.0 deg
/gate/source/VoxSource/gps/ang/minphi 0.0  deg
/gate/source/VoxSource/gps/ang/maxphi 360.0 deg
/gate/source/VoxSource/gps/energytype Mono
/gate/source/VoxSource/gps/ene/mono 140.5 keV # For Tc-99m
/gate/source/VoxSource/setIntensity 1
/gate/source/VoxSource/setPosition -223.589814 -222.650361 -167.5 mm
/gate/source/VoxSource/dump 1
```
The default position of the voxelized source is in the 1<sup>st</sup> quarter, so the voxelized source has to be shifted over half its dimension in the negative direction on each axis. As the voxel size is 1 mm<sup>3</sup>, dimensions of the image (*provided in the file name*) need to be divided by two along X, Y, Z dimensions.

The voxelized attenuation phantom can be used as attenuation map in GATE via the following command lines. Note, the voxelized phantom should **A)** not collide with any other system components and **B)** be contained entirely within its *'mother'* volume (*e.g., world here*). 
```ruby
/gate/world/daughters/name VoxAttn
/gate/world/daughters/insert ImageNestedParametrisedVolume # Or ImageRegularParametrisedVolume
/gate/VoxAttn/geometry/setImage Fillable_Jaszczak_238x237x134_Atn.h33
/gate/VoxAttn/geometry/setRangeToMaterialFile Attenuation_Jaszczak_Range.dat
/gate/VoxAttn/placement/setTranslation  0. 0. 0. cm
/gate/VoxAttn/attachPhantomSD
```

Indices in the voxelized attenuation image are translated into materials via the parameters defined in the *'Attenuation_Jaszczak_Range.dat'* file. For example, the following indices to materials conversion,
```ruby
2
0 0 Air
1 1 Water
```

<a name="BrainPerfusion"></a>
# [6. Brain Perfusion](#BrainPerfusion)

## 6.1 Description

We provide the brain perfusion attenuation and activity voxelized phantoms in interfile format (*16-bit unsigned integer, \*.i33 for raw data and \*.h33 for the header files*). The brain perfusion phantom can emulate a clinical <sup>99mTc</sup> ECD/HMPAO or <sup>123I</sup>-IMP brain perfusion distribution as imaged 1h post injection. It was derived from the open-access [Oasis phantom library](https://www.oasis-brains.org).
For additional information, please see below references,
> - PJ LaMontagne, TLS Benzinger, JC Morris, S Keefe, R Hornbeck, C Xiong, E Grant, J Hassenstab, K Moulder, A Vlassenko, ME Raichle, C Cruchaga, and D Marcus (2019). [OASIS 3: Longitudinal Neuroimaging, Clinical, and Cognitive Dataset for Normal Aging and Alzheimer Disease]([https://iopscience.iop.org/article/10.1088/1361-6560/acbde2](https://www.medrxiv.org/content/10.1101/2019.12.13.19014902v1)), medRxiv.

> - C Lindsay, B Auer, Y Yang, LR Furenlid, and and MA King (2018, November). Creation of a population of patient phantoms for deep learning-based denoising of spect brain imaging. In 2019, 7th International Workshop on Computational Human Phantoms.

<p align="center">
<img width="700" alt="Screen Shot 2023-06-20 at 10 44 55 PM" src="https://github.com/BenAuer2021/Phantoms-For-Nuclear-Medicine-Imaging-Simulation/assets/84809217/f5e26af5-a280-4526-bd11-482d4af3d34a">
</p>

The brain perfusion activity phantom can be simulated  (**Brain_Perfusion_Source_120x120x120.i33**) as being filled with uniform tracer activity in striatum:grey matter:white matter : lungs : liver : background of 10.1:9.3:2.3:6.0:11.4:1. The integer values are set to 10 for the soft tissue, 100 for the skin, 230 for the white matter, 920 for the grey matter, and 1012 for the striatum. It consists of 120x120x120 voxels of 2.0x2.0x2.0 mm<sup>3</sup> (size of 3.5 MB).
  
The voxelized attenuation phantom (**Brain_Perfusion_Attn_72x90x77.i33**) can be used for attenuation correction for SPECT or PET reconstruction and/or as attenuation media for simulation. The integer values are set to 0 for the air, 1 for the skull, 2 for the soft tissue, and 3 for the brain. It consists of 72x90x77 voxels of 2x2x2 mm<sup>3</sup> (size of 998 kB). The air volume surrounding the head was reduced to the maximum to avoid overlap with the system components in GATE or any other simulation software. The attenuation phantom cannot make use of any interpolation counter to the activity phantom as it must strictly represent materials as binary values (0 for material 1, 1 for material 2, 3 for material 3,...), without the use of floating-point numbers. Thus, eventhough they show similar distribution on the figure above the same voxelized phantoms cannot be used for source and attenuation definition.

<a name="SourceI123"></a>
## [6.2 Usage in GATE](#SourceI123)

The voxelized phantoms (*interfile format*) can be loaded in GATE via the following command lines for a <sup>99m</sup>Tc source, where *'VoxSource'* is the source volume name,
```ruby
/gate/source/addSource VoxSource voxel
/gate/source/VoxSource/reader/insert image
/gate/source/VoxSource/imageReader/translator/insert linear
/gate/source/VoxSource/imageReader/linearTranslator/setScale 0.03 Bq
/gate/source/VoxSource/imageReader/readFile PATH_TO/Brain_Perfusion_Source_120x120x120.h33
/gate/source/VoxSource/imageReader/verbose 1
/gate/source/VoxSource/gps/particle gamma
/gate/source/VoxSource/gps/ang/type iso
/gate/source/VoxSource/gps/ang/mintheta 0.0 deg
/gate/source/VoxSource/gps/ang/maxtheta 180.0 deg
/gate/source/VoxSource/gps/ang/minphi 0.0  deg
/gate/source/VoxSource/gps/ang/maxphi 360.0 deg
/gate/source/VoxSource/gps/energytype Mono
/gate/source/VoxSource/gps/ene/mono 140.5 keV # For Tc-99m
#/gate/source/VoxSource/gps/ene/mono 159.0 keV # For I-123 primary emission
/gate/source/VoxSource/setIntensity 1
/gate/source/VoxSource/setPosition -120.0 -120.0 -120.0 mm
/gate/source/VoxSource/dump 1
```
The default position of the voxelized source is in the 1<sup>st</sup> quarter, so the voxelized source has to be shifted over half its dimension in the negative direction on each axis. As the voxel size is 1 mm<sup>3</sup>, dimensions of the image (*provided in the file name*) need to be divided by two along X, Y, Z dimensions.

The following example shows how to define a source of <sup>123</sup>I gammas. The gamma emission data is from the IAEA database: https://www-nds.iaea.org/relnsd/vcharthtml/VChartHTML.html.
Note that this example shows the gamma emissions only, so the simulation output will be missing the X-ray peaks seen in true <sup>123</sup>I spectra. 

`VS_gamma` is the name of the voxelised gamma source 
```ruby
# Set verbosity (2 = every event)
# Good to set to 2 initially to check output is as expected
/gate/source/VS_gamma/gps/verbose 0

/gate/source/addSource VS_gamma voxel
/gate/source/VS_gamma/reader/insert image
/gate/source/VS_gamma/imageReader/translator/insert linear
/gate/source/VS_gamma/imageReader/linearTranslator/setScale 0.05 Bq
/gate/source/VS_gamma/imageReader/readFile PATH_TO/Brain_Perfusion_Source_120x120x120.h33
/gate/source/VS_gamma/imageReader/verbose 1
/gate/source/VS_gamma/gps/particle gamma
/gate/source/VS_gamma/gps/ang/type iso
/gate/source/VS_gamma/setPosition -120.0 -120.0 -120.0 mm

# Force unstable
/gate/source/VS_gamma/setForcedUnstableFlag  true
/gate/source/VS_gamma/setForcedHalfLife 47602.8 s

/gate/source/VS_gamma/gps/particle    gamma
/gate/source/VS_gamma/gps/ene/type  User
/gate/source/VS_gamma/gps/hist/type    energy

/gate/source/VS_gamma/gps/ene/min  0.150 MeV
/gate/source/VS_gamma/gps/ene/max  1.037 MeV

# ------------------hist of emissions----------------------------- #
/gate/source/VS_gamma/gps/hist/point 0.1589999 0.0
/gate/source/VS_gamma/gps/hist/point 0.159 83.6
/gate/source/VS_gamma/gps/hist/point 0.1590001 0.0

/gate/source/VS_gamma/gps/hist/point 0.17419999 0.0
/gate/source/VS_gamma/gps/hist/point 0.1742 0.0008
/gate/source/VS_gamma/gps/hist/point 0.1742001 0.0

/gate/source/VS_gamma/gps/hist/point 0.18261999 0.0
/gate/source/VS_gamma/gps/hist/point 0.18262 0.013
/gate/source/VS_gamma/gps/hist/point 0.18262001 0.0

/gate/source/VS_gamma/gps/hist/point 0.19069999 0.0
/gate/source/VS_gamma/gps/hist/point 0.1907 0.0005
/gate/source/VS_gamma/gps/hist/point 0.19070001 0.0

/gate/source/VS_gamma/gps/hist/point 0.19217999 0.0
/gate/source/VS_gamma/gps/hist/point 0.19218 0.0177
/gate/source/VS_gamma/gps/hist/point 0.19218001 0.0

/gate/source/VS_gamma/gps/hist/point 0.1972299 0.0
/gate/source/VS_gamma/gps/hist/point 0.19723 0.00033
/gate/source/VS_gamma/gps/hist/point 0.19723001 0.0

/gate/source/VS_gamma/gps/hist/point 0.19822999 0.0
/gate/source/VS_gamma/gps/hist/point 0.19823 0.0033
/gate/source/VS_gamma/gps/hist/point 0.19823001 0.0

/gate/source/VS_gamma/gps/hist/point 0.206799 0.0
/gate/source/VS_gamma/gps/hist/point 0.2068 0.0033
/gate/source/VS_gamma/gps/hist/point 0.2068001 0.0

/gate/source/VS_gamma/gps/hist/point 0.20779999 0.0
/gate/source/VS_gamma/gps/hist/point 0.2078 0.0011
/gate/source/VS_gamma/gps/hist/point 0.2078001 0.0

/gate/source/VS_gamma/gps/hist/point 0.247969999 0.0
/gate/source/VS_gamma/gps/hist/point 0.24797 0.0693
/gate/source/VS_gamma/gps/hist/point 0.247970001 0.0

/gate/source/VS_gamma/gps/hist/point 0.25750999 0.0
/gate/source/VS_gamma/gps/hist/point 0.25751 0.0015
/gate/source/VS_gamma/gps/hist/point 0.257510001 0.0

/gate/source/VS_gamma/gps/hist/point 0.2589999 0.0
/gate/source/VS_gamma/gps/hist/point 0.259 0.0009
/gate/source/VS_gamma/gps/hist/point 0.2590001 0.0

/gate/source/VS_gamma/gps/hist/point 0.27835999 0.0
/gate/source/VS_gamma/gps/hist/point 0.27836 0.0023
/gate/source/VS_gamma/gps/hist/point 0.2783001 0.0

/gate/source/VS_gamma/gps/hist/point 0.28102999 0.0
/gate/source/VS_gamma/gps/hist/point 0.28103 0.072
/gate/source/VS_gamma/gps/hist/point 0.28103001 0.0

/gate/source/VS_gamma/gps/hist/point 0.28531999 0.0
/gate/source/VS_gamma/gps/hist/point 0.28532 0.0043
/gate/source/VS_gamma/gps/hist/point 0.28533001 0.0

/gate/source/VS_gamma/gps/hist/point 0.29518999 0.0
/gate/source/VS_gamma/gps/hist/point 0.29519 0.001588
/gate/source/VS_gamma/gps/hist/point 0.29519001 0.0

/gate/source/VS_gamma/gps/hist/point 0.3293799 0.0
/gate/source/VS_gamma/gps/hist/point 0.32938 0.0026
/gate/source/VS_gamma/gps/hist/point 0.32938001 0.0

/gate/source/VS_gamma/gps/hist/point 0.33069999 0.0
/gate/source/VS_gamma/gps/hist/point 0.3307 0.0116
/gate/source/VS_gamma/gps/hist/point 0.3307001 0.0

/gate/source/VS_gamma/gps/hist/point 0.34372999 0.0
/gate/source/VS_gamma/gps/hist/point 0.34373 0.0043
/gate/source/VS_gamma/gps/hist/point 0.34373001 0.0

/gate/source/VS_gamma/gps/hist/point 0.34635999 0.0
/gate/source/VS_gamma/gps/hist/point 0.34636 0.12
/gate/source/VS_gamma/gps/hist/point 0.34636001 0.0

/gate/source/VS_gamma/gps/hist/point 0.40501999 0.0
/gate/source/VS_gamma/gps/hist/point 0.40502 0.0027
/gate/source/VS_gamma/gps/hist/point 0.40502001 0.0

/gate/source/VS_gamma/gps/hist/point 0.43749999 0.0
/gate/source/VS_gamma/gps/hist/point 0.4375 0.0008
/gate/source/VS_gamma/gps/hist/point 0.43750001 0.0

/gate/source/VS_gamma/gps/hist/point 0.44001999 0.0
/gate/source/VS_gamma/gps/hist/point 0.44002 0.388
/gate/source/VS_gamma/gps/hist/point 0.44002001 0.0

/gate/source/VS_gamma/gps/hist/point 0.45475999 0.0
/gate/source/VS_gamma/gps/hist/point 0.45476 0.0034
/gate/source/VS_gamma/gps/hist/point 0.45476001 0.0

/gate/source/VS_gamma/gps/hist/point 0.50532999 0.0
/gate/source/VS_gamma/gps/hist/point 0.50533 0.288
/gate/source/VS_gamma/gps/hist/point 0.50533001 0.0

/gate/source/VS_gamma/gps/hist/point 0.52896999 0.0
/gate/source/VS_gamma/gps/hist/point 0.52897 1.27
/gate/source/VS_gamma/gps/hist/point 0.52897001 0.0

/gate/source/VS_gamma/gps/hist/point 0.53853999 0.0
/gate/source/VS_gamma/gps/hist/point 0.53854 0.31
/gate/source/VS_gamma/gps/hist/point 0.53854001 0.0

/gate/source/VS_gamma/gps/hist/point 0.55604999 0.0
/gate/source/VS_gamma/gps/hist/point 0.55605 0.0025
/gate/source/VS_gamma/gps/hist/point 0.55605001 0.0

/gate/source/VS_gamma/gps/hist/point 0.56278999 0.0
/gate/source/VS_gamma/gps/hist/point 0.56279 0.0009
/gate/source/VS_gamma/gps/hist/point 0.56279001 0.0

/gate/source/VS_gamma/gps/hist/point 0.57399 0.0
/gate/source/VS_gamma/gps/hist/point 0.574 0.005852
/gate/source/VS_gamma/gps/hist/point 0.57401 0.0

/gate/source/VS_gamma/gps/hist/point 0.57825999 0.0
/gate/source/VS_gamma/gps/hist/point 0.57826 0.0016
/gate/source/VS_gamma/gps/hist/point 0.57826001 0.0

/gate/source/VS_gamma/gps/hist/point 0.59968999 0.0
/gate/source/VS_gamma/gps/hist/point 0.59969 0.0026
/gate/source/VS_gamma/gps/hist/point 0.59969001 0.0

/gate/source/VS_gamma/gps/hist/point 0.61004999 0.0
/gate/source/VS_gamma/gps/hist/point 0.61005 0.0011
/gate/source/VS_gamma/gps/hist/point 0.61005001 0.0

/gate/source/VS_gamma/gps/hist/point 0.62457999 0.0
/gate/source/VS_gamma/gps/hist/point 0.62458 0.078
/gate/source/VS_gamma/gps/hist/point 0.62458001 0.0

/gate/source/VS_gamma/gps/hist/point 0.62825999 0.0
/gate/source/VS_gamma/gps/hist/point 0.62826 0.0016
/gate/source/VS_gamma/gps/hist/point 0.62826001 0.0

/gate/source/VS_gamma/gps/hist/point 0.68793999 0.0
/gate/source/VS_gamma/gps/hist/point 0.68794 0.0268
/gate/source/VS_gamma/gps/hist/point 0.68794001 0.0

/gate/source/VS_gamma/gps/hist/point 0.73586999 0.0
/gate/source/VS_gamma/gps/hist/point 0.73587 0.047
/gate/source/VS_gamma/gps/hist/point 0.73587001 0.0

/gate/source/VS_gamma/gps/hist/point 0.76084999 0.0
/gate/source/VS_gamma/gps/hist/point 0.76085 0.00063
/gate/source/VS_gamma/gps/hist/point 0.76085001 0.0

/gate/source/VS_gamma/gps/hist/point 0.78359999 0.0
/gate/source/VS_gamma/gps/hist/point 0.7836 0.053
/gate/source/VS_gamma/gps/hist/point 0.7836001 0.0

/gate/source/VS_gamma/gps/hist/point 0.83709999 0.0
/gate/source/VS_gamma/gps/hist/point 0.8371 0.00047
/gate/source/VS_gamma/gps/hist/point 0.8371001 0.0

/gate/source/VS_gamma/gps/hist/point 0.87751999 0.0
/gate/source/VS_gamma/gps/hist/point 0.87752 0.00072
/gate/source/VS_gamma/gps/hist/point 0.87752001 0.0

/gate/source/VS_gamma/gps/hist/point 0.89479999 0.0
/gate/source/VS_gamma/gps/hist/point 0.8948 0.0007
/gate/source/VS_gamma/gps/hist/point 0.89480001 0.0

/gate/source/VS_gamma/gps/hist/point 0.89819999 0.0
/gate/source/VS_gamma/gps/hist/point 0.8982 0.0006
/gate/source/VS_gamma/gps/hist/point 0.8982001 0.0

/gate/source/VS_gamma/gps/hist/point 0.90911999 0.0
/gate/source/VS_gamma/gps/hist/point 0.90912 0.0013
/gate/source/VS_gamma/gps/hist/point 0.909120001 0.0

/gate/source/VS_gamma/gps/hist/point 1.03662999 0.0
/gate/source/VS_gamma/gps/hist/point 1.03663 0.00077
/gate/source/VS_gamma/gps/hist/point 1.03663001 0.0
###################################################

/gate/source/list
/gate/source/VS_gamma/dump 1

```
The voxelized attenuation phantom can be used as attenuation map in GATE via the following command lines. Note, the voxelized phantom should **A)** not collide with any other system components and **B)** be contained entirely within its *'mother'* volume (*e.g., world here*). 
```ruby
/gate/world/daughters/name VoxAttn
/gate/world/daughters/insert ImageNestedParametrisedVolume # Or ImageRegularParametrisedVolume
/gate/VoxAttn/geometry/setImage Brain_Perfusion_Attn_72x90x77.h33
/gate/VoxAttn/geometry/setRangeToMaterialFile Attenuation_Brain_Range.dat
/gate/VoxAttn/placement/setTranslation  0. 0. 0. cm
/gate/VoxAttn/attachPhantomSD
```

Indices in the voxelized attenuation image are translated into materials via the parameters defined in the *'Attenuation_Brain_Range.dat'* file. For example, the following indices to materials conversion,
```ruby
4
0 0 Air
1 1 Skull
2 2 TissueSoft
3 3 Brain
```

# 7. Brain Dopamine DaT

## 7.1 Description

We provide the brain DaT attenuation and activity voxelized phantoms in interfile format (*16-bit unsigned integer, \*.i33 for raw data and \*.h33 for the header files*). The brain DaT phantom can emulate a clinical <sup>123I</sup>-FP-CIT (Ioflupane) brain DaT distribution as imaged 4h post injection. It was derived from the open-access [Oasis phantom library](https://www.oasis-brains.org).
For additional information, please see below references,
> - PJ LaMontagne, TLS Benzinger, JC Morris, S Keefe, R Hornbeck, C Xiong, E Grant, J Hassenstab, K Moulder, A Vlassenko, ME Raichle, C Cruchaga, and D Marcus (2019). [OASIS 3: Longitudinal Neuroimaging, Clinical, and Cognitive Dataset for Normal Aging and Alzheimer Disease]([https://iopscience.iop.org/article/10.1088/1361-6560/acbde2](https://www.medrxiv.org/content/10.1101/2019.12.13.19014902v1)), medRxiv.

> - C Lindsay, B Auer, Y Yang, LR Furenlid, and and MA King (2018, November). Creation of a population of patient phantoms for deep learning-based denoising of spect brain imaging. In 2019, 7th International Workshop on Computational Human Phantoms.

<p align="center">
<img width="700" alt="Screen Shot 2023-06-20 at 11 18 11 PM" src="https://github.com/BenAuer2021/Phantoms-For-Nuclear-Medicine-Imaging-Simulation/assets/84809217/7af618e5-6fb0-465c-8534-e6408c5f4214">
</p>

The brain DaT activity phantom can be simulated (**DaT_Source_120x120x120.i33**) as being filled with uniform tracer activity in striatum:background of 8:1. The integer values are set to 10 for the background and 800 for the striatum. It consists of 120x120x120 voxels of 2.0x2.0x2.0 mm<sup>3</sup> (size of 3.5 MB).
  
The voxelized attenuation phantom is similar to the one described in the [brain perfusion section](#BrainPerfusion) (**DaT_Attenuation_72x90x77.i33**) can be used for attenuation correction for SPECT or PET reconstruction and/or as attenuation media for simulation.

## 7.2 Usage in GATE

The voxelized phantoms (*interfile format*) can be loaded in GATE via the following command lines for a simple monoenergetic source, where *'VoxSource'* is the source volume name,
```ruby
/gate/source/addSource VoxSource voxel
/gate/source/VoxSource/reader/insert image
/gate/source/VoxSource/imageReader/translator/insert linear
/gate/source/VoxSource/imageReader/linearTranslator/setScale 0.03 Bq
/gate/source/VoxSource/imageReader/readFile PATH_TO/DaT_Source_120x120x120.h33
/gate/source/VoxSource/imageReader/verbose 1
/gate/source/VoxSource/gps/particle gamma
/gate/source/VoxSource/gps/ang/type iso
/gate/source/VoxSource/gps/ang/mintheta 0.0 deg
/gate/source/VoxSource/gps/ang/maxtheta 180.0 deg
/gate/source/VoxSource/gps/ang/minphi 0.0  deg
/gate/source/VoxSource/gps/ang/maxphi 360.0 deg
/gate/source/VoxSource/gps/energytype Mono
/gate/source/VoxSource/gps/ene/mono 159.0 keV # For I-123 primary emission
/gate/source/VoxSource/setIntensity 1
/gate/source/VoxSource/setPosition -120.0 -120.0 -120.0 mm
/gate/source/VoxSource/dump 1
```
The following example shows how to define a source of all <sup>123</sup>I gammas. The gamma emission data is from the IAEA database: https://www-nds.iaea.org/relnsd/vcharthtml/VChartHTML.html.
Note that this example shows the gamma emissions only, so the simulation output will be missing the X-ray peaks seen in true <sup>123</sup>I spectra. 

`VS_gamma` is the name of the voxelised gamma source 
```ruby
# Set verbosity (2 = every event)
# Good to set to 2 initially to check output is as expected
/gate/source/VS_gamma/gps/verbose 0

/gate/source/addSource VS_gamma voxel
/gate/source/VS_gamma/reader/insert image
/gate/source/VS_gamma/imageReader/translator/insert linear
/gate/source/VS_gamma/imageReader/linearTranslator/setScale 0.05 Bq
/gate/source/VS_gamma/imageReader/readFile PATH_TO/DaT_Source_120x120x120.h33
/gate/source/VS_gamma/imageReader/verbose 1
/gate/source/VS_gamma/gps/particle gamma
/gate/source/VS_gamma/gps/ang/type iso
/gate/source/VS_gamma/setPosition -120.0 -120.0 -120.0 mm

# Force unstable
/gate/source/VS_gamma/setForcedUnstableFlag  true
/gate/source/VS_gamma/setForcedHalfLife 47602.8 s

/gate/source/VS_gamma/gps/particle    gamma
/gate/source/VS_gamma/gps/ene/type  User
/gate/source/VS_gamma/gps/hist/type    energy

/gate/source/VS_gamma/gps/ene/min  0.150 MeV
/gate/source/VS_gamma/gps/ene/max  1.037 MeV

# ------------------hist of emissions----------------------------- #
/gate/source/VS_gamma/gps/hist/point 0.1589999 0.0
/gate/source/VS_gamma/gps/hist/point 0.159 83.6
/gate/source/VS_gamma/gps/hist/point 0.1590001 0.0

/gate/source/VS_gamma/gps/hist/point 0.17419999 0.0
/gate/source/VS_gamma/gps/hist/point 0.1742 0.0008
/gate/source/VS_gamma/gps/hist/point 0.1742001 0.0

/gate/source/VS_gamma/gps/hist/point 0.18261999 0.0
/gate/source/VS_gamma/gps/hist/point 0.18262 0.013
/gate/source/VS_gamma/gps/hist/point 0.18262001 0.0

/gate/source/VS_gamma/gps/hist/point 0.19069999 0.0
/gate/source/VS_gamma/gps/hist/point 0.1907 0.0005
/gate/source/VS_gamma/gps/hist/point 0.19070001 0.0

/gate/source/VS_gamma/gps/hist/point 0.19217999 0.0
/gate/source/VS_gamma/gps/hist/point 0.19218 0.0177
/gate/source/VS_gamma/gps/hist/point 0.19218001 0.0

/gate/source/VS_gamma/gps/hist/point 0.1972299 0.0
/gate/source/VS_gamma/gps/hist/point 0.19723 0.00033
/gate/source/VS_gamma/gps/hist/point 0.19723001 0.0

/gate/source/VS_gamma/gps/hist/point 0.19822999 0.0
/gate/source/VS_gamma/gps/hist/point 0.19823 0.0033
/gate/source/VS_gamma/gps/hist/point 0.19823001 0.0

/gate/source/VS_gamma/gps/hist/point 0.206799 0.0
/gate/source/VS_gamma/gps/hist/point 0.2068 0.0033
/gate/source/VS_gamma/gps/hist/point 0.2068001 0.0

/gate/source/VS_gamma/gps/hist/point 0.20779999 0.0
/gate/source/VS_gamma/gps/hist/point 0.2078 0.0011
/gate/source/VS_gamma/gps/hist/point 0.2078001 0.0

/gate/source/VS_gamma/gps/hist/point 0.247969999 0.0
/gate/source/VS_gamma/gps/hist/point 0.24797 0.0693
/gate/source/VS_gamma/gps/hist/point 0.247970001 0.0

/gate/source/VS_gamma/gps/hist/point 0.25750999 0.0
/gate/source/VS_gamma/gps/hist/point 0.25751 0.0015
/gate/source/VS_gamma/gps/hist/point 0.257510001 0.0

/gate/source/VS_gamma/gps/hist/point 0.2589999 0.0
/gate/source/VS_gamma/gps/hist/point 0.259 0.0009
/gate/source/VS_gamma/gps/hist/point 0.2590001 0.0

/gate/source/VS_gamma/gps/hist/point 0.27835999 0.0
/gate/source/VS_gamma/gps/hist/point 0.27836 0.0023
/gate/source/VS_gamma/gps/hist/point 0.2783001 0.0

/gate/source/VS_gamma/gps/hist/point 0.28102999 0.0
/gate/source/VS_gamma/gps/hist/point 0.28103 0.072
/gate/source/VS_gamma/gps/hist/point 0.28103001 0.0

/gate/source/VS_gamma/gps/hist/point 0.28531999 0.0
/gate/source/VS_gamma/gps/hist/point 0.28532 0.0043
/gate/source/VS_gamma/gps/hist/point 0.28533001 0.0

/gate/source/VS_gamma/gps/hist/point 0.29518999 0.0
/gate/source/VS_gamma/gps/hist/point 0.29519 0.001588
/gate/source/VS_gamma/gps/hist/point 0.29519001 0.0

/gate/source/VS_gamma/gps/hist/point 0.3293799 0.0
/gate/source/VS_gamma/gps/hist/point 0.32938 0.0026
/gate/source/VS_gamma/gps/hist/point 0.32938001 0.0

/gate/source/VS_gamma/gps/hist/point 0.33069999 0.0
/gate/source/VS_gamma/gps/hist/point 0.3307 0.0116
/gate/source/VS_gamma/gps/hist/point 0.3307001 0.0

/gate/source/VS_gamma/gps/hist/point 0.34372999 0.0
/gate/source/VS_gamma/gps/hist/point 0.34373 0.0043
/gate/source/VS_gamma/gps/hist/point 0.34373001 0.0

/gate/source/VS_gamma/gps/hist/point 0.34635999 0.0
/gate/source/VS_gamma/gps/hist/point 0.34636 0.12
/gate/source/VS_gamma/gps/hist/point 0.34636001 0.0

/gate/source/VS_gamma/gps/hist/point 0.40501999 0.0
/gate/source/VS_gamma/gps/hist/point 0.40502 0.0027
/gate/source/VS_gamma/gps/hist/point 0.40502001 0.0

/gate/source/VS_gamma/gps/hist/point 0.43749999 0.0
/gate/source/VS_gamma/gps/hist/point 0.4375 0.0008
/gate/source/VS_gamma/gps/hist/point 0.43750001 0.0

/gate/source/VS_gamma/gps/hist/point 0.44001999 0.0
/gate/source/VS_gamma/gps/hist/point 0.44002 0.388
/gate/source/VS_gamma/gps/hist/point 0.44002001 0.0

/gate/source/VS_gamma/gps/hist/point 0.45475999 0.0
/gate/source/VS_gamma/gps/hist/point 0.45476 0.0034
/gate/source/VS_gamma/gps/hist/point 0.45476001 0.0

/gate/source/VS_gamma/gps/hist/point 0.50532999 0.0
/gate/source/VS_gamma/gps/hist/point 0.50533 0.288
/gate/source/VS_gamma/gps/hist/point 0.50533001 0.0

/gate/source/VS_gamma/gps/hist/point 0.52896999 0.0
/gate/source/VS_gamma/gps/hist/point 0.52897 1.27
/gate/source/VS_gamma/gps/hist/point 0.52897001 0.0

/gate/source/VS_gamma/gps/hist/point 0.53853999 0.0
/gate/source/VS_gamma/gps/hist/point 0.53854 0.31
/gate/source/VS_gamma/gps/hist/point 0.53854001 0.0

/gate/source/VS_gamma/gps/hist/point 0.55604999 0.0
/gate/source/VS_gamma/gps/hist/point 0.55605 0.0025
/gate/source/VS_gamma/gps/hist/point 0.55605001 0.0

/gate/source/VS_gamma/gps/hist/point 0.56278999 0.0
/gate/source/VS_gamma/gps/hist/point 0.56279 0.0009
/gate/source/VS_gamma/gps/hist/point 0.56279001 0.0

/gate/source/VS_gamma/gps/hist/point 0.57399 0.0
/gate/source/VS_gamma/gps/hist/point 0.574 0.005852
/gate/source/VS_gamma/gps/hist/point 0.57401 0.0

/gate/source/VS_gamma/gps/hist/point 0.57825999 0.0
/gate/source/VS_gamma/gps/hist/point 0.57826 0.0016
/gate/source/VS_gamma/gps/hist/point 0.57826001 0.0

/gate/source/VS_gamma/gps/hist/point 0.59968999 0.0
/gate/source/VS_gamma/gps/hist/point 0.59969 0.0026
/gate/source/VS_gamma/gps/hist/point 0.59969001 0.0

/gate/source/VS_gamma/gps/hist/point 0.61004999 0.0
/gate/source/VS_gamma/gps/hist/point 0.61005 0.0011
/gate/source/VS_gamma/gps/hist/point 0.61005001 0.0

/gate/source/VS_gamma/gps/hist/point 0.62457999 0.0
/gate/source/VS_gamma/gps/hist/point 0.62458 0.078
/gate/source/VS_gamma/gps/hist/point 0.62458001 0.0

/gate/source/VS_gamma/gps/hist/point 0.62825999 0.0
/gate/source/VS_gamma/gps/hist/point 0.62826 0.0016
/gate/source/VS_gamma/gps/hist/point 0.62826001 0.0

/gate/source/VS_gamma/gps/hist/point 0.68793999 0.0
/gate/source/VS_gamma/gps/hist/point 0.68794 0.0268
/gate/source/VS_gamma/gps/hist/point 0.68794001 0.0

/gate/source/VS_gamma/gps/hist/point 0.73586999 0.0
/gate/source/VS_gamma/gps/hist/point 0.73587 0.047
/gate/source/VS_gamma/gps/hist/point 0.73587001 0.0

/gate/source/VS_gamma/gps/hist/point 0.76084999 0.0
/gate/source/VS_gamma/gps/hist/point 0.76085 0.00063
/gate/source/VS_gamma/gps/hist/point 0.76085001 0.0

/gate/source/VS_gamma/gps/hist/point 0.78359999 0.0
/gate/source/VS_gamma/gps/hist/point 0.7836 0.053
/gate/source/VS_gamma/gps/hist/point 0.7836001 0.0

/gate/source/VS_gamma/gps/hist/point 0.83709999 0.0
/gate/source/VS_gamma/gps/hist/point 0.8371 0.00047
/gate/source/VS_gamma/gps/hist/point 0.8371001 0.0

/gate/source/VS_gamma/gps/hist/point 0.87751999 0.0
/gate/source/VS_gamma/gps/hist/point 0.87752 0.00072
/gate/source/VS_gamma/gps/hist/point 0.87752001 0.0

/gate/source/VS_gamma/gps/hist/point 0.89479999 0.0
/gate/source/VS_gamma/gps/hist/point 0.8948 0.0007
/gate/source/VS_gamma/gps/hist/point 0.89480001 0.0

/gate/source/VS_gamma/gps/hist/point 0.89819999 0.0
/gate/source/VS_gamma/gps/hist/point 0.8982 0.0006
/gate/source/VS_gamma/gps/hist/point 0.8982001 0.0

/gate/source/VS_gamma/gps/hist/point 0.90911999 0.0
/gate/source/VS_gamma/gps/hist/point 0.90912 0.0013
/gate/source/VS_gamma/gps/hist/point 0.909120001 0.0

/gate/source/VS_gamma/gps/hist/point 1.03662999 0.0
/gate/source/VS_gamma/gps/hist/point 1.03663 0.00077
/gate/source/VS_gamma/gps/hist/point 1.03663001 0.0
###################################################

/gate/source/list
/gate/source/VS_gamma/dump 1

```
The voxelized attenuation phantom can be used as attenuation map in GATE via the following command lines. Note, the voxelized phantom should **A)** not collide with any other system components and **B)** be contained entirely within its *'mother'* volume (*e.g., world here*). 
```ruby
/gate/world/daughters/name VoxAttn
/gate/world/daughters/insert ImageNestedParametrisedVolume # Or ImageRegularParametrisedVolume
/gate/VoxAttn/geometry/setImage DaT_Attenuation_72x90x77.h33
/gate/VoxAttn/geometry/setRangeToMaterialFile Attenuation_Brain_Range.dat
/gate/VoxAttn/placement/setTranslation  0. 0. 0. cm
/gate/VoxAttn/attachPhantomSD
```

Indices in the voxelized attenuation image are translated into materials via the parameters defined in the *'Attenuation_Brain_Range.dat'* file. For example, the following indices to materials conversion,
```ruby
4
0 0 Air
1 1 Skull
2 2 TissueSoft
3 3 Brain
```

# 8. Brain Tumor/GlioBlastoma

## 8.1 Description

We provide the brain glioblastoma attenuation and activity voxelized phantoms in interfile format (*16-bit unsigned integer, \*.i33 for raw data and \*.h33 for the header files*). The brain glioblastoma phantom can emulate a clinical <sup>123I</sup>CLINDE or <sup>131I</sup> brain tumor distribution. It was derived from the benchmark data of the [Bratumia segmentation software](http://mia-software.artorg.unibe.ch/BraTumIA/).
For additional information, please see below references,
> - Auer B, Kalluri KS, Abayazeed A et al. 2020. [Aperture size selection for improved brain tumor detection and quantification in multi-pinhole 123I-CLINDE SPECT imaging](https://ieeexplore.ieee.org/abstract/document/9508019). In 2020 IEEE Nuclear Science Symposium and Medical Imaging Conference (NSS/MIC) (pp. 1-2).

<p align="center">
<img width="700" alt="Screen Shot 2023-06-20 at 11 43 32 PM" src="https://github.com/BenAuer2021/Phantoms-For-Nuclear-Medicine-Imaging-Simulation/assets/84809217/11c2437c-2e83-4222-b93e-a77b78751434">
</p>

The brain glioblastoma activity phantom can be simulated (**Glioblastoma_Source_92x102x65.i33**) as being filled with uniform tracer activity in tumor:background of 3:1. The integer values are set to 1 for the background and 3 for the non-enhancing and MR-enhancing tumor volumes. Necrosis and Edema volumes uptake were set to 0.  It consists of 92x102x65 voxels of 2.0x2.0x2.0 mm<sup>3</sup> (size of 1.2 MB).
  
The voxelized attenuation phantom (**Glioblastoma_Attn_92x102x65.i33**) can be used for attenuation correction for SPECT reconstruction and/or as attenuation media for simulation. The integer values are set to 0 for the air, 1 for the head. It consists of 92x102x65 voxels of 2x2x2 mm<sup>3</sup> (size of 1.2 MB). The air volume surrounding the head was reduced to the maximum to avoid overlap with the system components in GATE or any other simulation software.

## 8.2 Usage in GATE

The voxelized phantoms (*interfile format*) can be loaded in GATE via the following command lines for a monoenergetic <sup>123</sup>I source, where *'VoxSource'* is the source volume name,
```ruby
/gate/source/addSource VoxSource voxel
/gate/source/VoxSource/reader/insert image
/gate/source/VoxSource/imageReader/translator/insert linear
/gate/source/VoxSource/imageReader/linearTranslator/setScale 0.03 Bq
/gate/source/VoxSource/imageReader/readFile PATH_TO/Glioblastoma_Source_92x102x65.h33
/gate/source/VoxSource/imageReader/verbose 1
/gate/source/VoxSource/gps/particle gamma
/gate/source/VoxSource/gps/ang/type iso
/gate/source/VoxSource/gps/ang/mintheta 0.0 deg
/gate/source/VoxSource/gps/ang/maxtheta 180.0 deg
/gate/source/VoxSource/gps/ang/minphi 0.0  deg
/gate/source/VoxSource/gps/ang/maxphi 360.0 deg
/gate/source/VoxSource/gps/energytype Mono
/gate/source/VoxSource/gps/ene/mono 159.0 keV # For I-123 primary emission
#/gate/source/VoxSource/gps/ene/mono 364.0 keV # For I-131 primary emission
/gate/source/VoxSource/setIntensity 1
/gate/source/VoxSource/setPosition -92.0 -102.0 -65.0 mm
/gate/source/VoxSource/dump 1
```
An example of a complete source of <sup>123</sup>I gammas consisting of all the gammas emissions can be found in the [previous section](SourceI123). The following example shows how to define a source of <sup>131</sup>I gammas. The gamma emission data is from the IAEA database: https://www-nds.iaea.org/relnsd/vcharthtml/VChartHTML.html. Note that this example shows the gamma emissions only, so the simulation output will be missing the X-ray peaks or beta emissions seen in true <sup>131</sup>I energy spectra. 

`VS_gamma` is the name of the voxelised gamma source 
```ruby
# Set verbosity (2 = every event)
# Good to set to 2 initially to check output is as expected
/gate/source/VS_gamma/gps/verbose 0

/gate/source/addSource VS_gamma voxel
/gate/source/VS_gamma/reader/insert image
/gate/source/VS_gamma/imageReader/translator/insert linear
/gate/source/VS_gamma/imageReader/linearTranslator/setScale 0.05 Bq
/gate/source/VS_gamma/imageReader/readFile PATH_TO/Glioblastoma_Attn_92x102x65.h33
/gate/source/VS_gamma/imageReader/verbose 1
/gate/source/VS_gamma/gps/particle gamma
/gate/source/VS_gamma/gps/ang/type iso
/gate/source/VS_gamma/setPosition -92.0 -102.0 -65.0 mm

# Force unstable
/gate/source/VS_gamma/setForcedUnstableFlag  true
/gate/source/VS_gamma/setForcedHalfLife 692988 s

/gate/source/VS_gamma/gps/particle    gamma
/gate/source/VS_gamma/gps/ene/type  User
/gate/source/VS_gamma/gps/hist/type    energy

/gate/source/VS_gamma/gps/ene/min    0.08 MeV
/gate/source/VS_gamma/gps/ene/max    0.636989 MeV

 # ---------------------------------------------------- #
/gate/source/VS_gamma/gps/hist/point 0.080175 0.0
/gate/source/VS_gamma/gps/hist/point 0.080185 2.61615
/gate/source/VS_gamma/gps/hist/point 0.080195 0.0

/gate/source/VS_gamma/gps/hist/point 0.08589 0.0
/gate/source/VS_gamma/gps/hist/point 0.0859 8.965e-05
/gate/source/VS_gamma/gps/hist/point 0.08591 0.0

/gate/source/VS_gamma/gps/hist/point 0.16392 0.0
/gate/source/VS_gamma/gps/hist/point 0.16393 0.0211085
/gate/source/VS_gamma/gps/hist/point 0.16394 0.0

/gate/source/VS_gamma/gps/hist/point 0.177204 0.0
/gate/source/VS_gamma/gps/hist/point 0.177214 0.26895
/gate/source/VS_gamma/gps/hist/point 0.177224 0.0

/gate/source/VS_gamma/gps/hist/point 0.23217 0.0
/gate/source/VS_gamma/gps/hist/point 0.23218 0.0031785
/gate/source/VS_gamma/gps/hist/point 0.23219 0.0

/gate/source/VS_gamma/gps/hist/point 0.272488 0.0
/gate/source/VS_gamma/gps/hist/point 0.272498 0.0576205
/gate/source/VS_gamma/gps/hist/point 0.272508 0.0

/gate/source/VS_gamma/gps/hist/point 0.284295 0.0
/gate/source/VS_gamma/gps/hist/point 0.284305 6.12065
/gate/source/VS_gamma/gps/hist/point 0.284315 0.0

/gate/source/VS_gamma/gps/hist/point 0.29579 0.0
/gate/source/VS_gamma/gps/hist/point 0.2958 0.001793
/gate/source/VS_gamma/gps/hist/point 0.29581 0.0

/gate/source/VS_gamma/gps/hist/point 0.30239 0.0
/gate/source/VS_gamma/gps/hist/point 0.3024 0.004727
/gate/source/VS_gamma/gps/hist/point 0.30241 0.0

/gate/source/VS_gamma/gps/hist/point 0.318078 0.0
/gate/source/VS_gamma/gps/hist/point 0.318088 0.077425
/gate/source/VS_gamma/gps/hist/point 0.318098 0.0

/gate/source/VS_gamma/gps/hist/point 0.324641 0.0
/gate/source/VS_gamma/gps/hist/point 0.324651 0.02119
/gate/source/VS_gamma/gps/hist/point 0.324661 0.0

/gate/source/VS_gamma/gps/hist/point 0.325779 0.0
/gate/source/VS_gamma/gps/hist/point 0.325789 0.273025
/gate/source/VS_gamma/gps/hist/point 0.325799 0.0

/gate/source/VS_gamma/gps/hist/point 0.35839 0.0
/gate/source/VS_gamma/gps/hist/point 0.3584 0.0163
/gate/source/VS_gamma/gps/hist/point 0.35841 0.0

/gate/source/VS_gamma/gps/hist/point 0.364479 0.0
/gate/source/VS_gamma/gps/hist/point 0.364489 81.5
/gate/source/VS_gamma/gps/hist/point 0.364499 0.0

/gate/source/VS_gamma/gps/hist/point 0.404804 0.0
/gate/source/VS_gamma/gps/hist/point 0.404814 0.054605
/gate/source/VS_gamma/gps/hist/point 0.404824 0.0

/gate/source/VS_gamma/gps/hist/point 0.44959 0.0
/gate/source/VS_gamma/gps/hist/point 0.4496 0.007335
/gate/source/VS_gamma/gps/hist/point 0.44961 0.0

/gate/source/VS_gamma/gps/hist/point 0.502994 0.0
/gate/source/VS_gamma/gps/hist/point 0.503004 0.359415
/gate/source/VS_gamma/gps/hist/point 0.503014 0.0

/gate/source/VS_gamma/gps/hist/point 0.636979 0.0
/gate/source/VS_gamma/gps/hist/point 0.636989 7.1557
/gate/source/VS_gamma/gps/hist/point 0.636999 0.0
###################################################

/gate/source/list
/gate/source/VS_gamma/dump 1

```
The voxelized attenuation phantom can be used as attenuation map in GATE via the following command lines. Note, the voxelized phantom should **A)** not collide with any other system components and **B)** be contained entirely within its *'mother'* volume (*e.g., world here*). 
```ruby
/gate/world/daughters/name VoxAttn
/gate/world/daughters/insert ImageNestedParametrisedVolume # Or ImageRegularParametrisedVolume
/gate/VoxAttn/geometry/setImage Glioblastoma_Attn_92x102x65.h33
/gate/VoxAttn/geometry/setRangeToMaterialFile Attenuation_Glioblastoma_Range.dat
/gate/VoxAttn/placement/setTranslation  0. 0. 0. cm
/gate/VoxAttn/attachPhantomSD
```

Indices in the voxelized attenuation image are translated into materials via the parameters defined in the *'Attenuation_Glioblastoma_Range.dat'* file. For example, the following indices to materials conversion,
```ruby
2
0 0 Air
1 1 TissueSoft
```

## 9 <sup>177</sup>Lu-DOTATATE patient data 

### 9.1 Description 

We provide the attenuation and activity voxelized phantoms for a patient therapy with  <sup>177</sup>Lu-DOTATATE.  The files are in interfile format (*16-bit unsigned integer, \*.i33 for raw data and \*.h33 for the header files*). The voxelised phantoms are based on a low-dose CT image of a female patient who underwent <sup>177</sup>Lu-DOTATATE therapy at the Christie NHS Foundation Trust, Manchester, UK. Registration and segmentation of the liver, spleen, both kidneys and three tumours was performed by Dr Emma Page at the Christie NHS Foundation Trust, Manchester, UK. The images are for a single bed position. The patient bed has also been manually segmented to permit its material to be specified in GATE. 

Both the attenuation and activity voxelised phantoms are of size 108x70x90 voxels. The pixel size is 3.9063 x 3.9063 mm<sup>2</sup> and the slice thickness is  4.4196 mm.  <br />

The attenuation model: patient15_LuDOTATATE_attn.i33 <br />
The source model: patient15_LuDOTATATE_src.i33

GATE applies a flip in the y-axis, so these phantoms have already been flipped to correct for that. A screenshot of the attenuation file (re-oriented) is shown below: 

![LuP15_attn_full](https://github.com/BenAuer2021/Phantoms-For-Nuclear-Medicine-Imaging-Simulation/assets/55833314/ce97cbf1-4faa-405e-abcf-20e1d86698ba)

Two transaxial slices of the source phantom (pink) overlaid on the attenuation model (blue) are shown below. The top image shows the liver, kidneys and spleen and the bottom shows two of the tumours: 
![LuP15_src_full](https://github.com/BenAuer2021/Phantoms-For-Nuclear-Medicine-Imaging-Simulation/assets/55833314/7694440e-dee4-488b-8d4b-ef15b69bf9c9)
![LuP15_src_full_tumours](https://github.com/BenAuer2021/Phantoms-For-Nuclear-Medicine-Imaging-Simulation/assets/55833314/5aa4f593-9731-44ce-8b2b-94d398eae0c3)


### 9.2 Usage in GATE

As shown in Section 6.2, the  voxelized attenuation phantom can be used as attenuation map in GATE via the following command lines. Note, the voxelized phantom should **A)** not collide with any other system components and **B)** be contained entirely within its *'mother'* volume (*e.g., world here*). 
```ruby
/gate/world/daughters/name VoxAttn
/gate/world/daughters/insert ImageNestedParametrisedVolume # Or ImageRegularParametrisedVolume
/gate/VoxAttn/geometry/setImage PATH_TO/patient15_LuDOTATATE_attn.h33
/gate/VoxAttn/geometry/setRangeToMaterialFile PATH_TO/Attenuation_LuPatient_Range.dat
/gate/VoxAttn/placement/setTranslation  0. 0. 0. cm
/gate/VoxAttn/attachPhantomSD
```

The header file `patient15_LuDOTATATE_attn.h33` must follow a specific format. Note that the line `!name of data file := ./PathTo/patient15_LuDOTATATE_attn.i33` must specify the path to the .i33 file **relative to the GATE macro** and not relative to the header file.  


Indices in the voxelized attenuation image are translated into materials via the parameters defined in the *'Attenuation_LuPatient_Range.dat'* file. Since this phantom is based on a low-dose CT, very specific material definition for different organs is not possible. The following indices to materials conversion can be used

```ruby
-2000 100 Air
100 220 CompressedAir
220 500 Lung
500 950 AdiposeTissue
950 1000 Muscle
1000 1150 SoftTissue
1120 2150 Bone
3000 3050 CarbonFiber
```
The first and second column state the range of voxel values in the attenuation image which correspond to the given material. The thirs column must match the name of a material defined in your materials.dB file. Tissue densities and compositions can be found in (for example) the supplemental matieral of ICRP Publication 110. 


The source image allows specific activities to be defined separately for the liver, spleen, left and right kidneys and the three tumours. The source model defines the following values: 
|     Region        |     Voxel value    |     Number   of source voxels    |
|-------------------|--------------------|----------------------------------|
|     Liver         |     15000          |     15381                        |
|     Spleen        |     16000          |     1535                         |
|     L   Kidney    |     17000          |     2052                         |
|     R   Kidney    |     18000          |     2493                         |
|     Tumour1       |     19000          |     93                           |
|     Tumour2       |     20000          |     55                           |
|     Tumour3       |     21000          |     90                           |

The following example shows how to define a source of <sup>177</sup>Lu gammas for each region. The gamma emission dats is from the IAEA database: https://www-nds.iaea.org/relnsd/vcharthtml/VChartHTML.html.
Note that this example shows the gamma emissions only,  so the simulation output will be missing the X-ray peaks and Bremsstrahlung from the betas seen in true <sup>177</sup>Lu spectra. 

`VS_gamma` is the name of the voxelised gamma source 
```ruby

#  Voxelised gamma source of 177Lu
#  For 1MBq of 17Lu,
#  Gamma activity = 0.172688 MBq (scale activity file accordingly)

# Add new voxelised source
/gate/source/addSource VS_gamma voxel
/gate/source/VS_gamma/reader/insert image
/gate/source/VS_gamma/imageReader/translator/insert range
/gate/source/VS_gamma/imageReader/rangeTranslator/readTable PATH_TO/patient15_activity_235MBq.dat
/gate/source/VS_gamma/imageReader/rangeTranslator/describe 1
/gate/source/VS_gamma/imageReader/readFile patient15_LuDOTATATE_src.h33

# THE DEFAULT POSITION OF THE VOXELIZED SOURCE IS IN THE 1ST QUARTER
# SO THE VOXELIZED SOURCE HAS TO BE SHIFTED OVER HALF ITS DIMENSION IN THE NEGATIVE DIRECTION ON EACH AXIS
/gate/source/VS_gamma/setPosition -210.9402 -136.7205 -198.882 mm

# Attach to voxel phantom
/gate/source/VS_gamma/attachTo VoxAttn

/gate/source/VS_gamma/gps/ang/type iso

# Set verbosity (2 = every event)
# Good to set to 2 initially to check output is as expected
/gate/source/VS_gamma/gps/verbose 0

# Force unstable
/gate/source/VS_gamma/setForcedUnstableFlag  true
# Half life is 6.647 days
/gate/source/VS_gamma/setForcedHalfLife 574067.52 s

/gate/source/VS_gamma/gps/particle    gamma
/gate/source/VS_gamma/gps/ene/type  User
/gate/source/VS_gamma/gps/hist/type    energy

/gate/source/VS_gamma/gps/ene/min    0.07 MeV
/gate/source/VS_gamma/gps/ene/max    0.321315901 MeV

# ------------------hist of emissions----------------------------- #
/gate/source/VS_gamma/gps/hist/point 0.0716419999 0.0
/gate/source/VS_gamma/gps/hist/point 0.071642 0.164
/gate/source/VS_gamma/gps/hist/point 0.0716420001 0.0

/gate/source/VS_gamma/gps/hist/point 0.1129497999 0.0
/gate/source/VS_gamma/gps/hist/point 0.1129498 6.23
/gate/source/VS_gamma/gps/hist/point 0.11294980001 0.0

/gate/source/VS_gamma/gps/hist/point 0.1367244999 0.0
/gate/source/VS_gamma/gps/hist/point 0.1367245     0.0465
/gate/source/VS_gamma/gps/hist/point 0.13672450001 0.0

/gate/source/VS_gamma/gps/hist/point 0.2083661999 0.0
/gate/source/VS_gamma/gps/hist/point 0.2083662     10.41
/gate/source/VS_gamma/gps/hist/point 0.20836620001 0.0

/gate/source/VS_gamma/gps/hist/point 0.2496741999 0.0
/gate/source/VS_gamma/gps/hist/point 0.2496742     0.1997
/gate/source/VS_gamma/gps/hist/point 0.24967420001 0.0

/gate/source/VS_gamma/gps/hist/point 0.3213158999 0.0
/gate/source/VS_gamma/gps/hist/point 0.3213159     0.2186
/gate/source/VS_gamma/gps/hist/point 0.3213159001 0.0
###################################################

/gate/source/list
/gate/source/VS_gamma/dump 1

```
The file `patient15_activity_235MBq.dat` permits the definition of activity in each region of the source image, based on its voxel values. 
The following example sets a total activity of 235 MBq in the phantom.

``` ruby
7
15000.0 15000.0 1167.645
16000.0 16000.0 3847.511
17000.0 17000.0 2625.665
18000.0 18000.0 2576.813
19000.0 19000.0 28595.65
20000.0 20000.0 25746.21
21000.0 21000.0 9210.026667
```
The first row specifies the number of active regions we want to set. The following rows specify a voxel range to set to an activity (Bq/voxel). For example, here we have 3847.511 Bq/voxel in the spleen which has 1535 source voxels. Therefore we define a total gamma activity of 5.91 MBq. Note that <sup>177</sup>Lu has a total gamma decay branching ratio of  0.172688, so this corresponds to a total activity of <sup>177</sup>Lu  of 34.2 MBq. 
