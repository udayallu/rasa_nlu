# Rasa NlU Demo
## Requirements 
1. We need to Install the anaconda in the PC/mac
2. You need to have Microosft Visual C++ Build Tools if you are running on mac
3. We will be working with the **Anaconda prompt** in the demo.

## Steps
1. Open the Anaconda Prompt in the **Admin mode**.
![](https://github.com/udayallu/rasa_nlu/blob/master/Photos/img1.PNG)
2. We need to create an Virtual Environment for Python.
```
conda create -n [type venv name] [type python= version] pip
```
Example
```
conda create -n py35 python=3.5 pip
```
3. Activating the Virtual Environemnt 
```
activate [env-name]
```
example
```
activate py35
```
4. Creating Directory inside the following virtual environment
-  C:\Users\rucha.vaishampayan\AppData\Local\Continuum\anaconda3\Anaconda 3.5\envs\py35\ **demobot_folder**
- First browse to the p35 folder using **cd** command
- then create a new folder **demobot_folder** using **mkdir** command
- After creating the folder go to the **next steps**

5. Install the following Libraries one by one 
##Package installations
- python -m pip install -U spacy
- python -m spacy download en
- python -m pip install -U scikit-learn
- python -m pip install -U numpy
- python -m pip install -U scipy
- python -m pip install -U sklearn-crfsuite
- python -m pip install -U duckling
- python -m pip install  rasa_nlu
Note: If you face any error while installing the above commands please resolve them and install again.

6. To train the data to rasa we need a json data, i'm building nlu for a restaruent search bot
- so i build some dailogs in json using this website https://github.com/RasaHQ/rasa-nlu-trainer
-save this as testdata.json
- create a folder named **data** inside the **demobot_folder**
- copy the testdata.json into the **data** folder that you have creatd
```
{
  "rasa_nlu_data": {
    "common_examples": [
      {
        "text": "hi",
        "intent": "Greet",
        "entities": []
      },
      {
        "text": "Hello",
        "intent": "Greet",
        "entities": []
      },
      {
        "text": "hola",
        "intent": "Greet",
        "entities": []
      },
      {
        "text": "hay",
        "intent": "Greet",
        "entities": []
      },
      {
        "text": "hi it's me ",
        "intent": "Greet",
        "entities": []
      },
      {
        "text": "hi i'm ",
        "intent": "Greet",
        "entities": []
      },
      {
        "text": "show me chinese restaurants",
        "intent": "restaurant_search",
        "entities": [
          {
            "start": 8,
            "end": 15,
            "value": "chinese",
            "entity": "cuisine"
          }
        ]
      },
      {
        "text": "i'm looking for a place to eat",
        "intent": "restaurant_search",
        "entities": []
      },
      {
        "text": "show me good restaurants",
        "intent": "restaurant_search",
        "entities": []
      },
      {
        "text": "show me restaurants in Mumbai",
        "intent": "restaurant_search",
        "entities": [
          {
            "start": 23,
            "end": 29,
            "value": "Mumbai",
			 "synonyms": ["bombay"],
            "entity": "Town"
          }
        ]
      },
      {
        "text": "show me restaurants in North Mumbai",
        "intent": "restaurant_search",
        "entities": [
          {
            "start": 29,
            "end": 35,
            "value": "Mumbai",
            "entity": "Town"
          },
          {
            "start": 23,
            "end": 28,
            "value": "North",
            "entity": "area"
          }
        ]
      },
      {
        "text": "i'm looking for a place to eat in North Mumbai",
        "intent": "restaurant_search",
        "entities": [
          {
            "start": 34,
            "end": 39,
            "value": "North",
            "entity": "area"
          },
          {
            "start": 40,
            "end": 46,
            "value": "Mumbai",
            "entity": "Town"
          }
        ]
      },
      {
        "text": "places to eat in South Mumbai",
        "intent": "restaurant_search",
        "entities": [
          {
            "start": 17,
            "end": 22,
            "value": "South",
            "entity": "area"
          },
          {
            "start": 23,
            "end": 29,
            "value": "Mumbai",
            "entity": "Town"
          }
        ]
      },
      {
        "text": "restaurants in East Mumbai",
        "intent": "restaurant_search",
        "entities": [
          {
            "start": 14,
            "end": 19,
            "value": " East",
            "entity": "area"
          },
          {
            "start": 20,
            "end": 26,
            "value": "Mumbai",
            "entity": "Town"
          }
        ]
      },
      {
        "text": "show me restaurants in Bagalore",
        "intent": "restaurant_search",
        "entities": [
          {
            "start": 23,
            "end": 31,
            "value": "Bagalore",
            "entity": "Town"
          }
        ]
      },
      {
        "text": "show  restaurants in South Bangalore ",
        "intent": "restaurant_search",
        "entities": [
          {
            "start": 27,
            "end": 37,
            "value": "Bangalore ",
			 "synonyms": ["blr","bengaluru","bangalore city"],
            "entity": "Town"
          },
          {
            "start": 21,
            "end": 26,
            "value": "South",
            "entity": "area"
          }
        ]
      }
    ]
  }
}
```

7. We also need anathore json file for configuration, name the file as config_spacy1.json
- all the code inside poinintg to the path of a file or folder
- save the json file with below code and place it in the **demobot_folder**
- in the below code **path** we mention named as project, so we need to actually create a folder named **Project** inside the demobot folder.
- create onemore folder named **bot_rasa** inside the project.
```
{
    "pipeline": "spacy_sklearn",
    "path": "./projects",
    "data": "./data/testData.json",
	"project": "bot_rasa"
	}
  
```
8. Now run the following command (Note-  edit the path and filenames according with your sytstem path)
```
python -m rasa_nlu.train -c "C:\Users\uday.a\AppData\Local\Continuum\anaconda3\envs\py35\demobot_folder\config_spacy1.json"
```
9. Now after running the above command you will get a **Finished Training Message** and a **mode-code** make a note of the code.
![](https://github.com/udayallu/rasa_nlu/blob/master/Photos/img10.PNG)
10. now go to the config_spacy1.json and edit the code, by adding a "," after the project name and add the following code
- note the model code is what you got before and save it 
```
	"server_model_dir" : "./model_20180425-160432"
```
11. After add the model id, the code should look like this
```
{
   "pipeline": "spacy_sklearn",
    "path": "./projects",
    "data": "./data/testData.json",
	"project": "bot_rasa",
	"server_model_dir" : "./model_20180426-143455"
	}
```
12. Now run the following code
```
python -m rasa_nlu.server -c “C:\Users\uday.a\AppData\Local\Continuum\anaconda3\envs\py35\demobot_folder\config_spacy1.json”

```
![](https://github.com/udayallu/rasa_nlu/blob/master/Photos/img11.PNG)
- the service will start now, if not started press enter and wait in the cmd 

13. Now go to http://localhost:5000/ in your browser, it should show you the following msg

14. Now your rasa service is running 
![](https://github.com/udayallu/rasa_nlu/blob/master/Photos/img12.PNG)

15. Now you need to try different sentences to check the model accuracy 
- in the below url i'm trying to query the url as "show reasturants in west bangalore"
- the prkect name is your bot_rasas which is the folder name we created inside the folder.
- %20 is the space
http://localhost:5000/parse?project=bot_rasa&q=show%20%reasturants%20in%20west%20banaglore

![](https://github.com/udayallu/rasa_nlu/blob/master/Photos/Capture.PNG)

