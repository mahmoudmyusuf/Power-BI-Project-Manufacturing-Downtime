# 📊 DAX Measures for Manufacturing Line Productivity

## 📂 MeasuresTable

### 📁 Folder: Percentage (%)
- **Count/Batch** = `[Downtime Count]/[All Batches]`
- **Downtime Percentage** = `[Total Downtime Hours]/[Productive Hours]`
- **Operator %** = `[No. of Operators]/[All Operators]`
- **Product %** = `[No. of Products]/[All Products]`

### 📁 Folder: All
- **All Batches**:
  ```DAX
  CALCULATE(
      DISTINCTCOUNT(LineFacts[Batch]),
      ALLEXCEPT(LineFacts, DimDate[Date], DimProducts[Product], LineFacts[Operator])
  )
  ```
- **All Operators**:
  ```DAX
  CALCULATE(
      DISTINCTCOUNT(LineFacts[Operator]),
      ALL(LineFacts)
  )
  ```
- **All Products**:
  ```DAX
  CALCULATE(
      DISTINCTCOUNT(LineFacts[Product]),
      ALL(LineFacts)
  )
  ```

### 📁 Folder: Average (AVG)
- **Average Batch Time**:
  ```DAX
  AVERAGEX(SUMMARIZE(LineFacts, LineFacts[Batch], LineFacts[Actual Period]),
  LineFacts[Actual Period])
  ```
- **Average Batch Time Target**:
  ```DAX
  AVERAGEX(SUMMARIZE(LineFacts, LineFacts[Batch], LineFacts[Target batch time (min)]),
   LineFacts[Target batch time (min)])
  ```
- **Average Downtime Minutes**:
  ```DAX
  COALESCE(CALCULATE(AVERAGE(LineFacts[Downtime in minutes]), LineFacts[Downtime in minutes]<>0), 0)
  ```
- **MTBF Hours** (Mean Time Between Failures):
  ```DAX
  [Total Operating Hours]/[Downtime Count]
  ```

### 📁 Folder: Count
- **Downtime Count**:
  ```DAX
  COALESCE(CALCULATE(COUNT(LineFacts[downtime Factor]), LineFacts[downtime Factor]<>0), 0)
  ```
- **No. of Batches**:
  ```DAX
  COALESCE(DISTINCTCOUNT(LineFacts[Batch]), 0)
  ```
- **No. of Flavors**:
  ```DAX
  CALCULATE(
      DISTINCTCOUNT(DimProducts[Flavor]),
      FILTER(DimProducts, DimProducts[Product] IN VALUES(LineFacts[Product]))
  )
  ```
- **No. of Operators**:
  ```DAX
  COALESCE(DISTINCTCOUNT(LineFacts[Operator]), 0)
  ```
- **No. of Products**:
  ```DAX
  COALESCE(DISTINCTCOUNT(LineFacts[Product]), 0)
  ```
- **No. of Sizes**:
  ```DAX
  CALCULATE(
      DISTINCTCOUNT(DimProducts[Size (ml)]),
      FILTER(DimProducts, DimProducts[Product] IN VALUES(LineFacts[Product]))
  )
  ```

### 📁 Folder: Current & Previous Day
- **DayBatches**:
  ```DAX
  VAR LastDay = [LastDay]
  RETURN CALCULATE(DISTINCTCOUNT(LineFacts[Batch]), LineFacts[Date] = LastDay)
  ```
- **DayBefore**:
  ```DAX
  VAR LastDay = [LastDay]
  RETURN MAXX(FILTER(ALL(LineFacts), LineFacts[Date] < LastDay), LineFacts[Date])
  ```
- **DayDowntime**:
  ```DAX
  VAR LastDay = [LastDay]
  RETURN CALCULATE(SUM(LineFacts[Downtime in minutes]), LineFacts[Date] = LastDay)
  ```
- **DayErrorsFrequency**:
  ```DAX
  VAR LastDay = [LastDay]
  RETURN CALCULATE(COUNT(LineFacts[Downtime in minutes]), LineFacts[Date] = LastDay)
  ```
- **Diff-Batches**:
  ```DAX
  VAR DayBefore = [DayBefore]
  RETURN [DayBatches] - CALCULATE(DISTINCTCOUNT(LineFacts[Batch]), LineFacts[Date] = DayBefore)
  ```
- **Diff-Downtime**:
  ```DAX
  VAR DayBefore = [DayBefore]
  RETURN ([DayDowntime] - CALCULATE(SUM(LineFacts[Downtime in minutes]), LineFacts[Date] = DayBefore)) / 60
  ```
- **Diff-Error**:
  ```DAX
  VAR DayBefore = [DayBefore]
  RETURN [DayErrorsFrequency] - CALCULATE(COUNT(LineFacts[Downtime in minutes]), LineFacts[Date] = DayBefore)
  ```
- **LastDay**:
  ```DAX
  MAX(LineFacts[Date])
  ```

### 📁 Folder: Sum
- **Productive Hours**:
  ```DAX
  SUMX(SUMMARIZE(LineFacts, LineFacts[Batch], LineFacts[Target batch time (min)]),
  LineFacts[Target batch time (min)]) / 60
  ```
- **Total Downtime Hours**:
  ```DAX
  COALESCE(SUM(LineFacts[Downtime in minutes]) / 60, 0)
  ```
- **Total Downtime Negative**:
  ```DAX
  -COALESCE(SUM(LineFacts[Downtime in minutes]), 0)
  ```
- **Total Operating Hours**:
  ```DAX
  SUMX(SUMMARIZE(LineFacts, LineFacts[Batch], LineFacts[Actual Period]), LineFacts[Actual Period]) / 60
  ```

### 📁 Folder: Tools
- **Disc-Select**:
  ```DAX
  SELECTEDVALUE(DimDowntimeFactors[Description])
  ```
- **Factor Color**:
  ```DAX
  VAR Machine = CALCULATE(SUM(LineFacts[Downtime in minutes]),
    DimDowntimeFactors[Error Type] = "Machine Error")
  VAR Operator = CALCULATE(SUM(LineFacts[Downtime in minutes]),
    DimDowntimeFactors[Error Type] = "Operator Error")
  VAR NoError = CALCULATE(SUM(LineFacts[Downtime in minutes]),
    DimDowntimeFactors[Error Type] = "No Error")
  VAR MaxValue = MAX(Machine, MAX(Operator, NoError))
  RETURN SWITCH(TRUE(), MaxValue = 0, "#08D731",
    Machine = MaxValue,"#999999",
    Operator = MaxValue, "#D64550",
    NoError = MaxValue, "#08D731",
    "Black")
  ```
- **Factor-Select**:
  ```DAX
  SELECTEDVALUE(DimDowntimeFactors[Factor])
  ```
- **Title**:
  ```DAX
  MIN(DimDate[Date]) & " to " & MAX(DimDate[Date])
  ```
