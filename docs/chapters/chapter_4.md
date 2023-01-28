# 4. Implementing all  Services :hammer_and_wrench:

:house: [Overview](../../README.md)


<br>
<br>

## PING Service Implementation  

:tada: **DIY** :  Lets's implement  the **PING** service with the specification below


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
- Create a **PING** endpoint with 
  - "GET" method  
  - "/ping" as path 
  - under the "Health" tag 
  - controller/endpoint located at "wine_predictor_api.services.healthcheck.ping"
  - returns **200** as response code when the server is UP

<br>

**CONTROLLER DESIGN**

<br>

- Create a module  **healthcheck.py** under package **wine_predictor_api.services**
- In the **healthcheck.py** module create a function **ping** with no parameter that returns the tuple (message, status_code) below
  - "pong", 200


<br>

**CONNECT SPEC TO CONTROLLER**

<br>


<br>

**Connexion Object**

<br>

To instantiate  a connexion object that will automagically links the specs to the controller, follow the following instructions  

- In **wine_predictor_api/specs/__init__.py** add the below to easily reference your YAML specs 
  
  ```python 
    import os


    def where():
        return os.path.dirname(os.path.realpath(__file__))
  ```
- Then in **/src/wine_predictor_api/__init__.py**  create and initialize your connexion object refering the YAML spec  as described below 
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

   
> **Warning** :warning: : Though this launcher file can be commited on git, it is not meant to be executed during the development phase and **NOT**  on the production server.  

<br>

**Logging**

<br>

In order to properly log all activities **DEBUG, INFO, ERROR ...** for more accountablity, we will use the in-built python **logging** lib configured with  an external YAML file. 

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
 

- Then, initialize your logging object in **/src/wine_predictor_api/__init__.py**
  
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

  logger.debug("Writing for accuntability ...")
  logger.info("Just an information ...")
  logger.error("An error occured ...")
  

  ``` 

<br>

**Configuration**

<br>

- We will also create a JSON configuration file  **config.json** in the root project
  **./config.json**
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

- Load  your configuration file in  **/src/wine_predictor_api/__init__.py**
  ``` python
  ...

  from typing import Dict, Any
  import json

  ...

  def init_config():
    with open(os.getenv('API_CONFIG', "config.json")) as stream:
        return json.load(stream)
    
  ...

  api_config: Dict[str, Any] = init_config()

  ...

   __all__ = ['logger', 'api_config']

  ```

- Access the **api config** object  in any of your modules following the example below  
  
  ```python 

  from wine_predictor_api import api_config

  logger.debug("Writing for accuntability ...")
  logger.info("Just an information ...")
  logger.error("An error occured ...")

  ```

- Let's specify our API Config file  in the **launcher.sh**

    ```sh 
    export API_CONFIG="config.json"

    ...

    python -m flask run -h 0.0.0.0 -p $PORT
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

- Draft
- Draft
- Draft 

<br>

**CONTROLLER DESIGN**

<br>


- Draft
- Draft
- Draft 


<br>

**CONNECT SPEC TO CONTROLLER**

<br>

- Draft
- Draft
- Draft 


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


<br>

**CONTROLLER DESIGN**

<br>


<br>


**CONNECT SPEC TO CONTROLLER**

<br>

</details>

<br>
<br>


> :camera: Find [here]() the current state of the project as at now


<br>
<br>

---


[ << ( 3. Understanding OpenAPI ) ](../chapters/chapter_3.md#openapi-specs-structure) &nbsp;&nbsp; |  &nbsp;&nbsp;  [ ( 5. Enforcing Security ) >>](../chapters/chapter_5.md)  
 
