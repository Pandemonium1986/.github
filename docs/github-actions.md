# GitHub actions convention

## Files name

### Naming convention

A workflow file in the `.github/workflows` folder **MUST** be named by its technology or the service it provides.

- .github/workflows/docker.yml
- .github/workflows/molecule.yml
- .github/workflows/linter.yml

The name of the workflow file **MAY** contain the target provided by the technology or service it provides

- .github/workflows/docker-build-and-run.yml
- .github/workflows/molecule-test.yml
- .github/workflows/ansible-run-playbook.yml

### About called workflows

A called-workflow file **MUST** be named in the `.github/workflows` of pandemonium1986's `.github` repo and **MUST** be named with prefix `called-`.

- .github/workflows/called-linter.yml
- .github/workflows/called-docker.yml
- .github/workflows/called-molecule.yml

## Workflows, jobs and steps name

### Workflows naming convention

A workflow in the `.github/workflows` folder **MUST** be named by the technology it provides suffixe with `Workflow`.

```yml
name: "GitHub Super-Linter Workflow"
name: "Docker Workflow"
name: "Molecule Workflow"
```

### Jobs naming convention

A job within a workflow **MUST** be titled by the technology and named by the service it provides suffixed with the technology as a prefix.

```yml
jobs:
  lint:
    name: "Gsl: Lint code base"
  docker:
    name: "Docker: Build and push"
  molecule:
    name: "Molecule: All in one"
```

### Steps naming convention

A step within a job **MUST** be named by the service it provides suffixed with the technology as a prefix.

```yml
steps:
  - name: "Init: Run checkout"
    uses: actions/checkout@v2
  - name: "Molecule: Create"
    run: molecule create
  - name: "Molecule: Converge"
    run: molecule converge
  - name: "Molecule: Idempotence"
    run: molecule idempotence
  - name: "Molecule: Verify"
    run: molecule verify
  - name: "Molecule: Destroy"
    run: molecule destroy
```

### Called workflows naming convention

A called-workflow follow the same convention than standard worflow and **MUST** be named with prefix `Called:`.

```yml
jobs:
  called-linter:
    name: "Called: GitHub Super-Linter Workflow"
```
