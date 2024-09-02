# HPE Machine Learning Inference Software (MLIS) Demo using HuggingFace Chat UI

## Objective: 
- Demo chat with LLMs using HPE MLIS LLM endpoint and HuggingFace chat UI

##  Steps:
  - Step 1: Deploy LLM endpoint using HPE MLIS 
  - Step 2: Deploy HF chat UI

## Step 1: Deploy LLM endpoint using HPE MLIS 
  
  - At a high-level, there are 04 main objects: 

  1. Registris: Configure where models come from. Registries are the storage locations for your models and deployments. Any storage solution that supports the S3 protocol can be used as a registry. Once added, you can use the models to create a service and deploy it for inferencing. Ref[https://docs.ai-solutions.ext.hpe.com/products/mlis/latest/registries/]. 

  2. Packaged models: A packaged model describes the model and user code that you want to deploy as an inference service. Ref[https://docs.ai-solutions.ext.hpe.com/products/mlis/latest/packaged-models/].

  3. Deployment: Deployments spin up the actual instances that run your inference services. They provide an endpoint that can be used by your applications to interact with your models. Ref[https://docs.ai-solutions.ext.hpe.com/products/mlis/latest/deployments/].

  4. Token: Access tokens enable you to control who can use protected deployments. Ref [https://docs.ai-solutions.ext.hpe.com/products/mlis/latest/tokens/].

  - Service deployment journey. Once a deployment is create an endpoint is available.  

  ```sh
  Set up Registry --> Add Registry --> Add Model --> Create Deployment
  ```

## Step 2: Deploy HF chat UI

  - Fork repo from the original HugginfFace Chat UI repo[https://github.com/huggingface/chat-ui].

  - Create a copy of .env and name the file .env.local in the same directory

  - Check here how you run your own models using a custom endpoint[https://github.com/huggingface/chat-ui?tab=readme-ov-file#running-your-own-models-using-a-custom-endpoint].
  
  ```yml
  {
  // rest of the model config here
  "endpoints": [{
    "type" : "tgi",
    "url": "https://HOST:PORT",
    }]
  }
  
  ```
  Below is an example of an OpenAI-compatible endpoint configuration supported by HPE MLIS. 
  
  ```yml
  MODELS=`[
  { 
    "name": "meta-llama--Llama-2-7b-chat-hf",
    "id": "meta-llama--Llama-2-7b-chat-hf",
    "parameters": {
      "temperature": 0.9,
      "top_p": 0.95,
      "repetition_penalty": 1.2,
      "top_k": 50,
      "truncate": 1000,
      "max_new_tokens": 1024,
      "stop": []
    },
    "endpoints": [{
      "type" : "openai",
      "baseURL": "http://dcao-llama-chatbot.default.mlds-kserve.us.rdlabs.hpecorp.net/v1"
    }],
  },
  ]`
  ```
  
  - Make sure you have MongoDB running locally
  
  ```sh
  docker run -d -p 27017:27017 --name mongo-chatui mongo:latest
  ```

  - Start the chat UI 
  ```sh
  cd chat-ui
  npm install
  npm run dev -- --open
  ```

  Finally, access the HF Chat UI via http://localhost:5173/ . Happy chatting!

### Troubleshoot issues (Optional)

- 
