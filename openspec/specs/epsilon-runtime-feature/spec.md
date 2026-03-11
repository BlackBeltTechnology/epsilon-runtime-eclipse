# epsilon-runtime-feature Specification

## Purpose
The `epsilon-runtime.feature` module packages the BlackBelt Epsilon execution engine (`hu.blackbelt.epsilon.runtime-execution`) as an installable Eclipse feature, declaring its required dependencies on Eclipse Core Runtime, EMF, and Epsilon Common.

## Architecture
- **Feature ID:** `hu.blackbelt.epsilon.runtime.feature`
- **Version:** `2.8.0.qualifier`
- **Included Plugin:** `hu.blackbelt.epsilon.runtime-execution` (version 0.0.0 — resolved at build time)
- **Required Imports:** `org.eclipse.core.runtime`, `org.eclipse.emf.ecore`, `org.eclipse.emf.ecore.xmi`, `org.eclipse.ui`, `org.eclipse.ui.ide`, `org.eclipse.epsilon.evl.emf.validation`, `org.eclipse.epsilon.common`

## Requirements

### Requirement: Feature descriptor SHALL declare the runtime-execution plugin
The feature.xml SHALL include `hu.blackbelt.epsilon.runtime-execution` as its sole included plugin.

#### Scenario: Feature resolves runtime plugin
- **GIVEN** the `epsilon-runtime.feature` is built by Tycho
- **WHEN** the P2 resolver processes `feature.xml`
- **THEN** the `hu.blackbelt.epsilon.runtime-execution` bundle is included in the feature

### Requirement: Feature SHALL declare all required Eclipse and Epsilon dependencies
The feature.xml SHALL import all Eclipse platform and Epsilon plugins required for the execution engine to function.

#### Scenario: Feature resolves in a target platform with Epsilon 2.8
- **GIVEN** a target platform containing Eclipse SDK and Epsilon 2.8 update site
- **WHEN** the feature is installed
- **THEN** all imported plugins (`org.eclipse.core.runtime`, `org.eclipse.emf.ecore`, `org.eclipse.emf.ecore.xmi`, `org.eclipse.epsilon.common`, `org.eclipse.epsilon.evl.emf.validation`) are resolved

### Requirement: Feature SHALL be installable from the P2 update site
The feature SHALL be included in the `site` module's P2 repository under the "Epsilon Runtime" category.

#### Scenario: Install runtime feature from update site
- **GIVEN** a user adds the P2 update site URL to their Eclipse installation
- **WHEN** they browse the "Epsilon Runtime" category
- **THEN** `hu.blackbelt.epsilon.runtime.feature` appears and can be installed
