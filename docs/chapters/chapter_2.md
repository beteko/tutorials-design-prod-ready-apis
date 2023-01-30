# 2. Starting with Connexion / Specs First :link:

:house: [Overview](../../README.md)

## What is connexion ? 

[Connexion](https://github.com/spec-first/connexion) is a framework abstracting Flask that enable the design an API only by drafting the documentation (OpenAPI /Swagger YAML spec)  then maps the endpoints to your Python functions. 

Also known as **Spec-First** Framework, the philosophy of **Connexion** is to start your API design by defining the interface/ specs then **connect** to your controllers without worrying about the underlying Framework (**Flask**).


## What are the advantages of using this library  ?

- Simplify the development process through a document-driven design
- Ensure up-to-date documentation
- API design is not Framework-Specific 
- Automatic Validation of requests and endpoint parameters 
- Automatic serialization of response payloads
- Handles OAuth 2 token-based authentication
- Provides a Web Swagger Console UI


## How can I use it ? 

- First activate your virtual **venv** and upgrade the package installer **pip** 
    ```
    source venv/bin/activate
    pip install -U pip
    ```
- Then install the **connexion** python library with a default swagger-ui  connexion[swagger-ui]
    ```
    pip install connexion[swagger-ui]
    ```
- Draft your OpenAPI specs/ API Documentation  
  - More details in the next chapter (chap.3)
- Implement your controllers 
  - More details in Chap.4 
- Connect the endpoints (in your spec) to their respective handlers / controllers  
  - More details in Chap.4  
- Instantiate a **connexion** object referencing your specs.
  - More details in Chap.4  


:thought_balloon:: Make sure to include  **connexion\[swagger-ui\]**  as part of the dependencies in the file  **requirements.txt** 

<br>
<br>

---


[ << ( 1. Project Structure ) ](../chapters/chapter_1.md#the-end-in-mind) &nbsp;&nbsp; |  &nbsp;&nbsp;  [ ( 3. Understanding OpenAPI ) >>](../chapters/chapter_3.md#openapi-specs-structure)  
 