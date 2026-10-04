## AWS Identity and Access Management (IAM)
- [ ] Grant the minimum privileges needed to complete tasks by applying the Principle of Least Privilege.[cite: 4, 5]
- [ ] Ensure permission policies attached to roles or users grant only the minimal required access.[cite: 4, 5]
- [ ] Verify that role credentials dynamically expire over time.[cite: 5]

## Amazon EC2
- [ ] Enforce a default deny policy for inbound network traffic.[cite: 6]
- [ ] Restrict SSH (Port 22) access exclusively to specific, trusted IPs to prevent automated botnet brute-force attacks.[cite: 6, 7]
- [ ] Use secure SSH key pair authentication instead of passwords.[cite: 6]
- [ ] Rotate SSH keys regularly.[cite: 6]

## Amazon S3
- [ ] Properly configure bucket access policies to prevent the downloading and public exposure of confidential files.[cite: 2, 3]
- [ ] Turn on "Block Public Access" settings at both the bucket and account levels.[cite: 8, 9]
- [ ] Enable default server-side encryption (such as SSE-S3 or SSE-KMS) to automatically encrypt uploaded objects at rest.[cite: 8, 9]
- [ ] Enable object versioning to keep multiple iterations of objects, protecting data from accidental deletions or malicious overwrites.[cite: 8, 9]

## Docker
- [ ] Use minimal base images (e.g., Alpine) to shrink the container's attack surface.[cite: 11]
- [ ] Add a `.dockerignore` file to exclude local build artifacts and sensitive files from the build process.[cite: 11]
- [ ] Avoid running processes as root by creating a dedicated non-root user and group within the Dockerfile to enforce least privilege.[cite: 11]
- [ ] Never bake credentials or API keys directly into images or layers.[cite: 11]
- [ ] Inject secrets dynamically at runtime utilizing environment variables or secret mounts.[cite: 11]
- [ ] Tag images with proper semantic versions and test them locally with restricted runtime privileges.[cite: 11]

## GitHub
- [ ] Automate secret and code scanning (such as CodeQL) to detect hardcoded vulnerabilities, secrets, and API keys.[cite: 13, 14]
- [ ] Enable Push Protection to block secret leaks before code is pushed to the repository.[cite: 13, 14, 15]
- [ ] Keep project dependencies secure by enabling Dependabot for continuous vulnerability monitoring and automated security update pull requests.[cite: 13, 14]
- [ ] Enforce strict branch protections on production and main branches to prevent unreviewed changes.[cite: 13]
- [ ] Require peer pull request reviews and passing status checks before code can be merged.[cite: 13]
- [ ] Prevent force pushes and unauthorized deletions on protected branches.[cite: 13]
- [ ] Store application credentials securely using GitHub Secrets or `.env` files instead of committing them to the codebase.[cite: 15]