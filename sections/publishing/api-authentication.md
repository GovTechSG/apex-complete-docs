# API Authentication

This section serves as a guide to help you understand various types of API authentication.

- [JWT](#jwt)
- [API Key](#api-key)
- [OAuth 2.1](#oauth-21)

## JWT

JWT authentication is suitable for sensitive APIs.

| Audience | Use cases | Examples  |  Additional controls implemented by APEX | Other recommendations                               |
|----------|----------------------------------|----------------------------------------------|------------------------------------------------------------|-----------------------------------------------------|
| Government to Government | • Functional APIs<br>•  Sensitive APIs  |  • Medisave balance <br>  • Update new OneServicecase | Default APEX controls | • MTLS (to provider platform)<br>• Use Javascript origins |
| Government to Business| Sensitive APIs | Vehicle leasing information | Default APEX controls | Use Javascript origins

## API Key

API key authentication is ideal for read-only APIs with non-sensitive information. 

| Audience | Use cases | Examples |  Additional controls implemented by APEX  | Other recommendations                               |
|----------|----------------------------------|----------------------------------------------|------------------------------------------------------------|-----------------------------------------------------|
| Government to Government | Read-only APIs   (Non-sensitive) | GST calculator                                | • Default APEX controls<br>• Throttled TPS at application level | • Monthly API key rotation<br> • Use Javascript origins    |
| Government to Business | Read-only APIs   (Non-sensitive) | Bus arrival   GST calculator                 | • Default APEX controls<br>• Throttled TPS at application level | • Monthly API key rotation<br> • Use Javascript origins   |                                             |

## OAuth 2.1

OAuth 2.1 authentication is recommended for APIs requiring user consent.

| Audience | Use case  | Examples | Additional controls implemented by APEX  |
|----------|----------------------------------------------|----------------------------------|------------------------------------------------------------|
| Government to Busniess  | APIs which require user consent  | • Payroll APIs<br>• NDI Consent platform | | Default APEX controls  | 