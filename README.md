# Github Actions

## How it works

- Every GitHub Action runs in a Docker container and requires a Dockerfile
- A workflow uses and can contain many actions
- An action has it's own directory where the associated Dockerfile and all other assets go
- The Dockerfile defines the environment in which the action will run

Example Dockerfile:

```Dockerfile
FROM debian:9.5-slim

LABEL "com.github.actions.name"="Hello World"
LABEL "com.github.actions.description"="Write arguments to the standard output"
LABEL "com.github.actions.icon"="mic"
LABEL "com.github.actions.color"="purple"

LABEL "repository"="http://github.com/octocat/hello-world"
LABEL "homepage"="http://github.com/actions"
LABEL "maintainer"="Octocat <octocat@github.com>"

ADD entrypoint.sh /entrypoint.sh
ENTRYPOINT ["/entrypoint.sh"]
```

- The entrypoint.sh script will be run in Docker, and it will define what the action is really going to be doing
- the entrypoint.sh file must therefore also reside in the folder associated with the action

### Workflow Files

- Workflows are defined in special files in the .github/workflows directory, named main.yml
- Workflows can execute based on your chosen event

Example workflow file:

```YAML
name: A workflow for my Hello World file
on: push
```

- `name: A workflow for my Hello World file` gives your workflow a name. This name appears on any pull request or in the Actions tab. The name is especially useful when there are multiple workflows in your repository.
- `on: push` indicates that your workflow will execute anytime code is pushed to your repository, using the push event.

### Actions

Workflows piece together jobs, and jobs piece together steps. Actions can be used from within the same repository, from any other public repository, or from a published Docker container image.

Example action block added to the workflow file:

```YAML
name: A workflow for my Hello World file
on: push

jobs:
  build:
    name: Hello world action
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@master
    - uses: ./action-a
      env:
        MY_NAME: "Mona"
```
