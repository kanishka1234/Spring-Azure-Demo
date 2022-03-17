# Spring-Azure-Demo
You’re now in the production slot, which is not recommended for setting up CI/CD.Learn more
Deploy and build code from your preferred source and build provider. Learn more

Source*
Building with GitHub Actions.Change provider.
GitHub
App Service will place a GitHub Actions workflow in your chosen repository to build and deploy your app whenever there is a commit on the chosen branch. If you can't find an organization or repository, you may need to enable additional permissions on GitHub. Learn more

Signed in as
kanishka1234Change Account
In order to sign in with a different account, please go to www.github.com and log out of the account currently signed into GitHub.
Organization*
kanishka1234
Repository*
Spring-Azure-Demo
Branch*
main
Build
Runtime stack
Java
Version
8.0
Java web server stack
Java SE
Workflow Configuration
File with the workflow configuration defined by the settings above.


Workflow Configuration
File path: .github/workflows/main_SpringBootDem.yml

If an existing workflow configuration exists, it will be overwritten.
# Docs for the Azure Web Apps Deploy action: https://github.com/Azure/webapps-deploy
# More GitHub Actions for Azure: https://github.com/Azure/actions

name: Build and deploy JAR app to Azure Web App - SpringBootDem

on:
  push:
    branches:
      - main
  workflow_dispatch:

jobs:
  build:
    runs-on: windows-latest

    steps:
      - uses: actions/checkout@v2

      - name: Set up Java version
        uses: actions/setup-java@v1
        with:
          java-version: '8'

      - name: Build with Maven
        run: mvn clean install

      - name: Upload artifact for deployment job
        uses: actions/upload-artifact@v2
        with:
          name: java-app
          path: '${{ github.workspace }}/target/*.jar'

  deploy:
    runs-on: windows-latest
    needs: build
    environment:
      name: 'production'
      url: ${{ steps.deploy-to-webapp.outputs.webapp-url }}

    steps:
      - name: Download artifact from build job
        uses: actions/download-artifact@v2
        with:
          name: java-app

      - name: Deploy to Azure Web App
        id: deploy-to-webapp
        uses: azure/webapps-deploy@v2
        with:
          app-name: 'SpringBootDem'
          slot-name: 'production'
          publish-profile: ${{ secrets.AzureAppService_PublishProfile_d3a189fea8cc46d8b8b04affc8fa51cf }}
          package: '*.jar'
