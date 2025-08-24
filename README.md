
# {{% heading "Overview" %}}

The `reference-docs` repository provides automated tools and workflows for generating Kubernetes API, CRD, and component documentation. Its purpose is to ensure that reference documentation remains accurate, consistent, and up to date across Kubernetes releases. Contributors use this repo to build, update, and publish docs for core APIs, controllers, CLI tools, and custom resources.

# {{% heading "Prerequisites" %}}

Before using any tools in this repository, ensure you have:
- Go (>=1.18)
- Python (for some scripts)
- Make (for build automation)
- Docker (optional, for containerized builds)
- Access to Kubernetes source code and OpenAPI specs
- Familiarity with Hugo and Kubernetes documentation standards
- Environment variables:
	- `GO111MODULE=on`
	- `GOPATH` (if using legacy Go modules)

# {{% heading "Documentation Generation Workflow" %}}

1. **Configure Source Files:** Place OpenAPI specs, CRD definitions, or config files in the appropriate versioned subfolders.
2. **Run Generators:** Use the provided Go scripts or Makefiles in each subfolder to generate documentation. Most tools support CLI flags for version, format, and output location.
3. **Review and Integrate Output:** Generated docs are written to output folders for integration with the main documentation site.
4. **Customize Templates:** Adjust formatting and sections by editing templates in each tool's folder.

# {{% heading "Folder Breakdown" %}}

- [`gen-apidocs/`](./gen-apidocs/README.md): Generates API reference documentation from OpenAPI specs. [See gen-apidocs documentation]
- [`gen-compdocs/`](./gen-compdocs/README.md): Produces component-level documentation for Kubernetes controllers and services. [See gen-compdocs documentation]
- [`gen-kubectldocs/`](./gen-kubectldocs/README.md): Automates generation of `kubectl` command reference docs across versions. [See gen-kubectldocs documentation]
- [`gen-resourcesdocs/`](./gen-resourcesdocs/README.md): Builds resource-level documentation for core and custom resources. [See gen-resourcesdocs documentation]
- [`genref/`](./genref/README.md): Generates reference docs for APIs and components in multiple formats. [See genref documentation]

# {{% heading "Problems or Limitations" %}}

- Manual updates required for new Kubernetes versions and resources
- Some scripts lack robust error handling and logging
- Output formatting may require manual review and adjustment
- Folder structure can be complex for new contributors

# {{% heading "Suggested Improvements" %}}

- Automate detection and integration of new versions and resources
- Refactor templates and scripts for easier customization
- Improve error reporting and validation across tools
- Streamline folder structure for better usability
- Integrate CI/CD for automated documentation builds

# {{% heading "Benefits" %}}

- Ensures consistent, high-quality reference docs for Kubernetes
- Reduces manual effort for maintainers and contributors
- Supports multiple versions and formats
- Facilitates onboarding and review for new contributors

# {{% heading "Whatsnext" %}}

- Review the documentation pages for each major folder:
	- [gen-apidocs](./gen-apidocs/README.md)
	- [gen-compdocs](./gen-compdocs/README.md)
	- [gen-kubectldocs](./gen-kubectldocs/README.md)
	- [gen-resourcesdocs](./gen-resourcesdocs/README.md)
	- [genref](./genref/README.md)
- Consult the [Kubernetes SIG Docs contribution guide](https://github.com/kubernetes/community/tree/master/sig-docs) for best practices
- Explore integration points in `content/en/docs/Reference/` for publishing generated docs
