# Early-diagnosis-of-Alzheimer-through-Deep-Learning

This repository currently consists of only the pre-processed ADNI data along with the Fractional Anisotropy (FA), Mean Diffusivity (MD), Axial Diffusivity (AD) results generated using our proposed model that leverages the attention mechanism.


## The main contributions of this research are:
- The proposed model efficiently learns spatial correlation in neighboring voxels, thereby improving the robustness of diffusion tensor imaging parameters.
- The proposed method speeds up the estimation of DTI parameters using sparse measurements.
- The proposed model exhibits the capability to quantitative diffusion parameters and identify early-stage Alzheimer's disease.


# Getting Started: DTI Data Preprocessing Workflow

The following steps outline the data preprocessing workflow for DTI (Diffusion Tensor Imaging):

### Step 1: DTI Model Fit
Apply the DTI model fit from the DIPY Python package [Garyfallidis et al., 2014](https://www.frontiersin.org/articles/10.3389/fninf.2014.00008/full) to process each DWI (Diffusion-Weighted Imaging) image individually. This results in the computation of six diffusion components: Dxx, Dxy, Dyy, Dxz, Dyz, and Dzz of the diffusion tensor.

### Step 2: Voxel Selection
Select 100,000 voxels from each DWI image based on their fractional anisotropy (FA) score. Voxels with an FA score of zero are excluded, and the selection is uniformly distributed within the range (0, 1).

### Step 3: Dataset Generation
Generate 100,000 tuples of (input, ground-truth), where each tuple comprises:
- An input 5 × 5 × 5 voxel patch with 41 diffusion directions per voxel.
- A ground-truth 5 × 5 × 5 voxel patch with a 6 × 1 vector per voxel, representing the corresponding six diffusion components of the diffusion tensor.

### Step 4: Signal Concatenation
Concatenate the diffusion signals of the neighboring 5 × 5 × 5 patches to construct a 125 × 41 matrix for each input voxel.

### Step 5: Direction and Signal Concatenation
Combine the diffusion directions (3 × 41) with the diffusion signals to create a 128 × 41 matrix for each input voxel.

### Step 6: Zero-padding
Zero-pad the input data for each tuple to form a 128 × 100 matrix. The corresponding ground truth is represented as a 125 × 6 matrix. This processed dataset is denoted as "ADNI − 41."

### Step 7: Qball-based Interpolation
Perform Qball-based interpolation [Tuch, 2004](https://pubmed.ncbi.nlm.nih.gov/15562495/); [Garyfallidis et al., 2014](https://www.frontiersin.org/articles/10.3389/fninf.2014.00008/full) on the 41-directional input data from the "ADNI − 41" dataset. This step yields 41-directional, 21-directional, and 5-directional diffusion signals, utilizing the DIPY package.

### Step 8: Direction and Signal Concatenation (Interpolated Data)
Combine the diffusion directions with the interpolated diffusion signals for each tuple. This results in input vectors of sizes 125 × 41, 125 × 21, and 125 × 5, along with a ground truth matrix of size 125 × 6.

### Step 9: Zero-padding (Interpolated Data)
Zero-pad the input data for each tuple to create a 128 × 100 matrix.

### Step 10: Dataset Naming
The resulting training datasets are named as follows:
- "ADNI − 41" for the dataset with 41 diffusion directions.
- "ADNI − 21" for the dataset with 21 diffusion directions.
- "ADNI − 5" for the dataset with 5 diffusion directions.

These datasets correspond to the respective diffusion direction configurations.



