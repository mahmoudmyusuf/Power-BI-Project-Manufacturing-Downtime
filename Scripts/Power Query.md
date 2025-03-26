# Power Query Transformations

## Creating Parameters

### Base Folder Path Parameter
This parameter `BaseFolderPath` allows for a single point of modification for file sources, making data loading more efficient.

```powerquery
BaseFolderPath = "C:\Your\Base\Path"
```

## Data Extraction and Transformation

### Data Dictionary
Extracts a CSV file with data dictionary details.

```powerquery
let
    BaseFolder = BaseFolderPath,
    FileName = "data_dictionary.csv",
    FullPath = BaseFolder & "\" & FileName,
    
    Source = Csv.Document(File.Contents(FullPath), [Delimiter=",", Columns=3, Encoding=1252, QuoteStyle=QuoteStyle.None]),
    #"Changed Type" = Table.TransformColumnTypes(Source, {{"Column1", type text}, {"Column2", type text}, {"Column3", type text}}),
    #"Promoted Headers" = Table.PromoteHeaders(#"Changed Type", [PromoteAllScalars=true]),
    #"Filtered Rows" = Table.SelectRows(#"Promoted Headers", each ([Field] <> ""))

in
    #"Filtered Rows"
```

### Line Productivity
Loads manufacturing productivity data from an Excel sheet.

```powerquery
let
    BaseFolder = BaseFolderPath,
    FileName = "Manufacturing_Line_Productivity.xlsx",
    FullPath = BaseFolder & "\" & FileName,
    
    Source = Excel.Workbook(File.Contents(FullPath), null, true),
    #"Line productivity_Sheet" = Source{[Item="Line productivity",Kind="Sheet"]}[Data],
    #"Removed Other Columns1" = Table.SelectColumns(#"Line productivity_Sheet", {"Column1", "Column2", "Column3", "Column4", "Column5", "Column6"}),
    #"Promoted Headers" = Table.PromoteHeaders(#"Removed Other Columns1", [PromoteAllScalars=true]),
    #"Changed Type" = Table.TransformColumnTypes(#"Promoted Headers", {{"Date", type date}, {"Product", type text}, 
        {"Batch", Int64.Type}, {"Operator", type text}, {"Start Time", type datetime}, {"End Time", type datetime}}),
    #"Added Actual Batch time" = Table.AddColumn(#"Changed Type", "Actual Period", each Duration.TotalMinutes([End Time]-[Start Time]), Int64.Type)

in
    #"Added Actual Batch time"
```

### Line Downtime
Processes downtime data and unpivots factors for analysis.

```powerquery
let
    BaseFolder = BaseFolderPath,
    FileName = "Manufacturing_Line_Productivity.xlsx",
    FullPath = BaseFolder & "\" & FileName,
    
    Source = Excel.Workbook(File.Contents(FullPath), null, true),
    #"Line downtime_Sheet" = Source{[Item="Line downtime",Kind="Sheet"]}[Data],
    #"Removed Top Rows" = Table.Skip(#"Line downtime_Sheet",1),
    #"Promoted Headers" = Table.PromoteHeaders(#"Removed Top Rows", [PromoteAllScalars=true]),
    #"Unpivoted Other Columns" = Table.UnpivotOtherColumns(#"Promoted Headers", {"Batch"}, "Factor", "Downtime in minutes"),
    #"Changed Type" = Table.TransformColumnTypes(#"Unpivoted Other Columns",{{"Batch", Int64.Type}, {"Factor", Int64.Type}, 
        {"Downtime in minutes", Int64.Type}})

in
    #"Changed Type"
```

### LineFacts
Merges `Line Productivity` and `Line Downtime` to create a comprehensive dataset.

```powerquery
let
    Source = Table.NestedJoin(#"Line productivity", {"Batch"}, #"Line downtime", {"Batch"}, "Line downtime", JoinKind.LeftOuter),
    #"Expanded Line downtime" = Table.ExpandTableColumn(Source, "Line downtime", {"Factor", "Downtime in minutes"}, 
        {"Line downtime.Factor", "Line downtime.Downtime in minutes"}),
    #"Renamed Columns" = Table.RenameColumns(#"Expanded Line downtime",{{"Line downtime.Factor", "downtime Factor"},
        {"Line downtime.Downtime in minutes", "Downtime in minutes"}}),
    #"Replaced Value" = Table.ReplaceValue(#"Renamed Columns", null, 0, Replacer.ReplaceValue, {"downtime Factor", "Downtime in minutes"}),
    #"Changed Type1" = Table.TransformColumnTypes(#"Replaced Value",{{"downtime Factor", Int64.Type}, {"Downtime in minutes", Int64.Type}})

in
    #"Changed Type1"
```

### DimDowntimeFactors
Creates a dimension table for downtime factors with an additional factor for `Zero Downtime`.

```powerquery
let
    BaseFolder = BaseFolderPath,
    FileName = "Manufacturing_Line_Productivity.xlsx",
    FullPath = BaseFolder & "\" & FileName,
    
    Source = Excel.Workbook(File.Contents(FullPath), null, true),
    #"Downtime factors_Sheet" = Source{[Item="Downtime factors",Kind="Sheet"]}[Data],
    #"Promoted Headers" = Table.PromoteHeaders(#"Downtime factors_Sheet", [PromoteAllScalars=true]),
    #"Added Error Type" = Table.AddColumn(#"Promoted Headers", "Error Type", 
        each if[Operator Error]="Yes" then "Operator Error" else "Machine Error", type text),
    #"Added Requirement" = Table.AddColumn(#"Added Error Type", "Required", 
        each if[Operator Error]="Yes" then "Requires Training" else "Requires Maintenance", type text),
    #"Removed Columns" = Table.RemoveColumns(#"Added Requirement",{"Operator Error"}),
    #"Changed Type" = Table.TransformColumnTypes(#"Removed Columns",{{"Factor", Int64.Type}, {"Description", type text}})

in
    #"Changed Type"
```

## Summary
This Power Query script processes multiple data sources, ensuring structured transformations, merging fact tables, and defining dimension tables for effective analysis in Power BI.
