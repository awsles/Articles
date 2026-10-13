# APIs for use by AI

## Introduction

APIs were built for application development... Now AI agents are calling them ad hoc. What could possibly go wrong? Just because an AI agent can call an API doesn't mean it should. In this article, I explore why the agentic era requires a new security and governance paradigm for APIs accessed by AI.

## The AI Agent that ran Amok

"Clean up my inbox by suggesting unimportant messages to delete." Sounds innocent enough. 
In February 2026, Summer Yue - Meta’s Director of Alignment and Safety at its Superintelligence Lab - accidentally had hundreds of emails wiped from her personal Gmail inbox by an autonomous OpenClaw AI agent [see here]. Summer asked the agent to triage her inbox and suggest emails to delete, explicitly instructing it not to take any action without her confirmation first. While the agent worked well on a smaller test inbox, scaling it up to her large real-world inbox triggered context window compaction – a process where the AI compresses older chats to stay within its token memory limits. It is suspected that this context window compaction silently discarded her crucial ("confirm before acting") instruction and the agent began rapidly deleting more than 200 emails.
While the AI agent had the technical permission to perform the delete operations, it lacked appropriate guardrails around that access.

## APIs are designed for Applications; not for AI

Historically, applications were developed with structured, predetermined workflows. Developers explicitly selected API operations, built validations into the application, and defined when sensitive functions could execute. AI has introduced runtime decision-making where agents dynamically select and compose actions beyond predefined application workflows. Depending on the instructions, available tools, and data encountered, the AI can independently choose operations and sequences thereof that the original API designers did not intend or anticipate. Many traditional APIs are simply not appropriate for autonomous agents. 
Granting broad unfettered access to APIs results in “excessive agency”. OWASP defines this (here) as “the vulnerability that enables damaging actions to be performed in response to unexpected, ambiguous or manipulated outputs from an LLM, regardless of what is causing the LLM to malfunction.” Think of it as excessive permissions and functionality access given to AI.
Prompt instructions are not enough
Instructions such as “confirm before deleting” are useful but are not enforceable security boundaries, as exemplified by Summer Yue’s email debacle. AI models can misunderstand instructions, lose relevant context, or even encounter malicious instructions embedded in retrieved documents, emails, or tool responses. MCP provides a standardized mechanism for exposing tools and resources, with authorization capabilities, but its presence does not automatically establish appropriate business-level controls for every action. An AI agent may be properly authenticated and authorized to invoke a tool while still using it in a way that violates the user's actual intent. Security policy must be enforced by systems outside the model, not merely described to the model.

## The case for Agentic Interfaces: APIs for (use by) AI

APIs should be purpose-built to expose approved business capabilities to agents, while independently enforcing security, safety, authorization, and compliance policies. These agentic interfaces may sit in front of existing developer APIs, preventing dangerous operations, narrowing scope, constraining inputs, checking for sensitive data, and/or adding consent requirements. These agentic interfaces should expose what an agent is permitted to accomplish, rather than every underlying technical operation that happens to exist (which traditional developer APIs expose).
In Summer Yue’s case, an agentic-ready “Delete Email” API could have forced a confirmation and/or preserved an “undo” capability (e.g., move to wastebasket rather than permanently delete).

## What makes an API AI-ready?

Key elements that come to mind include:
* *Independent AI Credentials* – The AI agents use a credential that can only access the approved agentic API interfaces. This prevents an AI from discovering another API and attempting to use those same credentials therein.
* *Fine-Grained Authorization* – The APIs should enforce fine-grained authorization, not only for the AI agent, but also for the operations on whose behalf it is operating (referred to as “transitive” or “delegated” authorization). The authorization should match intent.
* *Human Approvals* – Sensitive and bulk operations might require human approval to complete – possibly out-of-band to the normal agentic flow.
* *Transactional / Roll-back* – Operations performed by AI should have an “undo” capability (where feasible) so that unintended consequences can be quickly remediated.
* *Sensitive Data Filtering* – AI-ready APIs can potentially inspect and prevent data inadvertently passed into AI for sensitive and personal information (i.e., subject to GDPR, CCPA, HIPAA, etc.).
* *API-Tuning* – The way in which agentic AI interacts with APIs is often different than how a traditional application interacts. AI-ready APIs can be optimized for agentic AI performance.
* *Operational Limits* – AI-ready APIs can impose rate limits and other measures to reduce blast-radius.
* *Comprehensive Logging* – AI-ready APIs can implement more detailed logging and auditing for compliance purposes.
These are just some of the measures that can be used. What others come to mind? 
 
Not every API needs to be rewritten. Low risk and non-destructive APIs may be suitable as-is, or with only slight modification for AI performance.

## API Gateway for AI Governance

An API gateway acts as a single-entry point and traffic controller between clients and a collection of backend services and their APIs. This potentially provides a natural point for exposing AI-ready APIs and enforcing AI governance. The gateway could evaluate requests, validate identity and scope, check organizational policies, manage approvals, transform requests, block risky actions, inspect responses, and record audit evidence. Some aspects however (such as rollback capability) will require changes to the underlying services.
The bottom line is that Enterprises can build on their existing API investments while introducing a consistent security boundary for agentic access, evolving that to meet their security and compliance needs.

## In closing…

For audit and compliance purposes, organizations need to answer questions such as: Who initiated an action? Which agent executed it? What information did it access? What controls were evaluated? Was approval required? Can the operation be investigated, reversed, or explained? Was any sensitive data leaked? Compliance is not achieved merely by adding logging, but by having a consistent API enforcement layer which makes evidence collection and policy enforcement easier.
We spent decades building APIs that let applications interact with our most valuable systems and our most sensitive data. Now we're connecting autonomous AI agents to those same interfaces, often without reconsidering the assumptions under which they were designed. We cannot simply trust that an AI agent will always make the right decision. We must design our systems so that an incorrect decision cannot automatically become a catastrophic action.
The future isn't just APIs that AI can call. It's APIs designed for AI to call safely.

