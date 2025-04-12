## Integration of a Mathematical Calulations with a Chat Completion System using LLM Function-Calling

### AIM:
To design and implement a Python function for calculating the volume of a cylinder, integrate it with a chat completion system utilizing the function-calling feature of a large language model (LLM).

### PROBLEM STATEMENT:
To calculate Volume of Cylinder by using LLM Function-Calling by passing arguments through function call and display volume in JSON format.

### DESIGN STEPS:

#### STEP 1:
Import required libraries: os, openai, json, math. Load the API key securely using dotenv to access OpenAI.

#### STEP 2:
Create the volume_of_cylinder(radius, height, unit="meter") function.Calculate volume using the formula: 𝑉=𝜋×radius^2×height. Store the result in a dictionary with radius, height, volume, and unit. Return the data in JSON format.

#### STEP 3:
Create a user message to request cylinder volume calculation and show the calculated volume in JSON format. Send a request to openai.ChatCompletion.create(), including:
1. Model: "gpt-3.5-turbo"
2. Messages: User input
3. Functions: Defined function metadata
4. Function Call: Explicitly request "volume_of_cylinder"
   
### PROGRAM:
```
import os
import openai

from dotenv import load_dotenv, find_dotenv
_ = load_dotenv(find_dotenv()) # read local .env file
openai.api_key = os.environ['OPENAI_API_KEY']
```
```
import math
def calculate_cylinder_volume(radius, height):
    """
    Calculate the volume of a cylinder using the formula:
    Volume = π * r^2 * h
    """
    if radius <= 0 or height <= 0:
        return "Radius and height must be positive numbers."
    
    volume = math.pi * (radius ** 2) * height
    return round(volume, 2)
```
```
def chat_with_openai(prompt):
    response = openai.ChatCompletion.create(
        model="gpt-4",
        messages=[
            {"role": "system", "content": "You are an assistant that helps calculate the volume of a cylinder."},
            {"role": "user", "content": prompt},
        ],
        functions=[
            {
                "name": "calculate_cylinder_volume",
                "description": "Calculate the volume of a cylinder given radius and height.",
                "parameters": {
                    "type": "object",
                    "properties": {
                        "radius": {"type": "number", "description": "Radius of the cylinder (in units)"},
                        "height": {"type": "number", "description": "Height of the cylinder (in units)"},
                    },
                    "required": ["radius", "height"],
                },
            }
        ],
        function_call="auto",  
    )
    
    if "function_call" in response["choices"][0]["message"]:
        function_name = response["choices"][0]["message"]["function_call"]["name"]
        arguments = eval(response["choices"][0]["message"]["function_call"]["arguments"])
        if function_name == "calculate_cylinder_volume":
            radius = arguments["radius"]
            height = arguments["height"]
            return calculate_cylinder_volume(radius, height)
    
    return response["choices"][0]["message"]["content"]
```
```
radius = float(input("Enter the radius of the cylinder: "))
height = float(input("Enter the height of the cylinder: "))

prompt = f"What is the volume of a cylinder with a radius of {radius} and a height of {height}?"
result = chat_with_openai(prompt)
print("Result:", result)
```
### OUTPUT:
![image](https://github.com/user-attachments/assets/99fd384a-092b-4aa7-b609-d263be1c5cdb)


### RESULT:
Thus, The integration of a Mathematical Calulations with a Chat Completion System using LLM Function-Calling is executed successfully.
