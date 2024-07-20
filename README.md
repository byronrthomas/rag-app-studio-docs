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
2. Generate a token for your account that has write access - see [these more detailed instructions](./hugging-face-token.md) if you're unsure
3. Request access to the Mistral 7B model - [request access to Mistral 7b model](./mistral-gated-access-request.md) - **YOU MUST have access to this model** as it is the default and RAG App Studio won't start without it
4. Request access to any other of the LLM models you want to use, following the instructions to [request access to Mistral 7b model](./mistral-gated-access-request.md), but visiting other HuggingFace pages. You can find out about each model on its HuggingFace page.
   1. [google/gemma-2b-it](https://huggingface.co/google/gemma-2b-it)
   2. [google/gemma-7b-it](https://huggingface.co/google/gemma-2b-it)
   3. [google/gemma-2-9b-it](https://huggingface.co/google/gemma-2-9b-it)
   4. [meta-llama/Llama-2-7b-chat-hf](https://huggingface.co/meta-llama/Llama-2-7b-chat-hf)
   5. [meta-llama/Meta-Llama-3-8B-Instruct](https://huggingface.co/meta-llama/Meta-Llama-3-8B-Instruct)

## RAG App Studio Builder

The builder application is where you build your RAG application. The builder works in a single-application model, where you are only working on a single application in a single builder process, and if you happen to relaunch it you will pick up the same application again, but it does have the capability to support [developing multiple applications](./multi-application-support.md) by changing the way the builder is launched to specify which of your apps to work on.

### Launching the builder

In order to launch an instance of the builder, you need to create a template on Theta EdgeCloud to run a custom AI workload:

SCREENSHOT in here

1. Navigate to the custom template page and click to add a template (see screenshot)
2. Enter the following details for the template:
   1. Name - you can enter anything that helps you, I suggest `RAGBuild`
   2. Container Image URL - must be set to `byronthomas712/rag-app-studio-builder:1.0`
   3. Container Port - must be set to `8000`
   4. Environment variables - must be set to `{"HUGGING_FACE_HUB_TOKEN":"hf_SECRET"}` if your HuggingFace hub token is `hf_SECRET` - please be careful to enter this correctly or the app won't start

Here is a screenshot showing how it would look if your token is `hf_SECRET` - **you must replace this value with your actual token!**

SCREENSHOT again
