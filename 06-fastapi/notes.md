Vibha, since you've already learned Python, Machine Learning algorithms, and the basics of API development, let's understand FastAPI from the beginning in simple Hindi, with practical examples and interview-focused explanations.

We'll use an example of your future ML project: a house price prediction model. You'll understand how to take a trained ML model, create an API using FastAPI, and connect it to your React frontend.

# 1. FastAPI क्या है?

FastAPI एक Python web framework है, जिसका इस्तेमाल Python में APIs बनाने के लिए किया जाता है।

आसान भाषा में समझो:

* आपने Python में एक Machine Learning model बनाया।

* अब आप चाहते हो कि React frontend से user घर का area, bedrooms और location जैसी जानकारी enter करे।

* आपका ML model उस input के आधार पर price predict करे।

* Prediction React frontend पर दिखाई दे।

React और Python ML model के बीच communication करवाने के लिए हम FastAPI का इस्तेमाल कर सकते हैं।

## FastAPI का काम कैसे होता है?

React frontend

User inputs: area, bedrooms

HTTP request with JSON

FastAPI

Receives and validates input

ML model

Predicts house price

FastAPI response

JSON prediction result

React displays the predicted price

### Interview answer

What is FastAPI?

FastAPI is a modern, high-performance Python web framework used to build APIs. It supports automatic request validation using Python type hints and Pydantic, automatic interactive API documentation, and asynchronous programming.

# 2. FastAPI क्यों इस्तेमाल करते हैं?

मान लो तुमने Python में Linear Regression या Random Forest से एक ML model train किया है।

अब तुम्हें इस model को किसी दूसरी application में use करना है, जैसे React.

सिर्फ Python notebook में `model.predict()` चलाने से React application सीधे उस model को access नहीं कर सकती।

इसके लिए हमें एक interface चाहिए।

FastAPI उस interface को बनाने में मदद करता है।

| Feature              | इसका फायदा                                                                     |
| -------------------- | ------------------------------------------------------------------------------ |
| Easy syntax          | Python के basic knowledge से API बना सकते हैं                                  |
| Automatic validation | गलत input को automatically reject कर सकता है                                   |
| High performance     | ASGI-based architecture और async support                                       |
| Auto documentation   | Swagger UI और OpenAPI documentation automatically generate                     |
| Type hints           | Input और output के expected data types define कर सकते हैं                      |
| ML integration       | Scikit-learn, TensorFlow, PyTorch जैसे models के साथ इस्तेमाल कर सकते हैं      |
| Async support        | I/O-bound operations में concurrent requests को efficiently handle कर सकते हैं |

FastAPI और Flask में अंतर:

* Flask एक lightweight Python web framework है, जिसमें बहुत कुछ manually configure करना पड़ता है।

* FastAPI में type-based validation और automatic API documentation जैसी सुविधाएँ built-in मिलती हैं।

* दोनों से APIs बना सकते हैं। हर project के लिए FastAPI ही जरूरी नहीं है।


# 3. FastAPI install कैसे करें?

सबसे पहले अपने project के लिए एक folder बनाओ।

```
fastapi-house-prediction/
│
├── main.py
└── requirements.txt
```

Terminal में ये commands चलाओ:

Bash

```
python -m venv venv
```

Windows PowerShell में virtual environment activate करो:

PowerShell

```
.\venv\Scripts\Activate.ps1
```

फिर FastAPI और Uvicorn install करो:

Bash

```
pip install fastapi "uvicorn[standard]"
```

`requirements.txt` में:

```
fastapi
uvicorn[standard]
```

यहाँ दो important चीजें हैं:

* FastAPI: API बनाने के लिए framework है।

* Uvicorn: ASGI server है, जो तुम्हारे FastAPI application को run करता है और HTTP requests receive करता है।

अगर तुम्हारे Windows में `python` command काम नहीं करती, तो पहले Python installation और PATH configuration ठीक करना होगा। तुमने पहले इसी तरह की समस्या देखी थी।

# 4. FastAPI में पहली API बनाना

अब `main.py` file बनाओ।

Python

Run

```
from fastapi import FastAPI

app = FastAPI()

@app.get("/")
def home():
    return {"message": "Welcome to FastAPI"}
```

अब इसे समझते हैं।

### `app = FastAPI()`

यह FastAPI application का instance बनाता है।

इसी `app` के अंदर हम routes, middleware और दूसरी configurations define करते हैं।

### `@app.get("/")`

