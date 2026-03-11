# epsilon-additional-features Specification

## Purpose
Additional Eclipse features that extend Epsilon with HUTN notation support, Picto model visualization, and Sirius widget integration.

## Architecture
- **`org.eclipse.epsilon.hutn.feature`** / `hutn.dt.feature` — Human Usable Textual Notation: a concrete syntax for creating EMF models using a human-readable text format
- **`org.eclipse.epsilon.picto.feature`** — Picto: a tool for generating visualizations (HTML, SVG, PlantUML diagrams) from models using EGL templates
- **`org.eclipse.epsilon.sirius.widget.feature`** — A widget for embedding Epsilon-based views within Sirius-based graphical editors

## Requirements

### Requirement: HUTN feature SHALL parse human-readable textual notation into EMF models
The `org.eclipse.epsilon.hutn.feature` SHALL provide a parser that converts HUTN text files into in-memory EMF model instances.

#### Scenario: Parse a HUTN file
- **GIVEN** the HUTN feature is installed and a `.hutn` file exists
- **WHEN** the HUTN parser processes the file
- **THEN** an in-memory EMF model is created matching the textual description

### Requirement: Picto feature SHALL generate model visualizations
The `org.eclipse.epsilon.picto.feature` SHALL render models as visual diagrams (HTML, SVG, PlantUML) within the Eclipse IDE.

#### Scenario: Display a model visualization
- **GIVEN** the Picto feature is installed and an EGL visualization template exists for a model type
- **WHEN** a user opens a model instance
- **THEN** Picto generates and displays the corresponding visualization in the Picto view

### Requirement: Sirius widget feature SHALL embed Epsilon views in Sirius editors
The `org.eclipse.epsilon.sirius.widget.feature` SHALL provide a widget that can be used inside Sirius-based graphical modeling editors.

#### Scenario: Embed Epsilon view in Sirius diagram
- **GIVEN** the Sirius widget feature is installed and a Sirius editor is configured with an Epsilon widget
- **WHEN** the Sirius editor opens a model
- **THEN** the Epsilon widget renders within the Sirius editor's properties view
