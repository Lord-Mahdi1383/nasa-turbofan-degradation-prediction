# overview
this project aims to predict remaining useful life (RUL) of NASA turbofan engines


# Dataset 
the dataset was taken from kaggle's website:    
https://www.kaggle.com/datasets/bishals098/nasa-turbofan-engine-degradation-simulation

this dataset consists of 4 different experiments ranging from FD001 (easiest with one fault mode) to FD004 (hardest with multiple fault modes)

each experiment has 3 text file:    
    - train: the train dataset (without the target feature)         
    - test: the test dataset used for testing the model             
    - RUL: the answers to the test file (y_test)                


## Dataset note
The CMAPSS dataset files are **not included** in this repository.  
Download them from Kaggle and place them in:

`Turbofan_Engine_Degradation_Simulation_Dataset/CMAPSSData/`


# Main Steps 

## FD001
FD001 is simpler:            
    - operating settings are nearly constant and can be dropped                    
    - useless sensors with near 0 variance can be dropped (decided from EDA)                    
    - train XGBoost on the reamaining sensors                    
    - since this dataset is fairly simple adding more features from feature engineering actually increases the RMSE                     
    - predict on the last cycle per engine                    

## FD004                    
FD004 has multiple operating regimes and sensor values depend on operating conditions. to handle this:                    
    - **drop low-variance / uninformative sensors** (selected via EDA and feature importance plot)                       
    - **cluster operating settings** using kmeans on 'op_setting_1', 'op_setting_2', 'op_setting_3'                    
    - **per-cluster z-score normalization** (keep raw data + add z-score features):
        - sensor_z = (sensor - mean_cluster) / std_cluster                    
    - **rolling median on raw sensors** with a window of 10 to reduce noise                    
    - train XGBoost with GroupKFold on engine_id to avoid leakage                    


# Instalation
## Clone The Repo
```bash
git clone https://github.com/Lord-Mahdi1383/nasa-turbofan-degradation-prediction.git
cd nasa-turbofan-degradation-prediction
```

## Install Dependencies
```bash
pip install -r requirements.txt
```
