## Understanding logs in io.Connect  
*Author: [Simeon Stoimenov](https://community.interop.io/u/simeon_s/summary)*

This guide breaks down how logging works across io.Connect Desktop and io.Connect Browser, what gets logged, and how to configure it for troubleshooting and observability.

It covers:
- The io.Connect logging architecture and where logs originate (runtime, apps, Interop, shell, extensions).
- Log levels, what each level includes, and how to adjust them via `system.json` or app config.
- The structure of log entries, including key fields that help you trace issues (timestamps, component names, correlation IDs, errors).
- Where logs are stored on disk in Desktop vs Browser and how rotation/retention works.
- How to enable more detailed logs for debugging issues in production or local development.

Full guide:  
https://community.interop.io/t/understanding-logs-in-io-connect/36
