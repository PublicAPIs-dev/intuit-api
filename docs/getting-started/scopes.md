---
layout: default
title: Scopes
nav_order: 2
parent: Getting Started
---

**Last updated**: June 07, 2024

**Read time**: 1 min

# Learn about Intuit scopes

In GraphQL, scopes limit what data your app can read and update. Instead of getting broad permissions for everything, set granular permissions so your app only focuses on what's necessary.

Here are the current scopes for Intuit API. 

| **Scope**                                     | **Description**                                                        | **Sensitive data**       |
|:----------------------------------------------|:-----------------------------------------------------------------------|:-------------------------|
| time-tracking.time-entry.read                               | Grants access to read all time entires                                 | |
| time-tracking.time-entry                | Grants access to read and write time entries                           |  |
| project-management.project                             | Grants access to read and write project            | |
| payroll.compensation.read                            | Grants access to read pay types            | |
| app-foundations.custom-field-definitions.read                            | Grants access to read custom field definitions            | |
| app-foundations.custom-field-definitions                            | Grants access to read and write ustom field definitions           | |



**Note**: Sensitive data requires additional legal agreement and manual onboarding with our support team. 