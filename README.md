
### LIVE DEMO : https://research-report-app.victoriousdesert-ed2bc2bc.eastus.azurecontainerapps.io/

Designed and deployed a production-ready AI Research Report Generator using FastAPI, Docker, Jenkins CI/CD, Azure Container Registry, and Azure Container Apps with automated deployment through GitHub webhooks.


### Different specialized agents decompose a complex problem into domain-specific reasoning, producing higher-quality results than a single general-purpose agent.

For example, imagine the research topic is:

"Should Tesla build humanoid robots?"

A single LLM may produce one answer.

A multi-agent system might create:

### Financial Analyst
│
└── Cost, ROI, revenue

### Market Analyst
│
└── Demand, competitors

### AI Research Analyst
│
└── Technical feasibility

### Ethics Analyst
│
└── Safety, regulation

### Manufacturing Analyst
│
└── Supply chain, production

Then a Supervisor Agent combines all of those perspectives into one final report.

This is why multi-agent systems often produce richer analyses.


90d1ffd7f2644a90a0e2ee92ceb104a4

 test 2
az ad sp create-for-rbac \
  --name "jenkins-research-report-sp" \
  --role Contributor \
  --scopes "//subscriptions/$(az account show --query id -o tsv)"



The Problem

After fixing the Python import errors locally, I rebuilt the Docker image and pushed it to Azure Container Registry (ACR). Then I triggered my Jenkins pipeline to deploy it to Azure Container Apps.

  Instead of assuming, I inspected the image itself.

I ran:

docker run --rm --entrypoint cat \
researchreportacr.azurecr.io/research-report-app:latest \
/app/research_and_analyst/utils/model_loader.py


When I looked at the output, I saw:

from .config_loader import load_config

That proved something very important.

The Docker image already contained the corrected code.

So now I knew:

Local Source Code     ✅ Correct
Docker Image          ✅ Correct

Now only one possibility remained.

###### Azure Container Apps was still running an older version of the image.

I saw something like:

Revision 0000001

That revision had been created before I fixed my code.

It was still the active revision serving traffic.

So although my latest Docker image existed inside Azure Container Registry, Azure Container Apps had not actually started a new revision using that updated image.

To force Azure to pull the correct image, I deployed using the image digest instead of the latest tag.

For example:

research-report-app@sha256:ff104039...
used this command
$ az containerapp update \
  --name research-report-app \
  --resource-group research-report-app-rg \
  --image researchreportacr.azurecr.io/research-report-app@sha256:ff1040390252e5bd68f4bb775cd05dec41f83a1

An image digest is a unique fingerprint of a Docker image.
Unlike the latest tag, it can never point to a different image.
This forced Azure Container Apps to download that exact image and create a new revision.

After deployment I saw:

Revision 0000002

instead of

Revision 0000001

That told me Azure was now running the new image.

## Finally, I opened the application URL and received:

HTTP 200 OK

The application was running successfully.


### Fine-tuned a Mistral-7B model using QLoRA on domain-specific medical data, reducing operational cloud hosting infrastructure costs by 60%."

#### 🔴 Bad: "Integrated LangChain and vector databases to build an internal Q&A tool."  

#### 🟢 Good: "Deployed a production RAG pipeline using PGVector. Reduced API token costs by 42% utilizing prompt compression and implemented an automated 'LLM-as-a-judge' evaluation framework to catch regressions."
