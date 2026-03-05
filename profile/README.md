<p align="center">
  <img src="sela.svg" alt="Sela Cloud Logo" width="300" />
</p>

# ☁️ Sela Engineering: Internal Member Guide

Welcome to the internal engineering guide for the Sela GitHub Organization! This document outlines our standards, workflows, and best practices for all Sela engineers, architects, and contributors. 

Our goal is to maintain high-quality, secure, and easily maintainable code across our multi-cloud ecosystem.

## 🔐 Access & Permissions

As an organization managing complex cloud environments, security and the principle of least privilege remain important, while still fostering a collaborative environment.

* **Default Access:** All members have `Write` access to internal repositories by default, allowing for seamless contributions across engineering teams.
* **Requesting Elevated Access:** If you require Admin privileges, repository creation rights, or access to restricted external client repositories, please request access via your team lead or open a Jira IT ticket.
* **External Collaborators:** Do not invite external contractors or clients directly to internal repositories without prior approval from the Security Operations team.

## 🏗️ Repository Standards

When creating a new repository, please adhere to the following guidelines:

* **Naming Conventions (Project Bifurcation):** Repositories must be prefixed based on whether they are for external clients or internal products. 
    * **External Projects:** Prefix with `external-`. For example, if the project is "finfactor", the repository must be named `external-finfactor`.
    * **Internal Products:** Prefix with `internal-`. For example, an internal platform should be named `internal-[productname]`.
* **Visibility:** Set new repositories to **Internal** by default. Public (Open Source) repositories require approval from the DevRel and Security teams.
* **Required Files:** Every repository should contain at a minimum:
    * `README.md` (Project description, setup, and deployment instructions)
    * `.gitignore` (Appropriate for the language/stack)
    * `CODEOWNERS` (To assign pull request review responsibilities)

## 💻 Development Workflow

1. **Branching:** We follow standard feature branching. Create descriptive branches off `main` (e.g., `feature/update-service-accounts`, `bugfix/python-dependency-error`).
2. **Commits:** Write clear, concise commit messages outlining *what* changed and *why*.
3. **Pull Requests:** Direct commits to `main` are restricted. All changes must be proposed via a Pull Request (PR).
4. **Reviews:** At least one approval from a designated `CODEOWNER` is required before merging. Ensure all automated CI/CD checks pass.

## 🛠️ Coding & Technology Best Practices

To ensure consistency and reliability across our engineering teams, please adhere to the following standards for our core stack:

### ☁️ Cloud Architecture & Security
* **Least Privilege:** Always apply the principle of least privilege. When writing custom IAM roles or managing service accounts, grant only the exact permissions required for the task.
* **Resource Tagging:** All cloud resources must be tagged with `Environment`, `Project`, and `Owner` to ensure proper cost tracking and FinOps management.
* **Stateless Design:** Architect applications to be stateless wherever possible to allow for seamless horizontal scaling.

### 🐍 Python
* **Style Guide:** Follow [PEP 8](https://peps.python.org/pep-0008/) standards. Use tools like `black` for formatting and `flake8` or `ruff` for linting.
* **Dependency Management:** Never install packages globally. Use `venv`, `Pipenv`, or `Poetry`. Pin your dependencies in a `requirements.txt` or `Pipfile.lock` to ensure reproducible builds.
* **Type Hinting:** Use type hints (`def check_policy(project_id: str) -> dict:`) to make data manipulation and API responses easier to understand and debug.
* **Cloud SDKs:** When writing automation scripts, utilize the official cloud SDKs and rely on Workload Identity rather than hardcoded service account keys.

### 🐚 Bash
* **Strict Mode:** Always start your Bash scripts with `set -euo pipefail`. This ensures the script exits immediately on errors, undefined variables, or pipeline failures.
* **Variable Quoting:** Always wrap your variables in double quotes (e.g., `"$MY_VAR"`) to prevent word splitting and globbing issues.
* **Know When to Switch:** If a Bash script requires complex data manipulation, arrays, or JSON parsing, rewrite the script in Python.

### 🏗️ Terraform (IaC)
* **State Management:** Never store `.tfstate` files locally or in version control. Always use a remote backend (e.g., S3/DynamoDB, GCS, or Azure Blob Storage) with state locking enabled.
* **Modularity:** Keep modules DRY (Don't Repeat Yourself). Break down monolithic configurations into reusable, logical modules.
* **Security Scanning:** Integrate tools like `tfsec` or `checkov` into your PR workflows to catch misconfigurations (like open security groups or unencrypted buckets) before deployment.

### 📄 YAML (CI/CD & Kubernetes)
* **Formatting:** Use spaces, never tabs. Stick to a consistent 2-space indentation.
* **Linting:** Validate all configurations using `yamllint` to catch syntax errors early in the pipeline.
* **DRY Configurations:** Utilize YAML anchors (`&`) and aliases (`*`) to avoid repeating blocks of configuration, especially in complex GitHub Actions workflows or GitLab CI files.

## 🛡️ Security Best Practices

* **Never Commit Secrets:** Do not hardcode API keys, service account JSON files, or database passwords in your code. Use GitHub Secrets or enterprise secret management tools.
* **Dependency Scanning:** Ensure Dependabot is enabled on your repositories to catch vulnerable packages automatically.
* **Automated Deployments:** When automating deployments via GitHub Actions, always use Workload Identity Federation to authenticate with cloud providers.

## 🆘 Support & Communication

Need help with a deployment, architectural decision, or access issue? Reach out on Teams
