# Storing Receipt Line-items Extracted Using Form Recognizer in CSV Format

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

3. You will be required to create a connection to the storage account. Fill in the information and click on Create to proceed. 

![image](https://user-images.githubusercontent.com/88718044/150132267-337aa540-f7ec-46f4-8a96-ffdd5d87d669.png)

4. You will be required to select a container in the storage account to monitor. Fill in the information and click on New step to proceed.

![image](https://user-images.githubusercontent.com/88718044/150132720-81ded702-b031-49fd-a569-ccd55b44dcc7.png)

5.From the "Azure Blob Storage" list of actions, select "Get blob content". 

![image](https://user-images.githubusercontent.com/88718044/150133252-07650a5a-cedd-42d8-894a-e60ec339b183.png)

6. Fill in the information and click on New step to proceed.

![image](https://user-images.githubusercontent.com/88718044/150160238-3158d125-d525-48e7-afa3-748c2aa8d400.png)

7. Search for "Form Recognizer" and select "Analyze Receipt" from the list of actions.

![image](https://user-images.githubusercontent.com/88718044/150135090-fcfbb294-cae9-471b-b903-7b146aaa82e5.png)

8. You will be required to create a connection to Form Recognizer. Fill in the information and click on Create to proceed.

![image](https://user-images.githubusercontent.com/88718044/150135287-5b6d4d44-cad3-4e08-8e34-8fdcb708226d.png)

9. Fill in the information and click on New step to proceed.

![image](https://user-images.githubusercontent.com/88718044/150135574-e5399848-76d7-4088-aca2-8e99e9fb7406.png)

10. From the "Variables" list of actions, select "Initialize variable".

![image](https://user-images.githubusercontent.com/88718044/150136043-cd2cf53d-d865-4f9b-9617-e491de3cdb50.png)

11. Create an empty array variable that will hold the line items. Fill in the information and click on New step to proceed.

![image](https://user-images.githubusercontent.com/88718044/150143477-64df897d-d3d7-4333-80e9-e92d240b82a4.png)

12. From the "Control" list of actions, select "For each".

![image](https://user-images.githubusercontent.com/88718044/150143731-8589d3e0-473f-417f-bd72-b4a87f35d975.png)

13. Search for "documentResults" and select it as the loop parameter. Click on Add an action inside the loop to proceed.

![image](https://user-images.githubusercontent.com/88718044/150144230-09d44f23-2182-48df-bf84-17cb5e0d3437.png)

14. Add another "For each" loop. Search for "items" and select "Items field Items" as the loop parameter. Click on Add an action inside the second loop to proceed.

![image](https://user-images.githubusercontent.com/88718044/150144798-06fff3f6-459e-4215-acba-bc528f753f4b.png)

15. From the "Data Operations" list of actions, select "Compose".

![image](https://user-images.githubusercontent.com/88718044/150151338-38fb893b-0856-4f43-b2a7-3cd38ce15244.png)

16. Use the following structure as input and replace the placeholders with the corrosponding dynamic values. Click on Add an action inside the second loop to proceed.

```json
{
  "item": "<Item field value Name>",
  "price": "<Item field value Price>",
  "quantity": "<Item field value Quantity>",
  "total_price": "<Item field value Total price>"
}
```

![image](https://user-images.githubusercontent.com/88718044/150153614-e85d1212-9036-416e-bca5-233301121ce6.png)

17. From the "Variables" list of actions, select "Append to array variable".

![image](https://user-images.githubusercontent.com/88718044/150154440-3c7633ac-7164-4b6b-a9fa-77d653036a55.png)

18. Select the name of the array and choose "Outputs" of "Compose" as the value. Click on New step outside the loops to proceed.

![image](https://user-images.githubusercontent.com/88718044/150154989-d860eddf-2c88-45e8-a478-fe37fd9914ff.png)

19. From the "Data Operations" list of actions, select "Create CSV table".

![image](https://user-images.githubusercontent.com/88718044/150155475-94a9483a-1bf2-4279-8f15-209e9e337540.png)

20. Fill in the information and click on New step to proceed. 

![image](https://user-images.githubusercontent.com/88718044/150155592-5442ae81-354e-4932-935d-934b218be597.png)

21. From the "Azure Blob Storage" list of actions, select "Create blob".

![image](https://user-images.githubusercontent.com/88718044/150155806-680b0be0-7d22-4684-b4df-daafbccd144b.png)

22. Fill in the information. For "Blob name" use a concat function to append "-line-items.csv" to the file name for the generated CSV file. For "Blob content", use "Outputs" of the "Create CSV table" action. Save your Logic App to proceed.

![image](https://user-images.githubusercontent.com/88718044/150156330-7ddb5b02-4eb2-4d5c-a917-3c779c17e706.png)

### Step 3: Testing the Logic App
1. From "Run Trigger", click on "Run".

![image](https://user-images.githubusercontent.com/88718044/150158782-5decf1eb-52d7-4b0a-b424-705f6263316f.png)

2. Upload the sample receipt "Receipt.jpg" to the container in the storage account.

![Receipt](https://user-images.githubusercontent.com/88718044/150165816-85e7bdc8-4437-4a6a-aa25-03cdc07a2e6d.png)

3. Wait for the Logic App flow to finish. 

![image](https://user-images.githubusercontent.com/88718044/150165394-d6f4675e-d563-462a-bd40-d4cc64f89806.png)

4. The resulted CSV file will contain all line items from the receipt.

![image](https://user-images.githubusercontent.com/88718044/150165472-b10accb5-f2ca-46f2-85b8-0db05dfb0562.png)

![image](https://user-images.githubusercontent.com/88718044/150165654-eb0acc24-fc4a-417f-95ff-2f637967d7b6.png)

## License
For all licensing information refer to [LICENSE]().
