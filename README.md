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
