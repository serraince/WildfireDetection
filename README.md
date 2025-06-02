# Wildfire Detection

**Summary:**  Detecting wildfires from Sentinel-2 data, using K-means clustering and Gaussian Mixture Models

## Project Abstract

This project was made as an assignment for University College London’s GEOL0069 AI for Earth Observation module. It encompasses two unsupervised learning techniques to detect areas burnt by wildfires. The code is applied to imagery from Manavgat, Turkey, from September 2021, a month after the wildfire occurred. Data from the Sentinel-2 satellite is used due to its suitability for the detection of burnt areas. 

<img width="568" alt="Screenshot 2025-06-01 at 22 40 00" src="https://github.com/user-attachments/assets/5588338c-4266-4bc4-91bd-7ce861b04aa3" />

## Prerequisites

1) Mount Drive
```python
from google.colab import drive
drive.mount('/content/drive')
```

2) Install
```python
!pip install rasterio
!pip install codecarbon
!pip install numpy
!pip install pandas
!pip install matplotlib
!pip install sklearn
!pip install scipy
```

## Data Access
To access the data used in this code:
1) Go to https://dataspace.copernicus.eu/
2) Open an account.
3) Search for Sentinel 2A Data Manavgat, Turkey on the 3rd of September, 2021.
4) Download the S2A_MSIL2A_20210903T083601_N0500_R064_T36SUF_20230108T150158.SAFE file and unzip to a preferred Google Drive folder.

<img width="1098" alt="Screenshot 2025-06-01 at 23 37 20" src="https://github.com/user-attachments/assets/ea153b66-05b2-49d7-be7f-806a1713b08c" />

## Background

In recent years, the prevalence of wildfires has increased, partially due to rising global temperatures caused by climate change (joint-research-centre.ec.europa.eu, 2023). Fast and accurate detection of wildfires is critical for minimising their devastating ecological, social, and economic impacts. Forest ecosystems play a large role in regulating the climate, hosting various species, and sustaining the livelihoods of nearby communities. The already-vulnerable Mediterranean region has already faced an increase in fire risk due to climate change (joint-research-centre.ec.europa.eu, 2023). This project proposes a method to make the remediation process faster.

Delays in wildfire detection can result in underestimating the extent of damage and incorrect responses, leading to large-scale habitat destruction, loss of species, and hindering the development of surrounding communities, preventing the progress towards several Sustainable Development Goals (Aydın-Kandemir & Demir, 2023). Thus, quick and precise identification of land change in fire-affected areas using Earth Observation and estimating the percentage of damage is essential for emergency response, post-fire strategization, biodiversity protection, and long-term land management planning. 

Satellite imagery can be used to determine land-use-land-change (LULC). Using Sentinel 2A imagery, this code implements unsupervised classification methods on burnt areas from the 2021 Manavgat mega wildfire in Turkey. Five spectral bands (B2, B3, B4, B8A, B12) were extracted from Sentinel-2 Level-2A imagery. The two methods used in this code are K-Means Clustering (KM) and Gaussian Mixture Models (GMM). Both algorithms have been previously used in academic literature exploring wildfire detection (Jain et. al, 2020).

## Sentinel-2 Satellite

The Sentinel-2 is a European mission that uses high-resolution multi-spectral imaging, and is used mainly for vegetation monitoring, soil cover, sea and river observations (Copernicus, 2025).  The twin satellites fly in the same orbit, but are placed at the opposite sites, and have a 5-day revisit frequency at the Equator (Copernicus, 2025). Sentinel-2 has 13 spectral bands, and its orbital swath width is 290 kilometres. 

The Multi-Spectral Instrument (MSI), which gathers the image data, uses a push-broom concept that works by collecting rows of image data across the orbital swath, and uses the forward motion of the satellite to provide new rows to gather (S2 Mission, n.d.). Light that hits Earth and is reflected back up to the MSI instrument is collected by a three-mirror telescope, and then focused onto two Focal Plane Assemblies (S2 Mission, n.d.). The Focal Plane Assemblies are divided into two: one is for VNIR wavelengths and the other for SWIR wavelengths (S2 Mission, n.d.). Both assemblies have 12 detectors that are arranged in two horizontal rows (S2 Mission, n.d.).

