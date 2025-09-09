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
- 🔑 **Flexible Authentication**: Supports both Personal Access Tokens (PAT) and GitHub App tokens

## 🔧 Usage

### Basic Usage with PAT

```yaml
- name: Sync Repository
  uses: ovas04/simple-sync-repo@v1
  with:
    source_pat: ${{ secrets.SOURCE_PAT }}
    target_pat: ${{ secrets.TARGET_PAT }}
    source_org: 'source-enterprise-org'
    repository_name: ${{ github.event.repository.name }}
```

### Advanced Usage with GitHub App Token

```yaml
- name: Sync Repository with Custom Configuration
  uses: ovas04/simple-sync-repo@v1
  with:
    source_pat: ${{ secrets.SOURCE_APP_TOKEN }}
    target_pat: ${{ secrets.TARGET_APP_TOKEN }}
    source_org: 'source-enterprise-org'
    repository_name: 'my-awesome-repo'
    source_token_type: 'github-app'
    target_token_type: 'github-app'
    branch: 'main'
    source_ghec_url: 'github.com'
    target_ghec_url: 'github.com'
```

## 📋 Inputs

| Input | Required | Default | Description |
|-------|----------|---------|-------------|
| `source_pat` | ✅ | - | Token for source organization (PAT or GitHub App token) |
| `target_pat` | ✅ | - | Token for target organization (PAT or GitHub App token) |
| `source_org` | ✅ | - | Source organization name |
| `repository_name` | ✅ | - | Repository name to sync |
| `branch` | ❌ | `main` | Branch to sync |
| `source_ghec_url` | ❌ | `github.com` | Source GitHub Enterprise Cloud URL |
| `target_ghec_url` | ❌ | `github.com` | Target GitHub Enterprise Cloud URL |
| `source_token_type` | ❌ | `pat` | Type of token for source (`pat` or `github-app`) |
| `target_token_type` | ❌ | `pat` | Type of token for target (`pat` or `github-app`) |

## 📤 Outputs

| Output | Description |
|--------|-------------|
| `sync_status` | Status of the synchronization (`success`, `no_changes`, `failure`) |
| `repository_url` | URL of the synchronized repository |
| `commits_synced` | Number of commits synchronized |

## 🔑 Prerequisites

### Authentication Options

#### Personal Access Token (PAT)
- Default authentication method
- **Scopes needed**: `repo`, `read:org`
- **Purpose**: Read access to source repositories and write access to target repositories
- **Setup**: Create PAT in GitHub settings and add as secrets

#### GitHub App Token (Alternative)
- More granular permissions and better security
- **Required Permissions**:
  - Repository: Read & Write
  - Organization: Read
- **Setup**: 
  1. Create GitHub App with required permissions
  2. Install app in source/target organizations
  3. Generate token and add as secret
  4. Set `*_token_type: 'github-app'` in workflow

### Setup Instructions

1. Navigate to **Settings** > **Secrets and variables** > **Actions**
2. Click **New repository secret**
3. Add your tokens as secrets:
   - For PAT: Add as `SOURCE_PAT` and `TARGET_PAT`
   - For GitHub App: Add as any name (e.g., `SOURCE_APP_TOKEN`) and set token type in workflow

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
- **GitHub Apps**: More secure with fine-grained permissions and automated token rotation

## 📊 Monitoring and Troubleshooting

### Common Issues

1. **Authentication Errors**: 
   - Verify PAT scopes and permissions
   - For GitHub Apps, ensure proper installation and permissions
2. **Token Type Mismatch**: Ensure `*_token_type` matches the type of token provided
3. **Status Check Failures**: Ensure tokens have proper permissions
4. **Sync Failures**: Check repository access and network connectivity

### Debug Information

The action provides comprehensive logging:
- Input validation results
- Authentication method used
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
