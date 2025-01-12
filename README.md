# Species Distribution Modeling for African Lions

Lions, scientifically known as *Panthera leo*, are majestic creatures often referred to as the "Kings of the Jungle." They have been central to countless household tales, animal-themed movies, and sources of inspiration. However, these magnificent animals now face a dire threat: their population is dwindling due to habitat fragmentation.  

Habitat fragmentation, primarily caused by human activities such as agriculture and urban development, isolates lion populations. This separation reduces their genetic diversity, making them more vulnerable to diseases and other challenges. Additionally, human-wildlife conflicts often lead to retaliatory killings, while illegal poaching further endangers these iconic creatures.  

## Project Objective  
Our project aims to develop a predictive model to identify **ecological corridors**—connected habitats that allow African lions to move safely between areas. These corridors will help mitigate the effects of habitat fragmentation and reduce human-wildlife conflicts by guiding lion movements away from human settlements.  

**Note:** The study is focused solely on African lions.  

---

## Dataset Description  
The initial dataset, titled `Sample.csv`, comprises **6,638 instances** and includes the following attributes:  

- `eventDate`
- `eventTime`
- `startDayOfYear`
- `endDayOfYear`
- `year`
- `month`
- `day`
- `continent`
- `decimalLatitude`
- `decimalLongitude`  

Among these, **decimalLatitude** and **decimalLongitude** are of primary importance as they encapsulate the spatial dimensions essential for comprehensive analysis.  

---

## Clustering for Noise Reduction  
To create a distribution model, we first tackled the challenge of dataset noise and outliers, which made direct modeling inefficient. We implemented **grid-based clustering** to consolidate spatial points into meaningful clusters, reduce noise, and improve computational efficiency without compromising critical ecological patterns.  

### Methodology  
1. **Clustering Tool:** After extensive deliberation, we selected **ArcGIS Pro's Fishnet tool** for grid-based clustering, instead of density-based methods.
2. **Grid Sizes:** We experimented with grid sizes:  
   - `1983x900`  
   - `2000x1000`  
   - `5000x2500`  
   - `10000x5000`  

   Each grid cell represented a cluster, with its central point referred to as the **cluster head**.  

3. **Spatial Join:** We mapped a spatial join between these clusters and the lion location points.  
   - If one or more lion locations fell within a cluster, the cluster head was treated as a single representative point.  

### Results  
- A grid size of **10000x5000** produced the optimal number of clusters.  
- This clustering process reduced the dataset to **1,384 instances**, significantly improving computational efficiency.  
- The processed dataset was saved as `updated_lion_location_data.csv`.  

---

## Species Distribution Model  
One of the major challenges in this project was that the dataset relied on **"presence-only data"**—data that confirms lion presence but lacks information about areas where lions are absent.  

Due to this limitation, we opted to use the **Maximum Entropy (MaxEnt)** algorithm, a robust tool for species distribution modeling with presence-only data.  

### Environmental Variables  
We hypothesized that precipitation significantly influences lion presence. Given the challenges of obtaining specific data like prey density or proximity to water, we used readily available climate data from **WorldClim**.  

The following bioclimatic variables were selected:  
- **BIO16**: Precipitation of the Wettest Quarter  
- **BIO17**: Precipitation of the Driest Quarter  
- **BIO18**: Precipitation of the Warmest Quarter  
- **BIO19**: Precipitation of the Coldest Quarter  

### Model Workflow  
1. **Data Preparation:** Climate data in TIFF format was converted to ASC format using **QGIS**.  
2. **Input Data:**  
   - Lion occurrence data: `updated_lion_location_data.csv`  
   - Selected climate variables  
3. **Modeling:** Using the **MaxEnt Tool**, the model iteratively adjusted probabilities to identify high- and low-probability zones for lion presence.  

---

## Results  
The species distribution model achieved an impressive **87% accuracy**, as measured by the **AUC (Area Under the Curve)** of the **ROC curve**. The results are detailed in the file `FinalPrediction.pdf`.  

- The model’s output includes a global density prediction map due to the global nature of the climate data. However, the predictions are customized for Africa, providing insights into specific environmental conditions suitable for African lions.  
- For instance, similarities in temperatures between Sudan and the deserts of Gujarat reveal potential non-African habitats where lions could survive.  

---

## Key Findings  
From the **Jackknife curve** and **response curve** analyses:  
- **BIO16 (Precipitation in the Wettest Quarter)** emerged as the most significant predictor of lion presence.  
- Other contributing factors: **BIO18**, **BIO17**, and **BIO19**.  
- The response curves showed a **bell-shaped pattern**, indicating an optimal range for lion presence based on these variables.  

### Conclusion  
The findings underscore the critical role of precipitation patterns in determining African lion presence. These results not only contribute to ecological research but also provide actionable insights for conservation efforts aimed at mitigating habitat fragmentation and ensuring the survival of African lions.  
