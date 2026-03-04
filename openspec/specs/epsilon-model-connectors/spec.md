# epsilon-model-connectors Specification

## Purpose
The model connector features (EMC — Epsilon Model Connectivity) provide drivers that allow Epsilon languages to interact with non-EMF data sources: CSV files, GraphML graphs, HTML documents, Java source code (via JDT), spreadsheets (generic and Excel-specific), Simulink models, and UML models.

## Architecture
Each connector consists of a runtime feature and an optional development tools (`.dt`) feature:

| Connector | Runtime Feature | DT Feature | Data Source |
|-----------|----------------|------------|-------------|
| CSV | `org.eclipse.epsilon.emc.csv.feature` | `org.eclipse.epsilon.emc.csv.dt.feature` | CSV files |
| GraphML | `org.eclipse.epsilon.emc.graphml.feature` | — | GraphML XML files |
| HTML | `org.eclipse.epsilon.emc.html.feature` | `org.eclipse.epsilon.emc.html.dt.feature` | HTML documents (via JSoup) |
| JDT | `org.eclipse.epsilon.emc.jdt.feature` | `org.eclipse.epsilon.emc.jdt.dt.feature` | Java source code via Eclipse JDT |
| Spreadsheets | `org.eclipse.epsilon.emc.spreadsheets.feature` | — | Generic spreadsheet abstraction |
| Excel | `org.eclipse.epsilon.emc.spreadsheets.excel.feature` | `org.eclipse.epsilon.emc.spreadsheets.excel.dt.feature` | Excel files (via Apache POI) |
| Simulink | `org.eclipse.epsilon.simulink.feature` | `org.eclipse.epsilon.simulink.dt.feature` | MATLAB/Simulink models |
| UML | `org.eclipse.epsilon.uml.feature` | `org.eclipse.epsilon.uml.dt.feature` | Eclipse UML2 models |

## Requirements

### Requirement: Each connector SHALL expose its data source as an Epsilon-queryable model
Each EMC driver SHALL implement the Epsilon model interface, allowing EOL and other Epsilon languages to query and modify the underlying data source.

#### Scenario: Query CSV data with EOL
- **GIVEN** the CSV connector feature is installed and a CSV file is registered as a model
- **WHEN** an EOL script queries `Row.all`
- **THEN** all CSV rows are returned as model elements with column-based properties

#### Scenario: Query HTML document with EOL
- **GIVEN** the HTML connector feature is installed and an HTML file is registered as a model
- **WHEN** an EOL script queries HTML elements
- **THEN** DOM elements are accessible as model elements via JSoup

#### Scenario: Query Excel spreadsheet with EOL
- **GIVEN** the Excel connector feature is installed and an `.xlsx` file is registered as a model
- **WHEN** an EOL script queries spreadsheet rows
- **THEN** Excel rows and cells are accessible as model elements via Apache POI

### Requirement: DT features SHALL provide Eclipse launch configuration support
Each `.dt.feature` SHALL contribute Eclipse launch configuration types for its model connector.

#### Scenario: Configure CSV model in Eclipse launch dialog
- **GIVEN** the `org.eclipse.epsilon.emc.csv.dt.feature` is installed
- **WHEN** a user creates an Epsilon launch configuration
- **THEN** "CSV Model" appears as a model type option in the configuration dialog

### Requirement: Connectors SHALL be independently installable
Each connector feature SHALL be installable without requiring other connector features (only core Epsilon dependencies).

#### Scenario: Install only the HTML connector
- **GIVEN** only `org.eclipse.epsilon.emc.html.feature` and `org.eclipse.epsilon.core.feature` are selected
- **WHEN** the installation resolves dependencies
- **THEN** the installation succeeds without requiring CSV, Excel, or other connectors
