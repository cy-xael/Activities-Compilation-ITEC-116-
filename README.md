# Activities-Compilation-ITEC-116-
Compilation of my activities in the subject ITEC 116 and serves as the subjects last requirements. 


# ITEC 116 - API Development Activities

This repository contains the code for the API development activities completed for ITEC 116. Each folder represents a different activity.

## Activity 2: Simple API with Factorial Calculation

**Folder:** `lab2`

This activity focuses on creating a basic API that calculates the factorial of a given number.

**Key Features:**

*   **FastAPI:** The API is built using the FastAPI framework.
*   **Endpoint:** The API exposes a single endpoint at `/factorial/{starting_number}` to calculate factorials.
*   **Input Validation:** The code includes basic validation to handle negative input numbers.

**Usage:**

1.  **Install Dependencies:** `pip install fastapi uvicorn`
2.  **Run the API:** `uvicorn main:app --reload`
3.  **Test the API:** Use curl or Postman to make GET requests to the `/factorial/{starting_number}` endpoint (e.g., `http://127.0.0.1:8000/factorial/5`).

## Activity 3: To-Do List API with CRUD Operations

**Folder:** `lab3`

This activity expands on the previous one by creating a To-Do List API that supports CRUD operations.

**Key Features:**

*   **CRUD Operations:** The API provides endpoints for creating, reading, updating, and deleting tasks.
*   **Data Validation:** Basic validation ensures that tasks have both a title and description.
*   **In-Memory Data Storage:** Tasks are stored in a simple in-memory list.

**Usage:**

1.  **Install Dependencies:** `pip install fastapi uvicorn pydantic`
2.  **Run the API:** `uvicorn main:app --reload`
3.  **Test the API:** Use curl or Postman to interact with the API endpoints:
    *   `/tasks/{task_id}` (GET): Retrieve a task by ID
    *   `/tasks` (POST): Create a new task
    *   `/tasks/{task_id}` (PATCH): Update a task
    *   `/tasks/{task_id}` (DELETE): Delete a task

## Activity 4: Versioning, Authentication, and Error Handling

**Folder:** `lab4`

This activity introduces versioning, authentication, and proper HTTP exception handling to the To-Do List API.

**Key Features:**

*   **Versioning:** Two API versions (`apiv1` and `apiv2`) are implemented.
*   **Authentication:** Requests are authenticated using an API key stored in a `.env` file (not included in the repository).
*   **HTTP Exception Handling:**
    *   **404 Not Found:** Returned for missing tasks.
    *   **401 Unauthorized:** Returned for invalid API keys.
    *   **201 Created:** Returned when a new task is created.
    *   **204 No Content:** Returned when a task is updated or deleted.

**Usage:**

1.  **Create a `.env` file:**  `LAB4_API_KEY=your_api_key` (replace `your_api_key` with the API key you set up)
2.  **Install Dependencies:** `pip install fastapi uvicorn pydantic`
3.  **Run the API:** `uvicorn main:app --reload`
4.  **Test the API:** Use curl or Postman with the correct API key in the `x-api-key` header to interact with the API endpoints.

## Activity 5: API Deployment on Render

**Folder:** `lab5`

This activity deploys the To-Do List API to Render.

**Key Features:**

*   **Render Deployment:** The API is deployed to the Render platform for easy access and scalability.
*   **Environment Variables:**  The API key is set as an environment variable on Render.

**Usage:**

1.  **Access the Deployed API:** The deployed API URL is in the format: `itec116_<surname>.onrender.com`.
2.  **Test the API:** Use curl or Postman to interact with the deployed API endpoints. Remember to include the correct API key in the `x-api-key` header.

