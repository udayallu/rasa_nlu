# Language Understanding with Rasa NLU
### Rasa NLU is an open-source tool for intent classification and entity extraction.
**For Example:** "I am looking for a Mexican restaurant in the center of town"
```
{
  "intent": "search_restaurant",
  "entities": {
    "cuisine" : "Mexican",
    "location" : "center"
  }
}
```
#### The intended audience is mainly people developing bots.
- You can use Rasa as a drop-in **replacement** for wit , LUIS , or Dialogflow, the only change in your code is to send requests to localhost instead 
-  https calls are slow, and you’re always constrained by the design choices that went into the API endpoints. Libraries, on the other hand, are hackable.
- Thirdly, there’s a good chance you’ll be able to build something that performs better on your data & use case. 
- You can think of Rasa NLU as a set of high level APIs for building your own language parser using existing NLP and ML libraries
- The **setup process** is designed to be as simple as possible. If you’re currently using wit, LUIS, or Dialogflow, you just:
1. download your app data from wit or LUIS and feed it into Rasa NLU
2. run Rasa NLU on your machine and switch the URL of your wit/LUIS/Dialogflow api calls to localhost:5000/parse.
3. Rasa NLU is written in Python, but it you can use it from any language through Using Rasa NLU as a HTTP server.
4. If your project is written in Python you can simply import the relevant classes.

## Training and Data Format
- The training data for Rasa NLU is structured into different parts, common_examples, entity_synonyms and regex_features. The most important one is common_examples.
```
{
    "rasa_nlu_data": {
        "common_examples": [],
        "regex_features" : [],
        "entity_synonyms": []
    }
}
```
- You can use **Chatito** , a tool for generating training datasets in rasa’s format using a simple DSL or **Tracy**, a simple GUI to create training datasets for rasa.
https://rodrigopivi.github.io/Chatito/
https://yuukanoo.github.io/tracy
- Entities are specified with a start and end value, which together make a python style range to apply to the string, e.g. in the example below, with text="show me chinese restaurants", then text[8:15] == 'chinese'.
- Entities can span multiple words, and in fact the value field does not have to correspond exactly to the substring in your example. That way you can map synonyms, or misspellings, to the same value.
```
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
}
```
## Entity Synonyms
- If you define entities as having the same value they will be treated as synonyms. Here is an example of that:
````
[
  {
    "text": "in the center of NYC",
    "intent": "search",
    "entities": [
      {
        "start": 17,
        "end": 20,
        "value": "New York City",
        "entity": "city"
      }
    ]
  },
  {
    "text": "in the centre of New York City",
    "intent": "search",
    "entities": [
      {
        "start": 17,
        "end": 30,
        "value": "New York City",
        "entity": "city"
      }
    ]
  }
]
```
- as you can see, the entity city has the value New York City in both examples, even though the text in the first example states NYC.
- Alternatively, you can add an “entity_synonyms” array to define several synonyms to one entity value. Here is an example of that:
```
{
  "rasa_nlu_data": {
    "entity_synonyms": [
      {
        "value": "New York City",
        "synonyms": ["NYC", "nyc", "the big apple"]
      }
    ]
  }
}
```
## POST /evaluate
- You can use this endpoint to evaluate data on a model. The query string takes the project (required) and a model (optional).
- You must specify the project in which the model is located. N.b. if you don’t specify a model, the latest one will be selected. 
- This endpoint returns some common sklearn evaluation metrics (accuracy, f1 score, precision, as well as a summary report).
```
$ curl -XPOST localhost:5000/evaluate?project=my_project&model=model_XXXXXX -d @data/examples/rasa/demo-rasa.json | python -mjson.tool

{
    "accuracy": 0.19047619047619047,
    "f1_score": 0.06095238095238095,
    "precision": 0.036281179138321996,
    "predictions": [
        {
            "intent": "greet",
            "predicted": "greet",
            "text": "hey",
            "confidence": 1.0
        },
        ...,
    ]
    "report": ...
}
```
## GET /status
- This returns all the currently available projects, their status (training or ready) and their models loaded in memory. also returns a list of available projects the server can use to fulfill /parse requests.
```
$ curl localhost:5000/status | python -mjson.tool

{
  "available_projects": {
    "my_restaurant_search_bot" : {
      "status" : "ready",
      "available_models" : [
        <model_XXXXXX>,
        <model_XXXXXX>
      ]
    }
  }
}
```
## Entity Extraction
- There are a number of different entity extraction components, which can seem intimidating for new users. Here we’ll go through a few use cases and make recommendations of what to use.

![]
