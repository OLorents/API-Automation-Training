# Automation API testing - how to start (checklist)

**If you are completely new to API testing, here are some points to start with.**

You must know and understand following tools that you will need to use in API testing:   

- **Nunit: is the most popular unit test framework for .NET**

   https://nunit.org/docs/2.6.3/getStarted.html
   Nunit vs other test frameworks: https://xunit.net/docs/comparisons.html

   **Important:**

    - **NUnit runner:**

      Nunit provides three different runners, which may be used to load and run your tests.
      https://nunit.org/docs/2.6.4/runningTests.html

    - **Assertions:**

      https://nunit.org/docs/2.6.3/classicModel.html

    - **Attributes:**

      https://nunit.org/docs/2.6.3/attributes.html  
      
      It will be enough to start with following attributes: 
      - Test
      - TestCase
      - TestFixture
      - TestFixtureSetUp
      - TestFixtureTearDown
      - SetUp
      - TearDown

- **RestSharp: probably, the most popular Rest API client library for .NET**
  
   https://code-maze.com/different-ways-consume-restful-api-csharp/
   
   https://www.ontestautomation.com/restful-api-testing-in-csharp-with-restsharp/
   
   http://restsharp.org/getting-started/

- **SqlClient: namespace in the .Net Data Provider for SQL Server**
  https://docs.microsoft.com/en-us/dotnet/api/system.data.sqlclient?view=netframework-4.8
  
  https://www.dotnetperls.com/sqlclient   

- **Newtonsoft.Json - Serialize and deserialize JSON in .NET**
  https://www.newtonsoft.com/json

  https://docs.microsoft.com/en-us/dotnet/standard/serialization/syste-text-json-how-to

  https://www.newtonsoft.com/json/help/html/SerializingJSON.htm/


**Main steps:**

- Create test class using [TestFixture] attribute
- Create test method using [Test] attribute
- Generate a RestClient for the URL
- Create request from client and specify the HTTP Method type
- Send the Request 
- Get the Response
- Validate Response

**Additional:**

- Deserialize Json response to C# object, helps to validate response content
- Check/update data in SQL

# Training: 

- Clone repo
- Open WebApp.sln in VS 

**Go to Startup.cs, set valid DB and credentials in lines 24 and 25**

    services.AddDbContext<ProductsDbContext>(option => option.UseSqlServer(@"Data Source = **{DB}; uid={uid}; pwd={pwd}**; Initial Catalog = ProductsDb;"));
  
    services.AddDbContext<CompanyDbContext>(option => option.UseSqlServer(@"Data Source = **{DB}; uid={uid}; pwd={pwd}**; Initial Catalog = CompaniesDb;"));

- Build project and start it. 

- Two databases will be created: ProductsDB and CompaniesDB 

- Cover API methods with API tests according to requirements described below.

- Find bugs (there are several).

**Requiremnts:**

 - All APIs produces and consumes application/json

 **Company API:**
 - Base Url- http://localhost:61536/api/Company

 **Create**:
  POST - /CreateCompany

    RequestBody
    {
        "CompanyId" : "1",
        "CompanyName":"TestCompanyName"
    }

    CompanyId - is unique, string.
    CompanyName - is required, string type, max length 10.  
    
    Response
    Status: 201 Created

    Errors:
    1. If no "CompanyName" in request
    400 Bad Request
    {
        "CompanyName": [
            "The CompanyName field is required."
        ]
    }

    2. If "CompanyName" length is more than 10
    400 Bad Request
    {
        "CompanyName": [
            "The field CompanyName must be a string or array type with a maximum length of '10'."
        ]
    }

    3. If "CompanyId" is not unique
    500 Server Error
    "CompanyId '{id}' is inserted before."


    4. If no "CompanyId" in request
    400 Bad Request
    {
        "CompanyId": [
            "The CompanyId field is required."
        ]
    }
    
**GetAll:**
  GET- /GetCompanies

    Response
    Status: 200 OK

    [
        {
            "companyId": "1",
            "companyName": "Test"
        },
        {
            "companyId": "2",
            "companyName": "Test1"
        }  
    ]
    
**GetById:**
  GET- /GetCompany/{CompanyId}
  
    Response
    Status: 200 OK {
     "companyId": "1",
     "companyName": "Test"
    }
    
**Update**:
   PUT: - /UpdateCompany/{companyId}

    RequestBody
    {
     "companyId": "1",
     "companyName": "Test"
    }

    Response
    Status: 200 OK
    "Company Updated..."

    Errors:
    1. If no "Record {id} exist in DB
    404 Not Found

    "No Record Found against this Id..."
    
 **DeleteById**
    DELETE: - /DeleteCompany/{id}
    
    Response
    Status: 200 OK    
    "Company deleted"

     Errors:
     1. If no "Record {id} exist in DB
        404 Not Found
        "No Record Found..."
        
**Product API:**

 Base Url- http://localhost:61536/api/Product

**Create:**
  POST:

    RequestBody
    {  
        "ProductName" : "Apple",
        "ProductPrice":"1000"
    }

    ProductPrice - is required, string type.
    ProductName- is required, string type.  

    Response
    Status: 201 Created

    Errors:
    1. If no "ProductName" in request
    400 Bad Request
    {
        "ProductName": [
            "The ProductName field is required."
        ]
    }

    2. If no "ProductPrice " in request
    400 Bad Request 
    {
        "ProductPrice ": [
            "The ProductPrice  field is required."
        ]
    }
    
**GetAll:**
    GET:
    
    Response
    Status: 200 OK

    [
        {
            "productId": "1",
            "productName": "Apple",
            "productPrice": "900"
        },
        {
            "productId": "2",
            "productName": "Nokia",
            "productPrice": "500"
        }  
    ]
    
**GetById:**
  GET- /{ProductId}
  
    Response
    Status: 200 OK
    {
        "productId": 1,
        "productName": "Iphone X",
        "productPrice": "1111"
    }
    
**Update:**
  PUT: - /{ProductId}

    RequestBody
    {
       "ProductId" : 1,
       "ProductName" : "Apple",
       "ProductPrice":"12",
    }
    Response
    Status: 200 OK
    "Product Updated..."
    
    Errors:
    1. If no "Record {id} exist in DB
    404 Not Found

    "No Record Found against this Id..."
    
**DeleteById:**
  DELETE: - /{ProductId}
  
    Response
    Status: 200 OK
    "Product deleted"

     Errors:
     1. If no "Record {id} exist in DB
        404 Not Found
        "No Record 
