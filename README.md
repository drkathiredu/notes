# Outline + Authentik on Coolify

Self-hosted Outline wiki with Authentik as the OIDC identity provider (web UI user management, no third-party OAuth).

## Deploy

1. In Coolify: **New Resource → Docker Compose → Public Repository**, paste this repo's URL.
2. Set two domains under the resource's network settings:
   - `outline` service → your Outline domain, internal port 3000
   - `authentik-server` service → your auth domain, internal port 9000
3. Copy `.env.example` values into Coolify's environment variables tab (`OUTLINE_URL`, `AUTHENTIK_URL`). Leave `OIDC_CLIENT_SECRET` as a placeholder for now.
4. Deploy. Coolify auto-generates all `$SERVICE_PASSWORD_*` / `$SERVICE_BASE64_64_*` secrets — check them under the resource's "Environment Variables" tab if you need them later.
5. Visit `<AUTHENTIK_URL>/if/flow/initial-setup/` → create your Authentik admin account.
6. In Authentik: **Applications → Providers → Create** (OAuth2/OIDC), redirect URI `<OUTLINE_URL>/auth/oidc.callback`. Copy the generated Client Secret.
7. Update `OIDC_CLIENT_SECRET` in Coolify's env vars with that value, redeploy Outline.
8. **Applications → Create** in Authentik, link to the provider you just made, name it "Outline".
9. **Directory → Users → Create** to add login accounts — this is your ongoing web-UI user management.
