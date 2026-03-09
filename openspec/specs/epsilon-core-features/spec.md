# epsilon-core-features Specification

## Purpose
The core feature modules (`org.eclipse.epsilon.core.feature` and `org.eclipse.epsilon.core.dt.feature`) package the upstream Eclipse Epsilon parsers, execution engines, and Eclipse development tools as installable features.

## Architecture
- **`org.eclipse.epsilon.core.feature`** — Runtime feature containing Epsilon language engines (EOL, ETL, EVL, EGL, EGX, EML, ECL, EPL, ERL, EMG)
- **`org.eclipse.epsilon.core.dt.feature`** — Development tools feature providing Eclipse IDE editors, launch configurations, and debugging support for Epsilon languages
- Both features reference plugins from the upstream Epsilon 2.8 P2 repository at `https://download.eclipse.org/epsilon/updates/2.8`

## Requirements

### Requirement: Core feature SHALL include all Epsilon language engines
The `org.eclipse.epsilon.core.feature` SHALL include runtime plugins for all supported Epsilon dialects.

#### Scenario: EOL execution available
- **GIVEN** the `org.eclipse.epsilon.core.feature` is installed
- **WHEN** an EOL script is submitted for execution
- **THEN** the EOL engine processes the script and returns results

#### Scenario: ETL transformation available
- **GIVEN** the `org.eclipse.epsilon.core.feature` is installed
- **WHEN** an ETL transformation is executed
- **THEN** the ETL engine performs model-to-model transformation

### Requirement: Development tools feature SHALL provide Eclipse IDE integration
The `org.eclipse.epsilon.core.dt.feature` SHALL include editors, syntax highlighting, and launch configurations for Epsilon languages.

#### Scenario: Epsilon editors registered in Eclipse
- **GIVEN** the `org.eclipse.epsilon.core.dt.feature` is installed in Eclipse
- **WHEN** a user opens a `.eol`, `.etl`, `.evl`, or `.egl` file
- **THEN** the corresponding Epsilon editor is activated with syntax highlighting

### Requirement: Core features SHALL be compatible with Epsilon version 2.8
Both features SHALL reference plugins from the Epsilon 2.8.0.202502191012 release.

#### Scenario: Version alignment
- **GIVEN** the parent POM defines `epsilon-version` as `2.8.0.202502191012`
- **WHEN** features are built
- **THEN** all resolved Epsilon plugins match the 2.8.x version range
