# YAML Pipelines for Optimizely 12+ / .NET 5+ Deployments
Reusable multi-stage YAML pipelines to setup CI/CD for Optimizely 12+ DXP deployments using Deployment API. 

Multi-stage pipelines allow for combined build and deploy processes in a single pipeline, providing a unified view of your deployment pipeline.

The pipelines are ideal for projects using a trunk based branching strategy, however the triggers can be tweaked to work with other branching models.

There are 2 pipelines, `Integration` and `Release`, which allows for concurrent development of consecutive releases workflows.

## Continuous Integration

The `Integration` pipeline supports direct deploy to the Integration environment using the Deployment API code package approach. This is the recommended approach to deploying to Integration.

The Integration pipeline is triggered when code is merged to `master` and performs a build, test and deploy to the Integration environment.

## Release

The `Release` pipeline is used to deploy planned releases and applys the following Release workflow to deploy to Preproduction and Production environments:
- is triggered on any change to `release/*` branch. Use consistent naming convention for release branch name e.g. `release/1.0` for deployment history purposes.
- creates a Nuget package and uploads it to the Optimizely DXP for deployment. The package name will be in the format _[app name].cms.app.<releasebranchname.buildnumber.revision>.nupkg_, e.g. customer.cms.app.1.0.20200527.1.nupkg
- allows for staged code deployments to Preproduction and Production environments (upon approval) 
- supports Smooth deployments / Zero downtime deployments
- allows for manual validation after Production slot deployment
- supports deployment of urgent fixes straight to Production, if required (by skipping Preproduction stage) 

## Variable Groups

Create variable groups 'dxp-inte' and 'dxp-release' for the 2 pipelines respectively, with the following variables (Deployment API credentials can be generated from the DXP Portal):
- ProjectId
- ApiKey
- ApiSecret (secret variable)

## Environments

Deployments history can be viewed from the _Pipelines -> Environments_ page on Azure DevOps. The deployed version <releasebranchname.buildnumber.revision> will be displayed against the respective environments.

Approvals can be setup on the Preproduction and Production environments to ensure approvers grant approval before deployment to the respective environment. 

## Limitations

- The pipelines currently only creates and deploys CMS code package however can be easily extended for Commerce packages


