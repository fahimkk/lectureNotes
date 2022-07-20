# FastAPI

## Introduction

FastAPI reads codes line by line.

installation

```bash
pip3 install fastapi
```

Install uvicorn server 

```bash
pip3 install uvicorn

or

pip3 install uvicorn[standard]
```

To run the server

```bash
uvicorn main:app --reload
```

here `main` is the file name (ie main.py) and `app` is the instance of FastAPI inside the main file and `--reload` is for live reload.

For debugging, inside the url we can add `docs` or `redoc`. 

`
localhost:8000/docs
localhost:8000/redoc
`

To use dynamic route use `{}`. ie
`@app.get('/blog/{var}')`. Before using the variable inside the function, we have to accept it in the function define.

```bash
@app.get('/blog/{id}')
def show(id):
    return {'data': id}
```

since from the browser we get the string as the default one. So here we can define the type for parameter.

```bash
@app.get('/blog/{id}')
def show(id: int):
    return {'data': id}
```

It throws error if the value is not a valid type.

To change the default port

```bash
if __name__ == "__main__":
    uvicorn.run(app, host="127.0.0.1", port=9000)
```
and run `python3 main.py`