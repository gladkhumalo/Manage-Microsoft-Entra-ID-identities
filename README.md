## How to Create Self-signed Certificate using PowerShell
As an administrator, ensuring secure communication over networks is a top priority. One way to achieve this is by using secure certificates. While CA-signed certificates (issued by a trusted third-party organization) provide a high level of security, they can be expensive and may not be necessary for all use cases. In such scenarios, a self-signed public certificate can serve cost-effective and convenient alternative.

Self-signed certificates are perfect for testing purposes and provide a secure solution for administrators who require a certificate-based solution. With just a single PowerShell cmdlet, you can easily create an SSL certificate that fits your needs and requirements.

This repository provides guidance on creating, exporting, and using self-signed certificates on Windows using PowerShell. 
Self-signed certificates are useful for development, testing, internal services, or lab environments.

### Prerequisites
* Windows 10 or higher / Windows Server 2016 or higher
* PowerShell (run as Administrator)

## 1. Create Your Own SSL Certificate using PowerShell
This is simple as it looks. Using the **‘New-SelfSignedCertificate’** cmdlet, you can create self-signed certificate in a jiffy.

```powershell
    New-SelfSignedCertificate `
      -DnsName "wutang.local", "127.0.0.1", "::1" `
      -CertStoreLocation "cert:\LocalMachine\My" `
      -FriendlyName "Local Dev HTTPS - Self-Signed" `
      -NotAfter (Get-Date).AddYears(5) `
      -KeyUsage DigitalSignature,KeyEncipherment `
      -TextExtension @("2.5.29.37={text}1.3.6.1.5.5.7.3.1") `
      -HashAlgorithm SHA256

