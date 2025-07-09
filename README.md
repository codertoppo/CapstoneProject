# Dynamic Parking Pricing 

## **Project Overview**

This project implements and analyzes a dynamic pricing system for parking lots based on real-time data. The primary goal is to develop and compare several pricing models that adjust parking fees based on factors like occupancy, time of day, vehicle type, and competitor pricing. The system also includes a basic rerouting suggestion feature to guide drivers to alternative parking locations when a lot is nearly full.

The entire analysis is conducted within a single Jupyter Notebook (Parking.ipynb), which covers the entire pipeline from data ingestion and cleaning to feature engineering, model implementation, simulation, and visualization.

## **Tech Stack**

This project is built entirely in Python using the following core libraries:

* **pandas**: For data manipulation, cleaning, and analysis.  
* **numpy**: For numerical operations, especially in the pricing model calculations.  
* **bokeh**: For creating interactive and insightful data visualizations to compare the performance of the pricing models.  
* **Jupyter Notebook**: As the interactive development environment for combining code, text, and visualizations.

## **Architecture and Workflow**

The project follows a sequential workflow, where data is processed through several stages to generate pricing simulations and visualizations.

### **Architecture Diagram (Mermaid)**

graph TD  
    A\[Start: dataset.csv\] \--\> B{Data Ingestion & Cleaning};  
    B \--\> C{Feature Engineering};  
    C \--\> D{Model Implementation};  
    subgraph D \[Pricing Models\]  
        D1\[Model 1: Linear Model\]  
        D2\[Model 2: Demand-Based Model\]  
        D3\[Model 3: Competitive Model\]  
    end  
    D \--\> E{Price Simulation};  
    E \--\> F{Visualization & Rerouting};  
    F \--\> G\[End: Interactive Plots & Suggestions\];

    style B fill:\#f9f,stroke:\#333,stroke-width:2px  
    style C fill:\#ccf,stroke:\#333,stroke-width:2px  
    style D fill:\#9cf,stroke:\#333,stroke-width:2px  
    style E fill:\#c9f,stroke:\#333,stroke-width:2px  
    style F fill:\#f9c,stroke:\#333,stroke-width:2px

### **Detailed Workflow Explanation**

1. **Data Ingestion and Cleaning**:  
   * The process begins by loading the dataset.csv file into a pandas DataFrame.  
   * Initial data cleaning involves standardizing column names (e.g., converting SystemCodeNumber to system\_code\_number) and handling potential inconsistencies.  
   * The LastUpdatedDate and LastUpdatedTime columns are combined into a single timestamp column of datetime type, which is crucial for time-series analysis. Redundant columns are then dropped.  
2. **Feature Engineering**:  
   * To feed the models meaningful data, several new features are created:  
     * **occupancy\_rate**: Calculated as occupancy / capacity, this is a key driver for pricing.  
     * **Time-Based Features**: hour\_of\_day and day\_of\_week are extracted from the timestamp to capture temporal demand patterns.  
     * **Categorical Encoding**: The vehicle\_type column is one-hot encoded into separate binary columns (e.g., vehicle\_car, vehicle\_bike) so it can be used in numerical models.  
   * Numerical features like queue\_length are normalized to a common scale (0 to 1\) to ensure they contribute fairly to the pricing models.  
3. **Model Implementation**:  
   * Three distinct pricing models are defined:  
     * **Model 1 (Linear Model)**: A simple baseline model where the price increases linearly with the occupancy\_rate.  
     * **Model 2 (Demand-Based Model)**: A more sophisticated model that calculates a "demand score" based on a weighted sum of multiple factors, including occupancy, queue length, special days, and vehicle type. The final price is a function of this demand score.  
     * **Model 3 (Competitive Model)**: This model builds on Model 2 by incorporating competitor data. It first calculates the demand-based price and then adjusts it based on the price of the nearest competitor lot. If our price is higher, it lowers it to remain competitive; if lower, it may raise it slightly.  
4. **Simulation and Visualization**:  
   * The script simulates the pricing for each parking lot over time using all three models.  
   * For the competitive model, it first identifies the nearest competitor for each lot using the Haversine formula to calculate distances between GPS coordinates.  
   * Using **Bokeh**, the script generates interactive line plots for each parking location, visualizing the price fluctuations from each of the three models over time. This allows for a direct visual comparison of their behavior.  
5. **Rerouting Suggestions**:  
   * A simple alerting mechanism is included. If a parking lot's occupancy rate exceeds 95% and its price is higher than a nearby competitor's, a rerouting suggestion is generated, providing an actionable insight for a potential user.

## **How to Run the Project**

1. **Clone the repository**:  
   git clone \<your-repository-url\>  
   cd \<repository-directory\>

2. **Ensure you have the dataset**:  
   * Place the dataset.csv file in the root directory of the project.  
3. **Install dependencies**:  
   * The notebook includes a cell to install the required libraries. Make sure you have pip installed.

pip install pandas numpy bokeh

4. **Run** the Jupyter **Notebook**:  
   * Launch Jupyter Notebook or JupyterLab:

   jupyter notebook

   * Open the Parking.ipynb file and run the cells sequentially.

