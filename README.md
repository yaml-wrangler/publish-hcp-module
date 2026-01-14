# HCP Terraform Module Publisher

A lightweight GitHub Action to publish Terraform modules to the HCP Private Registry.

## Usage

To automatically publish your module whenever a Release is created, add this to `.github/workflows/release.yml`.

**Note on Module Name:** We use `${{ github.event.repository.name }}` below to automatically set the module name to match your GitHub repository name (e.g., `terraform-aws-module`).

## Inputs 
### hcp_token - HCP API Token with "Manage Private Registry" permissions.
### organization - Your HCP Org Name (Case Sensitive).
### module_name - The name of the module in the registry.
### module_provider - The provider (e.g., aws, azurerm).
### version - The version number (e.g., 1.0.0).
### module_path - Path to Terraform files (Default: ./).

```yaml
name: Publish Module

on:
  release:
    types: [published]

jobs:
  publish:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Publish to HCP
        uses: yaml-wrangler/publish-hcp-module@main
        with:
          hcp_token: ${{ secrets.HCP_TERRAFORM_TOKEN }}
          organization: ${{ vars.HCP_ORGANIZATION }}
          module_name: ${{ github.event.repository.name }}
          module_provider: ${{ vars.HCP_MODULE_PROVIDER }}
          version: ${{ github.event.release.tag_name }}
```