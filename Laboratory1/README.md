
 
FastAPI Factorial Calculator API
 
This repository contains a simple API built with FastAPI that calculates the factorial of a given number.
 
API Endpoint
 
- /factorial/{starting_number}:  This endpoint accepts a positive integer ( starting_number ) as a path parameter and returns its factorial. If the input is 0, it returns  {"result": false} .  If the input is negative, it returns an appropriate error message.
 
Code (main.py)
 
python
  
from fastapi import FastAPI, HTTPException

app = FastAPI()

@app.get("/factorial/{starting_number}")
async def factorial(starting_number: int):
    """
    Calculates the factorial of a given number using a while loop.
    """
    if starting_number < 0:
        raise HTTPException(status_code=400, detail="Input must be a non-negative integer.")
    elif starting_number == 0:
        return {"result": False}
    else:
        result = 1
        i = 1
        while i <= starting_number:
            result *= i
            i += 1
        return {"result": result}

 
 
Dependencies
 
To run this application, you'll need the following dependencies:
 
- FastAPI:  pip install fastapi uvicorn 
 
Running the API
 
Save the code: Save the code above as  main.py  in your repository.
 
Install dependencies: Open your terminal, navigate to the directory containing  main.py , and run:   pip install fastapi uvicorn 
 
Run the API: Run  uvicorn main:app --reload  in your terminal. This will start the server.
 
Test the API: You can test the API using tools like curl or Postman.  For example, to calculate the factorial of 5, you would make a GET request to  http://127.0.0.1:8000/factorial/5 .
 
Example Usage (using curl)
 
bash
  
curl http://127.0.0.1:8000/factorial/5
 
 
This should return:
 
json
  
{"result": 120}
 
 
bash
  
curl http://127.0.0.1:8000/factorial/0
 
 
This should return:
 
json
  
{"result": false}
 
 
Error Handling
