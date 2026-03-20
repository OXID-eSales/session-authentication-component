# OXID eShop Session Authentication Component

Provides session-based authentication for OXID eShop Symfony controllers. Controllers annotated with `#[SessionUser]` or `#[AdminSessionUser]` require an active OXID session cookie before the request is processed.

> **Note:** Recommended for AJAX endpoints only. For stateless API access, use JWT authentication instead.

## How it works

The component registers two Symfony kernel event subscribers:

- **`SessionAuthListener`** — handles `#[SessionUser]` — requires an active frontend session (`sid` cookie)
- **`AdminSessionAuthListener`** — handles `#[AdminSessionUser]` — requires an active admin session (`admin_sid` cookie) with optional role checks

## Usage

Apply the attribute to an action method:

```php
use OxidEsales\SessionAuthComponent\Security\Attribute\SessionUser;
use OxidEsales\SessionAuthComponent\Security\Attribute\AdminSessionUser;

class MyController
{
    #[SessionUser]
    public function ajaxUserAction(): ResponseInterface
    {
        // requires active frontend session (sid cookie)
    }

    #[AdminSessionUser(roles: ['ROLE_ADMIN'])]
    public function ajaxAdminAction(): ResponseInterface
    {
        // requires active admin session (admin_sid cookie) with ROLE_ADMIN
    }
}
```

## Available roles

| Role | Description |
|---|---|
| `ROLE_ADMIN` | Admin session user |
| `ROLE_ADMIN_MALL` | Mall admin (full rights across all subshops) |

## Installation

```bash
composer require oxid-esales/session-authentication-component
```