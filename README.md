# TBD

By default, results from Form Recognizer are generated in JSON format, which is a great way of storing the information extracted from the scanned documents. Typically, converting that JSON output to CSV format is a straightforward process, which then can be used in further analysis in Power BI for example. One way of acheiving that conversion is by using Form Recognizer REST API with Python, which can be found here in this repo [Quickstart: Extract invoice data using the Form Recognizer REST API with Python](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/python/FormRecognizer/rest/python-invoices.md).

Although in the case of receipts or invoices, converting line items from JSON to CSV can be a challenging task, due to their dynamic nature. In this repo, we tackle the challange of storing line items extracted from receipts in CSV format, using a simple Logic App flow. 

## Pre-requisites
- Azure Subscription 
- Azure Logic App
- Storage Account
- Form Recognizer 
- Sample receipt

## Steps
### Step 1: Setting up the environment
Creat a new Resource Group in your Azure Subscription and provision the followng resources:
- Storage Account
- Logic App
- Form Recognizer 

![image](https://user-images.githubusercontent.com/88718044/150129298-2143e27a-0733-4eea-90fc-505f8fbddda4.png)

You can also deploy the required resources using this ARM template:

[![Deploy to Azure](https://aka.ms/deploytoazurebutton)](https://portal.azure.com/#create/Microsoft.Template/uri/)

### Step 2: Setting up the Logic App
1. Create a blank Logic App. 

![image](https://user-images.githubusercontent.com/88718044/150130109-4f7051fa-f14d-4bcf-ae76-e3832187fa19.png)

2. Search for "Azure Blob Storage" and select "When a blob is added or modified" trigger. 

![image](https://user-images.githubusercontent.com/88718044/150131381-14d8cd63-2f33-4d5d-bcc2-9baf97bcdc0f.png)

3. You will be required to create a connection to the storage account. Fill in the information and click on create to proceed. 

![image](https://user-images.githubusercontent.com/88718044/150132267-337aa540-f7ec-46f4-8a96-ffdd5d87d669.png)

4. You will be required to select a container in the storage account to monitor. Fill in the information and click on New step to proceed.

![image](https://user-images.githubusercontent.com/88718044/150132720-81ded702-b031-49fd-a569-ccd55b44dcc7.png)

5.From the "Azure Blob Storage" list of actions, select "Get blob content".

![image](https://user-images.githubusercontent.com/88718044/150133252-07650a5a-cedd-42d8-894a-e60ec339b183.png)



## License
For all licensing information refer to [LICENSE]().
