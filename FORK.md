# ricardo-lobo fork

Branch `lobo` = the latest stable upstream release + these changes:

- **One OAuth refresh at a time** (`stellantis.py`, `refresh_oauth_token_request`).
  Stellantis refresh tokens are single use; the scheduled refresh and the
  retry after a 401 could race with the same token and the loser got
  `invalid_grant`, which logged the account out.
- **Automatic new login** when Stellantis revokes the refresh token
  (`_relogin_with_stored_credentials`). Opt-in per account in the login step
  ("Keep the login to sign in again on its own"); the e-mail and password are
  stored in the config entry, masked in logs and redacted from diagnostics.
  At most once every 6 h, otherwise the normal reauth flow runs.
- `hacs.json` without `zip_release`, so HACS installs from the branch.

Install: HACS → ⋮ → Custom repositories → this repo, type Integration (after
removing the upstream repository from HACS; the config entry stays).

Sync: `.github/workflows/sync-upstream.yaml` merges each new stable upstream
release every Monday, or opens a PR when it conflicts. The upstream workflows
are disabled on this fork.
