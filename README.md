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

We provide the Defrise attenuation and activity voxelized phantoms in interfile format (*16-bit unsigned integer, \*.i33 for raw data and \*.h33 for the header files*). The 20-cm diameter cylindrical Defrise phantom can be used to estimate axial sampling of nuclear medicine imaging systems. It is based on the [Defrise Insert<sup>TM<sup> Model ECT/DEF/I](https://www.spect.com/wp-content/uploads/2020/06/Micro-Defrise-Phantom.pdf) from Data Spectrum Corporation, Durham, NC, USA. 

The 20-cm long Defrise phantom consists of a series of 12 20-cm diameter cold disks, and 13 hot gaps. The 8-mm disks are spaced equally 8 mm apart. The Defrise activity phantom (**Defrise_200x200x200.i33**) can be simulated as to be filled by uniform tracer activity. The integer values for the hot and cold disk are set to 10 and 0, respectively. It is for a voxel size of 1 mm<sup>3</sup>. It consists of 200x200x200 voxels 1 mm<sup>3</sup> (size of 16 MB).
  
The voxelized phantoms can be used in attenuation correction for SPECT or PET reconstruction or as attenuation media or activity sources for GATE simulation.

<p align="center">
<img width="1201" alt="Screen Shot 2022-06-05 at 2 51 39 PM" src="https://user-images.githubusercontent.com/84809217/172066089-1b02d124-c95c-4026-87d2-d52d7747ccf1.png">
</p>

The *Body, Air Cavity, Brain, Bronchi Tree, Liver, Lungs, and Skeleton* regions were set of values from 1 to 7 by increment of 1.

## 2.2 Usage in GATE


