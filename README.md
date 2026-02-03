# S3 + CloudFront Static Website (Terraform)

This Terraform config provisions a static website hosted in S3 and served through CloudFront with a custom domain and HTTPS.

## What It Creates

- An S3 bucket to store static site assets
- A CloudFront distribution with Origin Access Control (OAC) to read from S3 privately
- An ACM certificate in us-east-1 (required for CloudFront) validated via Route 53 DNS
- A Route 53 hosted zone  for the domain and an A record pointing the root domain to CloudFront

## Prerequisites

- Terraform installed
- AWS credentials configured (for example via `AWS_ACCESS_KEY_ID` / `AWS_SECRET_ACCESS_KEY` or AWS CLI profile)
- A domain you control (this config uses `kvbengodev.cloud`)

## Quick Start

1. Review and update these values in [main.tf](file:///c:/Users/Asus/Desktop/TerrformAWSProjects/S3_CloudFront/main.tf):
   - `domain_name` / `aliases` / Route53 zone name
   - S3 bucket name (must be globally unique)

2. Initialize and deploy:

   ```bash
   terraform init
   terraform fmt
   terraform validate
   terraform apply
   ```

3. Point your domain registrar to Route 53:
   - Terraform outputs `nameservers`
   - Set those name servers at your registrar (GoDaddy, Namecheap, etc.)

4. Upload your static site to S3:

   ```bash
   aws s3 sync ./site s3://s3-gibs-personal-portfolio
   ```

## Notes

- CloudFront distributions can take several minutes to deploy changes.
- HTTPS works only after the ACM certificate is issued and CloudFront finishes deployment.
- The S3 bucket is blocked from public access; CloudFront is the only intended reader via the bucket policy.
# S3_Portfolio_Web
Terraform IaC to deploy a secure static website on AWS: S3 for assets, CloudFront with Origin Access Control (OAC), HTTPS via ACM (us-east-1) DNS-validated in Route 53, plus hosted zone and records for a custom domain. Repeatable deploy + upload workflow.
