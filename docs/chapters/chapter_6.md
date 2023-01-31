# 6. Writing Unit Testing :ballot_box_with_check:

:house: [Overview](../../README.md)

<br>
<br>

## Get ready for Testing 

<br>

Before writing all unit tests, we will create test assets or resources, import a set of dependencies and configure the shared fixtures.

<br>

**IMPORT DEPENDENCIES**

Install on your active virtual environment the dependencies below  using `pip install <dep-1> <dep-2> ..<dep-n>` .  
- [pytest](https://docs.pytest.org/en/7.2.x/)
- [pytest-mock](https://pypi.org/project/pytest-mock/)
- [pytest-cov](https://pypi.org/project/pytest-cov/)


:information: preferably create a text file **dev-requirements.txt** in the root folder and include all dependencies above.  

**./dev-requirements.txt**
```txt
pytest
pytest-mock
pytest-cov
```
<br>

**CREATE TEST ASSETS**


- In the root folder, let's create a copy of  **config.json** named **config.template.json** where we wil substitutes all sensitive information by  `DUMMY` or  `TO_BE_FILLED` or any placeholder of your choice. Beyond the testing phase, this file will serve as a base for initializing the config file on the target servers. 

  **./config.template.json**
  ```json 
  {
    "spec_options": {
        "title": "Wine Quality Estimation API",
        "description": "Application Programming Interface that evaluates the quality of wine based on carefully preselected features.",
        "contact_name": "Beteko",
        "contact_website":"https://www.lipsum.com/suport",
        "contact_email":"beteko@donotcontactme.com"
    },
    "basic_cred": {
        "test_user": "test_pass"
    },
    "data":{
        "path": "TO_BE_FILLED"
    },
    "model":{
        "path": "TO_BE_FILLED"
    }
  }
  ```

- Create the folder `./src/tests`
- Create the package `assets` in `./src/tests`
  - Then, create an sample or exact copy the CSV dataset in the `assets` folder and name it  `sample_data.csv`
  - Also create a copy of of a saved  model file in the `assets` folder  then name it `test_model.jl`
- Finally implement a ***where** function in **./src/tests/__init__.py** module for facilitate referencing of the **asset** directory
    ```python
    import os


    def where():
        return os.path.dirname(os.path.realpath(__file__))
    ```
<br>


**DEFINE SHARED FIXTURES**

We will create a module **conftest.py** in the package **./src/tests** where will implemented all shared fixtures. 



<details>
    <summary>  <b> ./src/tests/conftest.py</b></summary>

```python
import pytest
import os
import base64
from tests.assets import where as asset_dir_path


@pytest.fixture()
def app():
    from wine_predictor_api import create_app
    app = create_app()
    app.config.update(
        TESTING=True,
        LOGIN_DISABLED=True
    )
    yield app


@pytest.fixture
def test_auth():
    test_cred = base64.b64encode(b"test_user:test_pass").decode('utf-8')
    return f"Basic {test_cred}"


@pytest.fixture
def test_data_path():
    return asset_dir_path() + "/sample_data.csv"


@pytest.fixture
def test_model_path():
    return asset_dir_path() + "/test_model.jl"


@pytest.fixture()
def app_client(app):
    return app.test_client()


@pytest.fixture()
def app_runner(app, pytest_configure):
    return app.test_cli_runner()


@pytest.hookimpl
def pytest_configure(config):
    os.environ["API_CONFIG"] = "config.template.json"
```
</details>


<br>
<br>

## Implement Unit Tests

<br>

let's set up unit tests on key services/functions  such  as **ping**, **train_model**, **estimate_wine_quality**. The goal here is not to exhaustively test all available sub-modules but rather prioritize and reach a coverage of about 80% . 

<br>

**"HEALTHCHECK" UNIT TEST**

**./src/tests/test_healthcheck.py**
```python 
def test_ping(app_client):
    # given
    endpoint = "/ping"

    # when
    response = app_client.get(endpoint)

    # then
    assert response.status_code == 200
    assert "pong" in response.text
```

Then  run all unit tests by executing the following command in your terminal 

```sh 
$ pytest -vs 

```

Here the flag `-v` activates the verbose mode while  `-s` is to display all message in the output stream if any. 


<br>

**"LEARNER" UNIT TESTS**

:tada: **DIY** : Let's implement together the unit test for the  **Learner** service 

<br>

**"PREDICTOR" UNIT TESTS**


:tada: **DIY** : Let's implement together the unit test for the  **Predictor** service


<br>
<br>

---


[ << ( 5. Enforcing Security ) ](../chapters/chapter_5.md#protect-all-services) &nbsp;&nbsp; |  &nbsp;&nbsp;  [ ( 7. Ensuring Linting & Type Checking ) >>](../chapters/chapter_7.md#linting-with-flake8)