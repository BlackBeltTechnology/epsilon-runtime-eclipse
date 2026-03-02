# p2-update-site Specification

## Purpose
The `site` module aggregates all Eclipse features and 112+ third-party artifacts into a P2 update site repository, organized into 17 categories, that can be used to install Epsilon into Eclipse-based products.

## Architecture
- **`site/pom.xml`** — Maven POM using `p2-maven-plugin` (v2.1.0) to generate the P2 repository during the `package` phase
- **`site/category.xml`** — Defines the 17 category groupings visible to users in Eclipse's "Install New Software" dialog
- **Output:** `site/target/repository/` — the generated P2 repository containing `content.jar`, `artifacts.jar`, and all plugin/feature JARs
- **Deployment:** Uploaded to `https://nexus.judo.technology/repository/p2-judong/epsilon-runtime-eclipse/<version>/` via the `release-p2-judong` Maven profile

### Categories Defined in category.xml

| Category ID | Label |
|-------------|-------|
| `epsilon_runtime` | Epsilon Runtime |
| `epsilon_runtime_source` | Epsilon Runtime Source |
| Epsilon Core | Parsers, execution engines, dev tools |
| Epsilon EMF Integration | EMF-based model management |
| Epsilon UML Integration | UML model management |
| Epsilon Simulink Integration | Simulink model management |
| Epsilon GMF Integration | GMF editor development |
| HUTN | Human Usable Textual Notation |
| Epsilon Spreadsheet Integration | Spreadsheet model support |
| Epsilon HTML Integration | HTML model support |
| Epsilon JDT Integration | Java source code management |
| Epsilon Sirius Integration | Sirius embedding |
| Epsilon CDO Integration | CDO repository connection |
| Epsilon JSON Integration | JSON model support |
| Epsilon YAML Integration | YAML model support |
| Picto | Model visualization |
| Epsilon Debug Adapter Support | Debug Adapter protocol |

## Requirements

### Requirement: Site SHALL aggregate all project features
The P2 update site SHALL include every Eclipse feature defined in the parent POM's module list.

#### Scenario: All features present in repository
- **GIVEN** the `site` module is built with `./mvnw clean package`
- **WHEN** the generated `target/repository/` is inspected
- **THEN** all 29 feature modules are present as installable units

### Requirement: Site SHALL include all required third-party bundles
The P2 repository SHALL bundle third-party libraries (Apache Commons, Guava, Logback, SLF4J, ANTLR, Apache POI, JSoup, etc.) so that features can be installed without external dependency resolution.

#### Scenario: Self-contained installation
- **GIVEN** a clean Eclipse installation with no additional update sites configured
- **WHEN** a user adds only this P2 site and installs the Epsilon Core feature
- **THEN** all transitive dependencies (ANTLR, SLF4J, Logback, Guava, Commons) are resolved from this site

### Requirement: Site SHALL organize features into categories
The `category.xml` SHALL assign each feature to a descriptive category for user-friendly browsing.

#### Scenario: Browse categories in Eclipse
- **GIVEN** the P2 update site URL is added to Eclipse
- **WHEN** a user opens "Install New Software" and selects this site
- **THEN** features are grouped under 17 named categories

### Requirement: Site SHALL be deployable to judong Nexus
The `release-p2-judong` profile SHALL upload the generated P2 repository to the judong Nexus P2 repository via WebDAV.

#### Scenario: Deploy P2 site
- **GIVEN** Nexus credentials are configured in Maven settings
- **WHEN** `./mvnw clean deploy -Prelease-p2-judong` is executed
- **THEN** the P2 repository is uploaded to `https://nexus.judo.technology/repository/p2-judong/epsilon-runtime-eclipse/<version>/`
