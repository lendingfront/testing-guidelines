# Testing Guidelines

This document contains the guidelines when working with tests in LendingFront repositories.

## Table of Contents

- [Context links](#context-links)
- [Summary](#summary)
- [Front end tests](#front-end-tests)
    - [Repositories](#repositories)
    - [Project Structure](#project-structure)
    - [Validations](#validations)
- [Api tests](#api-tests)
    - [Repositories](#api-repositories)
    - [Project Structure](#api-project-structure)
    - [Database](#api-database)
    - [Validations](#api-validations)
- [Mocks](#mocks)
    - [Creating a mock](#creating-a-mock)
- [Data files](#data-files)
    
# Context links

If you are a testing ninja you can avoid this section, otherwise is very important that you read carefully this 
resources:

 - [Functional testing in an environment of Flask micro-services](https://www.theodo.fr/blog/2015/07/functional-testing-in-an-environment-of-flask-micro-services/)
 - [Introduction to mocking](https://www.toptal.com/python/an-introduction-to-mocking-in-python)
 - [Python Mocking 101: Fake It Before You Make It](https://blog.fugue.co/2016-02-11-python-mocking-101.html)

# Summary

The testing guidelines are focused on two different types of repositories: front end and apis.

The first iteration of testing in LendingFront would consist on creating unit tests to flask endpoints following these
guidelines:

- Create unit tests ONLY for the entpoints that you just created or you are modifying
- Calls to API's have to be mocked (for examples of mocking take a look to [this](https://github.com/jhonjairoroa87/flask-endpoint-test-mocking/blob/master/test/unit/views_test.py) link)

## Front end tests

### Repositories

The repositories where DOM validations have to be implemented are:

- [LendingPortal](https://github.com/dariofvergara/lendingportal)
- [CustomerPortal](https://github.com/dariofvergara/customerportal)

### Project structure

An example of project structure for test is:
 ```
    lendingportal
    ├── app
    │   ├── __init__.py         # make it a package
    │   └── views
    │       └── application.py
    │       └── business.py
    └── test
        ├── __init__.py         # also make test a package
        └── unit                # unit tests folder
            └── app
                └── views
                    └── application_test.py
                    └── business_test.py
 ```
 
### Test Naming convention

### Test file name

The file that contain the test have to contain the word "test" at the end of the name. For example, if you are going to 
create a test for application.py, the corresponding test file would be application_test.py

### Test file folder

The folder that contains the test should mirror the project structure inside the test/unit folder. For example, if you 
are going to create a test for application.py that is located in lendingportal/app/views:

 ```
 lendingportal
    └── app     
        └── views
            └── application.py
 ```
 
The corresponding test file would be application_test.py and should be located in lendingportal/test/unit/app/views:

 ```
 lendingportal
    └── test
        └── unit
            └── app      
                └── views
                    └── application_test.py
 ```
  
        
### Validations

The validations have to be done comparing the elements in the DOM with the elements in memory:

    ```python
    # this is the rendered html
    response_data = """
    <html lang="en">
        <head>
            <meta charset="UTF-8">
            <title>Hello from Flask</title>
        </head>
        <body>
            <div class="flash">Medellin Python y Django Meetup</div>
            <div class="flash">PyLadies Medellin </div>
        </body>
    </html>
    """
    
    # this is the data in memory
    get_all_groups_mock_return = [{
        "city": "Medellin",
        "country_code": "CO",
        "country_name": "Colombia",
        "members": 848,
        "name": "Medellin Python y Django Meetup"
    }, {
        "city": "Medellin",
        "country_code": "CO",
        "country_name": "Colombia",
        "members": 150,
        "name": "PyLadies Medellin "
    }]
    
    # create soup object with the returned html file
    soup = BeautifulSoup(response_data, "html.parser")
    
    # look for all divs that it's class is 'flash'
    flask_divs = soup.find_all('div', {'class': "flash"})
    
    # verify the rendered divs number is the same number of the groups in memory
    self.assertEqual(len(flask_divs), len(get_all_groups_mock_return))
    
    # verify each div text is exactly the same of the the groups in memory
    for index, item in enumerate(get_all_groups_mock_return):
        text_div_element = flask_divs[index].text
        name_group_memory = get_all_groups_mock_return[index]['name']
        self.assertEqual(text_div_element, name_group_memory)
    ```

For further reference take a look to the following project : [flask-view-dom-testing](https://github.com/jhonjairoroa87/flask-view-dom-testing/blob/master/test/unit/views_test.py)
 
 
## API tests:

### API Repositories

The repositories where API validations have to be implemented are:

 - [PropertyService](https://github.com/dariofvergara/propertyservice)
 - [DocumentService](https://github.com/dariofvergara/documentservice)
 - [OriginationService](https://github.com/dariofvergara/originationservice)
 - [NotificationService](https://github.com/dariofvergara/notificationservice)
 - [LoanService](https://github.com/dariofvergara/loanservice)
 - [DataService](https://github.com/dariofvergara/dataservice)
 - [UserService](https://github.com/dariofvergara/userservice)
 - [DesicionService](https://github.com/dariofvergara/decisionservice )
 

### API Project structure

An example of project structure for test is:

```
    originationservice
    ├── app
    │   ├── __init__.py         # make it a package
    │   └── api
    │       └── application.py
    │       └── business.py
    │       └── owner.py
    │       └── report.py
    └── test
        ├── __init__.py         # also make test a package
        └── unit                # unit tests folder
            └── app
                └── api
                    └── application_test.py
                    └── business_test.py
                    └── owner_test.py
                    └── report_test.py
```    
    
### Api Test Naming convention

### Api Test file name

The file that contain the test have to contain the word "test" at the end of the name. For example, if you are going to 
create a test for business.py, the corresponding test file would be business_test.py

### Api Test file folder

The folder that contains the test should mirror the project structure inside the test/unit folder. For example, if you 
are going to create a test for business.py that is located in 'originationservice/app/api':

 ```
 originationservice
    └── app     
        └── api
            └── business.py
 ```
 
The corresponding test file would be business_test.py and should be located in 'originationservice/test/unit/api':

 ```
 originationservice
    └── test
        └── unit
            ├── api      
                └── views
                    └── business_test.py
 ```
 
 In this first iteration we are going to focus only on the endpoints contained in 'originationservice/app/api' folder.

### API Database

Do not mock interaction with database, make use of database interaction but instead of PostgreSQL use SQLite: 
 (For further detail take a look to [this](https://pythonhosted.org/Flask-Testing/) link )
 ```python
 def setUp(self):
    self.db_uri = 'sqlite:///' + os.path.join(basedir, 'test.db')
    app.config['TESTING'] = True
    app.config['WTF_CSRF_ENABLED'] = False
    app.config['SQLALCHEMY_DATABASE_URI'] = self.db_uri
    self.app = app.test_client()
    db.create_all()
 ```

### API Validations
 
The validations to the api response must include:

 - Code status of the response. Ie
 
 ```python 
 # validates the response status code is 200 OK
 self.assertEqual(response.status_code, 200, "the response status code should be 200")
 ```
 
 - Length of the response.data string should be greater that Cero (for services that return data)
 ```python
 # validates the rerponse data string length is greater that 0
 self.assertTrue(len(response.data) > 0, "the response data string lenght should be  greater than 0")
 ```
 - Validate that the response.data can be converted to a json object
 ```python
 # validate that the response.data string can be converted to json
 response_dict = {}
 try:
     response_dict = json.loads(response.data)
 except Exception:
     self.assertTrue(False, "the response.dict string cannot be parsed to a json object")
 ```
 
 - Validate that the response contains the proper json headers
 ```python
 # validate response json headers
 self.assertTrue('application/json' in response.headers['Content-Type'], "the response headers 'Content-Type' "
                                                                         "should contain application/json")
 ```
 
 - Validate that the converted json contains the expected data depending of the nfo the service is supposed to return
 ```python
 # Validate the response dict contains a 'result' key
 self.assertTrue('result' in response_dict, "the response dict should contain a 'result' key ")
 ```
 
 - The multiple exception cases have to be included in the tests, codes: 500, 401, 403, 404, etc
 
 
 For further reference take a look to the following project : [flask-endpoint-test-mocking](https://github.com/jhonjairoroa87/flask-endpoint-test-mocking/blob/master/test/unit/views_test.py)
 
### Mocks

The mocks will be placed in 'general' repository so they can be shared among other repository tests

 ```
 general
    └── test
        └── data 
            └── application.py # contains application endpoint constants 
            └── business.py # contains business endpoint constants 
            └── offer.py 
            mock 
            └── ls_api  # folder for every internal api mock
                └── business_loan_api.py
                └── funding_source_api.py
                └── one_time_payment_api.py
 ```
 
#### Creating a mock
 
To mock the method 'search_application' of the api located in 'general/util/os_api/application_api.py' is necessary to 
identify the data that is returned from that method which currently is something like this:

```
    {
        u'total_pages': 1, 
        u'objects': [...]
        u'num_results': 2, 
        u'page': 1
    }
```

One example of you can identify the returned data is by simply adding in the method a line that prints it return value.

Once you identify the current data you would create a mock file, in this case 'application_api.py' that would be 
placed in 'general/mock/os_api/application_api.py'. Its content would be a 'search_application' function that
returns a static dict:

```python

def search_application():

    return {
        u'total_pages': 1, 
        u'objects': [...]
        u'num_results': 2, 
        u'page': 1
    }
```

As you can see, a mock of a function is simply a function that returns an static value, in this case, a dictionary.

### Data files

Data file are meant to contain constant that will be used when creating a mock method.

An example of data file would be 'application.py' that would be located in 'general/test/data/application.py'

```
 general
    └── test
        └── data 
            └── application.py # contains application endpoint constants 
```

An example of constants defined in application.py would be BUSINESS_DATA and OWNERS_DATA :

 ```python
    BUSINESS_DATA = {
        "name": "Charlies Vintage Store #2",
        "tax_id": "100000000",
        ...,
        ...
    }

    OWNERS_DATA = {
        'owners-0-title': "Founder",
        'owners-0-first_name': "Victor",
        ...,
        ...
    }
    
    APPLICATION = {
        'application_number': '918971595362',
        'frequency_name': '',
        ...,
        ...
    }
 ```

A use case of data constants in mocks would be when creating the mock of 'create_application' function in the file
'general.tests.mock.os_api.application_api.py'

 ```python
    def create_application():      
    return {
        'status': "OK",
        'business': APPLICATION['business_id'],
        'application': APPLICATION['id'],
        'application_number': APPLICATION['application_number'],
        'display_status': APPLICATION['display_status']
    }
 ```
    
In this case the `APPLICATION` constant help us to ease the creation of create_application response
