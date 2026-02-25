# epsilon-emf-features Specification

## Purpose
The EMF integration features provide Eclipse Modeling Framework support for Epsilon, enabling model management, validation, wizard integration, GMF editor support, FlexMI import, and EUnit testing against EMF models.

## Architecture
- **`org.eclipse.epsilon.emf.feature`** — EMF model connector runtime
- **`org.eclipse.epsilon.emf.dt.feature`** — EMF development tools for Eclipse
- **`org.eclipse.epsilon.evl.emf.validation.feature`** — EVL-based EMF model validation
- **`org.eclipse.epsilon.ewl.emf.feature`** — EWL wizard integration with EMF
- **`org.eclipse.epsilon.ewl.gmf.feature`** — GMF graphical editor integration
- **`org.eclipse.epsilon.eunit.dt.emf.feature`** — EUnit testing tools for EMF
- **`org.eclipse.epsilon.flexmi.feature`** / `flexmi.dt.feature` — FlexMI flexible model import

## Requirements

### Requirement: EMF feature SHALL enable Epsilon operations on EMF models
The `org.eclipse.epsilon.emf.feature` SHALL provide the EMF model driver, allowing Epsilon languages to query and modify EMF-based models.

#### Scenario: Load and query an EMF model with EOL
- **GIVEN** the EMF feature is installed and an `.ecore` model is available
- **WHEN** an EOL script references the EMF model
- **THEN** the EMF driver loads the model and makes its elements accessible to EOL

### Requirement: EVL validation feature SHALL integrate with Eclipse validation framework
The `org.eclipse.epsilon.evl.emf.validation.feature` SHALL register EVL constraints as Eclipse EMF validation rules.

#### Scenario: EMF validation triggers EVL constraints
- **GIVEN** an EMF model has EVL constraints defined
- **WHEN** Eclipse's "Validate" action is invoked on the model
- **THEN** EVL constraints are evaluated and violations are reported as Eclipse markers

### Requirement: FlexMI feature SHALL support flexible model import
The `org.eclipse.epsilon.flexmi.feature` SHALL allow loading models from FlexMI's YAML/XML-like syntax into EMF.

#### Scenario: Parse a FlexMI file
- **GIVEN** the FlexMI feature is installed
- **WHEN** a `.flexmi` file is loaded
- **THEN** the file is parsed into an in-memory EMF model
