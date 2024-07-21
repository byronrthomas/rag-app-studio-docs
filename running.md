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


