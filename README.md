# cloudfront-cdn-optimization

# Global CDN Optimization (AWS CloudFront)

![Project Overview](images/cdn-card.png)

## Overview

This project demonstrates how to optimize a static website using **Amazon CloudFront**, AWS’s Content Delivery Network (CDN). By caching assets at edge locations worldwide, we significantly reduce latency, accelerate page load times, and provide secure HTTPS delivery for end users.

## Objectives

* **Enable HTTPS** between clients and origin servers
* **Accelerate content delivery** through global caching
* **Measure performance gains** before and after CDN integration

## Key Features

* **Global Edge Network**: Caches content at AWS edge locations across the globe
* **Automatic HTTPS/TLS**: Secure connections at no extra cost
* **Customizable Cache Policies**: Fine-tune TTLs and behavior per resource
* **Built‑In Protection**: Integrates with AWS Shield for DDoS mitigation and AWS WAF for application-layer security

## Architecture

```text
End User → CloudFront Edge Location → S3 Static Website (Origin)
```

## Prerequisites

* An **AWS account** with permissions for CloudFront and S3
* A **static website** already hosted in an S3 bucket (public website hosting enabled)
* **AWS CLI** installed (optional, for scripted deployments)
* A modern **web browser** with Developer Tools for performance testing

## Repository Structure

```bash
├── images/                       # Visual assets for this README
│   ├── pre-cdn-load.png          # Baseline performance test
│   ├── create-distribution.png   # CloudFront creation UI
│   ├── origin-config.png         # S3 origin configuration
│   ├── disable-waf.png           # Optional WAF settings
│   ├── distribution-status.png   # Distribution deployment status
│   ├── post-cdn-first-load.png   # First load via CDN
│   └── post-cdn-cached-load.png  # Subsequent cached load
└── README.md                     # Project documentation
```

## Setup & Deployment

### 1. Baseline Performance Test

1. Open your site’s URL (e.g., `http://your-bucket.s3-website-us-east-2.amazonaws.com`).
2. In Developer Tools → Network, enable **Disable cache** and reload.
3. Record the longest resource load time for comparison.

![Baseline Load Test](images/pre-cdn-load.png)

### 2. Create a CloudFront Distribution

1. Log into the AWS Console and navigate to **CloudFront → Create distribution**.
2. Select the **Web** delivery method.
3. Under **Origin settings**, enter your S3 static website endpoint as the Origin Domain Name.

![Create Distribution](images/create-distribution.png)
![Origin Configuration](images/origin-config.png)

### 3. Configure Distribution

* **Cache Policies**: Use the managed `CachingOptimized` policy or define custom TTLs.
* **Security**: Disable AWS WAF if you’re not using custom rules (optional). Otherwise, attach your WAF Web ACL.

![WAF Settings](images/disable-waf.png)

### 4. Deploy & Validate

1. Click **Create distribution**. Wait for the status to become **Deployed** (may take \~10–20 minutes).
2. Note the **Domain Name** (e.g., `d1234.cloudfront.net`).

![Distribution Status](images/distribution-status.png)

### 5. Post-Deployment Testing

1. Visit the CloudFront URL (e.g., `https://d1234.cloudfront.net`).
2. In DevTools, **Disable cache** and reload to measure the first load via CDN.
3. Reload again to measure the cached response time.

![First CDN Load](images/post-cdn-first-load.png)
![Cached Load](images/post-cdn-cached-load.png)

## Performance Results

| Test Scenario          | Load Time (ms) |
| ---------------------- | -------------: |
| Pre-CDN (first load)   |          `XXX` |
| Post-CDN (first load)  |          `YYY` |
| Post-CDN (cached load) |          `ZZZ` |

These metrics illustrate the dramatic improvement in delivery speed once assets are served from the nearest edge location.

## Next Steps

* **Custom Domain**: Use AWS Certificate Manager to attach an SSL certificate to a custom domain.
* **Advanced Caching**: Implement cache invalidation and versioning for dynamic content.
* **Security Hardening**: Enforce AWS WAF rules, origin access identity, and geo-restrictions.

