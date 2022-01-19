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


## License
For all licensing information refer to [LICENSE]().