B2, B3, and B4 bands were used to generate the RGB coloured images of the area, and the B8A and B12 bands were used for the algorithms.

<img width="715" alt="SattelliteDiagram" src="https://github.com/user-attachments/assets/f08d71f9-a7af-498f-aeec-48abdc6dddba" />

## Normalized Burn Ratio (NBR)

The Normalized Burn Ratio highlights burn areas in large fire zones, with a formula that combines near infrared (NIR), represented by Band 8a, and short infrared wavelengths (SWIR), represented by Band 12. Healthy vegetation corresponds to a high reflectance in the NIR, and a low reflectance in the SWIR. A high NBR value represents healthy vegetation like forests and a low NBR value represents bare ground and burnt areas.

The NBR formula is as such: 
NBR = (NIR-SWIR)/(NIR + SWIR)

## Methods:

<img width="518" alt="Screenshot 2025-06-02 at 05 13 28" src="https://github.com/user-attachments/assets/31d6449b-b40d-4f62-b5bb-0b76106073e0" />


### K-Means Clustering (KM)

K-means clustering is a type of unsupervised learning algorithm that partitions a dataset into a number (k) of clusters, based on the similarity of the features of the data (GEOL0069 Notebook, 2025). A k number of centroids (center points of each cluster) are designed, and each data point is assigned to the nearest centroid (GEOL0069 Notebook, 2025).

KM is suitable for large dataset applications where the data structure is not known beforehand (GEOL0069 Notebook, 2025). In the case of wildfires, satellite imagery can encompass large datasets, and thus KM is applicable in this case. This makes it suitable for different burn areas that could have different burn intensities and land cover. 

<img width="452" alt="Screenshot 2025-06-02 at 05 37 27" src="https://github.com/user-attachments/assets/adb4ff68-9895-4b59-be21-5becae7b0241" />


**Features of KM**
1) Choosing the number of clusters (K).
2) Randomly initialise centroids. 
3) Euclidian distance to the centroid.
4) Iteration step, where the centroids are updated as KM is repeated iteratively until the centroids to a simple spot, like a local maximum or minimum. 

### Gaussian Mixture Models (GMM)

GMMs are unsupervised clustering algorithms that model data as a mixture of different Gaussian distributions (GEOL0069 Notebook, 2025). They are soft-clustering methods, assigning probabilities to each point belonging to each cluster. Where K-Means is a more “strict” clustering method, GMM incorporates probability, making it more flexible. 

<img width="424" alt="Screenshot 2025-06-02 at 05 14 41" src="https://github.com/user-attachments/assets/6ad2746f-2b1f-4906-9ede-723820328c8a" />


**Features of GMM**

1) Number of components -similar to clusters in K-Means. 
2) Expectation-Maximization Algorithm, where the Expectation step calculates the probability of a point belonging to a cluster and the Maximisation step updates the parameters of the Gaussians and the process (GEOL0069 Notebook, 2025).
3) Cluster shapes - the shape and size of the clusters are determined by the covariance type of the Gaussians (GEOL0069 Notebook, 2025).
4) Repetition until the parameters converge. 

GMM is suitable for modelling complex shapes; hence, it was chosen for this project due to its ability to detect changes in natural land cover. Since natural land can be complex, the flexibility in GMM clustering is appropriate. 

In this code, a GMM was trained on stratified samples across the scene to cluster pixels into four dominant land cover classes: burnt area, forest, built-up area, and bare soil. The classification was carried out in a tile-based approach to manage large data efficiently.

## Analysis

Since there is no “ground-truth” data, there will be visual comparison of the three maps, and a comparison of the two models against each other using a confusion matrix. If the “ground-truth” data existed, there would have been two comparisons; one fro KM and one for GMM, each against the ground truth to predict accuracy. Thus, here accuracy only refers to how similar the two results are. 

### Quantitative Analysis

Class 0 (Burnt Area) achieves very high precision (1.00) but a moderate recall (0.73), which indicates that when the model predicts a pixel as burnt, it is highly accurate, but it misses a notable portion of burnt pixels. Class 1 (Forest) has strong precision (0.86) but only 50% recall, meaning many forest pixels are misclassified as other classes. Class 2 (Other) has high recall (0.86) but low precision (0.40), indicating that the model often over-predicts this class.

