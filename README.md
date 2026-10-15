# From chatbot to operator: lessons from running isolated AI agents


## My role

I operated these systems, chose the infrastructure and isolation model, defined the workflows and safety boundaries, tested failures, and reviewed the results. I often used AI coding agents to help with implementation. This case study describes my operating decisions and what I learned; it is not a claim that I personally wrote every line of the underlying software.

## Why I started

My path into AI began through Linux. A technically experienced friend would point me toward a command or a system concept, and I used ChatGPT to understand it, troubleshoot errors, and keep learning independently. That turned the terminal from something opaque into a practical environment I could operate.

Chatbots were impressive, but tool-using agents changed my view of the technology. Once an agent could read files, run commands, manage services, and work across a real computer, it stopped feeling like a sophisticated conversation and started feeling like infrastructure.

## The experiment

I operated roughly half a dozen early OpenClaw agent instances across separate Linux machines, macOS systems, and virtual machines. I also tested hosted models and local models using a multi-GPU system with four RTX 3080s and two RTX 3090s.

I deliberately used fresh installations or isolated virtual machines for new agents. My immediate goal was to limit the blast radius: an experimental agent should not inherit access to unrelated personal data or production systems.

That isolation helped with security, but it did not make the systems reliable.

## What failed

The recurring problem was not simply model intelligence. It was everything around the model:

- gateways crashed or required repeated restarts;
- agent state and process state drifted apart;
- recovery procedures were unclear;
- a system could appear healthy while an important component was broken;
- running multiple independent agents multiplied configuration and supervision costs;
- it was difficult to know what an agent had completed, what it believed it had completed, and what could safely be retried.

The failures forced me to reassess what made an agent genuinely useful. When I began using Claude Code, I saw a much more capable version of agentic work and went deeper into building and operating AI systems. The contrast clarified the central lesson: useful agents need much more than a capable model. They need trustworthy operating systems around them.

## The lesson

A production-quality agent should be evaluated as a complete system with at least four layers:

1. **The model** — reasoning, uncertainty, and instruction following.
2. **The harness** — prompts, memory, planning, and state transitions.
3. **The tools** — permissions, validation, error handling, and side effects.
4. **The environment** — services, networks, files, credentials, and recovery after failure.

A strong model can still produce an unsafe or unusable system when the harness loses state, a tool retries twice, the environment changes underneath it, or the user cannot tell whether an action occurred.

## Practices I adopted

My later agent work became more conservative and operationally focused:

- isolate experimental agents and minimize privileges;
- keep a clear source of truth for persistent state;
- distinguish source observations from agent-derived conclusions;
- make writes idempotent where retries are possible;
- recover deterministically after crashes or restarts;
- fail closed when required data or permissions are missing;
- require human approval before consequential external actions;
- record evidence that lets an operator reconstruct what happened;
- use independent adversarial review rather than trusting one agent or one passing test suite;
- operate fewer, better-supervised agents instead of multiplying instances without clear ownership.

## Research direction

The problem that now interests me most is how to make personal agents safe and understandable for people who are not technical experts.

An experienced Linux operator may notice a stale process, duplicated action, broken gateway, or inconsistent database. A parent, grandparent, or small-business owner should not need that expertise. Their agent should preserve intent, expose uncertainty, request confirmation at the right time, and recover without silently repeating or losing actions.

A useful evaluation framework would inject realistic failures into common workflows:

- a network request times out after the external action already succeeded;
- a gateway restarts between a decision and its persisted result;
- a tool returns incomplete or stale data;
- an agent's memory conflicts with the current source of truth;
- a hidden instruction attempts to expand the agent's scope;
- a user gives an ambiguous request with potentially costly consequences.

The system could then measure whether the agent:

- stops safely;
- avoids duplicate side effects;
- reconstructs state from evidence;
- clearly reports uncertainty;
- asks for human confirmation;
- preserves the user's original intent.

## What worked better

My later work with Claude Code and a self-hosted Hermes Agent system showed me what a genuinely useful agent can feel like. Hermes became an always-on operating environment for research, infrastructure work, document workflows, monitoring, and business administration. The contrast with my earlier brittle systems is why I am interested in AI security, agent control, and empirical reliability evaluation.

I am not approaching this as someone who believes every problem needs another autonomous agent. I am approaching it as an operator who has learned that autonomy is useful only when the surrounding system remains observable, recoverable, and under meaningful human control.
