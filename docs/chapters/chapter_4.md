# 4. Implementing all  Services :hammer_and_wrench:

:house: [Overview](../../README.md)


<br>
<br>

<img style="float: center;" src="../images/DIY-services.jpg">

## PING Service Implementation  

:tada: **DIY** :  let's implement  the **PING** service with the specification below


<details>
    <summary>  <b> Design specification</b></summary>

<br>

**SPECS DESIGN**

<br>

- Create a YAML openapi spec (3.0.0) in **./src/wine_predictor_api/specs/openapi_spec.yaml** 
- Create the <b>info </b>section containing the information below 
  - title 
  - description 
  - version (0.1.0)  
  - contact name 
  - contact email 
  - contact url/website  
  - version  
  
- Create only one <b>server</b> url with the default value  "/"
- Create one <b>tag</b> "Health"
- Create a **PING** endpoint 
  - with "GET" as  method  
  - with "/ping" as path 
  - under the "Health" tag
  - with the following description "Check the API health" 
  - without parameters
  - returns **200** as response code when the "API is up and running"

<br>

**CONTROLLER DESIGN**

<br>

- Create a module  **healthcheck.py** under package **wine_predictor_api.services**
- In the **healthcheck.py** module create a function **ping** with no parameter that returns the tuple (message, status_code) below
  - "pong", 200


<br>

**CONNECT SPEC TO CONTROLLER**

  - under  "/ping" path, reference the adequate request handler located at **wine_predictor_api.services.healthcheck.ping**


<br>

<br>

**Connexion Object**

<br>

To instantiate  a connexion object that will automagically links the specs to the controller, follow the following instructions  

- In **wine_predictor_api/specs/\_\_init\_\_.py** add the below to easily reference your YAML specs 
  
  ```python 
    import os


    def where():
        return os.path.dirname(os.path.realpath(__file__))
  ```
- Then in **/src/wine_predictor_api/\_\_init\_\_.py**  create and initialize your connexion object referring the YAML spec  as described below 
  ```python 
    import connexion
    from wine_predictor_api import specs


    def create_app():
        app = connexion.FlaskApp(__name__, specification_dir=specs.where())
        app.add_api("openapi_spec.yaml")
        return app.app
  ```

<br>

**Launcher**

<br>

We will create a launcher file  **launcher.sh** in the root folder to define your variables and  launch your API via **flask**. 

> **Reminder** :thought_balloon:: Flask should not be explicitly installed on your venv since it is already one of the dependency of **connexion**. kindly execute  `pip list ` in your IDE terminal to attest it. 

- Create the file **./launcher.sh** with the content below

    ```sh 
    export PYTHONPATH="src"
    export FLASK_APP="wine_predictor_api:create_app"
    export FLASK_DEBUG=true
    PORT=5000

    python -m flask run -h 0.0.0.0 -p $PORT
    ```
- Provide the **execute** permission to **launcher file** 
    ```sh 
    $ chmod +x launcher.sh
    ```

- Run your **launcher file** 
    ```sh 
    $ sh launcher.sh
    ```

- Access your swagger UI by appending `/ui` on your IP and PORT  `http://127.0.0.1:<PORT>`

  
> **Warning** :warning: : Though this launcher file can be committed on git, it is only meant to be executed during the development phase and **NOT**  on the production server.  

<br>

**Logging**

<br>

In order to properly log all activities **DEBUG, INFO, ERROR ...** for more accountability, we will use the in-built python **logging** lib configured with  an external YAML file. 