<img width="397" alt="Screenshot 2025-06-02 at 05 39 21" src="https://github.com/user-attachments/assets/c6adf2ba-3b2d-4b42-8ab0-034369906abc" />


### Qualitative Analysis:

Both maps output generally well with the RGB reference, and show a clear distinction of the burnt region in the centre. While GMM shows better spatial variation, it also over-predicts the “Other” class when it is the “Forest” class. K-Means, on the other hand, misclassified some of the burnt areas as the “Forest” class, though to a lesser degree than GMM’s misclassification. Since this project focuses on the ratio of burnt areas, even though GMM has a better spatial variation, KM performs better in detecting burnt vs forest areas; thus, in this case, KM is the preferable model. 

<img width="1106" alt="Screenshot 2025-06-02 at 05 15 12" src="https://github.com/user-attachments/assets/4b8f959d-50ab-4dea-acf3-78da90e9b6fc" />


## Environmental Cost: 2 Calculations

The rise in Artificial Intelligence (AI) usage has meant new ways to extract, interpret or extrapolate from data. However, it is important to note the downsides of using Artificial Intelligence, namely its energy intensity, and to weigh the advantages and disadvantages of using such technology in models. For example, if the model is used in a project that aims to decrease carbon emissions, an analysis of the model itself should be done to ensure that the preventative measure does not offset the decrease in emissions significantly.

### Manual Calculation

Energy use can be manually calculated through exploring scientific literature and making estimations. 
According to Garcia-Martin et. al's 2019 paper, energy consumption can be calculated by:

E = P(cpu) * T(cpu) + P(dram) * T(dram)

Where:
1. P(cpu) = average power consumption of the CPU in watts
2. T(cpu) = execution time in seconds
3. P(dram) = average power draw of memory (in watts)
4. T(dram) = time memory is actively used

For both cases, the running time of the clustering portion of the code (T(cpu)) was 15 seconds on average. Assuming memory was used for the full time period, T(dram) = 10 seconds.

For an average computer, P(cpu) is approximately 45 Watts while P(dram) is approximately 4 watts (Garcia-Martin et. Al, 2019). 

Therefore, for both KM and GMM, the below energy use can be approximated (Garcia et. al, 2019):

E = 15 * 45 + 4 * 10 = 715 Wh = 0.7kWh

For the United Kingdom’s electricity grid, the carbon intensity is approximately 0.018 kg CO2 per kWh. So this value would be 0.7 * 0.018 = 0.0126 kg of CO2 for each clustering event. To compare the two models, using additional libraries in the code can be beneficial.

### Using CodeCarbon
There are two main methods for estimating AI energy use. The first method is a supply-chain method that calculates energy consumption by scaling up from hardware specifications, including how many AI chips are deployed and their power usage (O’Donnell and Crownhart, 2025). The second approach is a bottom-up approach, which directly measures the energy used for specific models or tasks using models (O’Donnell and Crownhart, 2025). In this code, we have used an open-source model named CodeCarbon. CodeCarbon calculates energy use by tracking the CPU, GPU and RAM usage of a code, estimating the power draw of the hardware in watts, measuring how long the code lasts, and finally estimating carbon emissions from Electricity Maps linked to the device’s IP (mlco2.github.io, n.d.).

In general, GPUs require greater energy than CPUs (Huang et. al, 2009). Additionally, larger amounts of data correspond to more RAM needed, and more power consumption from the RAM. In GMM models, this is a limitation factor into the model’s success in laptop computers; this model used a smaller, cropped image to bypass the RAM overexertion issue, resulting in a lower energy use.

For a Python 3 runtime, a System RAM of 12.7GB max, a CPU from Google Cloud (model Intel(R) Xeon(R) CPU at 2.20GHz) and no GPU models, the K-Means Cluster with NBR and GMM both vary in values between 0.00005-0.0002 kWh. These values may vary slightly for different types of computers, runtimes and systems. Overall, their energy efficiency is quite similar, with KM requiring slightly more electricity.

