# GitHub Actions

This repository contains a proof of concept for a set of GitHub Actions that can be used to promote a deployment from one environment to another.

## Environments

The workflows defined in this repository manage deployment to three environments:
1. Test - Deployments to the test environment are done automatically when a PR is merged into the `main` branch.
2. Staging - Deployments to the staging environment are done manually by executing the _Promote test to staging_ workflow from the GitHub actions UI.
3. Production - Deployments to the production environment are done manually by executing the _Promote staging to production_ workflow from the GitHub actions UI. This step requires a manual approval because of the _Deployment protection rules_ defined in the GitHub repository settings.

Deploying to the staging and production environments default to a promotion from the preceding environment; test gets promoted to staging and staging gets promoted to production. When executing either of these workflows, the user can specify a tag to deploy, if omitted, the tag from the preceding environment is used.

## Deployment tags

These workflows assume no external versioning system is used. The idea is that the build phase will produce a container image using the current git SHA as the image tag. This SHA is used to identify the deployment throughout the deployment workflows.

The deployment directory contains a file for each environment that contains the SHA of the last deployment to that environment. These files get updated by the promotion workflows when a deployment to an environment is successful.

## Placeholder functionality

Being a simple proof of concept, the workflows are not complete. They are include the following placeholders:
1. In the `build-and-push.yaml` workflow, the job simply echos a message. In a proper implementation, the job would build the container image and push it to the container registry using the current git SHA as the image tag.
2. In the `_promote.yaml` workflow, the job simply echos a message. In a proper implementation, the job would deploy the container image to the target environment.