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

### **Build Dashboard:**  
Create a Power BI dashboard to visualize the answers to the identified questions.

**Deliverables:** ✅ Power BI dashboard.

## **Steps**  

📌 **Everyone records every step taken**  

---

# 📑 **Final Presentation**  

### **Prepare a Report & Presentation:**  
Summarize project work, including data analysis, model development, and deployment.

**Deliverables:** 📜 Final report & 🎤 Presentation.

## **Steps**  

📌 **Everyone records every step taken**  

---

# 🗣 **Discussion**  

📌 **Summarize discussions for each issue, state outcomes, and assign action items.**  

---

# 📝 **Summary**  

📌 **Summarize the status of each area/department.**  

---

This version integrates your new **Star Schema modification** explanation in a structured and visually appealing way. Let me know if you need any other updates! 🚀
