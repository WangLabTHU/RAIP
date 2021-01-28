## Welcome to RareDecipher

RareDecipher is a promising tool to solve the deconvolution problem of bulk data where rare compo-nents are of vital importance.

We have published the tools on github and it is easy to use. RareDecipher can handle various kinds of data including methylation data, RNA-Seq data and single cell data. 

## Tutorials
Here we offer tuorials on how to install and use this tool.

### Installation
#### Section 1: System Requirements
RAIP was implemented using python. It can be installed in Windows, Linux as well as MAC OS. RAIP requires python version >= 3 and all the dependent packages will be installed using pip.
#### Section 2: Installation Instruction
RAIP can be installed using pip by the following command:
```markdown
pip install RareDecipher
```

### Data preparation
RareDecipher depends on reference data and mixture data to infer the cell types fractions. So the users must provide RareDecipher the reference path and mixture path. The reference and mixture must be submitted in the following format:

All of these data should be .csv format. For reference.csv, the first column should be the markers name and the first row should corresponds to the cell types. For mixture.csv, the first column should be the markers and have the same order as the reference.csv, the first row represents the samples id.

We have provided the [demo data](https://github.com/WangLabTHU/RAIP/tree/main/demo_data). If you have any problems in running this tool, please refer to demo data and check that if your data preparation have exactly the same format as the demo data.


### How to use the RareDecipher

The RareDecipher offers one function to run the whole algorithms, and here we provide documentation of these function, please run the function as the following guidance:
```markdown
from RareDecipher.runDecipher import decipher

decipher(ref_path, mix_path, save_path='prop_predict.csv', marker_path='', scale=0.1, delcol_factor=10, iter_num=10, confidence=0.75, w_thresh=10, unknown=False, is_markers=False, is_methylation=False)
```
The meanings of all the parameters should be :
```
:param ref_path: Path to reference data
:param mix_path: Path to mixture data
:param marker_path: Path to markers, if users select to specify certain markers
:param save_path: save the results in this path
:param scale: control the convergence of SVR
:param delcol_factor: control the extent of removing collinearity
:param iter_num: iterative numbers of outliers detection
:param confidence: ratio of remained markers in each outlier detection loop
:param w_thresh: threshold to cut the weights designer
:param unknown: if there is unknown content
:param is_markers: if users choose to specify their own markers
:param is_methylation: if the data type belongs to methylation data
```

#### marker_path
Provide the csv path to the markers. The markers contains the marker name in the reference data and mixture data. The markers should be in the same format as the markers.csv in the demo data.

#### scale
This factor can change the speed of our method. For RNA-Seq data, the expression value is large so it can become very slow for the SVR to acheive the stop criterion. And the scale factor can control the stop criterion. For RNA-Seq data, we recommend scale = 0.1 and for methylation data we recommend scale = 1. If you find the algorithm is to slow, you can try to reduce the scale factor.

#### iter_num
This refers to the loop number in the outlier detection modules. More points will be seen as outliers if you increase the iter_num.

#### decol_factor

This parameter represents the loop number in the removing collinearity part. We recommend this factor should be in the range from 2 to 20. Larger decol_factor will lead to less search space for removing collinearity.

#### confidence
In the outlier detection part, we adaptively set the threshold according to the confidence. If the confidence=0.8, it means 20% of the markers will be seen as outliers in the first loop of the outlier detection.

#### unknown
To improve the practicality of RareDecipher, we consider there may be some cases where reference data cannot explains all cell types, and the left cell types will be treated as unknown. In this case, please set unknown=True and the output proportion vector will be less than one.
