# {{% heading "Overview" %}}

The `gen-apidocs` folder contains tools and configuration files for generating Kubernetes API reference documentation. It automates the extraction and formatting of API details from OpenAPI specifications, providing consistent, versioned docs for Kubernetes resources.

# {{% heading "Prerequisites" %}}

- Go (>=1.18)
- Docker (optional, for containerized builds)
- Access to Kubernetes OpenAPI specs (swagger.json)
- Environment variables:
  - `GO111MODULE=on`
  - `GOPATH` set if using legacy Go modules
- Recommended: Familiarity with Hugo and Kubernetes documentation conventions

# {{% heading "Usage / Process" %}}

1. **Configure API Versions:**
   - Place OpenAPI specs (`swagger.json`) and config files in the appropriate `v1_xx/` subfolder under `config/`.
2. **Run the Generator:**
   - Build and run `main.go` to generate docs:
     ```bash
     cd gen-apidocs
     go run main.go
     ```
   - Optionally, use Docker for isolated builds.
3. **Customize Output:**
   - Edit templates in `generators/` or config files in `config/` to adjust formatting or sections.
4. **Review Generated Docs:**
   - Output is typically written to a designated folder for integration with the main docs site.

## Example CLI Usage
```bash
# Generate docs for a specific version
GO111MODULE=on go run main.go --version v1_33
```

# {{% heading "Problems or Limitations" %}}

- Limited support for non-standard OpenAPI extensions
- Manual updates required for new Kubernetes versions
- Error handling and logging could be improved
- Some config files may lack documentation or examples

# {{% heading "Suggested Improvements" %}}

- Automate version detection and config updates
- Enhance error reporting and validation
- Add more usage examples and CLI flags documentation
- Refactor templates for easier customization
- Integrate with CI/CD for automated doc generation

# {{% heading "Benefits" %}}

- Ensures up-to-date, consistent API reference docs
- Reduces manual effort for doc generation
- Supports multiple Kubernetes versions
- Facilitates contributor onboarding and review

# {{% heading "Whatsnext" %}}

- See `gen-compdocs` for component documentation generation
- Review `content/en/docs/Reference/` for integration points
- Consult [Kubernetes SIG Docs contribution guide](https://github.com/kubernetes/community/tree/master/sig-docs) for best practices
- Explore `genref/` for additional reference doc tooling
