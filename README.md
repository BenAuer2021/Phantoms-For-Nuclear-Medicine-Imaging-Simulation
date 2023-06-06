# **Phantoms-For-Nuclear-Medicine-Imaging-Application**

The different digitial phantoms for medical imaging application are developed and maintained by Auer Benjamin from the Brigham and Women's Hospital and Harvard Medical School, Boston, MA, USA.

**Contacts:** bauer@bwh.harvard.edu

Table of contents:
```diff
- 1. Objective and Background
- 2. Description
  - 2.1 STL-based phantoms (mesh50_XCAT)
  - 2.2 Voxelized version of the STL-based phantoms
- 3. Usage in GATE
  - 3.1 STL-based phantoms (mesh50_XCAT)
  - 3.2 Voxelized version of the STL-based phantoms 
```

# 1. Objective

###### Objective
>The digital phantoms described and distributed below can be incorporated in simulation software, such as [GATE](http://www.opengatecollaboration.org) for emulating nuclear medicine acquisition. We provide a Defrise, Derenzo, and Jaszczak phantoms used in quality control procedures. Additionaly, we provide digital phantoms emulating clinical brain perfusion, dopamine transporter, and glioblastoma radiotracer distribution. 

The open-source **mesh50_XCAT** phantom based on the 4D extended cardiac-torso digital anthropomorphic ([XCAT](https://otc.duke.edu/industry-investors/available-technologies/xcat/)) human phantom can be downloaded [here](https://github.com/BenAuer2021/Mesh-based-Human-Phantom-for-Simulation/edit/main/README.md).

# 2. Defrise Phantom

## 2.1 Description

We provide the Defrise attenuation and activity voxelized phantoms in interfile format (*16-bit unsigned integer, \*.i33 for raw data and \*.h33 for the header files*). The 20-cm diameter cylindrical Defrise phantom can be used to estimate axial sampling of nuclear medicine imaging systems. It is based on the [Defrise Insert<sup>TM</sup> Model ECT/DEF/I](https://www.spect.com/wp-content/uploads/2020/06/Micro-Defrise-Phantom.pdf) from Data Spectrum Corporation, Durham, NC, USA. 

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

Indices in the voxelized attenuation image are translated into materials via the parameters defined in the *'Attenuation_Derenzo_Range.dat'* file. For example, the following indices to materials conversion,
```ruby
2
0 0 Air
1 1 Water
```

# 5. Jaszczak Phantom with fillable rods and spheres

## 5.1 Description

We provide the fillable Jaszczak attenuation and activity voxelized phantoms in interfile format (*16-bit unsigned integer, \*.i33 for raw data and \*.h33 for the header files*). The fillable Jaszczak phantom can be used to estimate tomographic uniformity, contrast, and spatial resolution in SPECT and PET. Counter to the regular Jaszczak phantom described in the section 4 above, it simulates spheres and rods filled with activity (instead of cold).

<p align="center">
<img width="904" alt="Screen Shot 2023-06-06 at 5 49 48 PM" src="https://github.com/BenAuer2021/Phantoms-For-Nuclear-Medicine-Imaging-Simulation/assets/84809217/3b0bfd85-8cb8-4edd-90da-499c29247dd1">
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
/gate/source/VoxSource/imageReader/readFile PATH_TO/Fillable_Jaszczak_238x237x134_without_bkg.h33 #w/o background
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

Indices in the voxelized attenuation image are translated into materials via the parameters defined in the *'Attenuation_Derenzo_Range.dat'* file. For example, the following indices to materials conversion,
```ruby
2
0 0 Air
1 1 Water
```

# 6. Brain Perfusion

## 6.1 Description

We provide the brain perfusion attenuation and activity voxelized phantoms in interfile format (*16-bit unsigned integer, \*.i33 for raw data and \*.h33 for the header files*). The brain perfusion phantom can emulate a clinical 99mTc ECD/HMPAO or I123-IMP brain perfusion distribution.

https://www.oasis-brains.org
OASIS 3: Longitudinal Neuroimaging, Clinical, and Cognitive Dataset for Normal Aging and Alzheimer Disease, Pamela J LaMontagne, Tammie L.S.
Benzinger , John C. Morris, Sarah Keefe, Russ Hornbeck, Chengjie Xiong , Elizabeth Grant, Jason Hassenstab , Krista Moulder , Andrei Vlassenko , Marcus E.
Raichle , Carlos Cruchaga , Daniel Marcus, 2019. medRxiv . doi : 10.1101/2019.12.13.19014902

C. Lindsay, B. Auer, Y. Yang, L. R. Furenlid, and M. A. King, “Creation of a population of patient phantoms for deep learning-based denoising of spect brain imaging,” 2019, 7th International Workshop on Computational Human Phantoms.

A brain phantom with source distribution for the perfusion imaging agent 123I-IMP as imaged 1h post injection was simulated in GATE [8]–[10]. The highenergy photons from 123I producing down-scatter interaction were included in these simulations.

fillable Jaszczak phantom can be used to estimate tomographic uniformity, contrast, and spatial resolution in SPECT and PET. Counter to the regular Jaszczak phantom described in the section 4 above, it simulates spheres and rods filled with activity (instead of cold).

<p align="center">
<img width="904" alt="Screen Shot 2023-06-06 at 5 49 48 PM" src="https://github.com/BenAuer2021/Phantoms-For-Nuclear-Medicine-Imaging-Simulation/assets/84809217/3b0bfd85-8cb8-4edd-90da-499c29247dd1">
</p>

This cylindrical phantom of 22 cm diameter by 16.7 cm in height, was adapted from a CT acquisition of a deluxe Jaszczak phantom<sup>TM</sup> from [Data Spectrum Corporation](http://www.spect.com/products-all.html). The phantom consists of 3 sectors, one uniform, one with hot spheres, and another **one** with **hot** rods. The spheres are 9.5, 12.7, 15.9, 19.1, 25.4, and 31.8 mm in diameter. The rods are 4.8, 6.4, 7.9, 9.5, 11.1, and 12.7 mm in diameter. The centre-to-centre distance between two adjacent rods was equal to two times the rod diameter.

The Jaszczak activity phantom can be simulated with (**Fillable_Jaszczak_238x237x134_with_bkg.i33**) and without (**Fillable_Jaszczak_238x237x134_without_bkg.i33**) background activity as being filled with uniform tracer activity in the rod, and sphere volumes. The integer values for the rods/spheres and cold regions are set to 10 and 0, respectively. The background value is 0 (no activity) or 1 (activity). It consists of 238x237x134 voxels of 0.939453x0.939453x1.25 mm<sup>3</sup> (size of 15.1 MB).
  
The voxelized attenuation phantom (**Fillable_Jaszczak_238x237x134_Atn.i33**) can be used for attenuation correction for SPECT or PET reconstruction and/or as attenuation media for simulation. The integer values are set to 1 within the phantom (as entirely filled with water) and 0 outside, respectively. It consists of 238x237x134 voxels of 0.939453x0.939453x1.25 mm<sup>3</sup> (size of 15.1 MB). The attenuation phantom cannot make use of any interpolation counter to the activity phantom as it must strictly represent materials as binary values (0 for material 1, 1 for material 2, 3 for material 3,...), without the use of floating-point numbers. Thus, eventhough they show similar distribution on the figure above the same voxelized phantoms cannot be used for source and attenuation definition.

## 6.2 Usage in GATE

The voxelized phantoms (*interfile format*) can be loaded in GATE via the following command lines for a <sup>99m</sup>Tc source, where *'VoxSource'* is the source volume name,
```ruby
/gate/source/addSource VoxSource voxel
/gate/source/VoxSource/reader/insert image
/gate/source/VoxSource/imageReader/translator/insert linear
/gate/source/VoxSource/imageReader/linearTranslator/setScale 0.03 Bq
/gate/source/VoxSource/imageReader/readFile PATH_TO/Fillable_Jaszczak_238x237x134_with_bkg.h33
/gate/source/VoxSource/imageReader/readFile PATH_TO/Fillable_Jaszczak_238x237x134_without_bkg.h33 #w/o background
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

Indices in the voxelized attenuation image are translated into materials via the parameters defined in the *'Attenuation_Derenzo_Range.dat'* file. For example, the following indices to materials conversion,
```ruby
2
0 0 Air
1 1 Water
```





