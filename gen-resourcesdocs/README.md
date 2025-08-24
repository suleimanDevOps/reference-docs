# gen-resourcesdocs

This directory contains tools and scripts for generating resource-level documentation for Kubernetes. It automates extraction, formatting, and organization of resource definitions, configuration examples, and API details across multiple Kubernetes versions.

# {{% heading "Overview" %}}

The `gen-resourcesdocs` folder provides tools and scripts for generating resource-level documentation for Kubernetes. It automates the extraction, formatting, and organization of resource definitions, configuration examples, and API details across multiple Kubernetes versions, supporting maintainers and contributors in keeping resource docs accurate and up to date.

# {{% heading "Prerequisites" %}}

- Go (>=1.18)
- Make (for build automation)
- Access to Kubernetes resource API definitions
- Environment variables:
  - `GO111MODULE=on`
  - `GOPATH` (if using legacy Go modules)
- Familiarity with Hugo and Kubernetes documentation standards

# {{% heading "Usage / Process" %}}

1. **Configure Resource Versions:**
  - Place version-specific API and config files in the appropriate subfolders under `api/` and `config/`.
2. **Build and Run the Generator:**
  - Use the Makefile or run Go directly:
    ```bash
    cd gen-resourcesdocs
    make
    # or
    go run cmd/main.go
    ```
3. **Customize Output:**
  - Edit templates in `templates/` or config files to adjust formatting or sections.
4. **Review Generated Docs:**
  - Output is typically written to a designated folder for integration with the main docs site.

## Example CLI Usage
```bash
# Generate docs for a specific resource version
GO111MODULE=on go run cmd/main.go --version v1.33
```

# {{% heading "Problems or Limitations" %}}

- Manual updates required for new resource versions
- Limited error handling and logging
- Some config files and templates may lack documentation
- Output formatting may require manual review

# {{% heading "Suggested Improvements" %}}

- Automate detection and integration of new resource versions
- Improve error reporting and validation
- Add more CLI usage examples and flag documentation
- Refactor templates for easier customization
- Integrate with CI/CD for automated doc generation

# {{% heading "Benefits" %}}

- Streamlines resource documentation updates
- Reduces manual effort for maintainers
- Supports consistent, high-quality docs across resources and versions
- Facilitates onboarding for new contributors

# {{% heading "Whatsnext" %}}

- See `gen-apidocs` for API reference doc generation
- See `gen-compdocs` for component documentation generation
- See `gen-kubectldocs` for kubectl command documentation generation
- Review `content/en/docs/Reference/` for integration points
- Consult [Kubernetes SIG Docs contribution guide](https://github.com/kubernetes/community/tree/master/sig-docs) for best practices
- Explore `genref/` for additional reference doc tooling
# Kubernetes API resources documentation generator

This tool extracts information from the OpenAPI specification file of the [Kubernetes API](https://github.com/kubernetes/kubernetes/blob/master/api/openapi-spec/swagger.json) and creates documentation in Markdown format, suitable for the [Kubernetes website](https://kubernetes.io/docs/reference/kubernetes-api/).

## Outline

The documentation is split into *parts*. Each part can contain any number of *chapters*. A chapter describes:

- a main Kubernetes resource,
- any resources and definitions associated with the main resource,
- any *Operations* operating on the resources documented in the chapter.

The parts and chapters are defined in the `config/<version>/toc.yaml` file.

```yaml
parts:
- name: Part I
  chapters:
  - name: Deployment
    group: apps
    version: v1
  - name: Pod
    group: ""
    version: v1
    otherDefinitions:
    - PodSpec
    - Container
    - Handler
    - NodeAffinity
    - PodAffinity
    - PodAntiAffinity
    - Probe
    - PodStatus
    - PodList
- name: Part II
  chapters:
  - name: Service
    group: ""
    version: v1
```

In this example, the first part contains two chapters and the second part one chapter.

The first chapter describes the main `Deployment` resource (from the `apps` group and the `v1` version) and its associated resources and definitions. By default, if no `otherDefinitions` are defined, the associated resources are the `List` resource and the `Spec` and `Status` definitions, if appropriate. In this case, `Deployment`, `DeploymentList`, `DeploymentSpec` and `DeploymentStatus` are documented.

The second chapter first describes the main `Pod` resource. The other resources and definitions documented in the chapter are listed in the `otherDefinitions` field.

## Definition Documentation

For each definition (including resources, which are definitions attached to a Group/Version), the fields are listed, with their type and documentation.

### Documentation of composite types

If the type of a field is a composite type, the Definition of this composite type is documented either inline if this Definition is not documented elsewhere, or the type name is a link pointing to the definition of the composite type.

```
PodSpec

- affinity (Affinity)   <-- composite type

  If specified, the pod's scheduling constraints
                                                              --\
  Affinity is a group of affinity scheduling rules.             |
                                                                |
  - affinity.nodeAffinity (NodeAffinity) <-- link to definition |
                                                                |
    Describes node affinity scheduling rules for the pod.       |    Inline
                                                                | definition of
  - affinity.podAffinity (PodAffinity) <-- link to definition   |    Affinity
                                                                | composite type
    Describes pod affinity scheduling rules (e.g. co-locate this|
    pod in the same node, zone, etc. as some other pod(s)).     |
                                                                |
  - affinity.podAntiAffinity (PodAntiAffinity) <-- link to def. |
                                                                |
    Describes pod anti-affinity scheduling rules (e.g. avoid    |
    putting this pod in the same node, zone, etc. as some other |
    pod(s)).                                                  --/
```

In this example, the `affinity` field type is `Affinity`, which is a composite type. Because the `Affinity` definition is not listed in any of the `otherDefinitions`, this definition is documented inline.

The `nodeAffinity` field type of the `Affinity` composite type is `NodeAffinity`, which is listed in a `otherDefinitions`. For this reason, this composite type is not documented inline, but the `NodeAffinity` is a link to the definition elsewhere in the documentation. Same for the `podAffinity` and `PodAntiAffinity` fields.

### Ordering and categorization of fields

By default, the fields of a Definition are rendered in alphabetic order. The `content/<version>/fields.yaml` file is used to order the fields in another order, and also to group fields into categories.

```yaml
- definition: io.k8s.api.core.v1.PodSpec
  field_categories:
  - name: Containers
    fields:
    - containers
    - initContainers
    - imagePullSecrets
    - enableServiceLinks
  - name: Volumes
    fields:
    - volumes
  - name: Scheduling
    fields:
    - nodeSelector
    - nodeName
    - affinity
    - tolerations
    - schedulerName
    - runtimeClassName
    - priorityClassName
    - priority
  [...]
```

In this example, the fields of the `PodSpec` definition are grouped and ordered in *Containers*, *Volumes*, *Scheduling* and other categories.

Note that if a Definition appears in the `fields.yaml` file and some fields of the definition do not appear in the list of fields, the program will indicate that these fields are missing.

The `name` attribute of `field_categories` in `fields.yaml` is optional. You can omit `name` when you want either to specify the order of the fields for a Definition  without creating a category, or to place some fields outside of any category, before other categories.

## Common definitions

The `toc.yaml` file defines a **Common Definitions** part, containing a list of Definitions that are used in different places in the Kubernetes API.

This way, a composite type can be documented in three places:

- inline where this composite type is used (by default),
- in a specific chapter, if the Definition is listed in `otherDefinitions`,
- in the **Common Definitions** part.
