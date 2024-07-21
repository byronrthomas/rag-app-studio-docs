<p align="center">
  <img src="./images/rag_app_studio_logo.png" width="1000px" alt="Logo">
</p>

# RAG App Studio

Your low-code solution to build your own private knowledge-base enhanced (RAG) generative AI chatbots on Theta EdgeCloud, using the latest LLM models!

No need to send all of your chats and data to OpenAI or other big-tech data gatherers, you control your own data and specialise your applications to do what you need.

## Pre-requisistes / initial setup

### Theta EdgeCloud

Theta EdgeCloud is a GPU-cloud for running AI workloads which RAG App Studio has been developed to run on. Theta EdgeCloud is a pay as you go service with a range of tiers of machine capabilities, some of which are quota-restricted. In order to get started you need to:

1. Sign up for [Theta EdgeCloud](https://www.thetaedgecloud.com)
2. Add some credit to your account
3. Request to get your machine quota increased, using the [quota settings page](https://www.thetaedgecloud.com/dashboard/settings/quota) - you will need a GV2 or GA1 machine at least to run any serious LLM models.

### HuggingFace Hub

HuggingFace is a repository for AI models, think of it like Github for models with a whole raft of additional collaboration features on top! RAG App Studio relies on it for two reasons: it has the source data for the open-source LLMs that we run in the app studio, and also we use **private** repositories as storage for the application that you build. Although open-source LLM models are public access, they are typically gated on HuggingFace, which means that you need to be logged in, and manually request access to the LLM data by accepting basic Ts & Cs over usage.

In order to use RAG App Studio, you need access to HuggingFace, you need to generate an appropriate access token and you need to get access to the models you want to use in the app. Follow the steps below to go through each of these.

1. Sign up for an account with HuggingFace hub
2. Generate a token for your account that has write access - see [these more detailed instructions](./hugging-face-token.md) if you're unsure - you should save this to a password manager to keep it safe
3. Request access to the Mistral 7B model - [request access to Mistral 7b model](./mistral-gated-access-request.md) - **YOU MUST have access to this model** as it is the default and RAG App Studio won't start without it
4. Request access to any other of the LLM models you want to use, following the instructions to [request access to Mistral 7b model](./mistral-gated-access-request.md), but visiting other HuggingFace pages. You can find out about each model on its HuggingFace page.
   1. [google/gemma-2b-it](https://huggingface.co/google/gemma-2b-it)
   2. [google/gemma-7b-it](https://huggingface.co/google/gemma-2b-it)
   3. [google/gemma-2-9b-it](https://huggingface.co/google/gemma-2-9b-it)
   4. [meta-llama/Llama-2-7b-chat-hf](https://huggingface.co/meta-llama/Llama-2-7b-chat-hf)
   5. [meta-llama/Meta-Llama-3-8B-Instruct](https://huggingface.co/meta-llama/Meta-Llama-3-8B-Instruct)

## Building your own document-enhanced AI chatbot

In order to build a chatbot specialised with knowledge that you want to query, you will normally:

1. Configure your knowledgebase
2. Set up a model and engineer your prompts
3. Profit

Building RAG-enhanced applications often takes a fair amount of experimentation and iteration, so you may need to [start from scratch sometimes](./multi-application-support.md#how-to-start-afresh) and build a new app based on what you've tried already and what you've learnt. You can find plenty of articles on using RAG and LLMs on sites like medium.com, here are a couple of starting points if you're interested:
* [A very detailed deep dive on a range of topics around how to build an app that uses LLMs](https://applied-llms.org/)
* [A write up on using RAG to make a searchable knowledgebase](https://towardsdatascience.com/how-i-turned-my-companys-docs-into-a-searchable-database-with-openai-4f2d34bd8736).

In RAG App Studio you will use the Builder app to experiment and configure your application, and then when it's ready for the general public, you will use the Runner app to interact with your application (which is more specialised to just handling chats and queries).

### Launching the builder app

The builder application is where you build your RAG application. The builder works in a single-application model, where you are only working on a single application in a single builder process, and if you happen to relaunch it you will pick up the same application again, but it does have the capability to support [developing multiple applications](./multi-application-support.md) by changing the way the builder is launched to specify which of your apps to work on.

In order to launch an instance of the builder, you need to create a template on Theta EdgeCloud to run a custom AI workload:

![Adding a custom AI workflow template in Theta EdgeCloud](./images/custom_template_creation.png)

1. Navigate to the custom template page and click to add a template (see screenshot above)
2. Enter the following details for the template:
   1. Name - you can enter anything that helps you, I suggest `RAGBuild`
   2. Container Image URL - must be set to `byronthomas712/rag-app-studio-builder:1.0`
   3. Container Port - must be set to `8000`
   4. Environment variables - must be set to `{"HUGGING_FACE_HUB_TOKEN":"hf_SECRET"}` if your HuggingFace hub token is `hf_SECRET` - please be careful to enter this correctly or the app won't start

Here is a screenshot showing how it would look if your token is `hf_SECRET` - **you must replace this value with your actual token!**

![Details to set up the builder template in Theta EdgeCloud](./images/builder_container_details.png)

Once you have the builder template, you just need to create a new model deployment from it:

1. Click on your template name, e.g. `RAGBuild`
2. Select a big enough machine - you need at least GV2 or GA1
3. Click create

**ANOTHER SCREENSHOT ideally**

Then wait, within around 5 minutes the builder should be fully loaded.

**TODO TODO TODO - explain how to find the UI and what to do next**

### Configuring your knowledgebase




## RAG App Studio Runner

**IMPORTANT:** you cannot run the runner unless you have built an application using the builder - there is nothing to run.

The runner application is where you run your RAG application. The runner works in a single-application model, where you are only running a single application in a single runner process, and by default you will run whatever was the latest application you worked on. It does have the capability to support [running multiple applications](./multi-application-support.md) by changing the way the runner is launched to specify which of your apps to work on.

### Launching the runner

In order to launch an instance of the runner, you need to create a template on Theta EdgeCloud to run a custom AI workload:

![Adding a custom AI workflow template in Theta EdgeCloud](./images/custom_template_creation.png)

1. Navigate to the custom template page and click to add a template (see screenshot above)
2. Enter the following details for the template:
   1. Name - you can enter anything that helps you, I suggest `RAGRunner`
   2. Container Image URL - must be set to `byronthomas712/rag-app-studio-runner:1.0` - **NOTE that this a different image to the builder template**
   3. Container Port - must be set to `8000`
   4. Environment variables - must be set to `{"HUGGING_FACE_HUB_TOKEN":"hf_SECRET"}` if your HuggingFace hub token is `hf_SECRET` - please be careful to enter this correctly or the app won't start

Here is a screenshot showing how it would look if your token is `hf_SECRET` - **you must replace this value with your actual token!**

![Details to set up the builder template in Theta EdgeCloud](./images/runner_container_details.png)

Once you have the builder template, you just need to create a new model deployment from it:

1. Click on your template name, e.g. `RAGRunner`
2. Select a big enough machine - you need at least GV2 or GA1
3. Click create

**ANOTHER SCREENSHOT ideally**

Then wait, within around 5 minutes the builder should be fully loaded.

**TODO TODO TODO - explain how to find the UI and what to do next**


