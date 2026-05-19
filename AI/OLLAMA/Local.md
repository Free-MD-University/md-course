Ollama can download and setup locally an LLM .
the Management and download is 100% setuped by OLLAMA . 
 

 # install 
 just install from the website 

  # LIST
  a command that allow to show all listed model : 
  ```shell
   ollama list
  ```
 # ModelFile

 is like a docker file . 
 we can say the model, the temperature, and a prompt at the beginning. 


 ```txt
    FROM {Model}:{Tag}
    PARAMETER temperature 0.5


    SYSTEM """ 
       BASE PROMPT YOUR MODEL 
    """

 ```

 ## build 
  to build this model we can use 
  ```shell
  ollama create {NEW_MODEL_NAME} -f ModelFile
  ```

 # run a model : 

 ## FROM A SHELL
 for running a model just type :
 ```
 ollama run {model}:{tag}
 ```
 a command prompt will be show and you can ask some auestion . 

 we can use some command like : 

 /show --> show model information .

 ## file : 
 we can add a file to the input by oassing a path (relative or not).

 ## FROM A REST API 
 when ollama run in a backend. we can use the rest endpoint to run the ollama 

 ```curl 
 http://localhost:11434/api/generate 
 {
    "model": model name
    "prompt": question,
    "format": "txt,json"# hot to format the response
    stream: true/ false # see the entire stream .
 }
 ```

## From python
there is an official ollama library in python . 

# Thinking 
https://ollama.com/blog/thinking