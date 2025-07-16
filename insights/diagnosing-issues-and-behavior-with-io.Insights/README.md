# Track down issues fast with io.Insights

_Author: [Velko Nikolov](https://github.com/staafl)_

Working in finance requires quick and precise identification of problems. In this article, we'll walk through how to leverage io.Insights to utilize io.Connect telemetry data, specifically tracing and logging, to swiftly identify application issues.

### Scenario Overview

We have two interconnected applications:

- **Client List**: Displays client information fetched from a backend, allowing user selection.
- **Portfolio Viewer**: Subscribes to selection changes from the Client List, fetching and displaying detailed portfolio data.
<img width="50%" height="50%" alt="image" src="https://github.com/user-attachments/assets/9725c636-a9ca-4737-8308-9bfef4779d46" />

The synchronization has stopped for a specific user, and we want to quickly trace and pinpoint the issue. We'll be using io.Insights to collect and pass all data and Grafana as an example platform for exploration and analysis. Any alternative of Grafana can be used in a similar way.

### Step-by-Step Diagnostic Process

#### 1\. Enable Observability

Before digging into the data we need to make sure io.Insights is enabled. This is done simply by adding the `"insights"` (**pre 10.0 versions** use `"otel"`)<sup>1</sup> top-level key in the system.json system configuration file of io.Connect Desktop located in the %LocalAppData%/interop.io/io.Connect Desktop/Desktop/config or by using the otel property of the configuration object when [initializing your Main app](https://docs.interop.io/browser/developers/browser-platform/setup/index.html) in io.Connect Browser, and specifying the publishing URL of the OTEL collector.

See the documentation about configuration here - https://docs.interop.io/insights/configuration/index.html#ioconnect_desktop

<sup>1</sup> The `"insights"` top level object is used to enable and configure io.Insights since **version 10.0**. The `"otel"` top level object is used to enable and configure io.Insights in **versions pre-10.0.** Don't worry - it's **backward compatible**.

Having io.Insights enabled will now expose tracing and log events for our platform. Next, head to the **Traces** tab.
<img width="50%" height="50%" alt="image" src="https://github.com/user-attachments/assets/929d4858-3b90-4273-be0b-426e7607443e" />

#### 2\. Open the Query Interface

Click the three-dot menu in the Traces panel and click **Explore**.

This brings up the TraceQL query builder. We can use this to filter the traces relevant to the issue.
<img width="50%" height="50%" alt="image" src="https://github.com/user-attachments/assets/075ad169-a252-47b6-bd1c-112f2bbf9b19" />

#### 3\. Filter by User and Application

Construct a query targeting the specific user and app:

    { span.user = "VelkoNikolov" && span.tracingAppName = "ClientList" }

<img width="60%" height="60%" alt="image" src="https://github.com/user-attachments/assets/1566d29a-1614-4a5d-b347-44059a53b2b8" />

And run the query to list out all traces related to that user. Under Service & Operations we can simply click the table row we are interested in to drill down into more details.

<img width="60%" height="60%" alt="image" src="https://github.com/user-attachments/assets/363b8c00-f797-4657-9393-8dd10bd43ba3" />

#### 4\. Inspect the Trace Tree

Click any trace row to view its hierarchical structure. Click `Span attributes` to explore more,

Each span represents a sub-operation. At the top, you'll typically see a span from `ClientList`, representing the update. Keep clicking downward through the spans to trace the flow.
<img width="60%" height="60%" alt="image" src="https://github.com/user-attachments/assets/3e055723-cba6-4c7f-bca4-c8d9e09eebe0" />

#### 5\. Spot the Failing Span

One of the spans will have an error event. Click it and check the **Events** tab.

The exception message shows: `"Failed to retrieve portfolio"`, indicating that the problem lies somewhere in the retrieval of the portfolio from the back-end service. So now we know where things are going wrong.
<img width="60%" height="60%" alt="image" src="https://github.com/user-attachments/assets/30cedade-7f02-45da-baec-1b454af0bc84" />

#### 6\. Dig into Backend Request

Since the back-end service also uses OTEL observability, we can dig deeper into its operation in the trace to find what the error was. Let's scroll down to the backend service spans (e.g., `ClientsService`) and open the request handler.

Click **Logs for this span** to open the log view.
<img width="60%" height="60%" alt="image" src="https://github.com/user-attachments/assets/879947de-3d9e-494b-be55-51b9f2cb16b9" />

#### 7\. Analyze the Log Message

Here, the log output makes it crystal clear:

    User VelkoNikolov does not have entitlement VIEW_EXTENDED_PORTFOLIO

This explains the error entirely - the user doesn't have the required access to see that data.
<img width="60%" height="60%" alt="image" src="https://github.com/user-attachments/assets/42d34bb9-7860-4fcd-800c-d955801a85eb" />

### Fixing the issue

- Let the entitlements team know what's missing.
- Inform the user about the permission settings.
- Optionally, investigate entitlement assignment via further trace exploration.

### Time saved

No ticket escalations and no guessing - finding a tricky problem took just few minutes. The out-of-the-box instruments provided by io.Connect + io.Insights gives you everything you need with minimum setup in the observability platform you are already familiar with.

**In the next article, we'll cover custom instruments and more advanced workflows across services.**
