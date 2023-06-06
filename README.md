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

/gate/world/daughters/name voxelPhantom
/gate/world/daughters/insert ImageNestedParametrisedVolume #ImageNestedParametrisedVolume
/gate/voxelPhantom/geometry/setImage Defrise_Atn_200x200x200.h33
/gate/voxelPhantom/geometry/setRangeToMaterialFile Attenuation_Defrise_Range.dat
/gate/voxelPhantom/placement/setTranslation  0. 0. 0. cm
#/gate/voxelPhantom/setSkipEqualMaterials 1
/gate/voxelPhantom/attachPhantomSD

/gate/source/addSource hof_brain voxel
/gate/source/hof_brain/reader/insert image
/gate/source/hof_brain/imageReader/translator/insert linear
/gate/source/hof_brain/imageReader/linearTranslator/setScale 0.03 Bq
/gate/source/hof_brain/imageReader/readFile Defrise_200x200x200.h33
/gate/source/hof_brain/imageReader/verbose 1
/gate/source/VS_gamma/gps/particle gamma
/gate/source/VS_gamma/gps/ang/type iso
/gate/source/VS_gamma/gps/ang/mintheta 0.0 deg
/gate/source/VS_gamma/gps/ang/maxtheta 180.0 deg
/gate/source/VS_gamma/gps/ang/minphi 0.0  deg
/gate/source/VS_gamma/gps/ang/maxphi 360.0 deg
/gate/source/hof_brain/gps/energytype Mono
/gate/source/VS_gamma/gps/ene/mono 140.5 keV
/gate/source/hof_brain/setIntensity 1
# THE DEFAULT POSITION OF THE VOXELIZED SOURCE IS IN THE 1ST QUARTER
# SO THE VOXELIZED SOURCE HAS TO BE SHIFTED OVER HALF ITS DIMENSION IN THE NEGATIVE DIRECTION ON EACH AXIS
/gate/source/hof_brain/setPosition -100.0 -100.0 -100.0 mm
/gate/source/hof_brain/dump 1


