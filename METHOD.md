# Waterline Construction Method

## Purpose

The Waterline Construction Method provides a structured way to model municipal waterline construction and installation. The method connects physical waterline components with installation activities, inspections, testing, and project requirements.

The purpose of the method is to make these relationships visible and consistent so that a project model can be used to understand what is being installed, what requirements apply, and what verification activities are needed.

## Method Structure

The method uses the Waterline Construction vocabulary and organizes the project around four description patterns:

1. Waterline Components
2. Installation Activities
3. Inspection and Testing
4. Requirements

Each pattern is provided as a reusable compose template with an editor. Project notebooks invoke these templates and bind them to the Waterline project description. This keeps the methodology in one location while allowing project models to reuse it without copying the editor definitions.

## Waterline Components

The component pattern is used to describe the physical items that make up the waterline system. The current vocabulary distinguishes four component types:

- Pipe
- Fitting
- Valve
- Hydrant

These types are kept separate so that the model identifies what kind of physical asset is being represented instead of treating every asset as a generic component.

Each component can include a description and verification status. Verification status is used to identify whether verification of a modeled component is pending or complete.

## Installation Activities

The installation pattern describes construction activities that install waterline components.

An installation activity should identify the component being installed. This creates traceability between the construction work and the physical asset. Connecting activities and components also allows requirements associated with a component to be related to the work that installs it.

## Inspection and Testing

The verification pattern describes inspection and testing activities.

Inspection and testing are modeled separately because they represent different forms of verification. An inspection may evaluate construction conditions or workmanship, while testing may evaluate the performance of an installed component.

The specific inspection and testing activities required can vary depending on the component and project requirements. The method therefore represents these activities explicitly rather than assuming that every component uses the same verification process.

## Requirements

The requirements pattern describes project requirements and connects them to the waterline components or installation activities to which they apply.

Requirements may include a description, requirement expression, and priority. A high-priority requirement can also be represented using the Critical Waterline Requirement concept defined by the vocabulary.

Connecting requirements to components and activities provides traceability between what the project requires and the construction work represented in the model.

## Business Rules

The method currently includes two business rules.

### Requirement Verification

When a requirement applies to a component and that component requires an inspection, the method can infer that the requirement requires that inspection for verification.

This rule connects requirements with verification activities without requiring the same relationship to be entered manually.

### Installation Requirement Applicability

When a requirement applies to a component and an installation activity installs that component, the method can infer that the requirement also applies to the installation activity.

This reduces duplicate modeling and maintains consistency between component requirements and the activities that install those components.

## Reusable Editors and Project Notebooks

The description patterns are implemented as reusable compose templates under the methodology folder. Project notebooks invoke these templates using the Waterline project description as their ontology context.

This separation keeps the methodology definitions reusable while allowing the project to work through live editors. The editors provide a structured interface for creating and reviewing instances without requiring the user to manually edit the underlying OML for every model element.

The Waterline method includes editors for components, installations, inspections, testing, and requirements.

## Dogfooding the Method

The methodology was tested by using its own editors to create Waterline project instances. At least ten instances were created through the live editors, including fittings, valves, hydrants, installation activities, inspection activities, and testing activities.

Using the method to build its own project model helped verify that the templates could be invoked from project notebooks, that existing model instances could be displayed, and that new instances could be written back to the Waterline description.

## Current Limitations and Open Issues

The methodology represents a focused subset of a municipal waterline construction project and is not intended to capture every piece of information used during construction.

Verification status currently distinguishes only Pending and Complete. A real project may require additional states or supporting evidence.

Inspection and testing activities are represented separately, but the current model does not capture all possible inspection results, test measurements, acceptance criteria, or construction records.

The methodology should be extended only when additional information is necessary to answer a project question. Information that does not provide useful model-based analysis should not be added only for completeness.

## Summary

The Waterline Construction Method provides a repeatable approach for modeling the relationship between waterline components, construction activities, verification activities, and requirements. The method uses reusable templates and live editors so that the modeling patterns can be applied consistently across the project.

The methodology is intended to support both model creation and later analysis. As additional questions are evaluated, gaps in the vocabulary, patterns, or project data can be identified and addressed when appropriate.