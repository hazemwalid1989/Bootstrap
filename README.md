# SAML Setup

## Customer Account

[Installing CF Template](https://github.com/New-Math-Data/cloudformation-setup-saml-gsuite/blob/main/CF_TEMPLATE_INSTALL.md)

## CLI Deploy
From Customer account
```bash
aws cloudformation deploy \
  --template-file main.yaml \
  --stack-name nmd-developer-access-saml \
  --parameter-overrides \
    idpName="NMDGoogle" \
    samlMetadata="<md:EntityDescriptor xmlns:md="urn:oasis:names:tc:SAML:2.0:metadata" entityID="https://accounts.google.com/o/saml2?idpid=C03vzt6hn" validUntil="2026-02-08T19:34:00.000Z">
<md:IDPSSODescriptor WantAuthnRequestsSigned="false" protocolSupportEnumeration="urn:oasis:names:tc:SAML:2.0:protocol">
<md:KeyDescriptor use="signing">
<ds:KeyInfo xmlns:ds="http://www.w3.org/2000/09/xmldsig#">
<ds:X509Data>
<ds:X509Certificate>MIIDdDCCAlygAwIBAgIGAXeISVJKMA0GCSqGSIb3DQEBCwUAMHsxFDASBgNVBAoTC0dvb2dsZSBJ bmMuMRYwFAYDVQQHEw1Nb3VudGFpbiBWaWV3MQ8wDQYDVQQDEwZHb29nbGUxGDAWBgNVBAsTD0dv b2dsZSBGb3IgV29yazELMAkGA1UEBhMCVVMxEzARBgNVBAgTCkNhbGlmb3JuaWEwHhcNMjEwMjA5 MTkzNDAwWhcNMjYwMjA4MTkzNDAwWjB7MRQwEgYDVQQKEwtHb29nbGUgSW5jLjEWMBQGA1UEBxMN TW91bnRhaW4gVmlldzEPMA0GA1UEAxMGR29vZ2xlMRgwFgYDVQQLEw9Hb29nbGUgRm9yIFdvcmsx CzAJBgNVBAYTAlVTMRMwEQYDVQQIEwpDYWxpZm9ybmlhMIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8A MIIBCgKCAQEAqKjXZa7wHzsQ7JTAU3Fp2lukxpjmCDcJ9AkvXgkIKR+ATrlD2+a3u8dOfvHvOvBY yn+47d0DmjDBr0rcThI6vHWghDxV/Kz50/vE43rtrqEYATxWtnsWjjYeeTfGwNLx/lz0dlyB+Q5I ig0/s9mTW6km8DEA7Bl/FIpoXniYvyfDn8dxSsHzuwS0R0rbdPzDceN5HDCziDHMEbZhYyCHj7sv mPynJkRZTGnFAxkBDASTzZvD4dW/35Vftl/5YExi8jkUstJR7UvV/dJMCsNolhngNVwuJyofF/vZ 64EVszzRmN6k2FyzRX/wq3bONnP03LWgOAhWrC2KkeDT3RDxgwIDAQABMA0GCSqGSIb3DQEBCwUA A4IBAQAZP5MVqePz+XChN2A9tgjudpLLAJKcrJ0Gi1InnaW4RiQ0TbsxagG22DNoCYMyXuTd9mxO cmxAkquODKhlcae6qf97A8o2Kv3iHdhBd1t2sPkevG29CaoZk0zOTimLGfJtDI8Lo+hrp+CwIyw8 vyfRbGmxINd2KV5Klp1JM3mRRLujA9aVsM9Kc16IKEvj5yDt1FjoFyNX9O4gtSqHq2K3kMm3gCtI 4stVECvzE8bVcpmApc/mhgWdnDCVt/T1p1YUeYVuBCp1y7YZ8KNAgrZOImp0KSuC+TmgjuiRCwnP vgGyGff5wdyMaIz3mLBUVUp+kL026pihgIs+7YwcEjV2</ds:X509Certificate>
</ds:X509Data>
</ds:KeyInfo>
</md:KeyDescriptor>
<md:NameIDFormat>urn:oasis:names:tc:SAML:1.1:nameid-format:emailAddress</md:NameIDFormat>
<md:SingleSignOnService Binding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-Redirect" Location="https://accounts.google.com/o/saml2/idp?idpid=C03vzt6hn"/>
<md:SingleSignOnService Binding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-POST" Location="https://accounts.google.com/o/saml2/idp?idpid=C03vzt6hn"/>
</md:IDPSSODescriptor>
</md:EntityDescriptor>" \
    custNameAbbreviation="customer_name" \
    accessPolicy="AdministratorAccess" \
  --capabilities CAPABILITY_NAMED_IAM \
  --region us-west-2
```
## NMD GSuite

1. Navigate to a user profile
   https://admin.google.com/ac/users/
1. Add the following to the Amazon section
   arn:aws:iam::{AWS_ACCOUNT_NUMBER}:role/NMD-Freeside-Admin,arn:aws:iam::{AWS_ACCOUNT_NUMBER}:saml-provider/GSuite

https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create_for-user_externalid.html

## Setup SSO

The SAML roles installed also work with AWS CLI credentials.
We use saml2aws:
```bash
brew install saml2aws
saml2aws configure 
   Configuration saved for IDP account: default
   saml2aws configure
   ? Please choose a provider: GoogleApps
   ? AWS Profile saml
   ? URL https://accounts.google.com/o/saml2/initsso?idpid=C03vzt6hn&spid=804580507359&forceauthn=false
   ? Username cking@newmathdata.com
   ? Password ***************
   ? Confirm ***************
   
   account {
     URL: https://accounts.google.com/o/saml2/initsso?idpid=C03vzt6hn&spid=804580507359&forceauthn=false
     Username: cking@newmathdata.com
     Provider: GoogleApps
     MFA: Auto
     SkipVerify: true
     AmazonWebservicesURN: urn:amazon:webservices
     SessionDuration: 3600
     Profile: saml
     RoleARN:
     Region:
   }
saml2aws login
   Using IdP Account default to access GoogleApps https://accounts.google.com/o/saml2/initsso?idpid=C03vzt6hn&spid=804580507359&forceauthn=false
   To use saved password just hit enter.
   ? Username cking@newmathdata.com
   ? Password
   
   Authenticating as cking@newmathdata.com ...
   Check your phone and tap 'Yes' on the prompt. Then press ENTER to continue.
   
   ? Please choose the role Account: 12345678901 / NMD-Admin
   Selected role: arn:aws:iam::12345678901:role/NMD-Admin
   Requesting AWS credentials using SAML assertion.
   Logged in as: arn:aws:sts::12345678901:assumed-role/NMD-Admin/cking@newmathdata.com
   
   Your new access key pair has been stored in the AWS configuration.
   Note that it will expire at 2024-11-18 11:43:24 -0800 PST
   To use this credential, call the AWS CLI with the --profile option (e.g. aws --profile saml ec2 describe-instances).
```
