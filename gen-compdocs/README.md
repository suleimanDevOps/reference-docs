# {{% heading "Overview" %}}

The `gen-compdocs` folder provides tools and scripts for generating Kubernetes component documentation. It automates the extraction, formatting, and post-processing of component-level docs, supporting maintainers and contributors in keeping documentation up to date and consistent.

# {{% heading "Prerequisites" %}}

- Go (>=1.18)
- Make (for build automation)
- Access to Kubernetes component source or API definitions
- Environment variables:
  - `GO111MODULE=on`
  - `GOPATH` (if using legacy Go modules)
- Familiarity with Hugo and Kubernetes documentation standards

# {{% heading "Usage / Process" %}}

1. **Build the Generator:**
   - Use the Makefile or run Go directly:
     ```bash
     cd gen-compdocs
     make
     # or
     go run main.go
     ```
2. **Generate Component Docs:**
   - The main entry point is `main.go`, which orchestrates doc generation using scripts in `comps/` and `generators/`.
   - Example:
     ```bash
     go run main.go --component kubelet
     ```
3. **Post-process Output:**
   - Use scripts in `generators/` for formatting and cleanup.
   - Output is typically integrated with the main documentation site.

## Example CLI Usage
```bash
# Generate docs for a specific component
GO111MODULE=on go run main.go --component kube-apiserver
```

# {{% heading "Problems or Limitations" %}}

- Manual updates required for new components or flags
- Limited error handling and logging
- Some scripts may lack documentation or usage examples
- Output formatting may require manual review

# {{% heading "Suggested Improvements" %}}

- Automate detection of new components and flags
- Improve error reporting and validation
- Add more CLI usage examples and flag documentation
- Refactor post-processing scripts for maintainability
- Integrate with CI/CD for automated doc generation

# {{% heading "Benefits" %}}

- Streamlines component documentation updates
- Reduces manual effort for maintainers
- Supports consistent, high-quality docs across components
- Facilitates onboarding for new contributors

# {{% heading "Whatsnext" %}}

- See `gen-apidocs` for API reference doc generation
- Review `content/en/docs/Reference/` for integration points
- Consult [Kubernetes SIG Docs contribution guide](https://github.com/kubernetes/community/tree/master/sig-docs) for best practices
- Explore `genref/` for additional reference doc tooling
