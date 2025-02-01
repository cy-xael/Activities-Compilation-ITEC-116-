
## To-Do List API with FastAPI

This repository contains a simple To-Do List API built with FastAPI, designed to handle basic CRUD operations (Create, Read, Update, Delete) on a list of tasks.

### Features

* **CRUD Operations:**
    * **Create:**  Create new tasks with a unique ID, title, description, and initial status (unfinished).
    * **Read:** Retrieve specific tasks by their ID.
    * **Update:**  Modify the title, description, or completion status of existing tasks.
    * **Delete:** Remove tasks from the list.
* **Data Validation:**
    * **Task Creation:** Ensures that both a title and description are provided when creating a new task.
* **Error Handling:**
    * **Task Not Found:** Returns an appropriate error message if the requested task ID does not exist. 
* **Simple Data Storage:**
    * Tasks are stored in an in-memory list (`task_db`). This is suitable for basic demonstrations, but for a real-world application, you would typically use a database (e.g., PostgreSQL, MongoDB).
* **Swagger UI Documentation:**
    * Includes Swagger UI for easy exploration and interaction with the API. 

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
 
API Endpoints
 
- GET /tasks/{task_id}: Retrieve a specific task by its ID.
 
- POST /tasks: Create a new task.
 
- PATCH /tasks/{task_id}: Update an existing task.
 
- DELETE /tasks/{task_id}: Delete a task.
 
Next Steps
 
- Database Integration: For a more robust application, integrate a database for persistent data storage.
 
- User Authentication: Add user accounts and authentication to manage tasks for different users.
 
- Additional Validation:  Implement more comprehensive validation rules for task data (e.g., length constraints, data types, etc.).
 
- Error Handling Enhancement:  Provide more granular error messages and codes to guide users.
 
