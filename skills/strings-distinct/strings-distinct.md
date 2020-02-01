# Azure Cognitive Search Python Custom Skill Duplicates removal - Distinct

This code is a Python Custom Skill, for Azure Cognitive Search, based on Azure Functions for Python. It removes duplicated terms from the input text. 

It is specially useful when you are processing per page and the same entities or key phrases are extracted in all pages, creating duplicated values within your enrichment pipeline. You can use this skill to clean it before the data push into the Azure Cognitive Search Index. Details:

+ The Expected input is something like "text": "[BRAZIL],[BRAZIL],[ARGENTINA],[BRAZIL]"
+ The output will be "text": "[BRAZIL],[ARGENTINA]"
+ This code is case sensitive. Change it if you need.

## Required steps

1. Follow [this](https://docs.microsoft.com/en-us/azure/azure-functions/functions-create-first-function-python) tutorial.
1. Use the Python code below as your **__init__.py** file. Customize it with your storage account details, also with your csv file name and target column. As you can see below, my sample csv file target column name is **Term**. That helps the idea that this code will extract pre-defined terms from the documents content.
1. Don't forget to add **azure.functions** to your requirements.txt file.
1. Connect your published custom skill to your Cognitive Search Enrichment Pipeline. Plesae check the section below the code in this file. For more information, click [here](https://docs.microsoft.com/en-us/azure/search/cognitive-search-create-custom-skill-example#connect-to-your-pipeline).

## Python Code

The Python code for this skill is [here](./__init__.py). 

## Sample Input

Use the JSON input below to test your function. Get familiar with the code behavior in the different situations. 

This test text is a tribute to the most popular football club in the world, [Flamengo](https://en.wikipedia.org/wiki/Clube_de_Regatas_do_Flamengo), from Rio de Janeiro. It was founded in 1895 and has over 45 million fans in Brazil alone. The team was [champion](https://www.youtube.com/watch?time_continue=11&v=371FOyquzno) in its two most important matches of 2019, the Brazilian championship and the Copa Libertadores of America.

```json
 {
    "values": [
      {
        "recordId": "0",
        "data":
           {
            "text": ["FLAMENGO","VASCO","FLAMENGO","FLUMINENSE","FLAMENGO"]
           }
      } ,
        {
        "recordId": "1",
        "data":
           {
            "text": [""]
           }
      } ,    
      {
        "recordId": "2",
        "data":
           {
            "text": ["FLAMENGO","Flamengo","flamengo","FLAMENGO"]
           }
      } 
    ]
}
```

## Expected Output

```json
{
    "values": [{
        "recordId": "0",
        "data": {
            "text": ["FLAMENGO", "VASCO", "FLUMINENSE"]
        }
    }, {
        "recordId": "1",
        "data": {
            "text": [""]
        }
    }, {
        "recordId": "2",
        "data": {
            "text": ["FLAMENGO", "Flamengo", "flamengo"]
        }
    }]
}
```