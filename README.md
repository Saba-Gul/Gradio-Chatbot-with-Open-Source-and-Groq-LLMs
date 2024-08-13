# Gradio Chatbot with Open Source and Groq LLMs

This project demonstrates the implementation of a chatbot using Gradio and two different language models: an open-source model from Hugging Face (`TinyLlama/TinyLlama-1.1B-Chat-v1.0`) and a model from Groq API (`llama3-8b-8192`). The chatbot is designed to respond in a specific style, such as a pirate or a helpful assistant, depending on the system instructions provided.

## Features

- **Gradio Interface**: A simple and interactive web-based interface for chatting with the AI models.
- **Hugging Face Model**: Utilizes `TinyLlama/TinyLlama-1.1B-Chat-v1.0` for generating text in response to user inputs.
- **Groq API Integration**: Implements Groq API to accelerate token generation with open-source LLMs like `llama3-8b-8192`.
- **Customizable System Prompts**: Allows setting specific instructions for the chatbot's behavior (e.g., responding in the style of a pirate or as a helpful assistant).
- **Adjustable Model Parameters**: Includes parameters like `temperature`, `max_tokens`, and `top_p` to control the randomness, length, and diversity of the generated text.

## Installation

To run the project locally, you'll need to install the required Python packages. You can install them using the following commands:

```bash
pip install gradio transformers groq
```

## Usage

1. **Hugging Face Model (TinyLlama):**

   ```python
   from transformers import pipeline
   import gradio as gr
   
   model = pipeline("text-generation", model="TinyLlama/TinyLlama-1.1B-Chat-v1.0", device_map="auto")

   def chat(message, history):
       messages = [
           {"role": "system", "content": "You are a friendly chatbot who always responds in the style of a pirate"},
           {"role": "user", "content": message},
       ]
       return model(messages, max_new_tokens=64)[0]['generated_text'][2]['content']

   demo = gr.ChatInterface(fn=chat, title="Open Source chatbot")
   demo.launch(debug=True)
   ```

2. **Groq API Model (Llama3-8b-8192):**

   ```python
   from groq import Groq
   import gradio as gr
   
   client = Groq(api_key='<your-groq-api-key>')

   def chat(message, history):
       chat_completion = client.chat.completions.create(
           messages=[
               {"role": "system", "content": "you are a helpful assistant."},
               {"role": "user", "content": message},
           ],
           model="llama3-8b-8192",
           temperature=0.5,
           max_tokens=1024,
           top_p=1,
           stop=None,
           stream=False,
       )
       return chat_completion.choices[0].message.content

   demo = gr.ChatInterface(fn=chat, title="Open Source chatbot")
   demo.launch(debug=True)
   ```

## Running the Chatbot

1. Replace `<your-groq-api-key>` with your actual Groq API key.
2. Run the Python scripts to start the chatbot interface.
3. Interact with the chatbot via the Gradio interface.

