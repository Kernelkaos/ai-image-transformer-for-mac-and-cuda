# Security Policy

## Supported versions

Only the latest release on the `main` branch receives security fixes.

## Reporting a vulnerability

Please **do not** report security issues in public issues or pull requests.

Use GitHub's private reporting instead: go to the **Security** tab of this repository and choose **Report a vulnerability**.

Please include:

- A description of the problem and its impact.
- Steps to reproduce it.
- The affected version or commit.

You can expect an initial response within 7 days.

## Good practices for users

- Keep the default `--host 127.0.0.1` so the app is only reachable from your own machine.
- Avoid `--share` on machines with sensitive data. It exposes the app to the internet.
- Download models only from sources you trust.
