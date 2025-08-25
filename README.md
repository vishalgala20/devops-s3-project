🌊 DevOps S3 Website Project

This project demonstrates hosting a static website on AWS S3 with a CI/CD pipeline using GitHub Actions.
Every time you push code to GitHub, your website automatically updates!

📌 Features

Static website hosted on Amazon S3.

Automated deployment using GitHub Actions.

Environment variables securely stored in GitHub Secrets.

Example website: Beach Scenario 🌴🌊

🛠️ Setup Steps
1. Clone the repository
git clone https://github.com/<your-username>/devops-s3-project.git
cd devops-s3-project

2. Create an S3 Bucket
aws s3 mb s3://<your-bucket-name> --region ap-south-1


Enable static hosting on your bucket:

aws s3 website s3://<your-bucket-name>/ --index-document index.html --error-document index.html


Update bucket policy to allow public read (if required).

3. Add AWS Credentials to GitHub Secrets

Go to GitHub → Repo Settings → Secrets and Variables → Actions → New Repository Secret
Add the following:

AWS_ACCESS_KEY_ID

AWS_SECRET_ACCESS_KEY

AWS_REGION → ap-south-1

S3_BUCKET → your bucket name

4. GitHub Actions Workflow

.github/workflows/deploy.yml

name: Deploy to S3

on:
  push:
    branches:
      - main

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout repository
        uses: actions/checkout@v3

      - name: Configure AWS credentials
        uses: aws-actions/configure-aws-credentials@v4
        with:
          aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
          aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
          aws-region: ${{ secrets.AWS_REGION }}

      - name: Deploy to S3
        run: aws s3 sync . s3://${{ secrets.S3_BUCKET }} --delete

5. Test the Pipeline

Modify your index.html (e.g., update beach background 🌴).

Commit and push:

git add .
git commit -m "Updated website with beach background"
git push origin main


GitHub Actions will run automatically → your site updates in S3.

🌍 Access Your Website

Your site will be available at:

http://<your-bucket-name>.s3-website-ap-south-1.amazonaws.com

📖 Learning Outcomes

Hosting websites on AWS S3.

Automating deployments with GitHub Actions.

Managing secrets in GitHub for security.

Building a simple CI/CD pipeline.
