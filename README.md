# Simple Sync Repository

[![GitHub Action](https://img.shields.io/badge/GitHub-Action-blue?logo=github)](https://github.com/marketplace/actions/simple-sync-repository)
[![License](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)
[![Version](https://img.shields.io/badge/version-v1.0.0-blue)](https://github.com/ovas04/simple-sync-repo/releases)

A lightweight and reliable GitHub Action designed to synchronize repositories between organizations and GitHub Enterprise instances. Simple to use, powerful in functionality, and built for enterprise environments.

## 🚀 Key Features

- 🔄 **Simple Repository Sync**: Effortless synchronization preserving all history and changes
- 🏢 **Multi-Platform Support**: Works seamlessly with GitHub.com and GitHub Enterprise Server
- 📊 **Detailed Reporting**: Comprehensive logs and GitHub Actions summaries
- 🎯 **Flexible Configuration**: Easy setup with sensible defaults
- 🔒 **Enterprise Security**: Secure token handling and validation
- ⚡ **Lightweight**: No unnecessary complexity, just pure synchronization

## 🔧 Usage

### Basic Usage

```yaml
- name: Sync Repository
  uses: ovas04/simple-sync-repo@v1
  with:
    source_pat: ${{ secrets.SOURCE_PAT }}
    target_pat: ${{ secrets.TARGET_PAT }}
    source_org: 'source-enterprise-org'
    repository_name: ${{ github.event.repository.name }}
```

### Advanced Usage

```yaml
- name: Sync Repository with Custom Configuration
  uses: ovas04/simple-sync-repo@v1
  with:
    source_pat: ${{ secrets.ENTERPRISE_GITHUB_TOKEN }}
    target_pat: ${{ secrets.TARGET_PAT }}
    source_org: 'source-enterprise-org'
    repository_name: 'my-awesome-repo'
    branch: 'main'
    source_ghec_url: 'github.com'
    target_ghec_url: 'github.com'
```

### Complete Workflow Example

```yaml
name: Sync Origin Repository

on:
  workflow_dispatch:
  schedule:
    - cron: '0 0 28 * *'  # Monthly sync on the 28th
  pull_request_target:
    branches:
      - main

jobs:
  sync-branch:
    if: github.event.repository.fork == false
    runs-on: ubuntu-latest
    steps:
      - name: Checkout Repository
        uses: actions/checkout@v4
        with:
          fetch-depth: 0

      - name: Sync with Source Repository
        id: sync
        uses: ovas04/simple-sync-repo@v1
        with:
          source_pat: ${{ secrets.ENTERPRISE_GITHUB_TOKEN }}
          target_pat: ${{ secrets.TARGET_PAT }}
          source_org: 'source-enterprise-org'
          repository_name: ${{ github.event.repository.name }}

      - name: Display Sync Results
        run: |
          echo "Sync Status: ${{ steps.sync.outputs.sync_status }}"
          echo "Repository URL: ${{ steps.sync.outputs.repository_url }}"
          echo "Commits Synced: ${{ steps.sync.outputs.commits_synced }}"
```

## 📋 Inputs

| Input | Required | Default | Description |
|-------|----------|---------|-------------|
| `source_pat` | ✅ | - | Personal Access Token for source organization |
| `target_pat` | ✅ | - | Personal Access Token for target organization |
| `source_org` | ✅ | - | Source organization name |
| `repository_name` | ✅ | - | Repository name to sync |
| `branch` | ❌ | `main` | Branch to sync |
| `source_ghec_url` | ❌ | `github.com` | Source GitHub Enterprise Cloud URL |
| `target_ghec_url` | ❌ | `github.com` | Target GitHub Enterprise Cloud URL |

## 📤 Outputs

| Output | Description |
|--------|-------------|
| `sync_status` | Status of the synchronization (`success`, `no_changes`, `failure`) |
| `repository_url` | URL of the synchronized repository |
| `commits_synced` | Number of commits synchronized |

## 🔑 Prerequisites

### Required Secrets

#### `SOURCE_PAT` - Source Organization Token
- **Scopes needed**: `repo`, `read:org`
- **Purpose**: Read access to source repositories
- **Permissions**: Must have access to the source organization

#### `TARGET_PAT` - Target Organization Token  
- **Scopes needed**: `repo`, `admin:org`, `write:org`
- **Purpose**: Create status checks and push to target repository
- **Permissions**: Must be able to push to repository and create status checks
- **Important**: Must be configured as a bypass actor in branch protection rules

### Setup Instructions

1. Navigate to **Settings** > **Secrets and variables** > **Actions**
2. Click **New repository secret**
3. Add `SOURCE_PAT` and `TARGET_PAT` with their respective tokens

## 🔄 Enterprise Integration

This action is designed for enterprise environments where repository synchronization is needed:

- **Multi-Organization Support**: Seamlessly sync between different GitHub organizations
- **Enterprise Server Compatible**: Works with GitHub Enterprise Server instances
- **Compliance Ready**: Detailed logging for audit and compliance requirements
- **Automated Workflows**: Perfect for scheduled and event-driven synchronization

### Use Cases

- **Organization Migration**: Moving repositories between organizations
- **Content Synchronization**: Keeping repositories up-to-date across organizations
- **Enterprise Compliance**: Maintaining synchronized copies for compliance
- **Development Workflows**: Automated synchronization in CI/CD pipelines

## 🛡️ Security Considerations

- **Token Security**: All tokens are handled securely without logging
- **Minimal Permissions**: Only requires necessary scopes for operation
- **Bypass Configuration**: Requires proper bypass setup for status checks
- **Audit Trail**: Provides detailed logging for compliance

## 📊 Monitoring and Troubleshooting

### Common Issues

1. **Authentication Errors**: Verify PAT scopes and permissions
2. **Status Check Failures**: Ensure TARGET_PAT has bypass permissions
3. **Sync Failures**: Check repository access and network connectivity

### Debug Information

The action provides comprehensive logging:
- Input validation results
- Synchronization progress
- Status check creation details
- Summary with commit counts and URLs

## 🤝 Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🔗 Related Projects

- [Enterprise Repository Bridge](https://github.com/ovas04/enterprise-repository-bridge) - Complete repository cloning solution
- [GitHub Actions Documentation](https://docs.github.com/en/actions) - Official GitHub Actions docs

## 📞 Support

For questions or issues, please open an issue on GitHub or contact the maintainers.