<img width="589" alt="Screenshot 2025-06-02 at 05 39 34" src="https://github.com/user-attachments/assets/30cafbcc-3fe4-4e08-a7d9-75e67f349300" />

AI codes that involve images are more energy and carbon intensive than those with just text (Luccioni et. al, 2024). Also, model training is more carbon intensive than inference; since this model uses two clustering models for unsupervised learning, it requires no training, and is thus more energy-efficient compared to supervised training techniques (Luccioni et. al, 2024).  On average, image-to-category techniques emitted less than text-to-image and image-to-text techniques (Luccioni et. al, 2024).

CodeCarbon's values are lower than the estimate above, likely because it does not consider GPU use, while the data from the estimations does (mlco2.github.io, n.d.). Thus, the estimation above should be trusted more, however, CodeCarbon gives an idea for a comparison between the two models. 

This model can be made more energy efficient by decreasing the amount of data plugged into each model, using renewable energy for electricity, and disregarding the non-NBR K-means clustering system and GMM. Overall, for this method of wildfire detection, GMM uses less electricity, although it is less accurate than KM. 

## For a demonstration of the code, please look at this video explanation:
[https://youtu.be/cJyGR89mbCs] 


## References

Aydin-Kandemir, F. and Demir, N. (2023). 2021 Turkey mega forest Fires: Biodiversity measurements of the IUCN Red List wildlife mammals in Sentinel-2 based burned areas. Advances in Space Research. doi:https://doi.org/10.1016/j.asr.2023.01.031.

Copernicus (2025). Sentinel-2 | Copernicus Data Space Ecosystem. [online] dataspace.copernicus.eu. Available at: https://dataspace.copernicus.eu/explore-data/data-collections/sentinel-data/sentinel-2.

Copernicus (n.d.). S2 Mission. [online] sentiwiki.copernicus.eu. Available at: https://sentiwiki.copernicus.eu/web/s2-mission [Accessed 1 Jun. 2025].

García-Martín, E., Rodrigues, C.F., Riley, G. and Grahn, H. (2019). Estimation of energy consumption in machine learning. Journal of Parallel and Distributed Computing, 134, pp.75–88. doi:https://doi.org/10.1016/j.jpdc.2019.07.007.

Huang, S., Xiao, S.Y. and Feng, W. (2009). On the energy efficiency of graphics processing units for scientific computing. doi:https://doi.org/10.1109/ipdps.2009.5160980.

Jain, P., Coogan, S.C.P., Subramanian, S.G., Crowley, M., Taylor, S. and Flannigan, M.D. (2020). A review of machine learning applications in wildfire science and management. Environmental Reviews, [online] 28(4), pp.478–505. doi:https://doi.org/10.1139/er-2020-0019.

joint-research-centre.ec.europa.eu. (2023). Wildfires in the Mediterranean: monitoring the impact, helping the response. [online] Available at: https://joint-research-centre.ec.europa.eu/jrc-news-and-updates/wildfires-mediterranean-monitoring-impact-helping-response-2023-07-28_en.

Luccioni, S., Yacine Jernite and Strubell, E. (2024). Power Hungry Processing: Watts Driving the Cost of AI Deployment? Association for Computing Machinery. doi:https://doi.org/10.1145/3630106.3658542.

mlco2.github.io. (n.d.). Methodology — CodeCarbon 2.3.4 documentation. [online] Available at: https://mlco2.github.io/codecarbon/methodology.html.

O’Donnell, J. and Crownhart, C. (2025). Everything you need to know about estimating AI’s energy and emissions burden. [online] MIT Technology Review. Available at: https://www.technologyreview.com/2025/05/20/1116331/ai-energy-demand-methodology/ [Accessed 1 Jun. 2025].

Ozkan, M. and Erkoyun, E. (2021). Turkish wildfires are worst ever, Erdogan says, as power plant breached. [online] Reuters. Available at: https://www.reuters.com/world/middle-east/fire-near-turkish-power-plant-under-control-local-mayor-2021-08-04/.

Tsamados, M. and Chen, W. (2022). Welcome to GEOL0069 AI for Earth Observation — GEOL0069 Guide Book. [online] Github.io. Available at: https://cpomucl.github.io/GEOL0069-AI4EO/intro.html [Accessed 1 Jun. 2025].