- First, create the file  **logging.yaml** in the root folder 


  **./logging.yaml**
  ```yaml 
  version: 1
  formatters:
    default:
      format: '%(asctime)s %(levelname)s %(name)s %(message)s'
  handlers:
    console:
      class: logging.StreamHandler
      level: DEBUG
      formatter: default
      stream: ext://sys.stdout
  root:
    level: DEBUG
    handlers: [console]
  ```

  :grey_exclamation: : Using a configuration file for logging allows you to change the **stream, level and handlers** depending on the target server/environment (Dev/UAT/ Prod). 
  
  For more information on logging file, kindly click [here](https://docs.python.org/3/library/logging.config.html) 
 

- Then, initialize your logging object in **/src/wine_predictor_api/\_\_init\_\_.py**
  
  ```python
    ...

    import yaml
    import logging.config

    ...

    def init_logger(name=None):
        with open(os.getenv('LOGGING_CONFIG', "logging.yaml")) as stream:
            logging_config = yaml.safe_load(stream)
            logging.config.dictConfig(logging_config)
            return logging.getLogger(name)

    ...

    logger = init_logger()

    ...

    __all__ = ['logger']

  ```

- Use the **logger** objects in any of your modules following the example below  
  
  ```python 

  from wine_predictor_api import logger

  logger.debug("Writing for accountability ...")
  logger.info("Just an information ...")
  logger.error("An error occurred ...")
  

  ``` 

<br>

**Configuration**

<br>

- We will also create a JSON configuration file  **config.json** in the root project
  **./config.json** that will externalize from the code base all  sensitive information  or servers (Test/UAT/Prod) specific variables.  

  ```json
    {
        "spec_options": {
            "title": "Wine Quality Estimation API",
            "description": "Application Programming Interface that evaluates the quality of wine based on carefully preselected features.",
            "contact_name": "Beteko",
            "contact_website":"https://www.lipsum.com/suport",
            "contact_email":"beteko@donotcontactme.com"
        },
        "data":{
            "path": "</path/to/wine/data>"
        }
    }
  ```

- Load  your configuration file in  **/src/wine_predictor_api/\_\_init\_\_.py**
  ``` python
  ...

  from typing import Dict, Any
  import json
  import os

  ...

  def init_config():
    with open(os.getenv('API_CONFIG', "config.json")) as stream:
        return json.load(stream)
    
  ...

  api_config: Dict[str, Any] = init_config()

  ...

   __all__ = ['logger', 'api_config']

  ```

- Access the **api_config** object  in any of your modules following the example below  
  
  ```python 

  from wine_predictor_api import api_config

  data_path = api_config.get("data").get("path")

  ```

- Let's specify our API Config file  in the **launcher.sh**

    ```sh 
    export API_CONFIG="config.json"

    ...

    python -m flask run -h 0.0.0.0 -p $PORT
    ```

- Also, we can even inject the  JSONObject **spec_options** (in the config file) to be rendered on our YAML spec (openapi_spec.yaml).  
  - In  **./src/wine_predictor_api/specs/openapi_spec.yaml**, replace all information defined in the **info** section by jinja Template variable as illustrated below. 
    ```yaml

    openapi: 3.0.0
    info:
      title: {{title}}
      description: {{description}}
      contact:
        name: {{contact_name}}
        email: {{contact_email}}
        url:  {{contact_website}}
      version: {{version}}

      ...

    ```

  - In **/src/wine_predictor_api/\_\_init\_\_.py** update the submodule  **create_app** to pass  as argument the JSONbject   **spec_options**  into the **connexion** object 
    ```python
      ...

      def create_app():
        spec_options = api_config.get("spec_options", {})
        spec_options['version'] = os.environ.get("API_VERSION", 'version_not_set')

        app = connexion.FlaskApp(__name__, specification_dir=specs.where())
        app.add_api("openapi_spec.yaml", arguments=spec_options)
        return app.app

    ...

    ```


</details>


<br>
<br>

## "Train Wine Estimator" Service Implementation  

:tada: **DIY** : Let's implement the **Get Wine Quality** service with the specification below 


<details>
    <summary>  <b> Design specification</b></summary>

<br>

**SPECS DESIGN**

<br>

within our YAML spec @**./src/wine_predictor_api/specs/openapi_spec.yaml** we will add a new service interface to automate the **training phase** of the wine predictor. 
  
- Add a new <b>tag</b> "Learning"
- Create a **Train Model** endpoint  
  - with "PATCH" as method  
  - with "/wine/model" as path 
  - under the "Learning" tag
  - with the following description "(Re)train the wine quality model based on a predefined dataset" 
  - without parameters
  - returns the responses code and description  below 
    -  **200** as response code when the "New model has been successfully trained but discarded"
    - **201** as response code when the "New model has been successfully trained and saved as default"
    - **404** as response code when the "Model path is not found"
    - **500** as response code when there is an "Internal server error"

<br>

**CONTROLLER DESIGN**

<br>

- Add in **./config.json**  the path for your model 
  ```json
    {
        ...

        "model":{
            "path": "</path/to/wine/model>"
        }
    }
  ```

- Create a module  **learner.py** under package **wine_predictor_api.services**
- In the **learner.py** module create  the functions below 
  -  **load_data**  submodule that loads the CSV dataset (path in config file )
     -  takes no argument
     -  returns the dataset as DataFrame 
  -  **evaluate_model** submodule that computes the accuracy of your model from the test split 
     -  takes three (3) parameters 
        -  the model 
        -  the test data (without prediction)
        -  the test target or ground truth (respective prediction of test data) 
     -  returns the evaluation score ( Mean Square Error )
  -  **save_model** submodule that saves the the hyper-parameters of your model 
     -  takes two(2) parameters 
        -  the model object
        -  the output path  (where the model will be saved)
     - returns the status of write operation 
  - **train_model** submodule that trains a model from the  train dataset, evaluate the model performance and save it if its performance is better the the existing model 
    - takes no argument
    - returns a tuple ( Description , Status Code )
  
<br>

**CONNECT SPEC TO CONTROLLER**

<br>

- under  "/wine/model" path, reference the adequate request handler located at **wine_predictor_api.services.learner.train_model**


</details>


<br>
<br>

## "Get Wine Quality" Service Implementation  

:tada: **DIY** : Let's implement the **Get Wine Quality** service with the specification below 


<details>
    <summary>  <b> Design specification</b></summary>

<br> 

**SPECS DESIGN**

<br>

within our YAML spec @**./src/wine_predictor_api/specs/openapi_spec.yaml** we will add another service interface to **predict the quality** of the wine. 
  
- Add a new <b>tag</b> "Prediction"
- Create a **Predict Wine Quality** endpoint  
  - with "GET" as method  
  - with "/wine/quality" as path 
  - under the "Prediction" tag
  - with the following description "Estimate the quality of a wine based on several preselected features" 
  - accepts the mandatory following query parameters
    - fixed acidity 
      - Nonvolatile, volatile acids of Wine. Value should be between 4.6 to 15.9
    - volatile_acidity
      - The amount of acetic acid in wine. Value should be between 0.12 to 1.58
    - citric_acid
      - Adds flavors to wine and is found in small quantity. Value should be between 0.0 to 1.0
    - residual_sugar
      - Sugar content after fermentation stops. Value should be between 0.9 to 15.5
    - chlorides
      - Residual Salt in the wine. Value should be between 0.012 to 0.611
    - free_sulfur_dioxide
      - The free form of SO2 exists in equilibrium between molecular SO2 and bisulfite ion. Value should be between 1.0 to 72.0
    - total_sulfur_dioxide
      - Amount of free and bound forms of SO2. Value should be between 6.0 to 289.0
    - density
      - The density of a substance is its mass per unit volume. Value should be between 0.99007 to 1.00368
    - ph
      -  Describes how acidic or basic a substance is. Value should be between 2.74 to 4.01
    - sulphates
       -  A wine additive that can contribute to sulfur dioxide gas (SO2) levels. Value should be between 0.33 to 2.0
    - alcohol
      - Percentage of alcohol content in the wine.  Value should be between 8.4 to 14.9
  - returns the responses code and description  below 
    - **200** as response code when the "Wine quality is successfully estimated"
    - **400** as response code when an "Missing/Invalid required parameter "
    - **404** Model path is not found"
    - **500** as response code when there is an "Internal server error"


<br>

**CONTROLLER DESIGN**

<br>


- Create a module  **predictor.py** under package **wine_predictor_api.services**
- In the **predictor.py** module create  the functions below 
  -  **load_model**  submodule that load the machine learning model from config file
     -  takes no argument
     -  returns model object  
  -  **get_features** submodule that returns the list of all 11 feature names for predicting the quality of the wine 
     -  takes no argument 
     - returns a list of 11 features
  -  **prepare_data** submodule that transforms all user inputs (11 params) from Dict to Numpy array   
     -  takes one parameter 
        -  data as **Dict**    
     -  returns data in Numpy Array with values ordered based on **get_features** output 
  - **estimate_wine_quality** submodule that estimate the quality of a wine from a defined set of features 
    - takes  one parameter 
      - user inputs (11 params) as **Dict**
    - returns a Tuple ( Estimation JSONObject , Status Code )
      - with **Estimation JSONObject** in the format below 
        - {"estimation": \<predicted_value\>}


<br>


**CONNECT SPEC TO CONTROLLER**

<br>


- under  "/wine/quality" path, reference the adequate request handler located at **wine_predictor_api.services.predictor.estimate_wine_quality**

</details>


<br>
<br>

---


[ << ( 3. Understanding OpenAPI ) ](../chapters/chapter_3.md) &nbsp;&nbsp; |  &nbsp;&nbsp;  [ ( 5. Enforcing Security ) >>](../chapters/chapter_5.md)  
 
