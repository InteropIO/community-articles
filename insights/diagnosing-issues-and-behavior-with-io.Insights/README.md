## Track down issues fast with io.Insights

This guide walks through a concrete troubleshooting scenario with two io.Connect apps (Client List and Portfolio Viewer) where updates silently stop for a single user and covers:
- Enabling io.Insights/OTEL in io.Connect Desktop and io.Connect Browser via `system.json` / main app config and pointing it at your OTEL collector.
- Using the Traces view and TraceQL to filter by user and app (e.g. `span.user`, `span.tracingAppName`) and isolate the problematic flow.
- Inspecting the trace tree to follow cross-app calls, identify the failing span, and see where errors are raised.
- Jumping from spans into backend service logs to read the exact error message (e.g. entitlement failures) instead of guessing.
- Using these insights to fix the issue (update entitlements, inform the user) and as a pattern for future io.Insights-powered investigations.

Read on the interop.io community:  
https://community.interop.io/t/track-down-issues-fast-with-io-insights/9
