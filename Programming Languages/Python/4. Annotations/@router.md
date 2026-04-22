In FastAPI, `@router` usually refers to decorators such as `@router.get()`, `@router.post()`, `@router.put()`, and `@router.delete()` that are used with an `APIRouter` object. `APIRouter` is designed to group related path operations together, and FastAPI recommends it for structuring larger applications across multiple files. It can be included directly in the main `FastAPI` app, or even nested inside another router.  

The core idea is that a router is like a smaller, modular route container. Instead of registering every endpoint directly on `app`, we create a router for a specific feature, define endpoints on that router, and then mount that router into the main application. This makes the code easier to organize, especially when different modules handle different business domains such as users, auth, orders, files, or admin APIs. FastAPI’s tutorial explicitly presents this as the standard approach for bigger applications, and notes that if you come from Flask, it is conceptually similar to Blueprints.  

For example:

```python
from fastapi import APIRouter

router = APIRouter()

@router.get("/users/")
async def read_users():
    return [{"username": "Alice"}, {"username": "Bob"}]
```

In this example, `router = APIRouter()` creates a router object. The decorator `@router.get("/users/")` means that this function should handle HTTP `GET` requests sent to the `/users/` path. So when a request reaches that path with the `GET` method, FastAPI calls `read_users()` and returns its result as the response. This is the same pattern shown in FastAPI’s official `APIRouter` reference.  

The router itself does not become active until it is included in the main application. That usually looks like this:

```python
from fastapi import FastAPI
from routers import users

app = FastAPI()
app.include_router(users.router)
```

Here, `app.include_router(users.router)` tells FastAPI to register all routes defined inside `users.router` as part of the application. Once included, those endpoints also appear in the generated OpenAPI schema and automatic docs UI such as `/docs`. FastAPI’s docs state that router-level metadata like `tags` and `responses` is also reflected in the generated OpenAPI documentation.  

A very common real-world pattern is to create routers with a shared prefix. For example:

```python
from fastapi import APIRouter

router = APIRouter(prefix="/users", tags=["users"])

@router.get("/")
async def list_users():
    return ["Alice", "Bob"]

@router.get("/{user_id}")
async def get_user(user_id: int):
    return {"user_id": user_id}
```

In this case, the router prefix `/users` is automatically added to every path inside that router. So `@router.get("/")` becomes `/users/`, and `@router.get("/{user_id}")` becomes `/users/{user_id}`. FastAPI documents `prefix` as a built-in router parameter for exactly this purpose, and `tags` is applied to all path operations in that router so they are grouped in the API docs.  

This shared-prefix style is useful because it keeps path definitions shorter and avoids repeating the same base path many times. It is especially helpful when all routes in a module belong to one resource, such as:

- `/users/...`
- `/orders/...`
- `/products/...`
- `/admin/...`

That is one of the most common usage cases for routers in production projects. FastAPI’s “Bigger Applications” tutorial presents routers as the main way to split these feature areas into separate files.  

Another important usage case is applying shared dependencies. `APIRouter` accepts a `dependencies` parameter, which means all endpoints inside that router can automatically use the same dependency logic. For example, a whole admin router might require authentication or permission checks. FastAPI documents router-level `dependencies` as a way to apply `Depends()` to all path operations in that router.  

A simplified example looks like this:

```python
from fastapi import APIRouter, Depends

def verify_token():
    ...

router = APIRouter(
    prefix="/admin",
    tags=["admin"],
    dependencies=[Depends(verify_token)]
)

@router.get("/dashboard")
async def read_dashboard():
    return {"message": "admin only"}
```

The practical meaning here is that every route in the `/admin` router shares the same dependency rule. This is useful when an entire group of endpoints should behave consistently, such as internal APIs, authenticated APIs, or tenant-scoped APIs. The main benefit is that you do not have to repeat the same dependency declaration on every function. This behavior is directly described in the `APIRouter` reference.  

Routers are also useful for applying shared documentation metadata. For example, `tags=["users"]` groups those endpoints under the same section in `/docs`, and `responses={...}` can document additional response types for all endpoints in that router. FastAPI explicitly states that these router-level settings are added to the generated OpenAPI schema.  

Another usage case is nesting routers. FastAPI allows one `APIRouter` to include another router before the parent router is included in the main app. This is useful in larger systems where, for example, an `api_v1` router contains sub-routers like `users`, `items`, and `auth`. FastAPI’s docs say that an `APIRouter` can be included in another `APIRouter`, not only in the main app.  

That can look like this:

```python
from fastapi import APIRouter
from .users import router as users_router
from .items import router as items_router

api_router = APIRouter(prefix="/api/v1")
api_router.include_router(users_router)
api_router.include_router(items_router)
```

This structure is useful when you want an extra grouping level, such as versioning the whole API under `/api/v1`. It also makes later upgrades easier, for example when creating `/api/v2` with different behavior while keeping the same overall project structure. The official docs support this include-router pattern.  

There is also a practical architectural reason to use routers: they separate business areas cleanly. A small project might place everything in one `main.py`, but as the API grows, that quickly becomes hard to maintain. Routers let each module define only its own endpoints and keep related schemas, dependencies, and service logic nearby. FastAPI’s tutorial says that while everything can be placed in one file, `APIRouter` is the convenience tool provided to structure bigger apps while keeping flexibility.  

So in real usage, routers are commonly used in cases like:

A user module, where all user-related endpoints are grouped in `routers/users.py`, such as user creation, login, profile lookup, and update operations. This keeps user APIs together instead of mixing them with unrelated endpoints. This modular “multiple files” pattern is the exact usage FastAPI highlights for bigger applications.  

An admin module, where all endpoints need a shared auth dependency, a shared prefix like `/admin`, and possibly shared tags for API docs. Router-level configuration makes this much cleaner than repeating the same settings on every endpoint. FastAPI documents `prefix`, `tags`, and `dependencies` as router-level settings for this purpose.  

API versioning, where a parent router groups versioned sub-routes like `/api/v1/users` and `/api/v1/orders`. Because routers can include other routers, this pattern scales well as the API becomes larger.  

Feature isolation in team development, where different developers or teams work on different routers without editing the same giant application file. While this is an architectural inference rather than a direct quote from the docs, it follows naturally from FastAPI’s official “multiple files” organization model.  

In conclusion, `@router.get(...)`, `@router.post(...)`, and similar decorators are used to bind Python functions to HTTP endpoints inside an `APIRouter`. The main purpose of routers is not just routing itself, but modular organization: grouping related endpoints, sharing prefixes and dependencies, improving docs structure, and making bigger FastAPI applications easier to maintain. A simple way to remember it is: a router is a feature-level container of endpoints, and `@router...` is the decorator that registers a function inside that container.