# Epsilon Runtime Eclipse - Project Documentation

## Project Overview


**Repository:** BlackBeltTechnology/epsilon-runtime-eclipse
**License:** Eclipse Public License 2.0 (EPL-2.0)
**Java Version:** 21 (Zulu JDK)
**Build System:** Maven 3.9.4 with Tycho 4.0.13

1. Packages the [Eclipse Epsilon](https://www.eclipse.org/epsilon/) model transformation framework (version 2.8) as installable Eclipse features
2. Produces a P2 update site aggregating 112+ artifacts (Epsilon engines, model connectors, and third-party libraries)
3. Provides a custom runtime feature (`epsilon-runtime.feature`) that wraps the `hu.blackbelt.epsilon.runtime-execution` plugin
4. Supports model-to-model transformation, code generation, model validation, comparison, migration, and refactoring using Epsilon language dialects (EOL, ETL, EVL, EGL, EGX, EML, ECL)

## Code Instructions

1. First think through the problem, read the codebase for relevant files.
2. Before you make any major changes, check in with me and I will verify the plan.
3. Please every step of the way just give me a high level explanation of what changes you made.
4. Make every task and code change you do as simple as possible. We want to avoid making any massive or complex changes. Every change should impact as little code as possible. Everything is about simplicity.
5. Maintain a documentation file that describes how the architecture of the app works inside and out.
6. Never speculate about code you have not opened. If the user references a specific file, you MUST read the file before answering. Make sure to investigate and read relevant files BEFORE answering questions about the codebase. Never make any claims about code before investigating unless you are certain of the correct answer - give grounded and hallucination-free answers.
7. For implementation use TDD (Test-Driven Development): write or update tests first to define the expected behaviour, verify they fail, then write the minimal implementation to make them pass.
8. Use DRY (Don't Repeat Yourself): extract reusable logic into separate classes, utilities, or components. If the same pattern appears in multiple places, refactor it into a shared helper.

## Directory Structure

```
epsilon-runtime-eclipse/
├── pom.xml                              # Parent POM (version, modules, profiles, plugin management)
├── .mvn/                                # Maven Wrapper config and JVM settings
├── epsilon-runtime.feature/             # Custom BlackBelt runtime feature
├── org.eclipse.epsilon.core.feature/    # Epsilon core runtime
├── org.eclipse.epsilon.core.dt.feature/ # Epsilon core development tools
├── org.eclipse.epsilon.emf.feature/     # EMF model connector
├── org.eclipse.epsilon.emf.dt.feature/  # EMF development tools
├── org.eclipse.epsilon.emc.*.feature/   # Model connectors (CSV, GraphML, HTML, JDT, Spreadsheets)
├── org.eclipse.epsilon.*.dt.feature/    # Development tool features (matching each connector)
├── org.eclipse.epsilon.flexmi.feature/  # FlexMI support
├── org.eclipse.epsilon.hutn.feature/    # HUTN (Human Usable Textual Notation)
├── org.eclipse.epsilon.picto.feature/   # Model visualization
├── org.eclipse.epsilon.simulink.feature/# Simulink model connector
├── org.eclipse.epsilon.uml.feature/     # UML model connector
├── org.eclipse.epsilon.sirius.widget.feature/ # Sirius widget integration
├── org.eclipse.epsilon.evl.emf.validation.feature/ # EMF validation
├── org.eclipse.epsilon.ewl.emf.feature/ # EWL/EMF integration
├── org.eclipse.epsilon.ewl.gmf.feature/ # GMF editor integration
├── org.eclipse.epsilon.eunit.dt.emf.feature/ # EUnit testing for EMF
├── site/                                # P2 update site (category.xml + aggregation POM)
├── .github/workflows/                   # CI/CD pipelines
└── logback-test.xml                     # Test logging configuration
```

## Core Modules

### Custom Runtime

| Module | Type | Purpose |
|--------|------|---------|
| `epsilon-runtime.feature/` | Eclipse Feature | Wraps `hu.blackbelt.epsilon.runtime-execution` — the BlackBelt Epsilon execution engine plugin |

### Epsilon Core Features

| Module | Type | Purpose |
|--------|------|---------|
| `org.eclipse.epsilon.core.feature/` | Eclipse Feature | Core Epsilon parsers and execution engines |
| `org.eclipse.epsilon.core.dt.feature/` | Eclipse Feature | Eclipse IDE development tools for Epsilon languages |

### EMF Integration Features

| Module | Type | Purpose |
|--------|------|---------|
| `org.eclipse.epsilon.emf.feature/` | Eclipse Feature | EMF-based model management |
| `org.eclipse.epsilon.emf.dt.feature/` | Eclipse Feature | EMF development tools for Eclipse |
| `org.eclipse.epsilon.evl.emf.validation.feature/` | Eclipse Feature | EMF model validation using EVL |
| `org.eclipse.epsilon.ewl.emf.feature/` | Eclipse Feature | EWL wizard integration with EMF |
| `org.eclipse.epsilon.ewl.gmf.feature/` | Eclipse Feature | GMF graphical editor integration |
| `org.eclipse.epsilon.eunit.dt.emf.feature/` | Eclipse Feature | EUnit testing tools for EMF models |
| `org.eclipse.epsilon.flexmi.feature/` | Eclipse Feature | FlexMI flexible model import |
| `org.eclipse.epsilon.flexmi.dt.feature/` | Eclipse Feature | FlexMI development tools |

### Model Connector Features

| Module | Type | Purpose |
|--------|------|---------|
| `org.eclipse.epsilon.emc.csv.feature/` | Eclipse Feature | CSV file model connector |
| `org.eclipse.epsilon.emc.csv.dt.feature/` | Eclipse Feature | CSV development tools |
| `org.eclipse.epsilon.emc.graphml.feature/` | Eclipse Feature | GraphML model connector |
| `org.eclipse.epsilon.emc.html.feature/` | Eclipse Feature | HTML model connector |
| `org.eclipse.epsilon.emc.html.dt.feature/` | Eclipse Feature | HTML development tools |
| `org.eclipse.epsilon.emc.jdt.feature/` | Eclipse Feature | Java source code model connector via JDT |
| `org.eclipse.epsilon.emc.jdt.dt.feature/` | Eclipse Feature | JDT development tools |
| `org.eclipse.epsilon.emc.spreadsheets.feature/` | Eclipse Feature | Spreadsheet model connector (generic) |
| `org.eclipse.epsilon.emc.spreadsheets.excel.feature/` | Eclipse Feature | Excel-specific spreadsheet connector |
| `org.eclipse.epsilon.emc.spreadsheets.excel.dt.feature/` | Eclipse Feature | Excel development tools |

### Additional Features

| Module | Type | Purpose |
|--------|------|---------|
| `org.eclipse.epsilon.hutn.feature/` | Eclipse Feature | HUTN (Human Usable Textual Notation) parser |
| `org.eclipse.epsilon.hutn.dt.feature/` | Eclipse Feature | HUTN development tools |
| `org.eclipse.epsilon.picto.feature/` | Eclipse Feature | Picto model visualization |
| `org.eclipse.epsilon.simulink.feature/` | Eclipse Feature | Simulink model connector |
| `org.eclipse.epsilon.simulink.dt.feature/` | Eclipse Feature | Simulink development tools |
| `org.eclipse.epsilon.sirius.widget.feature/` | Eclipse Feature | Sirius widget for embedding Epsilon in Sirius |
| `org.eclipse.epsilon.uml.feature/` | Eclipse Feature | UML model connector |
| `org.eclipse.epsilon.uml.dt.feature/` | Eclipse Feature | UML development tools |

### Update Site

| Module | Type | Purpose |
|--------|------|---------|
| `site/` | P2 Site | Aggregates all features + 112 third-party artifacts into a P2 repository; uses `p2-maven-plugin` and `category.xml` for categorization |

## Technology Stack

### Core Technologies
- **Eclipse Epsilon 2.8** — upstream model transformation framework (sourced from `download.eclipse.org/epsilon/updates/2.8`)
- **Eclipse Tycho 4.0.13** — Maven plugin for building Eclipse plugins, features, and P2 sites
- **Eclipse EMF** — core modeling framework required by Epsilon
- **p2-maven-plugin 2.1.0** — generates the P2 update site from Maven artifacts

### Build & Quality
- **Maven 3.9.4** with Maven Wrapper (`./mvnw`)
- **flatten-maven-plugin 1.1.0** — CI-friendly `${revision}` versioning
- **JaCoCo 0.8.12** — code coverage
- **SonarQube (sonar-maven-plugin 3.9.1.2184)** — static analysis
- **Surefire 3.5.1** — test execution with JDK 21 module opens
- **Logback 1.5.12 / SLF4J 2.0.15** — logging
- **Lombok 1.18.34** — boilerplate reduction (with delombok plugin)

## Build Commands

```sh
# Full build (all modules + P2 site)
./mvnw clean install

# Run tests only
./mvnw clean test

# Build a single module
./mvnw clean install -pl <module-name>

# Skip tests
./mvnw clean install -DskipTests

# Build with artifact signing
./mvnw clean install -Psign-artifacts

# Deploy to judong Nexus
./mvnw clean deploy -Prelease-judong

# Upload P2 site to judong
./mvnw clean deploy -Prelease-p2-judong
```

### Maven Profiles

| Profile | Purpose |
|---------|---------|
| `sign-artifacts` | GPG-sign artifacts using `sign-maven-plugin` |
| `release-dummy` | Deploy to local `/tmp/` directory for testing |
| `release-judong` | Deploy to judong Nexus (`nexus.judo.technology`) |
| `release-central` | Deploy to Maven Central via Sonatype OSSRH |
| `release-p2-judong` | Upload P2 repository to judong Nexus via WebDAV |
| `generate-github-asciidoc-diagrams` | Generate PNG diagrams from AsciiDoc using PlantUML |
| `update-source-code-license` | Update EPL-2.0 license headers in all source files |

## Key Configuration Files

| File | Purpose |
|------|---------|
| `pom.xml` | Parent POM: module list, dependency management, plugin configuration, profiles |
| `.mvn/jvm.config` | JVM arguments for Maven builds (`-Xms1024m -Xmx2048m`, `--add-opens` flags) |
| `.mvn/extensions.xml` | Maven wagon extensions for file and WebDAV artifact transport |
| `logback-test.xml` | Logback configuration for test execution (console appender, INFO level) |
| `site/category.xml` | P2 update site category definitions (17 categories) |
| `site/pom.xml` | P2 site generation: 112+ artifact list, p2-maven-plugin configuration |
| `*/src/main/resources/feature.xml` | Eclipse feature descriptors (included plugins, imported dependencies) |
| `.github/workflows/build.yml` | Main CI pipeline: build, test, deploy, release |
| `.github/workflows/release.yml` | Release orchestration: creates PRs to master and develop |

## Development Environment

**Required:**
- Java 21 JDK (Zulu distribution recommended)
- Maven 3.9.4+ (or use the included `./mvnw` wrapper)
- Git

**Optional:**
- Eclipse IDE (for working with feature.xml files)
- Access to `nexus.judo.technology` (for deployment)

## Git Workflow

- **Main Branch:** `develop`
- **Stable Branch:** `master`
- **Versioning:** `2.8.0-SNAPSHOT` using `${revision}` CI-friendly versioning
- **Strategy:** GitFlow — feature, release, bugfix, support, and hotfix branches
- **Commit Rule:** Every commit must reference a JIRA ticket (`JNG-xxx`)
- **Branch Naming:** `feature/JNG-NUMBER_short_summary`, `bugfix/JNG-NUMBER_short_summary`
- **Issue Tracking:** [JIRA](https://blackbelt.atlassian.net/jira/) (JNG project)

## Important Notes

1. This project contains **no custom Java source code in most modules** — the feature modules are descriptors that package upstream Eclipse Epsilon plugins. The only custom plugin is `hu.blackbelt.epsilon.runtime-execution` referenced by `epsilon-runtime.feature`.
2. The `site/pom.xml` is the most complex file (800+ lines) and defines the full artifact dependency graph for the P2 site. Changes to Epsilon version or adding new features typically require edits here.
3. Version updates are handled via CI — the `${revision}` property in the parent POM is overridden by the build pipeline. Do not hardcode version numbers in module POMs.
4. The build requires significant memory (`-Xmx2048m`) due to the size of the P2 site generation.
5. Upstream Epsilon artifacts are sourced from the P2 repository at `https://download.eclipse.org/epsilon/updates/2.8` — the version is controlled by the `epsilon-version` property (`2.8.0.202502191012`).

## Related Documentation

- [README.md](README.md) — Project overview and Epsilon language reference
- [CONTRIBUTING.md](CONTRIBUTING.md) — Development setup and submission guidelines
- [CI Flow](.github/CIFLOW.md) — Branching strategy, version numbering, and CI/CD pipeline details
- [Eclipse Epsilon Book](https://www.eclipse.org/epsilon/doc/book/) — Comprehensive guide to Epsilon languages
