| [14/12/2024] |
|----------------|

| Manufacturing Downtime Project |
|--------------------------------|

# Team 👥

> **Mohamed Saleh Hammam**  
> **Mahmoud Mohamed Abdel Aziz**  
> **Ahmed Mohamed Hanafy**  
> **Ayman Saad Abo Zamel**  
> **Mina Edwar Kodous**  

---

# 📊 Data Cleaning and Preprocessing

### **Data Preprocessing:**  
Clean and preprocess the data using Power BI.  

**Deliverables:** ✅ Cleaned dataset ready for analysis.

## **Steps**  

📂 **The data consists of 4 tables of data and 1 table as a data dictionary.**

### **Downtime Factors Sheet**
✅ Loaded sheet and selected **Downtime Factors Sheet**  
✅ Promoted header and changed type  
✅ Confirmed by data view: **NO Errors and NO Empty data**  

### **Line Productivity Sheet**
✅ Loaded sheet and selected **Line Productivity Sheet**  
✅ Promoted header and changed type  
✅ Confirmed by data view: **NO Errors and NO Empty data**  
✅ Kept only required columns  
✅ Calculated **Actual Period in minutes** to:  
   - Ensure correct values for further calculations  
   - No negative values confirmed, ensuring no errors in start and end times entry  

### **Line Downtime Sheet**
⚠️ Found extra undesired row → Removed **1 top row** before promoting header  
✅ Promoted header  
✅ Confirmed by data view: **NO Errors and NO Empty data in Batch** but other columns had empty values  
✅ Empty values due to factor data being displayed in separate columns  
🔄 Unpivoted **12 factors** into two columns: **Factor** and **Downtime (minutes)**  
✅ Confirmed by data view: **NO Errors and NO Empty data in remaining columns**  

### **Products Sheet**
✅ Loaded sheet and selected **Products Sheet**  
✅ Promoted header and changed type  
✅ Confirmed by data view: **NO Errors and NO Empty data**  
🔄 Converted all sizes to **ml** for consistency  
✅ Kept only necessary columns  

### **ERD (Entity Relationship Diagram)**
🖼️ ![](media/image2.png)  

### **📌 Schema Modification for Analysis**  
📊 Initially, we applied the first analysis using a **Snowflake Schema**, but in the final report, we changed it to a **Star Schema** for better performance.  

🔹 **Processing Steps for Star Schema:**  
✔ Added a new **Downtime Factor with ID 0** to represent **NO Downtime, No Error batches**  
✔ Merged **Line Downtime** and **Line Productivity** tables into a **new query**  
✔ Replaced **NULL values** for missing downtime factors with **0**, and downtime duration with **0**  
✔ This **denormalization process** improves efficiency when dealing with **large datasets**  

---

# 🔍 **Analysis Questions Phase**  

### **Determine Data Analysis Questions:**  
Determine all possible analysis questions that can be answered via the dataset and would interest the organization’s decision-makers.  

**Deliverables:** ✅ Set of analysis questions that can be answered via the dataset.

## **Steps**  

📊 **All Available Data**  
- **Downtime** ➝ [Factor – Operator Error (YES/NO)]  
- **Time** ➝ [Date – Start – End – Actual Period]  
- **Unique Identifiers** ➝ [Batch – Product – Operator – Flavor]  

📌 **To check Downtime (Sum, Average, Max, Min, Mode) vs:**  
✔ Factor  
✔ Operator Error (YES/NO)  
✔ Operator (Name)  
✔ Batch  
✔ Product  
✔ Flavor  

🛠 **Calculate the following statistics for Downtime (minutes) at different levels to identify trends and gain insights:**  
🔹 **Sum** ➝ Cumulative downtime  
🔹 **Average** ➝ Benchmark for performance  
🔹 **Min** ➝ Lowest downtime recorded  
🔹 **Max** ➝ Highest downtime recorded  
🔹 **Mode** ➝ Most frequent downtime factor  

---

### **📌 Analysis Structure:**
#### **📍 Layer One**
📊 Perform all calculations on the complete dataset **without filtering in measures**.  

#### **📍 Layer Two**
📌 Apply the above measures while filtering by:  
✔ Operator Error (Yes/No)  
✔ Each Downtime Factor  
✔ Each Batch  
✔ Each Product  
✔ Each Operator  

#### **📍 Layer Three**
📌 **Operator Error Downtime Factors:**  
🔍 Check the most frequent **Downtime Factor** for each **Operator**  
- **⚠️ If the same operator repeatedly causes the same factor:** → Needs **training**  
- **⚠️ If the same operator has multiple downtime factors:** → Needs **supervision**  
- **⚠️ If the same factor appears for multiple operators:** → **Team training** required on new technology/procedures  

