# 1. Project structure 

:house: [Overview](../../README.md)

Let's cheat a bit and get a  view into the target project structure ...

<details>
  <summary>Target Project Tree Structure</summary>

```
.
├── .github
│   └── workflows
│       ├── ci.yaml  
│       ├── cd.yaml    
│       └── cl.yaml 
│   
├── dataset
│   └── winequality.csv 
│
├── src
│   ├── tests
│   │   ├── assets
│   │   │   ├── sample_data.csv   
│   │   │   └── test_model.jl  
│   │   ├── conftest.py  
│   │   ├── test_healthcheck.py 
│   │   ├── test_learner.py 
│   │   └── test_predictor.py 
│   │ 
│   └── wine_predictor_api
│       ├── security
│       │   └── authentication.py 
│       ├── services
│       │   ├── healthcheck.py
│       │   ├── learner.py 
│       │   └── predictor.py
│       └── specs
│           └── openapi_spec.yaml
│ 
├── .gitignore
├── config.template.json
├── launcher.sh 
├── logging.yaml
├── MANIFEST.in 
├── README.md 
├── dev-requirements.txt 
├── requirements.txt 
├── setup.cfg
├── setup.py
└── VERSION    

```

</details>

---


First thing First .. Let's create the basic structure.

- Create a new repository **wine-predictor-api** on your personal github  ( including a README.md and **.gitignore** )

- Then clone that repository on your local machine 
    ```
    git clone https://github.com/<my-private-git-account>/wine-predictor-api.git
    ```

-  Browse into your folder **wine-predictor-api** then create and checkout into a feature branch `init`
    ```
    cd wine-predictor-api/
    git checkout -b init
    ```

- Create the below initial project tree

    ```
    .
    │   
    ├── dataset
    │   └── winequality.csv 
    │
    ├── src
    │   └── wine_predictor_api
    │
    ├── .gitignore   
    ├── config.template.json    
    ├── requirements.txt
    ├── README.md  
    └── VERSION 
    ```

Initializing them with : 
 - content of **dataset/winequality.csv** can be downloaded [here](../assets/winequality.csv)

 - **.gitignore** file content can be replaced by the generic Python .gitignore found [here](https://github.com/github/gitignore/blob/main/Python.gitignore)

 -  Below is the content of the **VERSION** file 
    ```
    0.1.0
    ```
 -  All other files can be left as such




---

[ << ( Overview ) ](../../README.md)  &nbsp;&nbsp; |  &nbsp;&nbsp;  [ ( Chapter 2 ) >>](../chapters/chapter_2.md)  
 
 