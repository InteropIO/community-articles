# Getting Started with io.Insights

*Author: Velko Nikolov*

This guide covers the essentials needed to start collecting and inspecting telemetry from io.Connect apps using io.Insights (OpenTelemetry-based tracing).

It includes:
- How tracing works in io.Connect Desktop and Browser, what data is emitted, and how spans are structured.
- Enabling io.Insights via the `system.json` (Desktop) or main app config (Browser), including exporter setup and pointing to your OTEL collector.
- Verifying whether telemetry is flowing using basic Grafana/Tempo queries.
- Understanding key span attributes (user, app name, component, error info) so you can filter and slice traces effectively.
- A minimal working example you can copy to enable tracing in your environment.

Full guide:  
https://community.interop.io/t/getting-started-with-io-insights/55
