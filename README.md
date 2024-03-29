# fashion-ecommerce
[Website Layout.pdf](https://github.com/Jayavarapu-Harshitha/fashion-ecommerce/files/8812414/Website.Layout.pdf)


demo url:
https://youtu.be/_8hyQirA4UQ

Services Used:
1. Azure app service
2. Azure Database for PostgreSQL

Project Description:

A mini ecommerce website where people can see various styles of dresses for all kind of ages . They can add their items into the cart and checkout for the payment. The orders section contains each an every order and the product information with the checkout option where customers can pay through Qr code scanner.

# Docs for the Azure Web Apps Deploy action: https://github.com/Azure/webapps-deploy
# More GitHub Actions for Azure: https://github.com/Azure/actions
# More info on Python, GitHub Actions, and Azure App Service: https://aka.ms/python-webapps-actions

name: Build and deploy Python app to Azure Web App - kluecommerce

on:
  push:
    branches:
      - master
  workflow_dispatch:

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v2

      - name: Set up Python version
        uses: actions/setup-python@v1
        with:
          python-version: '3.9'

      - name: Create and start virtual environment
        run: |
          python -m venv venv
          source venv/bin/activate
      
      - name: Install dependencies
        run: pip install -r requirements.txt
        
      # Optional: Add step to run tests here (PyTest, Django test suites, etc.)
      
      - name: Upload artifact for deployment jobs
        uses: actions/upload-artifact@v2
        with:
          name: python-app
          path: |
            . 
            !venv/

  deploy:
    runs-on: ubuntu-latest
    needs: build
    environment:
      name: 'Production'
      url: ${{ steps.deploy-to-webapp.outputs.webapp-url }}

    steps:
      - name: Download artifact from build job
        uses: actions/download-artifact@v2
        with:
          name: python-app
          path: .
          
      - name: 'Deploy to Azure Web App'
        uses: azure/webapps-deploy@v2
        id: deploy-to-webapp
        with:
          app-name: 'kluecommerce'
          slot-name: 'Production'
          publish-profile: ${{ secrets.AZUREAPPSERVICE_PUBLISHPROFILE_EA8B93C0DF2D4C84AE61AD0CF878965C }}
