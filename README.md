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

## 7 <sup>177</sup>Lu-DOTATATE patient data 

### 7.1 Description 

We provide the attenuation and activity voxelized phantoms for a patient therapy with  <sup>177</sup>Lu-DOTATATE.  The files are in interfile format (*16-bit unsigned integer, \*.i33 for raw data and \*.h33 for the header files*). The voxelised phantoms are based on a low-dose CT image of a female patient who underwent <sup>177</sup>Lu-DOTATATE therapy at the Christie NHS Foundation Trust, Manchester, UK. Registration and segmentation of the liver, spleen, both kidneys and three tumours was performed by Dr Emma Page at the Christie NHS Foundation Trust, Manchester, UK. The images are for a single bed position. The patient bed has also been manually segmented to permit its material to be specified in GATE. 

Both the attenuation and activity voxelised phantoms are of size 108x70x90. The pixel size is 3.9063 mm and the slice thickness is  4.4196 mm.  <br />

The attenuation model: patient15_LuDOTATATE_attn.i33 <br />
The source model: patient15_LuDOTATATE_src.i33

A screenshot of the attenuation file is shown below: 

![LuP15_attn_full](https://github.com/BenAuer2021/Phantoms-For-Nuclear-Medicine-Imaging-Simulation/assets/55833314/ce97cbf1-4faa-405e-abcf-20e1d86698ba)

Two images of the source phantom overlaid on the attenuation model is shown below. The top image shows the liverm kidneys and spleen and the bottom shows two of the tumours: 
![LuP15_src_full](https://github.com/BenAuer2021/Phantoms-For-Nuclear-Medicine-Imaging-Simulation/assets/55833314/7694440e-dee4-488b-8d4b-ef15b69bf9c9)
![LuP15_src_full_tumours](https://github.com/BenAuer2021/Phantoms-For-Nuclear-Medicine-Imaging-Simulation/assets/55833314/5aa4f593-9731-44ce-8b2b-94d398eae0c3)


### 7.2 Usage in GATE

As shown in Section 6.2, the  voxelized attenuation phantom can be used as attenuation map in GATE via the following command lines. Note, the voxelized phantom should **A)** not collide with any other system components and **B)** be contained entirely within its *'mother'* volume (*e.g., world here*). 
```ruby
/gate/world/daughters/name VoxAttn
/gate/world/daughters/insert ImageNestedParametrisedVolume # Or ImageRegularParametrisedVolume
/gate/VoxAttn/geometry/setImage patient15_LuDOTATATE_attn.h33
/gate/VoxAttn/geometry/setRangeToMaterialFile Attenuation_LuPatient_Range.dat
/gate/VoxAttn/placement/setTranslation  0. 0. 0. cm
/gate/VoxAttn/attachPhantomSD
```
Indices in the voxelized attenuation image are translated into materials via the parameters defined in the *'Attenuation_LuPatient_Range.dat'* file. Since this phantom is based on a low-dose CT, very specific material definition for different organs is not possible. The following indices to materials conversion was used

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
/gate/source/VS_gamma/imageReader/rangeTranslator/readTable patient15_activity_235MBq.dat
/gate/source/VS_gamma/imageReader/rangeTranslator/describe 1
/gate/source/VS_gamma/imageReader/readFile patient15_LuDOTATATE_src.h33

# THE DEFAULT POSITION OF THE VOXELIZED SOURCE IS IN THE 1ST QUARTER
# SO THE VOXELIZED SOURCE HAS TO BE SHIFTED OVER HALF ITS DIMENSION IN THE NEGATIVE DIRECTION ON EACH AXIS
/gate/source/VS_gamma/setPosition -210.9402 -136.7205 -198.882 mm

# Attach to voxel phantom
/gate/source/VS_gamma/attachTo voxelPhantom

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

/gate/source/VS_gamma/gps/ene/min    0.0716419 MeV
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
