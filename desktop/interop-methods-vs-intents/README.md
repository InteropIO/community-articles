# **Interop methods vs Intents**

_Authors: Martyn Bedford, Kalina Georgieva_

This guide explains how io.Connect Intents and Interop methods work, how they relate, and when to use each in your desktop workflows.

It covers:
- Interop methods as direct, service-like integrations: apps register methods (including streaming ones) that other apps invoke while the provider is running.
- Intents as higher-level action requests raised by apps and resolved via registered handlers and the Intent Resolver UI, built on top of Interop methods.
- Targeting differences: Intents abstract the target and rely on the resolver (and optional defaults), while Interop methods support explicit or automatic targeting across apps/instances.
- Interaction style differences: Intents are one-off action requests; Interop methods can be one-off or continuous, with support for real-time streaming.
- Runtime discovery of available Intents and Interop methods, including what happens when no handler is available.
- How io.Connect Intents align with the FDC3 standard and where io.Connect’s Interop API offers a richer model beyond basic FDC3-style patterns.

Full guide:  
https://community.interop.io/t/interop-methods-vs-intents/35
