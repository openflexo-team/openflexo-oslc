# Known defects — openflexo-oslc

## OSLC-D-1 — `oslc-ta` is stuck on Apache HttpClient 4  ·  `TODO`

**Symptom.** `oslc-ta` cannot leave HttpClient 4.5.14 (BUILD-F-2 in `openflexo-buildplugin/BACKLOG.md`).

**Verified (2026-10-25).**
- `FlexoOAuthHandlerImpl` calls `org.apache.http.*` directly; `FlexoOAuthHandler` uses
  `net.oauth.client.httpclient4.HttpClient4` from `net.oauth.core:oauth-httpclient4:20090913`.
- The Lyo client is `oslc-java-client` 3.0.0 (2015 snapshot). The latest stable, 4.1.0, still depends on
  `oauth-httpclient4` 20090913, so upgrading Lyo does not remove the dependency (an attempt is commented out in
  `oslc-ta/build.gradle`).

**Way out.** Rewrite the OAuth handling without `oauth-httpclient4` and drop the Lyo client, or wait for a Lyo release
that no longer needs it.
