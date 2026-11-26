---
title: "CodeLab: Secure Assignment Marking System"
date: 2026-11-26
draft: false
description: "A design overview of a controlled coding-assessment workflow and its security boundaries."
categories: ["Project"]
tags: ["Project"]
readingTime: true
---

## Project overview

CodeLab is presented here as a concept for a web-based coding assessment platform. Its goal is to give instructors a consistent workflow for preparing programming tasks, letting students complete work in an editor, and reviewing submissions with useful evidence. A controlled environment can reduce casual copying, but no browser-based system can prove that a student has never received outside help.

## Roles and workflow

### Administrator

An administrator prepares the course structure, manages classes, and assigns access to instructors and students. Clear role boundaries help prevent one class from seeing another class's submissions.

### Instructor

An instructor creates an assignment, writes the problem statement, defines allowed languages and test cases, and sets a deadline. Public examples can help students understand the interface, while private tests can check additional cases during marking.

### Student

A student opens the assigned task, writes code in the provided editor, runs permitted tests, saves progress, and submits before the deadline. The interface should communicate whether work is saved and whether submission is final.

## Integrity controls and their limits

A platform may apply controls such as restricting clipboard actions inside the assessment page, recording focus changes, keeping an audit trail of save and submit events, and checking that the submitted source matches the editor's saved state. These signals can help an instructor investigate unusual activity, but they are not proof of misconduct on their own. Browser focus can change for legitimate reasons, and client-side code is not a trusted security boundary.

A fair assessment system should therefore:

- Tell students clearly what is monitored and why.
- Collect only information needed for assessment integrity.
- Protect logs and submissions with access controls.
- Give instructors context instead of presenting automated flags as verdicts.
- Provide an appeal or review process for disputed cases.

## Backend authority and safer execution

The server should be authoritative for identity, assignment availability, deadlines, and final submission records. The browser should never be trusted to decide that a student is an instructor or that a deadline has changed.

Compilation and execution of student code require particular care. Treat submissions as untrusted input. Run them in isolated, resource-limited environments with restricted filesystem and network access, execution time limits, and cleanup after each run. Do not execute arbitrary student programs directly on the application server with its normal privileges.

## What instructors can review

A useful review page can show the submitted source, test outcomes, timestamps, and a concise activity timeline. Each signal should be labelled accurately: a focus change is a focus change, not a definitive indication of cheating. Human review remains necessary for consequential decisions.

## Possible technology layers

A typical implementation separates a browser interface, an application API, persistent storage, and an isolated execution service. Authentication and authorization protect the API; the database stores assignment and submission records; the execution worker evaluates code under strict limits.

## Conclusion

The strongest design is not the one that promises perfect surveillance. It is one that creates a clear assessment workflow, protects student data, runs untrusted code safely, and gives educators enough context to make fair decisions.
