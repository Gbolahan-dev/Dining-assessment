 Inu Dining Frontend (inu-dining-assessment)

This repository contains the Next.js (static export) frontend for the Inu Dining application.

It is automatically built by a Google Cloud Build pipeline and hosted as a static website on Google Cloud Storage, served via a global Google Cloud Load Balancer.

 🚀 Core Features

* Automated Deployment: Every `git push` to `main` automatically builds the static site and deploys it to Google Cloud Storage (GCS).
* Global CDN & SSL: The GCS bucket is served by a Google Cloud Load Balancer, providing a global CDN, caching, and a Google-managed SSL certificate.
* Full-Stack Application: Connects to the secure backend API at `https://inu-dining-api.duckdns.org`.

 🛠️ Tech Stack

* Framework: Next.js (with Static Export: `next build && next export`)
* CI/CD: Google Cloud Build
* Hosting: Google Cloud Storage (GCS)
* Networking: Google Cloud Load Balancer (GCLB)
* DNS: DuckDNS

## 🏛️ System Architecture

This repository's `cloudbuild.yaml` file defines a simple two-step CI/CD pipeline:

1.  `npm ci` & `next build && next export`: Installs all dependencies and builds the static frontend into the `/out` directory. The backend API URL (`https://inu-dining-api.duckdns.org`) is baked into the code at this step via the `_API_URL` substitution variable in the trigger.

2.  `gsutil rsync -r out/ gs://inu-dining-frontend`: This command (with the critical trailing slash `/`) syncs the *contents* of the `/out` directory to the root of the `inu-dining-frontend` GCS bucket.

 Important: Infrastructure Management

The infrastructure for this frontend (the GCS bucket, the Load Balancer, and the SSL certificate) is not managed in this repository.

It is managed by the `inu-dining-backend` project's Terraform code, specifically in the `gcs_frontend_lb.tf` file. This centralizes all project infrastructure into a single Terraform state.
