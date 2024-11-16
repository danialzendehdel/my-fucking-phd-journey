+++
date = '2024-11-16T22:37:09+01:00'
draft = false
title = 'Gemini API 1'
+++


## Gemini API

The required dependencies are:
```bash
pip install -U -q "google-generativeai>=0.8.3"
```
As a first step, you need to create a Gemini account and get your API key. You can do this by visiting the [Gemini API](https://ai.google.dev/gemini-api/docs/api-key). Once you have your API key, you can use the `Gemini` class to interact.

```python
import google.generativeai as genai
import os

genai.configure(api_key=os.environ["GOOGLE_API_KEY"])
```
**Note**: Replace `os.environ["GOOGLE_API_KEY"]` with your actual API key. You can create `.env` file in the root directory of your project and add the following line to it:
```bash
GOOGLE_API_KEY=your_api_key
```

* ### First Prompt: 
  * As a first step, you need to create a prompt. The prompt is a short description of the content you want to generate. For example, if you want to generate a sunset image, you can use the following prompt: "A beautiful sunset over the city skyline."
      ```python
      prompt = "A beautiful sunset over the city skyline."
      model = genai.GenerativeModel("gemini-1.5-flash")
      response = model.generate_content(prompt)
      print(response.text)
    
      ```
* ### Start a chat 
    * You can start a chat with the model by using the `start_chat` method. The model will respond to your messages based on the prompt you provide.
    * While you have the `chat` object around, the conversation state persists.
        ```python
        chat = model,start_chat(history=[])
        response = chat.send_message(prompt)
        print(response.text)
      
        response = chat.send_message('Can you tell something interesting about dinosaurs?')
        print(response.text)
      
        ```
* ### Gemini models
    * The available models can be found [models](https://ai.google.dev/gemini-api/docs/models/gemini)
    * You can use the `list_models` method to get the list of available models.
        ```python
        models = genai.list_models()
        print(models)
        ```
    * Or print a model specificly to monitor the details of the model.
        ```python
        model = genai.GenerativeModel("gemini-1.5-flash")
        print(model)
        ```
* ### Control Output length
  *   When using a Large Language Model (LLM) to generate text, the length of the output has implications for cost and performance. Producing more tokens requires more computation, which in turn increases energy usage, response time, and overall cost.

  * To control the length of the output, you can set the `max_output_tokens` parameter in the Gemini API. This parameter limits the number of tokens the model will generate, but it does not affect the style or substance of the text produced. The text will simply stop once it reaches the set limit. To ensure the output is concise and complete within this constraint, careful crafting of the input prompt might be necessary.
    ```python
    short_model = genai.GenerativeModel(
    'gemini-1.5-flash',
    generation_config=genai.GenerationConfig(max_output_tokens=200))

    response = short_model.generate_content('Write a 1000 word essay on the importance of olives in modern society.')
    print(response.text)
    ```
* ### Temperature
  *  Top-K & Top-P
  * Similar to temperature, the top-K and top-P parameters help manage the diversity of the model’s output.
  * Top-K is a positive integer that limits the selection to the most likely tokens. Setting it to 1 results in greedy decoding, where only the most probable token is chosen.

  * Top-P sets a cumulative probability limit, stopping token selection once this threshold is exceeded. A top-P of 0 mirrors greedy decoding, while a top-P of 1 allows for any token from the vocabulary.

  * When using both parameters, the Gemini API applies top-K first, then top-P, before sampling based on the set temperature.

   ```python
  
  model = genai.GenerativeModel(
    'gemini-1.5-flash-001',
    generation_config=genai.GenerationConfig(
        # These are the default values for gemini-1.5-flash-001.
        temperature=1.0,
        top_k=64,
        top_p=0.95,
    ))
  
  # When running lots of queries, it's a good practice to use a retry policy so your code
  # automatically retries when hitting Resource Exhausted (quota limit) errors.
  retry_policy = {
    "retry": retry.Retry(predicate=retry.if_transient_error, initial=10, multiplier=1.5, timeout=300)
   }

  for _ in range(5):
  response = high_temp_model.generate_content('Pick a random colour... (respond in a single word)',
                                              request_options=retry_policy)
  if response.parts:
    print(response.text, '-' * 25)

  story_prompt = "You are a creative writer. Write a short story about a cat who goes on an adventure."
  response = model.generate_content(story_prompt, request_options=retry_policy)
  print(response.text)
  ```

