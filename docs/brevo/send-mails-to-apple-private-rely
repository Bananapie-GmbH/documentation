# Sending Emails to Apple Private Relay via Brevo

## Overview

Apple Private Relay is a service that allows users to sign in with Apple using a private email address that forwards to their actual email. When sending emails to these private relay addresses, you need to properly configure your email service provider (ESP) to authenticate with Apple's relay service.

This guide covers how to configure Brevo (formerly Sendinblue) to successfully deliver emails to Apple Private Relay addresses.

## Prerequisites

- Access to your domain's DNS settings
- An Apple Developer account (individual or organization)
- A Brevo account configured for your domain
- Domain ownership verification completed

## Step 1: Configure DNS SPF Record

You need to add a Sender Policy Framework (SPF) record to your domain's DNS settings to authenticate your emails with Apple's private relay service.

### SPF Record Configuration

Add the following TXT record to your domain's DNS settings:

```
v=spf1 include:_spf.google.com include:spf.brevo.com mx ~all
```

**Record Details:**
- **Type**: TXT
- **Name**: @ (or your domain name)
- **Value**: `v=spf1 include:_spf.google.com include:spf.brevo.com mx ~all`

### Why This SPF Record?

- `include:_spf.google.com`: Authorizes Google's mail servers (if using Google Workspace)
- `include:spf.brevo.com`: Authorizes Brevo's mail servers to send on behalf of your domain
- `mx`: Allows your domain's MX record servers to send emails
- `~all`: Soft fail for all other servers (recommended for compatibility)

## Step 2: Configure Apple Developer Account

### Register Your Domain

1. Go to [Apple Developer Account - Services Configuration](https://developer.apple.com/account/resources/services/configure)
2. Click on **"Configure"** under **"Sign in with Apple for Email Communication"**
3. In the **Email Sources** section, click the **add button (+)**
4. Enter your domain(s) that will be used for email communication
5. Click **Next** and then **Register**
6. Verify that your domain passes the SPF check in the displayed table

### Register Email Addresses (Optional)

If you want to register specific email addresses instead of entire domains:

1. In the same **Email Sources** section, click the **add button (+)**
2. Enter a comma-delimited list of email addresses that will send emails
3. Click **Next** and then **Register**
4. Verify that the email source domain passes the SPF check

## Step 3: Configure Brevo

### Domain Authentication

1. Log into your Brevo account
2. Go to **Settings** > **Senders & IP**
3. Add your domain and complete the authentication process
4. Ensure your domain is verified and authenticated

### DKIM Configuration (Recommended)

For additional security, configure DKIM authentication:

1. In Brevo, go to **Settings** > **Senders & IP**
2. Enable DKIM for your domain
3. Add the provided DKIM DNS record to your domain's DNS settings
4. Verify DKIM is properly configured

## Authentication Requirements

Apple Private Relay requires proper email authentication using either:

### SPF Authentication
- The envelope sender domain must be registered in Apple Developer account
- Must pass SPF validation
- Registered domain and envelope sender domain must match exactly

### DKIM Authentication
- Email must be signed with DKIM
- DKIM domain must match the From: address domain
- DKIM signature must include the From: address
- Both domains must be registered in Apple Developer account

## Best Practices

### Email Configuration
- Use a consistent From: address that matches your registered domain
- Ensure your envelope sender (Return-Path) uses your registered domain
- Include both SPF and DKIM authentication when possible

### Monitoring
- Monitor bounce rates for Apple Private Relay addresses
- Check Apple Developer account for delivery notifications
- Set up proper bounce handling in Brevo

### Testing
- Send test emails to Apple Private Relay addresses
- Verify emails are delivered successfully
- Check email headers for authentication results

## Troubleshooting

### Common Issues

**SPF Check Fails in Apple Developer Console**
- Verify SPF record is correctly formatted
- Check DNS propagation (may take up to 24 hours)
- Ensure no conflicting SPF records exist

**Emails Bouncing**
- Confirm domain is registered in Apple Developer account
- Verify From: address domain matches registered domain
- Check that envelope sender domain is properly configured

**Authentication Failures**
- Ensure SPF record includes `include:spf.brevo.com`
- Verify DKIM is properly configured and signing emails
- Check that authentication domains match registered domains

### Verification Steps

1. Use DNS lookup tools to verify SPF record
2. Check Apple Developer account for domain validation status
3. Send test emails and monitor delivery
4. Review email headers for authentication results

## Additional Resources

- [Apple Developer Documentation - Configure Private Email Relay Service](https://developer.apple.com/help/account/capabilities/configure-private-email-relay-service/)
- [Brevo Documentation - Domain Authentication](https://help.brevo.com/hc/en-us/articles/209467485-Authenticate-your-domain)
- [SPF Record Testing Tools](https://www.kitterman.com/spf/validate.html)

