# Building multiple applications with RAG App Studio

RAG App Studio is designed to simplify the workflow for users wanting the simplest experience - developing only a single app.

However, behind the scenes it supports multiple applications, it just takes a little bit more effort to use it
like this.

## Applications vs repos

The builder and the runner communicate via repositories that are private to your HuggingFace hub account. A single
repository represents a single app. When you start the builder for the very first time, it creates a new repository
(and therefore a new app) for you. Each time you work on an app in the builder, it also updates a special
"preferences" repo on HuggingFace to remember that this repo is the active one. This allows the builder & runner
to launch into a single app without you needing to set any preferences yourself.

## Workflow to work on multiple apps

You can launch the builder app in a mode that will forget the last active repo, and create a fresh repo. You
just need to set the container environment variables like the below - replace `hf_SECRET` with your access token.

`{"CREATE_NEW_RAG_APP":"1","HUGGING_FACE_HUB_TOKEN":"hf_SECRET"}`

You can also launch the builder app in a mode where it ignores the last active repo, and just takes whatever 
repo / app you want. To do this, set the container environment variables like the below - replace `hf_SECRET` 
with your access token:

`{"HUGGING_FACE_HUB_TOKEN":"hf_SECRET","RAG_REPO_ID":"rag-studio-f9edf974"}`

Finally, you can launch the runner app in a mode where it ignores the last active repo, and just takes whatever 
repo / app you want. To do this, set the container environment variables like the below - replace `hf_SECRET` 
with your access token:

`{"HUGGING_FACE_HUB_TOKEN":"hf_SECRET","RAG_REPO_ID":"rag-studio-f9edf974"}`

### Putting it all together

So in order to work on multiple apps:
* Record the repo ID you want to keep working on whilst you start a new app, e.g. rag-studio-f9edf974
* Change your model templates in Theta EdgeCloud so that the container environment variables to specify this repo
* Add an additional model template to create a fresh app and note it's repo ID
* Then you can create templates to build & run with that app, by setting environment variables to target this second repo ID

So for example, you might have an "APP1 Builder" template that uses `"RAG_REPO_ID":"rag-studio-f9edf974"` and an "APP1 Runner" template that uses `"RAG_REPO_ID":"rag-studio-f9edf974"`, but you also have an "APP2 Builder" template that uses `"RAG_REPO_ID":"rag-studio-ee67e301` and an "APP2 Runner" template that uses `"RAG_REPO_ID":"rag-studio-ee67e301`