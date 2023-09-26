## Writing tests

To make sure my app is ready for a production workload I have to test at least:

1. My code is in compliance with conventions (public or company conventions)
2. My code doesn't have an identified security breach (I don't want to expose my customer data)
3. I'm able to execute my code in a controlled environment 
4. I'm able to make a request to get vote (I must have a 200 code as return on my GET request), since i'm in a controlled environment I must have 0 for each categories
5. I'm able to make a request to publish vote (I must have a 200 code as return on my POST request), since i'm in a controlled environment I must have 1 for the one i've voted and 0 to the other

## Workflows

### Open Pull Request

This workflow works when you're working on differents branches, and you want to merge your branch with ```main```

1. Pull Request is Opened
2. I'm checking if the code respect the Python convention using Flake8 (Because I'm respecting python writing conventions)
3. I'm checking if there is any CVEs opened, to make sure my code is secure. The last thing I want is to send security breach on my production environment
4. I'm making sure in a controlled environment (Here using docker-compose), my system is working as expected and i'm able to interact it
5. If and only if, all these requirements are meet, I'm able to merge into main my code

![Flow](docs/Simplon-PR-Opened-Workflow.drawio.svg)

### Merge Pull Request

This workflow will be triggered when a reviewer or my self gonna click on the merge button

1. Semantic release bot is triggered, it analyze commit messages to calculate the version number, when done, it publish a version on github https://www.conventionalcommits.org/en/v1.0.0/
2. I'm logging in my Docker Registry to being able to push my Docker image
3. I'm preparing the environment to being able to build my Dockerfile
4. I'm building my Dockerfile and name it using this format  ```<Docker Registry>/<Repository Name>:<Semantic Release version>``` (eg. joffreydupire/simplon-vote-app:v1.0.0)
then i'm pushing it to the registry.
5. Finally using ```helm upgrade``` command I'm deploying my Azure-Vote-App workload into my K8s cluster

> NOTES: From there, I could decide to deploy a canary or used a Blue/Green line 

![Flow](docs/Simplon-PR-Merged-Flow.drawio.svg)