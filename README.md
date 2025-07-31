# CICD-new


gcloud services enable run.googleapis.com artifactregistry.googleapis.com cloudbuild.googleapis.com iam.googleapis.com

gcloud artifacts repositories create hello-repo \
  --repository-format=docker \
  --location=us-central1 \
  --description="Artifact Registry for Python App"

  gcloud projects add-iam-policy-binding your-gcp-project-id \
  --member="serviceAccount:github-actions@your-gcp-project-id.iam.gserviceaccount.com" \
  --role="roles/run.admin"

gcloud projects add-iam-policy-binding your-gcp-project-id \
  --member="serviceAccount:github-actions@your-gcp-project-id.iam.gserviceaccount.com" \
  --role="roles/artifactregistry.writer"

gcloud projects add-iam-policy-binding your-gcp-project-id \
  --member="serviceAccount:github-actions@your-gcp-project-id.iam.gserviceaccount.com" \
  --role="roles/cloudbuild.builds.editor"

# Create Workload Identity Pool
gcloud iam workload-identity-pools create github-pool1 \
  --location="global" \
  --display-name="GitHub Pool"

# Create Provider
  gcloud iam workload-identity-pools providers create-oidc "github-provider" \
  --location="global" \
  --workload-identity-pool="github-pool" \
  --display-name="GitHub Actions Provider" \
  --issuer-uri="https://token.actions.githubusercontent.com" \
  --attribute-mapping="google.subject=assertion.sub,attribute.repository=assertion.repository" \
  --attribute-condition="attribute.repository == 'Sai-Kiran1426/CICD-new'"

  # Allow GitHub to impersonate your service account
gcloud iam service-accounts add-iam-policy-binding github-actions@your-gcp-project-id.iam.gserviceaccount.com \
  --role="roles/iam.workloadIdentityUser" \
  --member="principalSet://iam.googleapis.com/projects/PROJECT_NUMBER/locations/global/workloadIdentityPools/github-pool/attribute.repository/your-github-username/your-repo-name"


  Need to create deploy.yaml file

  .github/workflows/deploy.yaml - format for creating yaml file

  In deploy.yaml file we need to replace the project-id, service-account email, repo-name, WIF pool, WIF provider then click on commit changes.
  Once we click on commit changes.

  There will be Action running. Now you ca go to the Actions tab and check it.



  

