# Emblem Quickstart

## Introduction

This tutorial shows you how to run a setup script to deploy the Emblem application to Cloud run, configure authentication, initiate CI/CD, and explore operations monitoring.

[...]: # (TODO - Describe outcomes of tutorial in more detail.)

## Fork and clone Emblem

1.  Open Cloud Shell by clicking <walkthrough-cloud-shell-icon></walkthrough-cloud-shell-icon><walkthrough-spotlight-pointer spotlightId="devshell-activate-button">Activate Cloud Shell</walkthrough-spotlight-pointer>.

2.  Clone the Emblem application and cd into the `emblem` directory by clicking ![Copy to Cloud Shell](https://walkthroughs.googleusercontent.com/content/demo/images/copybutton.png) **Copy to Cloud Shell**, to the right of the following command, and then pressing `Enter`:

  ```bash
  git clone https://github.com/GoogleCloudPlatform/emblem.git
  cd emblem
  ```

The Emblem serverless application will be cloned to your local Cloud Shell instance.

[...]: # (TODO - add descriptive "click **Next**" instruction)

## Login
Set your application default credentials by running the command below in the terminal and logging in with your Google account:

```bash
gcloud auth application-default login
```

If you encounter auth errors, you may need to unset your `GOOGLE_APPLICATION_CREDENTIALS` in your terminal.

## Set projects

Emblem uses three projects: `prod`, `stage`, and `ops`. 

[...]: # (TODO - explain why we use 3 projects)

You can use existing Google Cloud projects or create new projects. Choose either **[Select existing projects](#select-existing-projects)** or **[Create new projects](create-new-projects)** below and follow the instructions.

---

### Select existing projects

Emblem uses Cloud Firestore in Native mode as the database for each project. Since the selected mode is permanent for a project, make sure that your existing projects are not already using Datastore mode by checking the configuration in the [Firestore console](https://console.cloud.google.com/firestore).

1. Set the project variables in your Cloud Shell terminal. Replace each `<prod>`, `<stage>`, and `<ops>` value with the corresponding project ID for your `prod`, `stage`, and `ops` projects.

  ```bash
  export PROD_PROJECT=<prod>
  ```
  ```bash
  export STAGE_PROJECT=<stage>
  ```
  ```bash
  export OPS_PROJECT=<ops>
  ```

---

### Create projects automatically
1. Set your billing account. (You can view your existing billing accounts by running `gcloud alpha billing accounts list` in the terminal.)
  ```bash
  export BILLING_ACCOUNT_NAME="My Billing Account"
  export EMBLEM_BILLING_ACCOUNT=$(gcloud alpha billing accounts list --filter "$BILLING_ACCOUNT_NAME" --format "value(name)")
  ```

2. Copy the commands below into your terminal to create new `prod`, `stage`, and `ops` projects.
  ```bash
  export PREFIX=$(gcloud config get-value account | cut -f1 -d"@")-emblem
  export PROD_PROJECT=$PREFIX-prod
  export STAGE_PROJECT=$PREFIX-stage
  export OPS_PROJECT=$PREFIX-ops

  gcloud projects create $PROD_PROJECT --billing-account $EMBLEM_BILLING_ACCOUNT
  gcloud projects create $STAGE_PROJECT --billing-account $EMBLEM_BILLING_ACCOUNT
  gcloud projects create $OPS_PROJECT --billing-account $EMBLEM_BILLING_ACCOUNT
  ```

---

## Run setup script

Run the following command in the terminal:
```bash
sh setup.sh
```

The script enables all necessary APIs, creates service accounts, deploys Emblem to Cloud Run, creates Cloud Build triggers,

Watch the terminal as Terraform builds and deploys the Emblem pipeline.

If you are prompted to enable any APIs for your projects, enter "y" in the terminal to enable them.

If you are prompted to select a region for the Cloud Run deployments, enter the number of the region closest to you. 

[...]: # (DEV_TODO: - add --region or --zone tag to the setup.sh gcloud commands to bypass this step.)

## Configure end-user authentication

The `setup.sh` script includes command line instructions to configure end-user authentication.

When you receive the prompt `"Would you like to configure end-user authentication?"`, enter `y` to proceed with the instructions.

If you choose not to configure end-user auth right now, you can always return to the command line instructions by running the [configure_auth.sh](./scripts/configure_auth.sh) script in the terminal:

```bash
sh ./scripts/configure_auth.sh
```

## Connect Cloud Build triggers

When you receive the prompt below:
`Connect your repos: https://console.cloud.google.com/cloud-build/triggers/connect?project=$OPS_PROJECT`

Click the link provided in the prompt and follow the instructions to connect your repository. 

Once your repo has been connected, click **Done**, then return to the Cloud Shell terminal and press any key to continue. You will be prompted to enter the `repo owner` and `repo name`; enter the values from your repository and confirm them by pressing `y`.

## Success!

Your Emblem application is now running!

### Next steps
Test the delivery pipeline by pushing a change to your Emblem repo. Check out the [Cloud Build dashboard](https://console.cloud.google.com/cloud-build/builds?authuser=3&project=$OPS_PROJECT).

[...]: # (DEV TODO - Print out links to application and relevant console items like Cloud Build & Logging in the terminal, which include the correct user projects.)