📌 **Non-Operator Error Downtime Factors:**  
🔍 Check the most frequent **Downtime Factor** for each **Product**  
- **⚠️ If the same product has repeated downtime for the same factor:** → Needs **new instructions/mentorship**  
- **⚠️ If the same factor affects multiple products:** → **Technology upgrade needed**  

---

# 📊 **Dashboard Phase**  


### **📊 Build Dashboard:**  
Create a Power BI dashboard to visualize key insights, including downtime analysis, operator contributions, and production efficiency.  

**Deliverables:** ✅ Power BI dashboard.  

🔗 **View Dashboard Here:** [Manufacturing Line Productivity Dashboard](https://app.powerbi.com/view?r=eyJrIjoiZmNiZTNlMjEtMDA4MC00ZjJjLWFmMGEtMjA2ZDgyMmZlOTUxIiwidCI6IjIwODJkZTQ2LTFhZmEtNGI2NC1hNDQwLTY1NThmODBlOTg0MCIsImMiOjh9)  

## **Steps**  

📌 **Data Exploration & Cleaning**: Discuss data structure, clean inconsistencies, and ensure data readiness.  
📌 **Creating Measures & KPIs**: Develop DAX measures for key indicators such as downtime percentage, operator efficiency, and production trends.  
📌 **Preliminary Visualizations**: Create exploratory charts to understand relationships between downtime, operator errors, and machine failures.  
📌 **Final Dashboard Design**: Structure interactive visuals with filters for total production, operator-based, and product-based perspectives.  
📌 **Downtime Analysis**: Compare downtime across operator errors and machine failures, ensuring actionable insights.  
📌 **Color Standardization**: Apply unified color schema for consistency:  
   - **Operator Errors**: 🔴 `#D64550`  
   - **Machine Failures**: ⚫ `#999999`  
   - **No Error**: 🟢 `#08D731`  


---




# 📑 **Final Presentation**  

### **Prepare a Report & Presentation:**  
Summarize project work, including data analysis, model development, and deployment.  

**Deliverables:** 📜 [Final Report](Report.md) & 🎤 [Presentation]([Report.md](https://gamma.app/docs/Manufacturing-Line-Productivity--qqup867hiqoir19?mode=doc)).  

## **Steps**  

📌 **Data Collection & Preprocessing**: Gather and clean data, ensuring consistency and accuracy.  
📌 **Dashboard Development**: Create an interactive Power BI dashboard with key metrics and filters.  
📌 **Downtime & Productivity Analysis**: Examine downtime causes, operator contributions, and efficiency trends.  
📌 **Recommendations**: Develop strategies for reducing downtime and optimizing production.  
📌 **Final Review & Presentation**: Compile findings into a structured report and prepare the presentation.  

---


---


# 🗣 **Discussion**  

📌 **Summarized discussions from project meetings, key takeaways, and assigned action items.**  

### **First Meeting**  
🔹 **Data Transformation Needs:**  
- The **Line Downtime** table needs restructuring into three columns: **Batch, Downtime Factor ID, and Downtime Minutes** for each factor.  
- The **Line Productivity** table requires a new column to calculate the **total production time** for each batch.  
- The **Products** table must have a **unified size measurement** across all entries.  

### **Second Meeting**  
📊 **Batch Production Analysis**  
- **Total Batches Produced**: Overview of production output.  
- **Average Production Time**: Identify variations in batch processing time.  
- **Operator Production Time**: Measure time spent by operators on production tasks.  

⚙️ **Productivity Analysis**  
- **Minimum, Maximum, and Average Batch Time Comparison**: Identify production consistency and efficiency.  

⏳ **Downtime Analysis**  
- **Total Downtime**: Summing up downtime minutes for each batch.  
- **Downtime by Factor**: Breakdown of downtime causes to pinpoint the most significant issues.  
- **Operator Error Analysis**: Assess downtime caused by operator-related errors using the **Operator Error** column.  

### **Third Meeting**  
🎨 **Dashboard Enhancements**  
- **Upgraded dashboard layout** to improve readability and usability.  
- **Unified color scheme** for better visualization:  
  - **Operator Errors**: 🔴 `#D64550`  
  - **Machine Errors**: ⚫ `#999999`  
  - **No Errors**: 🟢 `#08D731`  

📌 **Next Steps:** Continue refining data transformations and finalize dashboard components.  

---

# 📝 **Summary**  

This project analyzes **manufacturing line productivity** using **Power BI**, focusing on **batch production, downtime analysis, and operator contributions**. Key data transformations included restructuring the **Line Downtime** table, calculating **total production time**, and standardizing product sizes.  

The analysis covers **batch production trends, operator efficiency, and downtime factors**, identifying key bottlenecks such as **machine failures and operator errors**. The dashboard was upgraded with a **unified color scheme** and interactive visualizations to track **KPIs across batches, operators, and downtime causes**.  

**Next steps** involve refining data, optimizing visualizations, and implementing insights for efficiency improvements. 🚀


