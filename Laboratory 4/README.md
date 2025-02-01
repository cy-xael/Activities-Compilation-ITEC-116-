
To-Do List API with versioning, authentication, HTTP exception handling, environment variables, and the  README.md  file.
 
main.py
 
python
  
import os
from fastapi import FastAPI, HTTPException
from typing import Optional
from pydantic import BaseModel
from fastapi.middleware.cors import CORSMiddleware
import uvicorn
from dotenv import load_dotenv

load_dotenv()  # Load environment variables from .env
API_KEY = os.getenv("LAB4_API_KEY")

app = FastAPI()

# Allow requests from any origin for demonstration purposes.
# In production, you should restrict origins.
app.add_middleware(
    CORSMiddleware,
    allow_origins=["*"],
    allow_credentials=True,
    allow_methods=["*"],
    allow_headers=["*"],
)

# Initial Task Database
task_db = [
    {"task_id": 1, "task_title": "Laboratory Activity", "task_desc": "Create Lab Act 2", "is_finished": False}
]

# Model for Tasks (Data Validation)
class Task(BaseModel):
    task_id: Optional[int] = None
    task_title: str
    task_desc: str
    is_finished: bool = False

# Authentication Middleware
@app.middleware("http")
async def api_key_auth(request, call_next):
    """
    Authenticates requests using an API key from the .env file.
    """
    api_key = request.headers.get("x-api-key")
    if api_key == API_KEY:
        response = await call_next(request)
        return response
    else:
        raise HTTPException(status_code=401, detail="Invalid API Key")

# Version 1 Endpoints (apiv1)
@app.get("/apiv1/tasks/{task_id}")
async def get_task_v1(task_id: int):
    """
    Retrieves a specific task from the task database.
    """
    task = next((t for t in task_db if t["task_id"] == task_id), None)
    if task:
        return {"status": "ok", "task": task}
    raise HTTPException(status_code=404, detail=f"Task with ID {task_id} not found.")

@app.post("/apiv1/tasks")
async def create_task_v1(task: Task):
    """
    Creates a new task in the task database.
    """
    if not task.task_title or not task.task_desc:
        raise HTTPException(status_code=400, detail="Task title and description are required.")

    # Generate a new task ID
    new_task_id = max([task["task_id"] for task in task_db]) + 1
    task_db.append(
        {"task_id": new_task_id, "task_title": task.task_title, "task_desc": task.task_desc, "is_finished": task.is_finished}
    )
    return {"status": "ok", "task_id": new_task_id}

@app.patch("/apiv1/tasks/{task_id}")
async def update_task_v1(task_id: int, task: Task):
    """
    Updates an existing task in the task database.
    """
    for i, t in enumerate(task_db):
        if t["task_id"] == task_id:
            task_db[i]["task_title"] = task.task_title
            task_db[i]["task_desc"] = task.task_desc
            task_db[i]["is_finished"] = task.is_finished
            return {"status": "ok"}
    raise HTTPException(status_code=404, detail=f"Task with ID {task_id} not found.")

@app.delete("/apiv1/tasks/{task_id}")
async def delete_task_v1(task_id: int):
    """
    Deletes a task from the task database.
    """
    for i, t in enumerate(task_db):
        if t["task_id"] == task_id:
            del task_db[i]
            return {"status": "ok"}
    raise HTTPException(status_code=404, detail=f"Task with ID {task_id} not found.")

# Version 2 Endpoints (apiv2)
@app.get("/apiv2/tasks/{task_id}")
async def get_task_v2(task_id: int):
    """
    Retrieves a specific task from the task database.
    """
    task = next((t for t in task_db if t["task_id"] == task_id), None)
    if task:
        return {"status": "ok", "task": task}
    raise HTTPException(status_code=404, detail=f"Task with ID {task_id} not found.")

@app.post("/apiv2/tasks")
async def create_task_v2(task: Task):
    """
    Creates a new task in the task database.
    """
    if not task.task_title or not task.task_desc:
        raise HTTPException(status_code=400, detail="Task title and description are required.")

    # Generate a new task ID
    new_task_id = max([task["task_id"] for task in task_db]) + 1
    task_db.append(
        {"task_id": new_task_id, "task_title": task.task_title, "task_desc": task.task_desc, "is_finished": task.is_finished}
    )
    return {"status": "ok", "task_id": new_task_id}, 201  # Status Code 201 (Created)

@app.patch("/apiv2/tasks/{task_id}")
async def update_task_v2(task_id: int, task: Task):
    """
    Updates an existing task in the task database.
    """
    for i, t in enumerate(task_db):
        if t["task_id"] == task_id:
            task_db[i]["task_title"] = task.task_title
            task_db[i]["task_desc"] = task.task_desc
            task_db[i]["is_finished"] = task.is_finished
            return {"status": "ok"}, 204  # Status Code 204 (No Content)
    raise HTTPException(status_code=404, detail=f"Task with ID {task_id} not found.")

