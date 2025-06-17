# Inventory_Rec_System
This project provides an intelligent inventory management system designed for bars. It leverages historical consumption data to forecast demand for various alcoholic beverages, calculates optimal par levels, and simulates inventory performance to minimize holding and stockout costs. The system is built to handle intermittent demand patterns and provides actionable insights for inventory replenishment.
--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
## Features
- Data Ingestion & Preprocessing: Reads raw consumption data, handles missing values, caps outliers, and aggregates data to a daily level. It also fills in missing dates to create continuous time series for accurate forecasting.
- Demand Forecasting: Utilizes pmdarima (Auto-ARIMA) to automatically fit the best ARIMA model for each unique bar-alcohol-brand combination. It includes a fallback mechanism for intermittent or sparse demand, using a simpler average-based forecast.
- Inventory Optimization: Calculates optimal Par Levels using forecasted demand, lead time, and a specified service level. It incorporates safety stock calculations based on the standard deviation of forecast errors.
- Inventory Simulation: Simulates inventory movement over time, tracking stockouts, overstock situations, and calculating associated holding and stockout costs. This helps validate the effectiveness of the calculated par levels.
- Parallel Processing: The core analysis for each bar-alcohol-brand item is parallelized using concurrent.futures, significantly speeding up processing for large datasets.
- Comprehensive Reporting & Visualization: Generates a detailed report of recommended par levels, simulation outcomes (stockout days, costs, etc.), and visualizes key relationships like stockout days vs. forecast accuracy and total cost vs. recommended par level.
--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
## Installation
To get started with this project, clone the repository and install the necessary dependencies.
- git clone https://github.com/TanishqTeja03/Inventory_Rec_System.git
- cd Inventory_Rec_System
- pip install -r requirements.txt
### requirements.txt content:
- pandas
- numpy
- matplotlib
- scipy
- pmdarima
- scikit-learn
- openpyxl (For reading .xlsx files)
--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
## Usage
- Prepare your data: Ensure your consumption data is in an Excel file (e.g., Consumption Dataset.xlsx) with columns similar to: 'Date Time Served', 'Bar Name', 'Alcohol Type', 'Brand Name', and 'Consumed (ml)'.
- Place your data file in the same directory as the script, or update the data_file_path variable in the if __name__ == "__main__": block to point to your file's location.
- data_file_path = '/content/Consumption Dataset.xlsx' # Update this path if needed
- python Inventory_Rec_System.py
- The script will:
  1. Perform initial data exploration and display summary information and a consumption distribution plot.
  2. Process the raw data.
  3. Run parallel forecasting and inventory simulations for each unique item.
  4. Print a final report with recommended par levels and simulation results.
  5. Display insightful visualizations.
--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
## Configuration Parameters
You can adjust the following parameters at the beginning of the script to fine-tune the model and simulation:

- FORECAST_HORIZON = 14                 # Number of days to forecast into the future for replenishment decisions
- LEAD_TIME = 7                         # Number of days it takes for an order to arrive after being placed
- SERVICE_LEVEL = 0.95                  # Desired service level (e.g., 0.95 for 95% in-stock probability)
- MIN_PAR_LEVEL_ML = 1000               # Minimum recommended par level to prevent excessively low recommendations

- HOLDING_COST_PER_ML_PER_DAY = 0.0001  # Cost of holding 1 ml of inventory per day
- STOCKOUT_COST_PER_ML = 0.5            # Cost incurred for each ml of stockout

- ZERO_DEMAND_THRESHOLD = 0.7           # If proportion of zero demand days exceeds this, use simple average forecast
- MAX_ARIMA_ORDER = 3                   # Maximum 'p' or 'q' order for ARIMA model to limit complexity
- MAX_SEASONAL_ORDER = 1                # Maximum seasonal order for ARIMA
- ERROR_EVAL_WINDOW_MULTIPLIER = 1      # Multiplier for the historical window used when calculating forecast errors
--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
## Project Structure
- DataProcessor Class: Handles data loading, exploration, and cleaning. It aggregates raw transaction data into daily consumption per item and fills missing dates.
- DemandForecaster Class: Encapsulates the forecasting logic. It uses pmdarima for automated ARIMA modeling, with a fallback to simple averaging for intermittent demand. It also includes methods for evaluating forecast accuracy.
- InventoryManager Class: Responsible for calculating the optimal par levels based on forecasted demand, service level, and lead time. It also calculates holding and stockout costs.
- InventorySimulator Class: Simulates the inventory system over time, accounting for consumption, replenishment, stockouts, and overstock situations.
- Main Execution Block: Orchestrates the entire process, including parallel execution of the analysis for each item, collecting results, and generating final reports and visualizations.
--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
## Contributing
Contributions are welcome! If you have suggestions for improvements or new features, feel free to open an issue or submit a pull request.
--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
## License
This project is open-source and available under the MIT License.
