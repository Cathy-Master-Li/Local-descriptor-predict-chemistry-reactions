# SEMG-MIGNN
This is a repository for paper "Extrapolative and interpretable reaction performance prediction via chemical knowledge-based graph model".

# Abstract

Accurate prediction of reactivity and selectivity provides the desired guideline for synthetic development. Due to the high-dimensional relationship between molecular structure and synthetic function, it is challenging to achieve the predictive modelling of synthetic transformation with the required extrapolative ability and chemical interpretability. To meet the gap between the rich domain knowledge of chemistry and the advanced molecular graph model, herein we report a new knowledge-based graph model that embeds the digitalized steric and electronic information. In addition, a molecular interaction module is developed to enable the learning of the synergistic influence of reaction components. This knowledge-based graph model achieved excellent predictions of reaction yield and stereoselectivity, whose extrapolative ability was corroborated by additional scaffold-based data splittings and experimental verifications with new catalysts. Because of the embedding of local environment, the model allows the atomic level of interpretation of the steric and electronic influence on the overall synthetic performance, which serves as a useful guide for the molecular engineering towards the target synthetic function. This model offers an extrapolative and interpretable approach for reaction performance prediction, pointing out the importance of chemical knowledge-constrained reaction modelling for synthetic purpose. 

# Generation of SEMG

## Standardization of molecular orientation:

Based on the GFN2-xTB-optimized geometry, we standardized the molecular orientation to ensure the consistency of the encodings generated from different initial orientations. For each molecule, we selected three key atoms to determine the orientation of the molecule: the center of gravity, the atom closest to the center of gravity (atom1), and the atom furthest from the center of gravity (atom2). In step 1, the center of gravity is placed at the origin of the xyz coordinate system. In step 2, atom1 is rotated to the positive half of the z-axis, which determines the direction of the molecule along the z-axis. In step 3, atom2 is rotated to the yz plane and placed at the positive half of the y-axis, which determines the direction of the molecule along the y-axis.

![si_standardization](https://github.com/Shuwen-Li/SEMG-MIGNN/blob/main/picture/si_standardization.jpg)

## Embed steric information:

Based on standardized 3D structure, herein we took the chlorine atom of aryl halides as an example to illustrate the generation of steric information. With the chlorine atom at the center, a sphere of radius 10 Å was built. The radius of this sphere can be customized depending on the sizes of the molecules in the dataset. In our study, there were a number of fairly large molecules with more than 180 atoms. 10 Å was assigned as the radius of sphere to ensure the full description of the steric environment. The distance between the van der Waals surface and the spherical surface was calculated and mapped to the sphere. The steric mapping of the distance on the sphere was encoded as a two-dimensional matrix using equirectangular projection. Based on the selected number N (default is 10) of grids, the polar angle θ (0-π) dimension was evenly segmented to N parts (θi = π/N), and the azimuth angle φ (0-2π) dimension was evenly segmented to 2N parts (φj = π/N). This provided N × 2N distance matrix; in our study, a 10×20 matrix was generated for each atom. This 10×20 matrix was embedded into the node of molecular graph to describe the steric information of atom. Repeating the process for each atom in the molecule, the steric information was embedded in the nodes of the molecular graph.

![SEMG_steric](https://github.com/Shuwen-Li/SEMG-MIGNN/blob/main/picture/SEMG_steric.jpg)

## Embed electronic information:

Starting with the standardized 3D structure, the density matrix was generated by PySCF54 at the B3LYP/def2-SVP level. Taking the chlorine atom as an example, a 7×7×7 cubic grid with the chlorine atom at the center was generated. The side length of the cube was the van der Waals diameter of chlorine. According to the Cartesian coordinates of the chlorine atoms and the 7×7×7 cubic grid, the Cartesian coordinates of the 7×7×7 cubic grids’ center position were obtained. By evaluating electron density of the 7×7×7 cubic grids’ center position, a 7×7×7 tensor was generated and further used as the electronic information for embedding into the node of the molecular graph. Repeating the process for each atom in the molecule, the electronic information was embedded in the nodes of the molecular graph.

![SEMG_ed](https://github.com/Shuwen-Li/SEMG-MIGNN/blob/main/picture/SEMG_ed.jpg)

# MIGNN

The detailed workflow of the MIGNN model is shown as follows, which uses the chiral phosphoric acid-catalyzed thiol addition to N-acylimines as an example. 

![mignn_si](https://user-images.githubusercontent.com/71930017/188142704-cbf56a26-f2d0-4d69-a768-44d57d6f3f0d.jpg)

# Packages requirements
In order to run Jupyter Notebook involved in this repository, several third-party python packages are required. The versions of these packages in our station are listed below.
```
dgl = 0.7.2
matplotlib = 3.4.2
networkx = 2.6.3
numpy = 1.22.4  
pandas = 1.3.3 
pyscf = 2.0.1
rdkit = 2022.03.2   
scipy = 1.4.1 
seaborn = 0.11.1 
sklearn = 0.23.2  
tensorflow = 2.8.0-dev20211115
xgboost = 1.3.3 
```

# Demo & Instructions for use


Notebook 1 demonstrates how to generate local steric and electronic information in data1.

Notebook 2 demonstrates how to train and predict yield in data1.

Notebook 3 demonstrates how to use generate local steric and electronic information in data2.

Notebook 4 demonstrates how to train and predict enantioselectivity in data2.

Baseline folder demonstrates how to train and predict yield/enantioselectivity by baseline MG-GCN, SEMG-GCN, baseline MG-MIGNN, classical descriptors-models and SOTA models (MFF, Yield_Bert, DRFP).


# How to cite
The paper is under review.
# Contact with us
Email: hxchem@zju.edu.cn; shuwen-li@zju.edu.cn