@app.delete("/apiv2/tasks/{task_id}")
async def delete_task_v2(task_id: int):
    """
    Deletes a task from the task database.
    """
    for i, t in enumerate(task_db):
        if t["task_id"] == task_id:
            del task_db[i]
            return {"status": "ok"}, 204  # Status Code 204 (No Content)
    raise HTTPException(status_code=404, detail=f"Task with ID {task_id} not found.")

if __name__ == "__main__":
    uvicorn.run(app, host="0.0.0.0", port=8000, reload=True)
 
 
Key Changes:
 
- Versioning: The endpoints are organized under  /apiv1  and  /apiv2  paths.
 
- Authentication:
 
- The  api_key_auth  middleware checks for the API key in the  x-api-key  header.
 
- The API key is loaded from the  .env  file using  dotenv .
 
- HTTP Exception Handling:
 
- The  get_task  endpoints raise a 404 (Not Found) exception if a task is not found.
 
- The  create_task ,  update_task , and  delete_task  endpoints also raise 404 if the task is not found.
 
- The  create_task  endpoint now returns a 201 (Created) status code.
 
- The  update_task  and  delete_task  endpoints now return a 204 (No Content) status code.
 
- Environment Variables:
 
- The  LAB4_API_KEY  is loaded from the  .env  file using  dotenv .
 
- A  .gitignore  file is added to prevent the  .env  file from being committed to the repository.
 
README.md
 
markdown
  
## To-Do List API with FastAPI (Versioned and Authenticated)

This repository contains a To-Do List API built with FastAPI, demonstrating versioning, authentication, and proper HTTP exception handling.

### Features

* **Versioning:** Two API versions (`apiv1` and `apiv2`) are implemented.
* **Authentication:**  Requests are authenticated using an API key stored in a `.env` file.
* **HTTP Exception Handling:**
    * **404 Not Found:** Returned if a task with the specified ID is not found.
    * **401 Unauthorized:** Returned if the API key is invalid.
    * **201 Created:** Returned when a new task is successfully created.
    * **204 No Content:** Returned when a task is successfully updated or deleted.
* **Data Validation:**
    * **Task Creation:** Ensures that both a title and description are provided when creating a new task.
* **Simple Data Storage:**
    * Tasks are stored in an in-memory list (`task_db`). This is suitable for basic demonstrations, but for a real-world application, you would typically use a database (e.g., PostgreSQL, MongoDB).
* **Swagger UI Documentation:**
    * Includes Swagger UI for easy exploration and interaction with the API. 

### Usage

1. **Create a `.env` file:**
 
 
LAB4_API_KEY=your_api_key
 
plaintext
  
Replace `your_api_key` with a unique key.

2. **Install Dependencies:**
```bash
pip install -r requirements.txt
 
 
Run the API:
 
bash
  
uvicorn main:app --reload
 
 
Interact with the API (using curl):
 
 
- Example GET request (curl):
bash
  
curl -H "x-api-key: your_api_key" http://127.0.0.1:8000/apiv1/tasks/1 
 
 
- Example POST request (curl):
bash
  
curl -H "Content-Type: application/json" -H "x-api-key: your_api_key" -X POST -d '{"task_title": "Write Report", "task_desc": "Finish the final report", "is_finished": false}' http://127.0.0.1:8000/apiv2/tasks
 
 
Data Structure
 
Tasks are stored in a simple list called  task_db . Each task is represented as a dictionary with the following keys:
 
- task_id: Unique identifier for the task.
 
- task_title: Title of the task.
 
- task_desc: Description of the task.
 
- is_finished: Indicates if the task is completed (boolean).
 
API Endpoints
 
- GET /apiv1/tasks/{task_id}: Retrieve a specific task (version 1).
 
- POST /apiv1/tasks: Create a new task (version 1).
 
- PATCH /apiv1/tasks/{task_id}: Update an existing task (version 1).
 
- DELETE /apiv1/tasks/{task_id}: Delete a task (version 1).
 
- GET /apiv2/tasks/{task_id}: Retrieve a specific task (version 2).
 
- POST /apiv2/tasks: Create a new task (version 2).
 
- PATCH /apiv2/tasks/{task_id}: Update an existing task (version 2).
 
- DELETE /apiv2/tasks/{task_id}: Delete a task (version 2).
 
Next Steps
 
- Database Integration: For a more robust application, integrate a database for persistent data storage.
 
- User Authentication: Add user accounts and authentication to manage tasks for different users.
 
- Additional Validation:  Implement more comprehensive validation rules for task data (e.g., length constraints, data types, etc.).
 
- Error Handling Enhancement:  Provide more granular error messages and codes to guide users.
 
