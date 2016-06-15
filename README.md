# Testing Guidelines
This document contains the guidelines when working with tests in LendingFront repositories. The testing guidelines 
are focused on two different types or types or repositories: front end and apis.

The first iteration of testing in LendingFront would consist on creating unit tests to flask endpoints following these
guidelines:

- Create unit tests ONLY for the entpoints that you just created or you are modifying
- Calls to API's have to be mocked (for examples of mocking take a look to [this](https://github.com/jhonjairoroa87/flask-endpoint-test-mocking/blob/master/test/unit/views_test.py) link)
- Do not mock interaction with database, make use of database interaction but instead of PostgreSQL use SQLite: 
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
 
## DOM tests:

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
            └── mocks 
                └── utils # reflect structure in lendingportal utils folder
                    └── ls_api
                        └── business_loan_api.py
                        └── funding_source_api.py
                        └── one_time_payment.py
            └── views
                └── application_test.py
                └── business_test.py
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

### Repositories

The repositories where API validations have to be implemented are:

 - [PropertyService](https://github.com/dariofvergara/propertyservice)
 - [DocumentService](https://github.com/dariofvergara/documentservice)
 - [OriginationService](https://github.com/dariofvergara/originationservice)
 - [NotificationService](https://github.com/dariofvergara/notificationservice)
 - [LoanService](https://github.com/dariofvergara/loanservice)
 - [DataService](https://github.com/dariofvergara/dataservice)
 - [UserService](https://github.com/dariofvergara/userservice)
 - [DesicionService](https://github.com/dariofvergara/decisionservice )
 

### Project structure

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
            └── mocks
                └── utils # reflect structure in lendingportal utils folder
                    └── ns_api
                        └── notification_api.py
                        └── funding_source_api.py
            └── api
                └── application_test.py
                └── business_test.py
                └── owner_test.py
                └── report_test.py
```        

### Validations
 
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
 