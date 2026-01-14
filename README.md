############################################################################
## karthik's static-gcp-website
############################################################################

Google Cloud Storage(GCS) is used to host a static website on Google Cloud Platform(GCP).

## Overview
This project shows how to use Google Cloud Storage to set up a basic static website on Google Cloud Platform.

## Justification
Google Cloud Storage was chosen for this implementation because it is:
- Cost-effective
- Highly scalable
- Secure and reliable
- Easy to manage
- Well suited for static content hosting

## Pre-requisites
- I set up a free trial Google Cloud account that offered $300 in credit points.
- I haven't installed any extra components, set up or changed any organisations, folders, or project structures, or set up any VPCs or networks.

## How to use GCP Console to create a static website in GCP using GCS
## 1. Create a Cloud Storage Bucket
- Navigate to Google Cloud Console --> Cloud Storage --> Buckets --> Click "Create Bucket"
- A few configurations are required under the Create Bucket section, such as globally unique bucket names. I've set up unique_bucket_name as the GCS Bucket Name.
- Under "Location Type." In this instance, I've chosen the Australia-Southeast1 (Sydney) area.
- I've unchecked the "Public Access Prevention" section.
- The other configuration remains the same.
Note: Public access is enabled for demonstration purposes only and is not recommended for production environments.

## 2. Create a index file with a text "This is <<Your Name>> Website"
For reference, please see the index.html file that is attached.

## 3. Upload the file "index.html" to the GCS bucket
- Go to Google Cloud Console --> Cloud Storage --> Buckets --> Click "unique_bucket_name" (The GCS bucket that we've created in step 1)
- Click upload files --> Upload the index.html file that we've created in step 2

### 4. Make the Website publicly accessible
- Navigate to Google Cloud Console --> Cloud Storage --> Buckets --> Click "unique_bucket_name" (The GCS bucket that we've created in step 1)
- Select the check box beside the index.html file that we've uploaded in Step 3, and then go to "Permissions" tab
- Navigate to Permissions --> Grant Access --> In "Add Principals", enter "allUsers" --> In Assign Roles, enter "Storage Object Viewer" --> Save the changes
- This makes the website accessible to the general public in a read-only manner.

### 5. Verify the Website's access
- Refresh the GCP Console
- Navigate to Google Cloud Console --> Cloud Storage --> Buckets --> Click "unique_bucket_name" (The GCS bucket that we've created in step 1) --> Click on the index.html that we've created in step 2 --> we'll find a Public URL, visit the link and verify the webpage.

###################################################################
## What can be done with more time
###################################################################

- I would concentrate on enhancing the website's security, automation, and maintainability if I had more time.
- Firstly, I would use Cloud DNS to set up a custom domain rather than depending on the default Cloud Storage URL. I will configure HTTPS via a Google-managed SSL certificate on an HTTPS load balancer to ensure secure communication.
- After that, I would incorporate a CI/CD pipeline to ensure that website modifications are automatically implemented. Every time a change is merged into the main branch, for instance, I could use GitHub Actions or Cloud Build to publish changed static files to the GCS bucket. This reduces manual effort and minimizes the chance of deployment errors.
- I would enable Access Logging and Cloud Monitoring uptime checks to alert the team if the site experiences downtime or unusual activity.
- I would use Git/Bitbucket, & Terraform (Infrastructure as Code - IaC) to codify the whole environment. This guarantees that the creation of GCP projects, buckets, configurations, and permissions are version-controlled and can be quickly audited or replicated.

###################################################################
## Alternative Solutions
###################################################################

Before choosing Google Cloud Storage, I assessed the following GCP services.

## Firebase Hosting
Firebase Hosting provides fast and secure hosting for web apps. It has built-in support for CDNs and HTTPS, and Google takes care of content delivery and SSL certificate administration. For static websites, this greatly lowers operational overhead. Although Firebase Hosting is an excellent choice, it was not chosen for this purpose in order to show how to leverage fundamental GCP features like Cloud Storage and IAM directly instead of depending on a higher-level managed platform.

## App Engine 
It is a Platform as a Service (PaaS) that allows applications to be deployed without managing the underlying infrastructure. It is well suited for dynamic applications that require a managed runtime. For this use case, which involves serving a simple static website with minimal content, the additional runtime configuration and deployment complexity does not provide any meaningful benefit. Developers creating dynamic applications would find App Engine especially helpful because it manages infrastructure and scales automatically.

## Cloud Run
Cloud Run allows us to build and deploy applications or websites quickly on a fully managed platform. However, it is over-engineered for a single HTML file. Unlike Cloud Storage, which serves content instantly, Cloud Run introduces cold-start latency and requires managing a Dockerfile and storing container images in Artifact Registry. Cloud Run is better suited for dynamic web applications or APIs, but not for a static website.

###################################################################
## Production-grade - Multi-team Environment
###################################################################

## 1. Infrastructure as Code (IaC)
I will use Terraform to define and manage the infrastructure, with code stored in a version-controlled repository such as Bitbucket or GitHub. This allows teams to review and approve changes via pull requests, ensuring deployments are controlled, repeatable, and auditable.

## 2. Multi-Environment Strategy (Dev/Staging/Prod)
I would isolate environments at 3 levels: repository, Terraform workspace, and GCP project.
- Development Project: Developers can safely experiment with scripts and infrastructure changes in this project.
- Staging Project: A pre-production environment that duplicates production, allowing thorough testing before deployment.
- Production Project: The live environment where real users access the website, secured with strict access controls.

## 3. Continuous Integration/Continuous Delivery (CI/CD)
I will replace manual uploads with a CI/CD procedure that uses Google Cloud Build or GitHub Actions.
- Every time code is merged into the main branch, I will ensure that the most recent modifications are automatically deployed to the Cloud Storage bucket.
- Before upgrading the live site, I will set up pre-deployment validations, such as HTML integrity checks and security scans.

## 4. Global LB, CDN, & Security
I would set up an external HTTPS load balancer with Cloud CDN configured behind the Cloud Storage bucket. This reduces latency globally by serving content from Google’s edge locations. Additionally, I would enable Cloud Armor to give WAF protection against common web dangers and DDoS attacks, and use Google-managed SSL certificates to enforce HTTPS.

## 5. Monitoring & Logging
I will enable Cloud Logging on the load balancer and GCS bucket to record security and access events. Additionally, I'll utilise Cloud Monitoring to monitor traffic patterns, uptime, and performance. I'll also set up alerts for unusual activity, high latency, and outages.

