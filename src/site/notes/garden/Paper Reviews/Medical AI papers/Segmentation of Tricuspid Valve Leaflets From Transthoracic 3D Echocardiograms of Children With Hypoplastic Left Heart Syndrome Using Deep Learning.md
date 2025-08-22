---
{"dg-publish":true,"permalink":"/garden/paper-reviews/medical-ai-papers/segmentation-of-tricuspid-valve-leaflets-from-transthoracic-3-d-echocardiograms-of-children-with-hypoplastic-left-heart-syndrome-using-deep-learning/"}
---

## Introduction/ Abstract
Hypoplastic Left Heart Syndrome
HLHS happens happens when only the right ventricle and TV alone support circulation
TV -> Tricuspid Valve
3DE - > 3D Echocardiography
TEE -> Transesophagel Echocarciography

- Used an FCN to segment TVs from transthoracic 3DE
- Dataset split: 133 3DE for training, 28 for validation
- Metrics used: Dice Similarity and Meand Boundary Distance
- Results: 
	- Annular curve
		- DSC median: 0.86 
		- MDB median: 0.35
	- Merged
		- DSC average: 0.77
		- MDB median: 0.66

Annulus: the fibrous ring that serves as the base structure for the valves
Leaflets: the thin flaps that open and close with each heartbeat
Think of these as being hinges and doors.

Repairing TV is difficult due to the complex geometry
## Limitations: 
2DE: requires  mental reconstruction by the clinician
3DE: child transthoracic scans are of lower quality as compared to adults
TEE:  artifacts, less cooperation
## Dataset Details
### Basic Information
161 TEE 3DE images from 239 unique HLHS patients
133 for training (pre stage 1: 24; post-stage 1: 21; post-stage 2: 12; post-stage 3: 76)
28 for validation/testing
### Ground truth creation: 
First the annular curve is marked, after which the TV leaflet is segmented and then the valve quadrant landmarks corresponsing to APSL (Anterior, Posterior, Septal, Lateral) regions of the annulus are identified. Finally the commissures (boundaries between the leaflets and annulus) are marked; which are ASC, PSC, APC   

![Pasted image 20250820232241.png](/img/user/images/Pasted%20image%2020250820232241.png)
The Annular Curve with quadrant landmarks

## Model Architecture
VNet is the base architecture 
Made some changes to the overall architecture like changing from PReLU to ReLU based on experimental feedback. Changed the convolution filter sizes from 5x5x5 to 3x3x3. 

## Experiments and Methodology

The paper goes into detail about the different input configurations that they used to get the results.
The variables they explored are:
- Different types of input frames 
- Including the annular curve
- Including the Commissural Landmarks
- Resampling

They explored all these possibilities for each type of input frame
### Input frames
Different combinations of the input frames were given to the model for training. Each set of the input frames give a differing amount of information about the overall motion and states of the valves

- **Single-Phase**: only the targeted MS frame.

- **Two-Phase**: introduced the mid-diastolic (MD) frame in addition to the MS frame. Providing a diastolic frame (in which the TV is open) may introduce visual information not present in the MS frame to separate the valve leaflets.

- **Four-Phase**: added the end-systolic (ES) and end-diastolic (ED) frames, resulting in four frames (ES, ED, MS, MD) as FCN inputs.

- **Consecutive Systolic Phases (CSP)** : introduced ten frames surrounding the MS frame in addition to the MS frame. The MS frame and five adjacent frames before and five adjacent frames after the MS frame were provided to the FCN.

![Pasted image 20250820233103.png](/img/user/images/Pasted%20image%2020250820233103.png)
Since there are 3 additional input modifications, there are a total of 2^3 = 8 possiblites which are explored for all 4 of the input frames, thus coming up to 32 experiments in total.

## Results
The paper experiments with different amounts of data, in thte form of how the input frames that are given, and whether to include the annular curve, commissural landmarks and resampling. The best results were observed when the the input frame type was CSP, with annular curve, commissural landmarks and resampling; ie the maximum amount of data that could be provided. This also outperformed human.  
  
Individual leaflets: DSC = 0.81, MBD = 0.38 mm  
Merged valve: DSC = 0.85, MBD = 0.33 mm

## Paper Breakdown

**Category:** A clinical application research applying FCNs to segment tricuspid valve leaflets from 3D echocardiography in pediatric patients with congenital heart disease.

**Context:** Addresses a critical clinical need in pediatric cardiology where manual segmentation of tricuspid valves in HLHS patients is extremely time-consuming (2-4 hours) and variable between operators. Builds on existing work in cardiac image segmentation and atlas-based approaches, but represents the first application of deep learning to pediatric congenital valve segmentation.

**Correctness:** The methodology appears sound with appropriate validation protocols. However, there are concerns over the imbalanced training data across surgical stages (58% post-stage 3, only 9% post-stage 2)

**Contributions:**
- First deep learning approach for tricuspid valve segmentation in congenital heart disease
- Systematic evaluation of different input configurations (frame combinations, landmarks, preprocessing)
- Achieves segmentation accuracy comparable to human intra-observer variability

**Clarity:** Well-structured paper with clear methodology and comprehensive results reporting. The medical context is well-explained for a technical audience, though the extensive experimental permutations (32 different configurations) make the results section particularly intense.

**Additional Notes:** Your observation about MBD being potentially relevant for VR-Heart shell creation is astute - boundary accuracy would indeed be crucial for haptic applications. The signal dropout challenge they identify (where FCNs create holes that human annotators fill based on domain knowledge) is a common issue in medical AI that's worth considering for any cardiac application.

The paper's strength lies in its thorough experimental design, but the dataset limitations and class imbalance suggest results should be interpreted cautiously, especially for the underrepresented surgical stages.



Note: Should we also be thinking of using MBD if we are focusing on making shells for VR-Heart?
Things to follow up on: 
Wilcoxon sifned rank test for pairwise statistical comparison
shaprio-wilk test for normality

