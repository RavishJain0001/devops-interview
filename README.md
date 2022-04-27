## DevOps Engineer - technical interview - Solution
This file describes the solution for creating an Automated CI/CD for a simple weather app as provided as part of the technical interview. 

### Key Considerations
- A new workflow has been added that builds and then deploys the solution on Azure
- A couple of Integration tests have been added to the solution to show how tests will be run as part of the CI/CD. 
- Azure App services have been used for hosting the .net core website. An ARM template has been added to the solution to deploy the Azure App Service. 
- As part of the CD, 4 environments will be provisioned i.e. Dev, Test, Uat and Prod. 
- App Service staging slots have been used to ensure zero down time. 
- GitHub flow has been used as the branching strategy. GitHub branch policies have been configured accordingly. 
- Any secret value has been stored as part of GitHub secrets. 
- Static Analysis has been done using CodeQL.
- Codecov is used to upload code coverage and display as part of PR

### Branch Policies
- Main is locked for direct commit. Any commit to main can only be done via a PR.
- All checks in PR must pass successfully for merge to main. 

### Tests
A couple of tests has been added as part of test project which mainly tests the two core features of the web app i.e. Location search and Weather forecast. 

### Configuration Management
- All secure configurations have been stored as part of GitHub secrets. 
- 4 Environments have been defined and corresponding configurations have been defined as environment scoped secrets. 
- Azure service principal is used for Infra and App deployment and is stored as a secure configuration. 

### CI Features
CI Consists of a combination of actions as explained below - 
- As part of the CI, workflow will build the .net core solution using MSBuild by restoring the nugget packages first and then running the build. 
- It will run the Integrations tests defined in the solution and generate code coverage report. Code coverage report will be uploaded to Codecov and can be viewed as part of PR check. 
- Static Analysis has been done using CodeQL default policies. 
- Artifacts will be published as part of CI Job which then can be consumed by CD Jobs. 
- On every main commit, a new tag and a release will be added in GitHub main repo.

### CD Features
#### IaC
- Azure Web App ARM template is defined as IaC. 
- Azure App service Plan ARM is deployed manually for limiting the scope of this solution. 

#### Environments
- 4 environments are provisioned as part of CD jobs i.e. Dev, Test, Uat and Prod. Except Dev, all environments will require approval. 
- Dev and Test will be deployed from Feature branches to run automated tests as well as to ensure fully tested code is moved to main. These environments will also be deployed as part of PR checks.
- Named environments need not be persisted and can be deleted if only Automated testing is being done or can be deleted/created via a scheduled job for manual testing. This has been kept out of scope as part of this solution. 
- All environments will be deployed with main branch. 

![CI-CD Workflow](Docs/cicd.png)

#### Application Deployment
Artifacts created in build job will be used to deploy app to the Azure web app. 

### Pull Requests
As part of PR, 
- Application will be deployed to Dev and Test environments. Any build/test/deployment failure will fail PR.
- Code coverage is added as one of the checks. If code coverage goes down from the previous commit, PR check will fail. 
- CodeQL Static Analysis is added as one of the checks. 

![CI-CD Workflow](Docs/pr.png)

### Environment URLs
Dev - https://fsptest-dev.azurewebsites.net/
Test - https://fsptest-test.azurewebsites.net/
UAT - https://fsptest-uat.azurewebsites.net/
PROD - https://fsptest-prod.azurewebsites.net/



