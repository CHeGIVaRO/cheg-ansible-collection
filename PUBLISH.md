# Publishing to Ansible Galaxy

This document explains how to publish this collection to Ansible Galaxy.

## Prerequisites

1. **GitHub Account**: Your repository must be on GitHub
2. **Ansible Galaxy Account**: Create an account at [galaxy.ansible.com](https://galaxy.ansible.com)
3. **GitHub Token**: Generate a personal access token with `repo` permissions

## Steps to Publish

### 1. Prepare the Repository

Ensure your repository is ready:
- All files are committed and pushed to GitHub
- `galaxy.yml` contains correct metadata
- `README.md` is comprehensive
- Collection builds successfully

### 2. Build the Collection

```bash
# Build the collection
ansible-galaxy collection build

# This creates a .tar.gz file
ls -la cheg-ansible_collection-*.tar.gz
```

### 3. Test the Collection Locally

```bash
# Install locally to test
ansible-galaxy collection install cheg-ansible_collection-*.tar.gz --force

# Test with a playbook
ansible-playbook -i your-inventory.yml your-playbook.yml
```

### 4. Publish to Ansible Galaxy

#### Option A: Using GitHub Integration (Recommended)

1. Go to [galaxy.ansible.com](https://galaxy.ansible.com)
2. Log in with your GitHub account
3. Go to "My Content" → "Collections"
4. Click "Add Collection"
5. Select your GitHub repository
6. Galaxy will automatically import and publish your collection

#### Option B: Manual Upload

1. Go to [galaxy.ansible.com](https://galaxy.ansible.com)
2. Log in with your GitHub account
3. Go to "My Content" → "Collections"
4. Click "Upload Collection"
5. Upload the `.tar.gz` file you built

### 5. Verify Publication

After publishing, users can install your collection:

```bash
# Install from Galaxy
ansible-galaxy collection install cheg.ansible_collection

# Or specify version
ansible-galaxy collection install cheg.ansible_collection:==1.0.0
```

## Updating the Collection

### 1. Update Version

Edit `galaxy.yml`:
```yaml
version: 1.0.1  # Increment version
```

### 2. Create Git Tag

```bash
git add .
git commit -m "Release version 1.0.1"
git tag v1.0.1
git push origin main --tags
```

### 3. Galaxy Auto-Import

If using GitHub integration, Galaxy will automatically:
- Detect the new tag
- Build the collection
- Publish the new version

## Collection URL

Once published, your collection will be available at:
- **Galaxy**: https://galaxy.ansible.com/ui/repo/published/cheg/ansible_collection/
- **Install**: `ansible-galaxy collection install cheg.ansible_collection`

## Troubleshooting

### Common Issues

1. **Build Errors**: Check that all required files are present
2. **Import Failures**: Verify GitHub repository is public and accessible
3. **Version Conflicts**: Ensure version numbers are unique and follow semver

### Support

- [Ansible Galaxy Documentation](https://galaxy.ansible.com/docs/)
- [Collection Development Guide](https://docs.ansible.com/ansible/latest/dev_guide/developing_collections.html)
- [GitHub Issues](https://github.com/CHeGIVaRO/cheg-ansible-collection/issues)

## Best Practices

1. **Version Management**: Use semantic versioning (MAJOR.MINOR.PATCH)
2. **Documentation**: Keep README.md updated with examples
3. **Testing**: Test collection locally before publishing
4. **Dependencies**: Ensure all dependencies are properly declared
5. **Tags**: Use meaningful tags for better discoverability
