# Python Learning Notes

## FastAPI Basics

### Creating a Simple API

```python
from fastapi import FastAPI

app = FastAPI()

@app.get("/")
async def root():
    return {"message": "Hello World"}

@app.get("/items/{item_id}")
async def read_item(item_id: int):
    return {"item_id": item_id}
```

### Request Body

```python
from pydantic import BaseModel

class Item(BaseModel):
    name: str
    price: float
    is_offer: bool = None

@app.put("/items/{item_id}")
async def update_item(item_id: int, item: Item):
    return {"item_name": item.name, "item_id": item_id}
```

### Dependencies

```python
from fastapi import Depends, FastAPI

app = FastAPI()

async def common_parameters(q: str = None, skip: int = 0, limit: int = 100):
    return {"q": q, "skip": skip, "limit": limit}

@app("/items/")
async def read_items(commons: dict = Depends(common_parameters)):
    return commons
```

---

## Type Hints

```python
from typing import List, Optional

def greet(name: str) -> str:
    return f"Hello, {name}"

def process_items(items: List[int]) -> int:
    return sum(items)

def find_user(user_id: int) -> Optional[str]:
    # Returns None if not found
    pass
```

---

## Data Classes

```python
from dataclasses import dataclass
from typing import List

@dataclass
class User:
    name: str
    age: int
    email: str
    skills: List[str] = None
    
    def __post_init__(self):
        if self.skills is None:
            self.skills = []
```

---

## Decorators

```python
def my_decorator(func):
    def wrapper(*args, **kwargs):
        print("Before function call")
        result = func(*args, **kwargs)
        print("After function call")
        return result
    return wrapper

@my_decorator
def say_hello():
    print("Hello!")
```

---

## Context Managers

```python
from contextlib import contextmanager

@contextmanager
def managed_resource():
    resource = acquire_resource()
    try:
        yield resource
    finally:
        release_resource(resource)

with managed_resource() as resource:
    use(resource)
```
