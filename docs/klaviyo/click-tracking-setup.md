# Klaviyo Dedicated Click Tracking Setup Guide

Klaviyo wraps email links with tracking URLs to monitor click engagement. While this provides valuable analytics, it can hide your actual deep links behind generic tracking domains. This guide shows you how to set up dedicated click tracking to maintain analytics while preserving your brand's domain visibility and improving user trust and deeplinking.

This comprehensive guide walks you through setting up dedicated click tracking in Klaviyo to improve email deliverability and brand trust. Based on the official [Klaviyo documentation](https://help.klaviyo.com/hc/en-us/articles/360001550572#h_01HQ3KN3Y0W255N0NZE5WE2RM6).

## What is Dedicated Click Tracking?

Dedicated click tracking allows you to display your own domain on click tracking links instead of the default Klaviyo encoding. This means your customers will see your brand's domain when hovering over links in your emails rather than a long string of letters and numbers from Klaviyo.


## Setup

For more control or if automatic setup isn't available, you can manually configure dedicated click tracking.

#### Step 1: Add DNS Records

Add the following CNAME record to your DNS settings:

| Type  | Hostname/Name | Value              |
|-------|---------------|-------------------|
| CNAME | trk           | dct.klaviyodns.com |

#### Step 2: DNS Provider Configuration

The exact field names may vary depending on your DNS provider:
- Some providers use "Hostname", others use "Name"
- Some use "Value", others use "Points to" or "Target"
- The actual values you enter remain the same

#### Step 3: Disable Proxying (Important)

If your DNS provider offers proxying (like Cloudflare), **you must disable it** for the click tracking records. Proxying will prevent proper setup and validation.

#### Step 4: Contact Klaviyo Support

After updating your DNS records, contact Klaviyo support from your account to validate the records.

## SSL Certificate Setup

SSL certificates are **highly recommended** for dedicated click tracking domains to ensure:
- Secure HTTPS connections
- Increased customer trust
- Protection against security warnings

### Automatic SSL Provisioning

Klaviyo automatically generates SSL certificates when:
- Click tracking domain is set up dynamically by Klaviyo
- Manual setup points to `klaviyodns.com` domain
- CAA records are properly configured (if applicable)

### CAA Records Configuration

If your domain has CAA (Certification Authority Authorization) records, you must include this property:

| Type | Domain Name | Value            |
|------|-------------|------------------|
| CAA  | example.com | 0 issue pki.goog |

This allows Klaviyo to generate SSL certificates for your subdomain.

 # Testing

 Send yourself a test mail with a deeplink. See if app opens.