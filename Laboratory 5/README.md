## Activity 5: To-Do List API Deployment on Render

This folder contains the code for the To-Do List API deployed on Render. 

### Key Features

* **Render Deployment:** The API is deployed to the Render platform, making it accessible and scalable. 
* **Environment Variables:** The API key is securely stored as an environment variable on Render, ensuring it's not included in the codebase itself.
* **Versioning:**  Two API versions (`apiv1` and `apiv2`) are available.
* **Authentication:** Requests require a valid API key for authorization.
* **HTTP Exception Handling:** The API returns appropriate HTTP status codes and error messages for various scenarios:
    * **404 Not Found:** For missing tasks.
    * **401 Unauthorized:** For invalid API keys.
    * **201 Created:** For successful task creation.
    * **204 No Content:** For successful task updates and deletions.

### Usage

1. **Access the Deployed API:** The API is available at: `itec116_<surname>.onrender.com`. 
   * Replace `itec116_<surname>` with your actual surname. 

2. **Test the API:**  Use tools like curl or Postman to interact with the API.  
   * **Example using curl:**
     ```bash
     curl -H "x-api-key: your_api_key" https://itec116_<surname>.onrender.com/apiv1/tasks/1 
     ```
   * **API Key:** Replace `your_api_key` with the actual API key set up on Render.

### Setup on Render

* **Runtime:**  Python 3.9 or 3.10 (choose the version you used in your project).
* **Environment Variable:**
    * Name: `LAB4_API_KEY` (this is the same variable name used in your `main.py` file)
    * Value: Your API key 
* **Build Command:**  `pip install -r requirements.txt` 
* **Start Command:** `uvicorn main:app --reload`
