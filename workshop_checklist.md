## AWS Identity and Access Management (IAM)
- [ ] Grant the minimum privileges needed to complete tasks by applying the Principle of Least Privilege.
- [ ] Ensure permission policies attached to roles or users grant only the minimal required access.
- [ ] Verify that role credentials dynamically expire over time.

## Amazon EC2
- [ ] Enforce a default deny policy for inbound network traffic.
- [ ] Restrict SSH (Port 22) access exclusively to specific, trusted IPs to prevent automated botnet brute-force attacks.
- [ ] Use secure SSH key pair authentication instead of passwords.
- [ ] Rotate SSH keys regularly.

## Amazon S3
- [ ] Properly configure bucket access policies to prevent the downloading and public exposure of confidential files.
- [ ] Turn on "Block Public Access" settings at both the bucket and account levels.
- [ ] Enable default server-side encryption (such as SSE-S3 or SSE-KMS) to automatically encrypt uploaded objects at rest.
- [ ] Enable object versioning to keep multiple iterations of objects, protecting data from accidental deletions or malicious overwrites.

## Docker
- [ ] Use minimal base images (e.g., Alpine) to shrink the container's attack surface.
- [ ] Add a `.dockerignore` file to exclude local build artifacts and sensitive files from the build process.
- [ ] Avoid running processes as root by creating a dedicated non-root user and group within the Dockerfile to enforce least privilege.
- [ ] Never bake credentials or API keys directly into images or layers.
- [ ] Inject secrets dynamically at runtime utilizing environment variables or secret mounts.
- [ ] Tag images with proper semantic versions and test them locally with restricted runtime privileges.

## GitHub
- [ ] Automate secret and code scanning (such as CodeQL) to detect hardcoded vulnerabilities, secrets, and API keys.
- [ ] Enable Push Protection to block secret leaks before code is pushed to the repository.
- [ ] Keep project dependencies secure by enabling Dependabot for continuous vulnerability monitoring and automated security update pull requests.
- [ ] Enforce strict branch protections on production and main branches to prevent unreviewed changes.
- [ ] Require peer pull request reviews and passing status checks before code can be merged.
- [ ] Prevent force pushes and unauthorized deletions on protected branches.
- [ ] Store application credentials securely using GitHub Secrets or `.env` files instead of committing them to the codebase.