This tutorial will walk you through using Google Container Builder as a way to "bake" VMs using packer + of the popular configuration management systems.

Assumptions:
  * You have a GCP account
  * You are starting with a new project
  * You know how to use / install [gcloud](https://cloud.google.com/sdk/gcloud/)


In this tutorial you will do the following
1. Enable Container builder and Compute engine on your project.
2. Modify the roles of the container builder service account to enable GCE access for Container Builder on your behalf.
3. Build and publish a build step for packer in your project.
4. Create a build that bakes a VM image using a configuration management tool.


# Enable APIs and set up roles.
```
export PROJECT=$(gcloud info --format='value(config.project)')
export PROJECT_NUMBER=$(gcloud projects describe $PROJECT --format 'value(projectNumber)')
export CB_SA_EMAIL=$PROJECT_NUMBER@cloudbuild.gserviceaccount.com
gcloud services enable cloudbuild.googleapis.com
gcloud services enable compute.googleapis.com
gcloud projects add-iam-policy-binding $PROJECT --member=serviceAccount:$CB_SA_EMAIL --role='roles/iam.serviceAccountUser' --role='roles/compute.instanceAdmin.v1' --role='roles/iam.serviceAccountActor'
```

# Build the packer buildstep (from within the packer directory)

```
cd packer_buildstep
gcloud container builds submit --config cloudbuild.yaml .
```

# Kick off a bake via GCB with Ansible.

```
cd ansible
gcloud container builds submit --config cloudbuild.yaml .
```

