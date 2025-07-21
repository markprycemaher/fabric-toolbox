# Fabric Load Test Tool
This tool can be used to load test semantic models in a Fabric workspace

You can use it to fire queries on multiple threads and review results to observe how the semantic model performed under load.

## Usage
Open the RunLoadTest notebook and configure parameters 
Run the second code cell in the RunLoadTest notebook


## Instructions

Watch a video showing how to setup and run a load test using these Fabric Notebooks [here](https://youtu.be/0rSTDeC75vw)

1. **Get sample PBIX file from [here](https://learn.microsoft.com/en-us/power-bi/create-reports/sample-datasets#install-built-in-samples)**
   - Or use your own report and semantic model

1. **Upload sample Power BI PBIX file to target workspace**

1. **Capture DAX Queries**
   - Open and edit the report in Web Service
   - Download Power BI Report to local machine - use "A Copy of your report with a live connection to data online (.pbix)"
   - Open downloaded report in Power BI Desktop
        i. Start Performance Analyzer
        ii. Interact with the report to generate queries you'd like to include in load test (try to create variety)
        iii. Export PowerBIPerformanceData.json file when done to be uploaded to Lakehouse
        iv. Close Power BI Desktop

1. **Create Lakehouse to be used to store DAX queries for Load Test, Email Addresses (if using RLS) and log files.**
   - Create a subfolder in Files section called "PerfScenarios/Queries"
   - Upload PowerBIPerformanceData.json file from step 3 to this subfolder

1. **Upload two fabric notebooks to Fabric Workspace**
   - Download two notebooks (RunLoadTest.ipynb and RunPerfScenario.ipynb) from https://github.com/microsoft/fabric-toolbox/tree/main/tools/FabricLoadTestTool
   - Import two notebooks to Fabric workspace

    or

   - Use Semantic Link Labs to install in a new pure python notebook add the following four lines of code to a code cell then run.
    
```python        
%pip install -q --disable-pip-version-check semantic-link-labs
import sempy_labs as labs
labs.import_notebook_from_web(overwrite=True,notebook_name="RunLoadTest"        , url="https://raw.githubusercontent.com/microsoft/fabric-toolbox/main/tools/FabricLoadTestTool/RunLoadTest.ipynb")
labs.import_notebook_from_web(overwrite=True,notebook_name="RunPerfScenario"    , url="https://raw.githubusercontent.com/microsoft/fabric-toolbox/main/tools/FabricLoadTestTool/RunPerfScenario.ipynb")
```        
6.        
   - Stop and close notebook
   - Delete notebook used for importing loadtest notebooks

7. **Open RunPerfScenario notebook and connect to Lakehouse created at step 4**
   - Open Notebook
   - In Explorer Panel, on Items click ellipsis, then "Remove all Sources" (if required)
   - Click "Add data items" and choose existing Lakehouse created at step 4
   - Save and close notebook (no other changes required)

8. **Open Run Load Test notebook and connect it Lakehouse create at step 4**
   - Open Notebook
   - In Explorer Panel, on Items click ellipsis, then "Remove all Sources" (if required)
   - Click "Add data items" and choose existing Lakehouse created at step 4
   - Edit Load Test Parameters
        i. Change Load Test Name to preferred name (line 13)
        ii. Change Dataset name to name of semantic model to be tested e.g. "Customer Profitability Sample PBIX"
        iii. Set correct workspace name e.g. "Fabric Load Testing Demo"

1. **Adjust parameters and run**

## RLS
If testing using RLS, ensure a list of valid UPN (email addresses) get added to the users variable in the RunLoadTest notebook.  

This could be hardcoded if only using a small number of UPN's, or read in from a file accounts.

Ensure all upn's get added as members of a role for the semantic model being tested.  These accounts should also have "VIEW" rights to the Fabric workspace.

## Improvements
This is an open source project, so feel free to submit bug fixes and improvements.
