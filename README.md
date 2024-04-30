# WIP, this is Work in progress, don't look at me too much.

# Introduction

With more and more tools to interact with Outscale APIs, several formats were born which are more or less the same.

This page describe a standard way to handle parameters in various Outscale tools.

# Configuration order

Tools configuration can be done through different ways:

- Through a option directly passed to the tool:
  - command line argument
  - tool specific format (like terraform vars definition)
  - as code (like using a sdk)
- Through environment variable
- Through configuration file 

Each parameter is evaluated in the following order:

- Check for parameter value directly passed to the tool
- If not available, check for environment variable
- If not available, check for configuration file 
- If not available (or no profile can be found), check for legacy configuration system which was used prior to this standard (old configuration must still work), 
- If not available, use a default value when possible
- If no default value is possible, show a nice error message

# Environment variables


| Name | Type | Example | Description |
|------|------|---------|-------------|
| OSC_ACCESS_KEY | string | F4K4T706S9000EXAMPLE | Access Key |
| OSC_SECRET_KEY | string	| E4XJE8EJ98ZEJ18E4J9ZE84J19Q8E1000EXAMPLE | Secret Key |
| OSC_X509_CLIENT_CERT | string |	/etc/secret/client.crt | Full path to client certificate (.crt, public part) |
| OSC_X509_CLIENT_CERT_B64 | string |	dW5jZXJ0aWZpY2F0Cg== | Client certificate base64 encoded |
| OSC_X509_CLIENT_KEY | string | /etc/secret/client.key |	Full path to client key related to certificate (.key, secret part) |
| OSC_X509_CLIENT_KEY_B64 | string | dW5jbGVwcml2ZWUK |	client key related to certificate base64 encoded |
| OSC_REGION | string |	eu-west-2 | region |	
| OSC_ENDPOINT_API | string | https://api.eu-west-2.outscale.com | Endpoint of Outscale API (with protocol, nor partial URI). Note: this has recently changed. We used to include partial uri without protocol. Some software might not be compliant to this one yet |
| OSC_ENDPOINT_FCU | string | https://fcu.eu-west-2.outscale.com | Endpoint of FCU API (with protocol). Same note as for OSC_ENDPOINT_API |
| OSC_ENDPOINT_LBU | string | https://lbu.eu-west-2.outscale.com | Endpoint of LBU API (with protocol). Same note as for OSC_ENDPOINT_API |
| OSC_ENDPOINT_EIM | string |	https://eim.eu-west-2.outscale.com | Endpoint of EIM API (with protocol). Same note as for OSC_ENDPOINT_API |
| OSC_ENDPOINT_ICU | string |	https://icu.eu-west-2.outscale.com | Endpoint of ICU API (with protocol). Same note as for OSC_ENDPOINT_API |
| OSC_ENDPOINT_DIRECTLINK | string | https://directlink.eu-west-2.outscale.com | Endpoint of DirectLink API (with protocol). Same note as for OSC_ENDPOINT_API |
| OSC_ENDPOINT_OOS | string | https://oos.eu-west-2.outscale.com | Endpoint of OOS API (with protocol). Same note as for OSC_ENDPOINT_API |
| OSC_PROFILE | string | default | Profile to use in configuration file (default is "default") |
| OSC_LOGIN |	string | XXX.YYY.com | user login (email). Require if password/basic method, ignored otherwise |
| OSC_PASSWORD | string | wololo_or_strong_pwd | user password. Require if password/basic method, ignored otherwise |
| OSC_AUTH_METHOD | string | "accesskey" "basic" "none" | default accesskey, some program support "password" as a synonyme for "basic" |

Note: check for [Regions, Endpoints and Availability Zones Reference](https://docs.outscale.com/en/userguide/Regions-Endpoints-and-Subregions-Reference.html)

# Configuration file

The default configuration is a json file which is located by default in ~/.osc/config.json.

## Sample

```
{
  "default": {
    "access_key": "F4K4T706S9000EXAMPLE",
    "secret_key": "E4XJE8EJ98ZEJ18E4J9ZE84J19Q8E1000EXAMPLE",
    "x509_client_cert": "/etc/secret/client.crt",
    "x509_client_key": "/etc/secret/client.key",
    "region": "eu-west-2"
    "endpoints": {
      "api": "https://api.eu-west-2.outscale.com",
      "fcu": "https://fcu.eu-west-2.outscale.com",
      "lbu": "https://lbu.eu-west-2.outscale.com",
      "eim": "https://eim.eu-west-2.outscale.com",
      "icu": "https://icu.eu-west-2.outscale.com",
      "directlink": "directlink.eu-west-2.outscale.com",
      "oos": "oos.eu-west-2.outscale.com"
    }
  },
  "otherProfile": {
    ...
  }
}
```

## Format

| JsonPath | Type | Eq. Environement Variable | Notes |
|-|-|-|-|
| $.* |	dict |	| Profiles names. If no profile is provided through program's options or environment variable OSC_PROFILE, "default" profile is used. |If the profile cannot be found ("default" or other), no profile is used. |
| $.*.endpoints |	dict | | All endpoints referenced by name |
| $.*.access_key | string |	OSC_ACCESS_KEY | |
| $.*.secret_key | string | OSC_SECRET_KEY | |
| $.*.x509_client_cert | string | OSC_X509_CLIENT_CERT | |
| $.*.x509_client_cert_b64 | string | OSC_X509_CLIENT_CERT_B64 | |
| $.*.x509_client_key | string | OSC_X509_CLIENT_KEY	| |
| $.*.x509_client_key_b64 | string | OSC_X509_CLIENT_KEY_B64	| |
| $.*.region	string | OSC_REGION | |
| $.*.endpoints.api | string | OSC_ENDPOINT_API | |
| $.*.endpoints.fcu | string | OSC_ENDPOINT_FCU | |
| $.*.endpoints.lbu | string | OSC_ENDPOINT_LBU | |
| $.*.endpoints.eim | string | OSC_ENDPOINT_EIM | |
| $.*.endpoints.icu | string | OSC_ENDPOINT_ICU | |
| $.*.endpoints.directlink | string | OSC_ENDPOINT_DIRECTLINK | |
| $.*.endpoints.oos | string | OSC_ENDPOINT_OOS | |


# Keypair Directory

Some applications allow users to createKeypair. By default, those application should put created keypairs in:

~/.osc/keypairs/
