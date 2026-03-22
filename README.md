## 🔐 How to Create Self-signed Certificate using PowerShell
As an administrator, ensuring secure communication over networks is a top priority. One way to achieve this is by using secure certificates. While CA-signed certificates (issued by a trusted third-party organization) provide a high level of security, they can be expensive and may not be necessary for all use cases. In such scenarios, a self-signed public certificate can serve cost-effective and convenient alternative.

Self-signed certificates are perfect for testing purposes and provide a secure solution for administrators who require a certificate-based solution. With just a single PowerShell cmdlet, you can easily create an SSL certificate that fits your needs and requirements.

This repository provides guidance on creating, exporting, and using self-signed certificates on Windows using PowerShell. 
Self-signed certificates are useful for development, testing, internal services, or lab environments.

### 📋 Prerequisites
* Windows 10 or higher / Windows Server 2016 or higher
* PowerShell (run as Administrator)
* Basic understanding of certificates

## Step 1: Open PowerShell as Administrator
1. Click Start
2. Search for PowerShell
3. Right-click → Run as Administrator

## 1. Create Your Own SSL Certificate using PowerShell
This is simple as it looks. Using the **‘New-SelfSignedCertificate’** cmdlet, you can create self-signed certificate in a jiffy.

![Markdown Logo](assets/sample-cert.png)

```powershell
    New-SelfSignedCertificate `
      -DnsName "wutang.local", "127.0.0.1", "::1" `
      -CertStoreLocation "cert:\LocalMachine\My" `
      -FriendlyName "Local Dev HTTPS - Self-Signed" `
      -NotAfter (Get-Date).AddYears(5) `
      -KeyUsage DigitalSignature,KeyEncipherment `
      -TextExtension @("2.5.29.37={text}1.3.6.1.5.5.7.3.1") `
      -HashAlgorithm SHA256
```
This command creates a self-signed SSL/TLS certificate for local HTTPS development. Here's a breakdown of each parameter:
* ```New-SelfSignedCertificate``` — The cmdlet that generates the certificate.
* ```-DnsName "localhost", "127.0.0.1", "::1"``` — Adds Subject Alternative Names (SANs) to the certificate, so it's valid for all three common ways to refer to your local machine: hostname, IPv4 loopback, and IPv6 loopback.
* ```CertStoreLocation "cert:\LocalMachine\My"``` — Installs the certificate into the Windows Certificate Store under Local Machine → Personal. This makes it available system-wide, not just for the current user.
* ```-FriendlyName "Local Dev HTTPS - Self-Signed"``` — A human-readable label so you can easily identify it in the certificate manager (certmgr.msc).
* ```-NotAfter (Get-Date).AddYears(5)``` — Sets the expiry date to 5 years from now instead of the default 1 year.
* ```-KeyUsage DigitalSignature, KeyEncipherment``` — Restricts what the certificate's private key can be used for — signing data and encrypting key exchange, which are exactly what HTTPS (TLS) needs.
* ```-TextExtension @("2.5.29.37={text}1.3.6.1.5.5.7.3.1")``` — Sets the Extended Key Usage (EKU) to Server Authentication (OID 1.3.6.1.5.5.7.3.1), explicitly marking this as a server certificate used for TLS — required by modern browsers to trust it for HTTPS.
* ``` -HashAlgorithm SHA256``` - Signs the certificate using SHA-256, which is the modern standard (SHA-1 is deprecated and rejected by browsers).
