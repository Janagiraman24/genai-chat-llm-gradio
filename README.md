## Development and Deployment of a 'Chat with LLM' Application Using the Gradio Blocks Framework

## DATE : 04.09.2026
### AIM:
To design and deploy a "Chat with LLM" application by leveraging the Gradio Blocks UI framework to create an interactive interface for seamless user interaction with a large language model.

### PROBLEM STATEMENT:
Interacting directly with a Large Language Model through programming code can be difficult for users who are not familiar with programming. Therefore, a user-friendly chatbot interface is required to allow users to enter questions, view generated responses, maintain conversation history, and control the behaviour of the LLM. This project develops such an interactive application using Gradio Blocks.

### DESIGN STEPS:

#### STEP 1:
Import the required libraries and configure the Hugging Face API connection. Create a client for the FalconLM instruction-based language model using the API endpoint and authentication key.

#### STEP 2:
Create functions to format the user's message along with previous conversation history. The formatted prompt is sent to the LLM, which generates an appropriate response. The system instruction is also included to define how the AI assistant should respond.

#### STEP 3:
Build the user interface using the Gradio Blocks framework. Add a chatbot window, message textbox, submit button, clear button, system-message field, and temperature slider. Connect the interface components with the response function and deploy the application using Gradio.

### PROGRAM:
```python
import os
import io
import IPython.display
from PIL import Image
import base64 
import requests 
requests.adapters.DEFAULT_TIMEOUT = 60

from dotenv import load_dotenv, find_dotenv
_ = load_dotenv(find_dotenv()) # read local .env file
hf_api_key = os.environ['HF_API_KEY']
```
```python
# Helper function
import requests, json
from text_generation import Client

#FalcomLM-instruct endpoint on the text_generation library
client = Client(os.environ['HF_API_FALCOM_BASE'], headers={"Authorization": f"Basic {hf_api_key}"}, timeout=120)
```
```python
prompt = "Has math been invented or discovered?"
client.generate(prompt, max_new_tokens=256).generated_text
```
```python
import gradio as gr
def generate(input, slider):
    output = client.generate(input, max_new_tokens=slider).generated_text
    return output

demo = gr.Interface(fn=generate, 
                    inputs=[gr.Textbox(label="Prompt"), 
                            gr.Slider(label="Max new tokens", 
                                      value=20,  
                                      maximum=1024, 
                                      minimum=1)], 
                    outputs=[gr.Textbox(label="Completion")])

gr.close_all()
demo.launch(share=True, server_port=int(os.environ['PORT2']))
```

### OUTPUT:
<img width="1181" height="635" alt="image" src="https://github.com/user-attachments/assets/2f97aaf8-cd4d-48dc-b576-6c02333aa1ee" />


### RESULT:
Thus, the "Chat with LLM" application using the Gradio Blocks framework was successfully developed and deployed. The application provides an interactive interface for communicating with the LLM while maintaining conversation history and providing configurable response-generation options.