इसे decorator कहते हैं।

इसका मतलब है कि जब कोई client `/` endpoint पर GET request भेजेगा, तो नीचे दिया गया `home()` function execute होगा।

### `def home():`

यह एक Python function है, जो request handle करता है।

### `return {"message": "Welcome to FastAPI"}`

हमने Python dictionary return की है। FastAPI इसे automatically JSON response में convert कर देता है।

अब server start करो:

Bash

```
uvicorn main:app --reload
```

इस command का अर्थ:

| Part       | Meaning                                                  |
| ---------- | -------------------------------------------------------- |
| `uvicorn`  | Server                                                   |
| `main`     | Python file `main.py`                                    |
| `app`      | FastAPI instance                                         |
| `--reload` | Code changes पर development server automatically restart |

अब browser में खोलो:

`http://127.0.0.1:8000`

Response:

JSON

```
{
  "message": "Welcome to FastAPI"
}
```

Automatic API documentation

FastAPI में ये URLs भी automatically उपलब्ध होते हैं।

[http://127.0.0.1:8000/docs](http://127.0.0.1:8000/docs)

Swagger UI — यहाँ से API requests को test कर सकती हो।

[http://127.0.0.1:8000/redoc](http://127.0.0.1:8000/redoc)

ReDoc — API documentation को दूसरे format में दिखाता है।

Swagger UI में जाकर endpoint expand करो, `Try it out` पर click करो और `Execute` करके API का response देखो।

# 5. Routes और HTTP methods

अब API बनाते समय सबसे जरूरी concept समझते हैं।

FastAPI में हम HTTP methods के हिसाब से अलग-अलग endpoints बना सकते हैं।

| Method | FastAPI decorator | काम                                |
| ------ | ----------------- | ---------------------------------- |
| GET    | `@app.get()`      | Data प्राप्त करना                  |
| POST   | `@app.post()`     | नया data create करना               |
| PUT    | `@app.put()`      | Resource को पूरी तरह replace करना  |
| PATCH  | `@app.patch()`    | Resource के कुछ fields update करना |
| DELETE | `@app.delete()`   | Resource delete करना               |

Example:

Python

Run

```
from fastapi import FastAPI

app = FastAPI()

@app.get("/students")
def get_students():
    return {"message": "Get all students"}

@app.post("/students")
def create_student():
    return {"message": "Create student"}

@app.put("/students/{student_id}")
def update_student(student_id: int):
    return {"message": f"Update student {student_id}"}

@app.delete("/students/{student_id}")
def delete_student(student_id: int):
    return {"message": f"Delete student {student_id}"}
```

यहाँ `/students` और `/students/{student_id}` endpoints हैं।

ध्यान दो, same URL पर अलग HTTP methods हो सकते हैं। जैसे `/students` पर GET data fetch करेगा और POST नया student create करेगा।

Interview question: Route और endpoint में क्या अंतर है?

आम तौर पर endpoint किसी API resource का URL या operation-specific URL होता है। Route वह mapping है जो URL और HTTP method को एक handler function से जोड़ती है। Developers अक्सर इन दोनों terms को सामान्य बातचीत में एक-दूसरे के स्थान पर भी इस्तेमाल करते हैं।

# 6. Path parameters और Query parameters

यह बहुत important concept है क्योंकि real-world APIs में इनका लगातार इस्तेमाल होता है।

## A. Path parameters

जब किसी specific resource को identify करना हो, तब path parameter use करते हैं।

Example:

Python

Run

```
@app.get("/students/{student_id}")
def get_student(student_id: int):
    return {"student_id": student_id}
```

Request:

http

```
GET /students/101
```

Response:

JSON

```
{
  "student_id": 101
}
```

यहाँ `101` path parameter की value है।

`student_id: int` लिखने से FastAPI इस parameter को integer के रूप में validate और convert करता है। अगर value valid integer नहीं है, तो सामान्यतः `422` validation error मिलता है।

## B. Query parameters

जब filtering, searching या pagination जैसी functionality चाहिए, तब query parameters उपयोगी होते हैं।

Example:

Python

Run

```
@app.get("/students")
def get_students(
    city: str = "Pune",
    age: int = 20
):
    return {
        "city": city,
        "age": age
    }
```

Request:

http

```
GET /students?city=Mumbai&age=25
```

Response:

JSON

```
{
  "city": "Mumbai",
  "age": 25
}
```

यहाँ `city` और `age` query parameters हैं।

| Path parameter                | Query parameter                           |
| ----------------------------- | ----------------------------------------- |
| Resource identify करने के लिए | Filtering, sorting, pagination आदि के लिए |
| `/students/101`               | `/students?city=Pune`                     |
| अक्सर resource URL का हिस्सा  | `?` के बाद आता है                         |

# 7. Request body और Pydantic

अब मान लो कि तुम्हें नया student create करना है।

तुम्हें frontend से student का नाम, उम्र और email backend को भेजना होगा।

इस तरह के data को request body में भेज सकते हैं।

FastAPI में request body validation के लिए Pydantic models का उपयोग करते हैं।

Python

Run

```
from fastapi import FastAPI
from pydantic import BaseModel

app = FastAPI()

class Student(BaseModel):
    name: str
    age: int
    email: str

@app.post("/students")
def create_student(student: Student):
    return {
        "message": "Student created",
        "student": student
    }
```

अब इस endpoint पर POST request भेजो।

Request body:

JSON

```
{
  "name": "Vibha",
  "age": 28,
  "email": "vibha@example.com"
}
```

Response:

JSON

```
{
  "message": "Student created",
  "student": {
    "name": "Vibha",
    "age": 28,
    "email": "vibha@example.com"
  }
}
```

### यहाँ पीछे क्या हो रहा है?

1. FastAPI incoming JSON request को receive करता है।

2. Pydantic model `Student` के अनुसार fields validate होती हैं।

3. Valid data को `Student` object में parse किया जाता है।

4. फिर `create_student()` function को वह object मिलता है।

5. Function response return करता है।

अगर `age` में `"twenty"` भेजा, तो validation error आएगा क्योंकि integer expected है।

ध्यान दो: इस example में student सिर्फ response में लौट रहा है। Database में save नहीं हो रहा है। Database save करने के लिए अलग logic लिखना पड़ेगा।

### Pydantic का उपयोग क्यों करते हैं?

* Input data validation

* Data types define करना

* Missing या invalid fields detect करना

* Nested JSON data validate करना

* API documentation में request schema generate करना

Interview answer:

Pydantic is a Python data validation library. FastAPI uses Pydantic models to parse and validate request data, define schemas, and generate API documentation.


# 8. Response model क्या होता है?

अब तक हमने request body के लिए Pydantic model बनाया है। लेकिन response में भी हमें control चाहिए कि client को कौन-सा data भेजना है।

मान लो database में student के पास ये details हैं:

JSON

```
{
  "id": 101,
  "name": "Vibha",
  "email": "vibha@example.com",
  "password": "secret123"
}
```

हम frontend को password नहीं भेजना चाहते।

इसलिए अलग response model बना सकते हैं।

Python

Run

```
from fastapi import FastAPI
from pydantic import BaseModel

app = FastAPI()

class StudentResponse(BaseModel):
    id: int
    name: str
    email: str

@app.get("/students/{student_id}", response_model=StudentResponse)
def get_student(student_id: int):
    return {
        "id": student_id,
        "name": "Vibha",
        "email": "vibha@example.com",
        "password": "secret123"
    }
```

Response:

JSON

```
{
  "id": 101,
  "name": "Vibha",
  "email": "vibha@example.com"
}
```

यहाँ `response_model` response को validate और filter करने में मदद करता है।

ध्यान रखो: वास्तविक applications में password को database में भी plain text में store नहीं करना चाहिए। Password hashing और authentication के लिए अलग security practices जरूरी हैं।

# 9. HTTP status codes और error handling

जब हम APIs बनाते हैं, तो सिर्फ data भेजना काफी नहीं है। Client को यह भी बताना होता है कि request सफल हुई या उसमें कोई problem आई।

| Status code | Meaning               | कब इस्तेमाल होता है                          |
| ----------- | --------------------- | -------------------------------------------- |
| 200         | OK                    | GET या successful operation                  |
| 201         | Created               | नया resource successfully create हुआ         |
| 204         | No Content            | Successful operation, कोई response body नहीं |
| 400         | Bad Request           | Invalid request                              |
| 401         | Unauthorized          | Authentication missing या invalid            |
| 403         | Forbidden             | Permission नहीं है                           |
| 404         | Not Found             | Resource नहीं मिला                           |
| 422         | Unprocessable Content | Request validation fail                      |
| 500         | Internal Server Error | Unexpected server-side error                 |

FastAPI में status code set करने का उदाहरण:

Python

Run

```
from fastapi import FastAPI, status

app = FastAPI()

@app.post("/students", status_code=status.HTTP_201_CREATED)
def create_student():
    return {"message": "Student created"}
```

इस API से response आएगा:

http

```
HTTP/1.1 201 Created
```

## HTTPException का उपयोग

मान लो किसी student को search किया, लेकिन वह database में मौजूद नहीं है।

Python

Run

```
from fastapi import FastAPI, HTTPException

app = FastAPI()

students = {
    1: "Vibha",
    2: "Amit"
}

@app.get("/students/{student_id}")
def get_student(student_id: int):
    if student_id not in students:
        raise HTTPException(
            status_code=404,
            detail="Student not found"
        )

    return {"name": students[student_id]}
```

अगर `/students/10` request भेजोगे, तो response होगा:

JSON

```
{
  "detail": "Student not found"
}
```

और status code होगा `404`.

यहाँ `raise HTTPException()` का इस्तेमाल करके हम expected errors को clearly handle कर रहे हैं।

# 10. Sync और Async functions

यह topic interview में भी पूछा जा सकता है।

FastAPI में हम दो तरह के route handlers लिख सकते हैं:

Python

Run

```
@app.get("/sync")
def sync_route():
    return {"message": "Synchronous"}
```

Python

Run

```
@app.get("/async")
async def async_route():
    return {"message": "Asynchronous"}
```

Difference समझो:

* `def`: सामान्य synchronous Python function।

* `async def`: asynchronous function, जो `await` के साथ asynchronous operations perform कर सकता है।

उदाहरण के लिए, अगर API को किसी external service से data fetch करना है और वह service async HTTP client support करती है, तो async function उपयोगी हो सकता है।

Python

Run

```
import httpx
from fastapi import FastAPI

app = FastAPI()

@app.get("/external-data")
async def get_external_data():
    async with httpx.AsyncClient() as client:
        response = await client.get(
            "https://jsonplaceholder.typicode.com/posts/1"
        )

    return response.json()
```

इसके लिए `httpx` install करना होगा:

Bash

```
pip install httpx
```

### Behind the scenes

मान लो तुम्हारी API किसी external service से response का इंतजार कर रही है।

* Synchronous blocking operation में उस execution thread को इंतजार करना पड़ सकता है।

* Asynchronous I/O operation में `await` पर control event loop को वापस मिल सकता है, जिससे वह दूसरी eligible tasks को आगे बढ़ा सकता है।

इससे I/O-bound applications में concurrency बेहतर हो सकती है।

लेकिन एक जरूरी बात: `async` लगाने से CPU-heavy Machine Learning prediction automatically तेज नहीं हो जाती। Heavy computation के लिए अलग execution strategy की जरूरत हो सकती है।

Interview answer:

`async` and `await` allow asynchronous programming in FastAPI. They are especially useful for I/O-bound operations, such as database calls and external HTTP requests, when using compatible asynchronous libraries.

# 11. Dependency Injection क्या है?

FastAPI का एक बहुत उपयोगी feature है Dependency Injection।

आसान भाषा में, जब कोई function किसी दूसरी functionality पर depend करता है, तो उस functionality को बार-बार manually create करने की बजाय FastAPI के जरिए provide कर सकते हैं।

उदाहरण के लिए, हर protected endpoint में current user निकालने का logic दोबारा लिखने की जरूरत नहीं है।

पहले एक simple dependency बनाते हैं:

Python

Run

```
from fastapi import FastAPI, Depends

app = FastAPI()

def get_current_user():
    return {"username": "Vibha"}

@app.get("/profile")
def get_profile(user: dict = Depends(get_current_user)):
    return user
```

यहाँ क्या हो रहा है?

1. `get_current_user()` dependency function है।

2. `Depends(get_current_user)` FastAPI को बताता है कि इस function को dependency के रूप में use करना है।

3. FastAPI route execute करने से पहले dependency resolve करता है।

4. उसका result `user` parameter में मिलता है।

Real applications में यही pattern authentication, database sessions, shared validation और reusable business logic के लिए उपयोगी है।

ध्यान दो, ऊपर वाला example केवल demonstration है। इसमें वास्तविक authentication नहीं हो रहा है।

# 12. Middleware क्या होता है?

Middleware ऐसा code है जो request और response के बीच common processing करता है।

उदाहरण:

* Request logging

* CORS

* Authentication-related processing

* Timing information

* Common request/response modifications

एक simple middleware:

Python

Run

```
from fastapi import FastAPI, Request
import time

app = FastAPI()

@app.middleware("http")
async def add_process_time(request: Request, call_next):
    start = time.time()

    response = await call_next(request)

    process_time = time.time() - start
    response.headers["X-Process-Time"] = str(process_time)

    return response
```

यह middleware हर HTTP request पर execute होगा।

Flow:

Incoming request

Middleware

Logging, timing, etc.

Route handler

Response

Passes back through middleware

Middleware और dependencies अलग concepts हैं। Middleware आम तौर पर requests के व्यापक हिस्से पर लागू होता है, जबकि dependencies किसी specific route या router के लिए reusable functionality provide कर सकती हैं।

# 13. CORS क्या है और FastAPI में कैसे use करते हैं?

तुम React और FastAPI को अलग-अलग development servers पर run करोगी।

उदाहरण:

* React: `http://localhost:5173`

* FastAPI: `http://127.0.0.1:8000`

ये अलग origins हैं। Browser की same-origin policy के कारण cross-origin requests के लिए server को उचित CORS headers भेजने पड़ सकते हैं।

FastAPI में CORS middleware:

Python

Run

```
from fastapi import FastAPI
from fastapi.middleware.cors import CORSMiddleware

app = FastAPI()

app.add_middleware(
    CORSMiddleware,
    allow_origins=["http://localhost:5173"],
    allow_credentials=True,
    allow_methods=["*"],
    allow_headers=["*"]
)
```

यहाँ:

* `allow_origins`: कौन-से origins से browser requests की अनुमति है।

* `allow_credentials`: cookies या credentials के साथ requests की अनुमति।

* `allow_methods`: कौन-से HTTP methods allowed हैं।

* `allow_headers`: कौन-से request headers allowed हैं।

Production में अपने वास्तविक frontend origins specify करना बेहतर है। CORS authentication या authorization का replacement नहीं है। यह browser के cross-origin access rules को manage करता है।


# 14. FastAPI में authentication और security

जब तुम real-world API बनाओगी, तो हर endpoint को public रखना सही नहीं होता।

मान लो तुम्हारी application में ये endpoints हैं:

* `GET /products` — कोई भी user products देख सकता है।

* `POST /products` — केवल authorized admin product add कर सकता है।

* `GET /profile` — logged-in user अपना profile देख सकता है।

इसके लिए कुछ security concepts समझने जरूरी हैं।

| Concept          | Use                                                                           |
| ---------------- | ----------------------------------------------------------------------------- |
| Authentication   | User की identity verify करना                                                  |
| Authorization    | User को कौन-सी permission है, यह check करना                                   |
| JWT              | Token-based authentication implement करना                                     |
| OAuth2           | Authorization framework, जिसे authentication flows में भी use किया जा सकता है |
| Password hashing | Password को सुरक्षित तरीके से store करना                                      |
| HTTPS            | Network communication को encrypt करना                                         |
| Rate limiting    | Requests की संख्या सीमित करना                                                 |

FastAPI में `OAuth2PasswordBearer`, `HTTPBearer` जैसे security helpers मिलते हैं। ये authentication flows implement करने में मदद करते हैं, लेकिन केवल इन्हें add करने से पूरा authentication system तैयार नहीं हो जाता।

तुम्हारे MERN authentication project में जो JWT और protected routes का concept था, वही basic principles यहाँ भी लागू होते हैं। अंतर सिर्फ इतना है कि backend अब Python और FastAPI में होगा।

# 15. FastAPI के साथ ML model integrate कैसे करें?

अब सबसे practical example देखते हैं, जो तुम्हारे ML learning के लिए बहुत महत्वपूर्ण है।

मान लो तुमने Scikit-learn से house price prediction model train किया है और `house_model.pkl` नाम से save किया है।

एक basic project structure:

```
house-price-api/
│
├── main.py
├── house_model.pkl
├── requirements.txt
└── README.md
```

पहले trained model को load करने के लिए `joblib` install करो:

Bash

```
pip install joblib scikit-learn
```

अब `main.py`:

Python

Run

```
from fastapi import FastAPI
from pydantic import BaseModel
import joblib

app = FastAPI()

model = joblib.load("house_model.pkl")

class HouseInput(BaseModel):
    area: float
    bedrooms: int

@app.post("/predict")
def predict_price(house: HouseInput):
    input_data = [[
        house.area,
        house.bedrooms
    ]]

    prediction = model.predict(input_data)

    return {
        "predicted_price": float(prediction[0])
    }
```

यह example तभी सही prediction देगा जब तुम्हारा trained model इन्हीं दो features को इसी order में expect करता हो। वास्तविक projects में preprocessing pipeline को भी save और load करना बेहतर होता है।

## Behind the scenes

1. React user से area और bedrooms की values लेता है।

2. React `/predict` endpoint पर POST request भेजता है।

3. FastAPI Pydantic की मदद से input validate करता है।

4. FastAPI model को features देता है और `predict()` execute होता है।

5. Model prediction देता है। FastAPI उसे JSON में convert करके React को भेजता है।

6. React prediction को UI पर दिखाता है।

Request:

JSON

```
{
  "area": 1200,
  "bedrooms": 3
}
```

Response:

JSON

```
{
  "predicted_price": 7500000
}
```

यहाँ price सिर्फ example है, वास्तविक model output नहीं।

## React से FastAPI को call करना

JavaScript

```
const predictPrice = async () => {
  try {
    const response = await fetch(
      "http://127.0.0.1:8000/predict",
      {
        method: "POST",
        headers: {
          "Content-Type": "application/json"
        },
        body: JSON.stringify({
          area: 1200,
          bedrooms: 3
        })
      }
    );

    if (!response.ok) {
      throw new Error("Prediction failed");
    }

    const data = await response.json();
    console.log(data.predicted_price);
  } catch (error) {
    console.error(error);
  }
};
```

यही frontend और ML backend integration का basic flow है।

# 16. FastAPI में testing कैसे करें?

API बनाना ही काफी नहीं है। तुम्हें यह भी check करना है कि endpoints सही काम कर रहे हैं या नहीं।

इसके लिए तीन popular approaches हैं:

* Swagger UI: Browser से API test करने के लिए।

* Postman: Request methods, headers, query parameters और request body के साथ testing के लिए।

* Pytest और FastAPI TestClient: Automated testing के लिए।

उदाहरण:

Python

Run

```
from fastapi.testclient import TestClient
from main import app

client = TestClient(app)

def test_home():
    response = client.get("/")

    assert response.status_code == 200
    assert response.json() == {
        "message": "Welcome to FastAPI"
    }
```

इसके लिए install करो:

Bash

```
pip install pytest httpx
```

और फिर:

Bash

```
pytest
```

इस तरह तुम API के expected output और status code को automatically verify कर सकती हो।

# 17. FastAPI के important interview questions

इन questions को revise करना बहुत उपयोगी होगा।

Interview practice

0 / 10 revealed

1. What is FastAPI and why do we use it?

Show answer

2. What is the role of Uvicorn?

Show answer

3. What is Pydantic?

Show answer

4. What is the difference between path and query parameters?

Show answer

5. What is the difference between GET and POST?

Show answer

6. What is dependency injection in FastAPI?

Show answer

7. What is middleware?

Show answer

8. What is CORS?

Show answer

9. When should we use async def?

Show answer

10. How do you integrate a trained ML model with FastAPI?

Show answer

# 18. Final revision: FastAPI में क्या-क्या याद रखना है?

| Topic                | याद रखने वाली बात                                |
| -------------------- | ------------------------------------------------ |
| FastAPI              | Python API framework                             |
| Uvicorn              | ASGI server                                      |
| Route                | HTTP method और URL को handler से connect करता है |
| Pydantic             | Request validation और schema definition          |
| Path parameters      | Resource identify करते हैं                       |
| Query parameters     | Filtering, pagination आदि                        |
| Request body         | Client से आने वाला data                          |
| Response model       | Response का expected structure                   |
| HTTPException        | API errors raise करना                            |
| Dependency injection | Reusable dependencies provide करना               |
| Middleware           | Request और response के बीच common processing     |
| Async/await          | I/O-bound concurrency                            |
| CORS                 | Browser cross-origin access control              |
| Swagger UI           | Interactive API testing और documentation         |
| TestClient           | Automated API testing                            |
| Model integration    | Python ML model को API के जरिए expose करना       |

## तुम्हारे लिए recommended learning order

1. FastAPI basics और server setup

2. HTTP methods, routes और endpoints

3. Path parameters और query parameters

4. Request body और Pydantic

5. Response models और status codes

6. Error handling और validation

7. Dependency injection और middleware

8. CORS और authentication

9. Testing with Postman and pytest

10. Build a complete ML prediction API

11. Integrate it with React and deploy

