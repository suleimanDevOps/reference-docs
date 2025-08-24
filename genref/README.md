# Config API generator

This is a Config API reference generator that uses the
[k8s.io/gengo](https://godoc.org/k8s.io/gengo) project to parse types and
generate API documentation from it.

This tool relies only on the Go source code containing the type definitions.
For references to external type definitions, especially those from the
`k8s.io/kubernetes` repo, you can tell the tool to generate links to the
existing docs, which can be the GoDocs if no better option exists.

## Try it out

1. Clone this repository.

2. Make sure you have go1.15+ installed. Run `make` to build the `genref`
   binary:

   ```
   cd genref

   # {{% heading "Overview" %}}

   The `genref` folder provides tools and scripts for generating reference documentation for Kubernetes APIs and components. It supports multiple output formats (HTML, Markdown) and versions, enabling maintainers to produce consistent, up-to-date reference docs for a wide range of Kubernetes objects and configurations.

   # {{% heading "Prerequisites" %}}

   - Go (>=1.18)
   - Make (for build automation)
   - Access to Kubernetes API definitions and configs
   - Environment variables:
     - `GO111MODULE=on`
     - `GOPATH` (if using legacy Go modules)
   - Familiarity with Hugo and Kubernetes documentation standards

   # {{% heading "Usage / Process" %}}

   1. **Configure Reference Sources:**
      - Place API/config files in the appropriate location or update `config.yaml` as needed.
   2. **Build and Run the Generator:**
      - Use the Makefile or run Go directly:
        ```bash
        cd genref
        make
        # or
        go run main.go
        ```
   3. **Customize Output:**
      - Edit templates in `html/` or `markdown/` to adjust formatting or sections.
   4. **Review Generated Docs:**
      - Output is written to `output/html/` and `output/md/` for integration with the main docs site.

   ## Example CLI Usage
   ```bash
   # Generate reference docs in Markdown format
   GO111MODULE=on go run main.go --format markdown
   ```

   # {{% heading "Problems or Limitations" %}}

   - Manual updates required for new API versions
   - Limited error handling and logging
   - Some templates may lack documentation or examples
   - Output formatting may require manual review

   # {{% heading "Suggested Improvements" %}}

   - Automate detection and integration of new API versions
   - Improve error reporting and validation
   - Add more CLI usage examples and flag documentation
   - Refactor templates for easier customization
   - Integrate with CI/CD for automated doc generation

   # {{% heading "Benefits" %}}

   - Streamlines reference documentation updates
   - Reduces manual effort for maintainers
   - Supports consistent, high-quality docs across APIs and versions
   - Facilitates onboarding for new contributors

   # {{% heading "Whatsnext" %}}

   - See `gen-apidocs` for API reference doc generation
   - See `gen-compdocs` for component documentation generation
   - See `gen-kubectldocs` for kubectl command documentation generation
   - See `gen-resourcesdocs` for resource documentation generation
   - Review `content/en/docs/Reference/` for integration points
   - Consult [Kubernetes SIG Docs contribution guide](https://github.com/kubernetes/community/tree/master/sig-docs) for best practices

   # {{% heading "Changes and Improvements" %}}

   - Updated documentation to follow Kubernetes style guide and Hugo shortcodes
   - Added clear section headings for Overview, Prerequisites, Usage, Problems, Improvements, Benefits, and Whatsnext
   - Provided concise CLI usage examples and config guidance
   - Summarized and reorganized content for clarity and contributor onboarding

### Specify the output format

The tool can generate HTML pages directly or Markdown files if needed.
You can specify the output format using the `-f` flag. For example,

```shell
./genref -f markdown -include kubelet-config
```

### Customize the output template

The tool uses GoLang templates to generate HTML or Markdown.  The HTML
templates are under the `html/` subdirectory.  For Markdown, the templates are
under the `markdown/` subdirectory.

## Credit

This project is inspired and largely based on the
[gen-crd-api-reference-docs](https://github.com/ahmetb/gen-crd-api-reference-docs)
tool developed by Ahmet Alp Balkan (@ahmetb).

The tool was reworked for:

- refactored code structure;
- support to YAML config file so we can put comments in it;
- added (and tested) source for parsing Kubernetes unpublished API types;
- allow for parsing across packages which have emerged in many projects;
- use go.mod to manage package version for parsing;
- output styling changes to better align with Kubernetes API reference docs.
 
## TODOs

- [ ] Allow user to specify the top level structs.
- [ ] Add description to each API
- [ ] Mark packages that are used in the website so that they can be
      treated in a different way than other versions.

