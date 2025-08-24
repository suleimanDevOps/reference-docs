# {{% heading "Overview" %}}

The `gen-kubectldocs` folder contains tools and scripts for generating reference documentation for the `kubectl` command-line tool. It automates the extraction, formatting, and organization of command documentation across multiple Kubernetes versions, ensuring consistency and accuracy in the published docs.

# {{% heading "Prerequisites" %}}

- Go (>=1.18)
- Docker (optional, for containerized builds)
- Access to Kubernetes `kubectl` source or command definitions
- Environment variables:
  - `GO111MODULE=on`
  - `GOPATH` (if using legacy Go modules)
- Familiarity with Hugo and Kubernetes documentation standards

# {{% heading "Usage / Process" %}}

1. **Configure Versions:**
   - Place version-specific files (e.g., `toc.yaml`, static markdown includes) in the appropriate `v1_xx/` subfolder under `generators/`.
2. **Run the Generator:**
   - Build and run `main.go` to generate docs:
     ```bash
     cd gen-kubectldocs
     go run main.go
     ```
   - Optionally, use Docker for isolated builds.
3. **Customize Output:**
   - Edit templates in `generators/` or config files to adjust formatting or sections.
4. **Review Generated Docs:**
   - Output is typically written to a designated folder for integration with the main docs site.

## Example CLI Usage
```bash
# Generate docs for a specific kubectl version
GO111MODULE=on go run main.go --version v1_26
```

# {{% heading "Problems or Limitations" %}}

- Manual updates required for new `kubectl` versions
- Limited support for custom or third-party commands
- Some config files and templates may lack documentation
- Error handling and logging could be improved

# {{% heading "Suggested Improvements" %}}

- Automate detection and integration of new `kubectl` versions
- Enhance error reporting and validation
- Add more usage examples and CLI flags documentation
- Refactor templates for easier customization
- Integrate with CI/CD for automated doc generation

# {{% heading "Benefits" %}}

- Ensures up-to-date, consistent `kubectl` reference docs
- Reduces manual effort for doc generation
- Supports multiple Kubernetes versions
- Facilitates contributor onboarding and review

# {{% heading "Whatsnext" %}}

- See `gen-apidocs` for API reference doc generation
- See `gen-compdocs` for component documentation generation
- Review `content/en/docs/Reference/` for integration points
- Consult [Kubernetes SIG Docs contribution guide](https://github.com/kubernetes/community/tree/master/sig-docs) for best practices
- Explore `genref/` for additional reference doc tooling
