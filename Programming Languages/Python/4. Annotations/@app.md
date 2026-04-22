In FastAPI, `@app` usually refers to decorators such as `@app.get()`, `@app.post()`, `@app.put()`, and `@app.delete()` that are used directly on a `FastAPI` application instance. A `FastAPI` object is the main application object, and these decorators are used to register path operation functions on that application. FastAPI’s tutorial introduces this pattern with code such as `app = FastAPI()` followed by decorators like `@app.get("/")`.

The basic idea is that `app` is the central entry point of the whole FastAPI service. When we write `@app.get("/items")`, we are telling FastAPI that this function should handle HTTP `GET` requests sent to the `/items` path. So `@app` decorators are used to connect URL paths and HTTP methods to Python functions. This is the most direct way to define endpoints in a FastAPI application.

For example:

```python
from fastapi import FastAPI

app = FastAPI()

@app.get("/")
async def root():
    return {"message": "Hello World"}
```

In this example, `app = FastAPI()` creates the main application object. Then `@app.get("/")` registers the `root()` function as the handler for `GET /`. According to FastAPI’s official tutorial, this is the standard starting pattern for building an API.

Another example:

```python
from fastapi import FastAPI

app = FastAPI()

@app.post("/users/")
async def create_user():
    return {"message": "user created"}
```

Here, `@app.post("/users/")` means the function handles HTTP `POST` requests for `/users/`. So the decorator not only defines the path, but also defines which HTTP method is allowed for that endpoint. FastAPI’s “Path Operation Decorators” section explains that decorators such as `@app.get()` and `@app.post()` represent the operation type for the path.

The most common usage case for `@app` is in small or simple projects, where all endpoints can be written directly in one file, often `main.py`. This makes the structure very direct and easy to understand. FastAPI’s tutorial starts with this single-app, single-file style before introducing larger modular structures.

However, as a project grows, writing everything directly with `@app` in one file can become hard to maintain. That is why FastAPI also provides `APIRouter`, so related endpoints can be moved into separate modules and later included in the main app. In that sense, `@app` is usually the top-level registration style, while `@router` is the modular registration style used in bigger applications. FastAPI’s “Bigger Applications” tutorial presents `APIRouter` specifically for this purpose.

So in practice, `@app` is often used in cases like:

- defining the main entry endpoints of a small API
- writing quick prototype services
- handling root paths such as `/`
- registering endpoints directly when modular splitting is not yet necessary

In larger applications, `@app` is still important because the main `app` object is where routers are finally included, middleware is added, and the application is started. This follows FastAPI’s documented architecture around the central `FastAPI` app instance and included routers.

In conclusion, `@app` in FastAPI is used to register endpoint functions directly on the main application object. It is the most direct way to bind a Python function to an HTTP route. A simple way to remember it is: `@app.get(...)` or `@app.post(...)` means this function belongs directly to the main FastAPI application and handles requests for that path.