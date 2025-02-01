
1.  API Structure (main.py)
 
python
  
from fastapi import FastAPI, HTTPException
from typing import Optional
from pydantic import BaseModel

app = FastAPI()

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

# Get a Task by ID
@app.get("/tasks/{task_id}")
async def get_task(task_id: int):
    """
    Retrieves a specific task from the task database.
    """
    for task in task_db:
        if task["task_id"] == task_id:
            return {"status": "ok", "task": task}
    return {"error": f"Task with ID {task_id} not found."}

# Create a New Task
@app.post("/tasks")
async def create_task(task: Task):
    """
    Creates a new task in the task database.
    """
    if not task.task_title or not task.task_desc:
        return {"error": "Task title and description are required."}

    # Generate a new task ID
    new_task_id = max([task["task_id"] for task in task_db]) + 1
    task_db.append(
        {"task_id": new_task_id, "task_title": task.task_title, "task_desc": task.task_desc, "is_finished": task.is_finished}
    )
    return {"status": "ok", "task_id": new_task_id}

# Update a Task
@app.patch("/tasks/{task_id}")
async def update_task(task_id: int, task: Task):
    """
    Updates an existing task in the task database.
    """
    for i, t in enumerate(task_db):
        if t["task_id"] == task_id:
            task_db[i]["task_title"] = task.task_title if task.task_title else task_db[i]["task_title"]
            task_db[i]["task_desc"] = task.task_desc if task.task_desc else task_db[i]["task_desc"]
            task_db[i]["is_finished"] = task.is_finished if task.is_finished is not None else task_db[i]["is_finished"]
            return {"status": "ok"}
    return {"error": f"Task with ID {task_id} not found."}

# Delete a Task
@app.delete("/tasks/{task_id}")
async def delete_task(task_id: int):
    """
    Deletes a task from the task database.
    """
    for i, t in enumerate(task_db):
        if t["task_id"] == task_id:
            del task_db[i]
            return {"status": "ok"}
    return {"error": f"Task with ID {task_id} not found."}
 
 
2.  Requirements.txt
 
plaintext
  
fastapi
uvicorn
pydantic
 
 
3.  README.md
 
markdown
  
## To-Do List API with FastAPI

This repository contains a simple To-Do List API built with FastAPI. It demonstrates CRUD (Create, Read, Update, Delete) operations on a list of tasks.

### API Endpoints

* **GET /tasks/{task_id}**: Retrieve a specific task by its ID.
* **POST /tasks**: Create a new task.
* **PATCH /tasks/{task_id}**: Update an existing task.
* **DELETE /tasks/{task_id}**: Delete a task.

### Usage

1. **Install Dependencies:**
   ```bash
   pip install -r requirements.txt
 
 
Run the API:
 
bash
  
uvicorn main:app --reload
 
 
Interact with the API:
You can use tools like  curl  or Postman to interact with the API.
 
 
- Example GET request (curl):
bash
  
curl http://127.0.0.1:8000/tasks/1 
 
 
- Example POST request (curl):
bash
  
curl -X POST -H "Content-Type: application/json" -d '{"task_title": "Write Report", "task_desc": "Finish the final report", "is_finished": false}' http://127.0.0.1:8000/tasks
 
 
Data Structure
 
Tasks are stored in a simple list called  task_db . Each task is represented as a dictionary with the following keys:
 
- task_id: Unique identifier for the task.
 
- task_title: Title of the task.
 
- task_desc: Description of the task.
 
- is_finished: Indicates if the task is completed (boolean).
 
Validation
 
The API includes basic input validation for task creation. It ensures that both the title and description fields are provided. You can extend this validation to handle other scenarios (e.g., length restrictions, data types).
 
Swagger UI
 
The API includes Swagger UI documentation for easy exploration.  You can access it at  http://127.0.0.1:8000/docs .
 
Further Improvements
 
- Database Integration: Integrate a real database (e.g., PostgreSQL, MongoDB) for persistent task storage.
 
- Authorization/Authentication: Implement user accounts and authentication for managing tasks.
 
- Error Handling: Enhance error handling with more specific error codes and messages.
 
This README provides a comprehensive overview of your To-Do List API project. Remember to commit your code to your GitHub repository!
 
plaintext
  

**4.  Swagger UI Screenshot**

You'll need to run the API and then open `http://127.0.0.1:8000/docs` in your browser to see the Swagger UI documentation, which will provide a visual representation of the API endpoints.

**GitHub Link**
https://github.com/cy-xael/Laboratory-Activity-2.git


1. **Create a new GitHub repository**.
2. **Add your `main.py` file, `requirements.txt`, and `README.md` files to the repository**.
3. **Commit your changes** and push to your repository.

**Key Concepts:**

* **HTTP Methods:**  The API utilizes common HTTP methods to implement CRUD operations:
    * **GET**: Retrieve data.
    * **POST**: Create new data.
    * **PATCH**: Update existing data.
    * **DELETE**: Remove data.
* **Path Parameters**:  `task_id` is a path parameter, indicating the specific task being accessed.
* **Data Validation:** The `pydantic` library is used to define data models (`Task`) for validation.
* **Error Handling:** The API includes error messages to provide feedback to the user in case of invalid requests.

