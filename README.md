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
 - [Getting Started with Python Mock](https://myadventuresincoding.wordpress.com/2011/02/26/python-python-mock-cheat-sheet/)
 - [Python mock by example](http://www.alexandrejoseph.com/blog/2015-08-21-python-mock-example.html)

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
    │       └── business_loan.py
    └── test
        ├── __init__.py         # also make test a package
        └── unit                # unit tests folder
            └── app
                └── views
                    └── business_loan
                        └── board_loan_test.py # entire class that has all the tests for board_loan endpoint method
                        └── calculate_loan_test.py # entire class that has all the tests for calculate_loan endpoint method
                        └── change_payment_frequency_test.py # entire class that has all the tests for change_payment_frequency endpoint method
                        └── payment_frequency_test.py # entire class that has all the tests for payment_frequency endpoint method
 ```
 
### Test file name and folder

A test file is created per endpoint its name have to contain the word "test" at the end of the name. For example, if 
you are going to create a test for `business_loan.py:board_loan` method , the corresponding folder and test file would be 
`board_loan_test.py` and would be located in the folder `lendingfront.test.unit.app.views.business_loan`

Note that in the implementation code we are handling a file `business_loan.py` but in the test folder a `business_loan`
folder is created, inside is the test file for the endpoint, in the case, `board_loan_test.py`

The final result in the project structure would be


 ```
 lendingportal
    ├── app     
    │   └── views
    │       └── business_loan.py
    └── test
        └── unit
            └── app      
                └── views
                    └── business_loan
                        └── board_loan_test.py
 ```
 
 Inside `board_loan_test.py` file would be a class with the name of the endpoint `BoardLoanTest` that will containg 
 method that would be the test cases for the given endpoint. The new test cases will be name concatenating the endpoint 
 name, endpoint method the test case main feature and "_test". Some examples of test methods would be:
  - board_loan_get_success_test
  - board_loan_get_incomplete_params_error_test
  - board_loan_get_no_params_error_test
  - board_loan_post_invalid_method_error_test
  - board_loan_put_invalid_method_error_test
 
#### 
        
### Validations

All front end tests have to call at least the base front end validations located in `general.test.helper.front_end_testing_helper.py`

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
            └── api      
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

All API tests have to call at least the base API validations located in `general.test.helper.api_testing_helper.py`
 
For further reference take a look to the following project : [flask-endpoint-test-mocking](https://github.com/jhonjairoroa87/flask-endpoint-test-mocking/blob/master/test/unit/views_test.py)
 
### Mocks

The mocks will be placed in 'general' repository so they can be shared among other repository tests

 ```
 general
    └── test
        └── mock 
            └── ls_api  # folder for every internal api mock
                └── business_loan_api.py
                └── funding_source_api.py
                └── one_time_payment_api.py
 ```
 
#### Mocking classes
 
If you are mocking and entire class, the name of the class and the file name have to contain the word "mock" in the 
end. For example, if the file is `lending_portal_property_helper_mock.py`, the class would be 
`LendingPortalPropertyHelperMock`

The recently created mock class should inherit from the class Mock: 

 ```python
    class LendingPortalPropertyHelperMock(Mock):
 ```

The content of the mock class would be the exact method names of the original class returning the expected data type:

 ```python
   class LendingPortalPropertyHelperMock(Mock):
   
        def industry_list(self):
            return []
        
        def state_list(self):
            return []
        
        def legal_entity_types(self):
            return []
 ```
 
#### Mocking methods

If you are mocking a method, the name of the file where the mock is going to me located should be exactly the same to 
the name of the folder of the original method. So, if the original file is `general.utils.ls_api.business_loan_api.py`
the file to be created in the mock folder would be `general.test.mock.ls_api.business_loan_api.py`. The mock methos that is 
created have to be the same original method name adding '_test' string. For example, if the method to be mocked is
'get_business_loan' the name of the mock method would be 'get_business_loan_mock'. The final structure would be:

```
 general
    ├── utils
    │   └── ls_api 
    │       └── business_loan_api.py:get_business_loan
    └── test
        └── mock 
            └── ls_api 
                └── business_loan_api.py:get_business_loan_mock
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
            └── application.py # contains application constants 
            └── business.py # contains business constants 
            └── offer.py # contains offers constants 
```

An example of constants defined in application.py would be BUSINESS_DATA, OWNERS_DATA and APPLICATION :

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
