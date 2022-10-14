## Create Virtual Environment
- `python3.9 -m venv .venv`

## Tell VSCode which virtual env to use
- cmd + shift + p
- click "Python: Select Interpreter"
- select the one you created
- open a new terminal, you should see (.venv) now

## Install Requirements
- `pip install flask`

## Create app File
- `touch app.py`
- add this code:
```py
from flask import Flask

app = Flask(__name__)
```

## Run the app
- `flask run`
- ctrl + c to stop it

## Add Data
- For now, we'll store data in a list, not db
- add seed data in app.py:
```py
stores = [
  {
    "name": "My Store",
    "items": [
      {
        "name": "Chair",
        "price": 15.99
      }
    ]
  }
]
```

## Create GET Endpoint
- in app.py add this:
```py
@app.get("/store") # http://127.0.0.1:5000/store
def get_stores():
    return {"stores": stores}
```
- Note: for now, you have to restart your app when you make changes

## Create POST Endpoint
- in app.py add this:
```py
@app.post("/store") # http://127.0.0.1:5000/store
def create_store():
    pass
```
- `pass` just means don't do anything, it's a placeholder
- you'll get a 500 error because `TypeError: The view function for 'create_store' did not return a valid response. The function either returned None or ended without a return statement.`

## Edit POST Endpoint
- add this to the endpoint function:
```py
def create_store():
    request_data = request.get_json()
    new_store = {"name": request_data["name"], "items": []}
    stores.append(new_store)
    return new_store, 201
```
- change import line to `from flask import Flask, request`

## Add Item Creation Endpoint
- add this:
```py
@app.post("/store/<string:name>/item")
def create_item(name):
    pass
```

## Edit Item Endpoint
```py
def create_item(name):
    request_data = request.get_json()
    for store in stores:
        if store["name"] == name:
            new_item = {
              "name": request_data["name"], 
              "price": request_data["price"]
            }
            store["items"].append(new_item)
            return new_item, 201
    return {"message": "store not found"}, 404
```

## Get a Specific Store
```py
@app.get("/store/<string:name>")
def get_store(name):
    for store in stores:
        if store["name"] == name:
            return store
    return {"message": "store not found"}, 404
```