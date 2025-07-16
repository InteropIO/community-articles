# Track down issues fast with io.Insights

_Author: [Velko Nikolov](https://github.com/staafl)_

Working in finance requires quick and precise identification of problems. In this article, we'll walk through how to leverage io.Insights to utilize io.Connect telemetry data—specifically tracing and logging—to swiftly identify application issues.

### Scenario Overview

We have two interconnected applications:

- **Client List**: Displays client information fetched from a backend, allowing user selection.
- **Portfolio Viewer**: Subscribes to selection changes from the Client List, fetching and displaying detailed portfolio data.

The synchronization has stopped for a specific user, and we want to quickly trace and pinpoint the issue. We'll be using io.Insights to collect and pass all data and Grafana as an example platform for exploration and analysis. Any alternative of Grafana can be used in a similar way.

### Step-by-Step Diagnostic Process

#### 1\. Enable Observability

Before digging into the data we need to make sure io.Insights is enabled. This is done simply by adding the "insights" (**pre 10.0 versions** use "otel")<sup>1</sup> top-level key in the system.json system configuration file of io.Connect Desktop located in the %LocalAppData%/interop.io/io.Connect Desktop/Desktop/config or by using the otel property of the configuration object when [initializing your Main app](https://docs.interop.io/browser/developers/browser-platform/setup/index.html) in io.Connect Browser, and specifying the publishing URL of the OTEL collector.

See the documentation about configuration here - https://docs.interop.io/insights/configuration/index.html#ioconnect_desktop

<sup>1</sup> The "insights" top level object is used to enable and configure io.Insights since **version 10.0**. The "otel" top level object is used to enable and configure io.Insights in **versions pre-10.0.** Don't worry - it's **backward compatible**.

Having io.Insights enabled will now expose tracing and log events for our platform. Next, head to the **Traces** tab.

#### 2\. Open the Query Interface

Click the three-dot menu in the Traces panel and click **Explore**.

This brings up the TraceQL query builder. We can use this to filter the traces relevant to the issue.

#### 3\. Filter by User and Application

Construct a query targeting the specific user and app:

    { span.user = "VelkoNikolov" && span.tracingAppName = "ClientList" }

And run the query to list out all traces related to that user. Under Service & Operations we can simply click the table row we are interested in to drill down into more details.

#### 4\. Inspect the Trace Tree

Click any trace row to view its hierarchical structure. Click Span attributes to explore more,

Each span represents a sub-operation. At the top, you'll typically see a span from ClientList, representing the update. Keep clicking downward through the spans to trace the flow.

#### 5\. Spot the Failing Span

One of the spans will have an error event. Click it and check the **Events** tab.

The exception message shows: `"Failed to retrieve portfolio"`, indicating that the problem lies somewhere in the retrieval of the portfolio from the back-end service. So now we know where things are going wrong.

#### 6\. Dig into Backend Request

Since the back-end service also uses OTEL observability, we can dig deeper into its operation in the trace to find what the error was. Let's scroll down to the backend service spans (e.g., ClientsService) and open the request handler.

Click **Logs for this span** to open the log view.

#### 7\. Analyze the Log Message

Here, the log output makes it crystal clear:

    User VelkoNikolov does not have entitlement VIEW_EXTENDED_PORTFOLIO

This explains the error entirely - the user doesn't have the required access to see that data.

### Fixing the issue

- Let the entitlements team know what's missing.
- Inform the user about the permission settings.
- Optionally, investigate entitlement assignment via further trace exploration.

### Time saved

No ticket escalations and no guessing – finding a tricky problem took just few minutes. The out-of-the-box instruments provided by io.Connect + io.Insights gives you everything you need with minimum setup in the observability platform you are already familiar with.

**In the next article, we'll cover custom instruments and more advanced workflows across services.**
