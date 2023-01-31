# 5. Enforcing Security :lock:

:house: [Overview](../../README.md)

<br>
<br>

<img style="float: center;" src="../images/security.jpg">

## Protect All Services  

<br>

Let's add a Basic Authentication ( username,  password ) to protect our services. 

> :information_source:  For simplicity sake, we will save the user name and password within our config file. In normal circumstances, this need to be securely save under a secured database.

- In **./config.json**, we will add a key-value pair  for the basic authentication 
  
  ```
    {
        ...

        "basic_cred": {
            "<username-1>": "<userpass-1>"
        }

    }
  ```
- In **./src/wine_predictor_api/specs/openapi_spec.yaml** add the Basic Auth security scheme below under **components** 
  ```yaml
    ... 
    components:
        securitySchemes:
            basicAuth:
            type: http
            scheme: basic
            x-basicInfoFunc: wine_predictor_api.security.authentication.basic_auth
  ```

    Here the value of x-basicInfoFunc (**wine_predictor_api.security.authentication.basic_auth**) indicate the adequate handler/function that implements the security checks. Find more security schemes [here](https://swagger.io/docs/specification/authentication/)

- Hence, let's create the the module **authentication.py** under the package **./src/wine_predictor_api/security** .
- And implement the submodule **basic_auth** 
  ```python

    from wine_predictor_api import api_config

    PASSWD = api_config.get("basic_cred", {})


    def basic_auth(username, password):
    """
    Basic Auth implementation
    :param username:
    :param password:
    :return:
    """
        if PASSWD.get(username) == password:
            return {"sub": username}

        return None
  ``` 

<br>
<br>

## Except  PING Endpoint  

<br>

We  intentionally wish to disable security checks on the **PING** endpoint. How do we achieve this having that the security was applied on all the endpoints ? 

  - Just by specifying `security: [] ` under the desired path as demonstrated below 
    
    ```yaml 
    ...

    paths:
        /ping:
            get:
            summary: Check the API health
            security: []
            ...

    ...

    ```

<br>
<br>

## Our Project so Far ...

<br>


Below is our project tree/structure as at now 

<details>
  <summary>view tree and snapshot</summary>


```
.
│   
├── dataset
│   └── winequality.csv 
│
├── src
│   └── wine_predictor_api
│       ├── __init__.py
│       ├── security
│       │   ├── __init__.py
│       │   └── authentication.py
│       ├── services
│       │   ├── __init__.py
│       │   ├── healthcheck.py
│       │   ├── learner.py
│       │   └── predictor.py
│       └── specs
│           ├── __init__.py
│           └── openapi_spec.yaml
│
├── venv 
├── .gitignore
├── config.json
├── logging.yaml
├── launcher.sh
├── requirements.txt
├── README.md  
└── VERSION 
```

> :camera: Find [here](https://github.com/beteko/wine-predictor-snapshots/tree/main/iteration-1) the current state of the project as at now

</details>

<br>
<br>

---


[ << ( 4. Implementing Services ) ](../chapters/chapter_4.md) &nbsp;&nbsp; |  &nbsp;&nbsp;  [ ( 6. Writing Unit Testing ) >>](../chapters/chapter_6.md) 