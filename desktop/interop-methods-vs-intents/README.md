Authors: Martyn Bedford, Kalina Georgieva

One of the core strengths of io.Connect is enabling apps to invoke functionality in one another. This can be done using **Intents** and **Interop methods,** which serve different purposes and sometimes are confusing for new developers building integrations:

### Interop Methods

**Interop Methods** are a direct, *service-like* integration mechanism. With the Interop API, applications explicitly **register methods** that other apps can call, or **stream data** for others to subscribe to. These methods only exist while the provider app is running. If the app isn’t open, its methods aren’t available. For example, an app might register a method called "getPrice" that returns pricing data for a given stock symbol. Other apps can programmatically invoke that method to retrieve the data.

If multiple apps provide the same method, you can control how it’s targeted:

* Invoke on the **“best” app** (default — Platform usually chooses the first one running),
* A **specific app instance**,
* A **set of apps**, or
* **All available apps** simultaneously.

In addition to request–response calls, **Interop also supports streaming**. An app can expose a streaming data source (such as live market prices), and other apps can subscribe to those updates in real time. This makes Interop methods especially valuable for service-style capabilities like pricing engines, trading functions, or continuous data feeds.

### Intents

**Intents** are high-level *action requests* that an app can raise for another app to handle. Intents are typically user-driven or workflow-driven. When an app raises an Intent (identified by a name like “ViewOrder” or “TradeOrder”), the platform looks for any other app that has registered as a handler for that Intent. If multiple apps are available, the **Intent Resolver UI** prompts the user to choose which app should handle the request. Once selected, the handler app is launched (if not already running) and/or targeted to perform the action.

At a technical level, intents are **built on top of interop methods**. Once the platform resolves *which* app should handle an intent, it invokes the app’s underlying interop method, passing in the provided context and optionally returning a result. This allows intents to be both **flexible and loosely coupled** — the caller doesn’t need to know in advance which app or method will execute the request.

Intents are a great fit for loosely-coupled workflows. For example, a portfolio app can raise an intent "ViewChart" with context data (such as a ticker symbol). The user can then choose from one of several charting apps that registered to handle that action, and the chosen app opens directly with the relevant chart. In this way, intents allow dynamic discovery of capabilities across the desktop: you don’t need to know which app will handle the action — you just describe *what* you want to do, not *how* to do it.

Think of Intents like your phone’s share functionality: when you tap **Share** on a photo, the OS surfaces every app that has declared it can handle that action, and you pick one. Similarly, when an app raises an Intent like **ViewChart** with a ticker, the platform shows an Intent Resolver UI (if enabled) providing a list of the apps capable of handling it.

Io.Connect provides a default Intent Resolver UI, but developers can write their own UI and use it instead. Creating a custom Resolver UI can be useful to better integrate with an existing app ecosystem and provide a more tailored experience for users when selecting which app should handle an intent. A cool trick available in Intent Resolver UI since io.Connect Desktop 9.6 and io.Connect Browser 4.0 is that you can now save a chosen Intent handler as a default one for the raised Intent in the current app. This will prevent the Intent Resolver UI from opening on the next raise request, and the intent will be handled by the saved Intent Handler - https://docs.interop.io/browser/capabilities/data-sharing/intents/overview/index.html#intent_resolver.

Another powerful feature of Intents is that they can be **intercepted**. io.Connect lets you programmatically step into an Intent’s lifecycle to review, modify, or override requests and results before they reach the intended app or after the app responds. This makes it possible to enforce rules, enrich data, or apply consistent handling across the platform. A common use case is limiting the shown Intent Handlers in the Intent Resolver UI only to the opened apps running in the current workspace. This is a good opportunity to leverage Plugins to intercept the `raise` operation and dynamically modify the request to achieve the desired outcome.

### Key differences

Intents are used for raising an action (sometimes requiring a UI handoff or user selection of the target app) while Interop methods are used to *direct function calls or data exchange* between apps. Another difference is that Intents don’t support streaming, whereas Interop methods can stream real-time data to subscribers. Intents abstract a single target (multiple apps can handle the same Intent but only one is selected and often involve the **Intents Resolver UI** that asks the user to choose an app if needed). Interop methods, on the other hand may be invoked on the "best" app (default behaviour - usually the first one running in the Platform), on a specific app, on a set of apps, or on all available apps , establishing a more tightly-coupled contract (though discovery mechanisms exist to find which apps offer which methods). Intents are typically one-off requests to perform an action *with a given context*, whereas Interop method calls can be continuous or on-demand service calls and support streaming updates.

Above is an example of the Intents Resolver UI prompting the user to select an app to handle an action. In this case, an Intent to “NewTrade” could be handled by the Order or Trade app, and the user can pick one. Intents enable multiple apps to declare the same capability, offering flexibility at runtime.

Internally, both mechanisms complement each other. Use **Intents** when you want loose coupling and user-driven choice (e.g. open a contact in whichever app the user prefers), and use **Interop methods** when implementing well-defined services (e.g. a pricing service, or a function to execute a trade) that other apps call behind the scenes. Actually, Intents lead to Interop method calls under the hood, passing a context and returning a result.

Notably, io.Connect’s Intents API is fully compatible with the FDC3 standard for financial desktop interoperability, meaning apps using FDC3 Intents can interoperate with io.Connect Intents seamlessly. Meanwhile, the Interop API provides a richer request/ response and streaming model for integration beyond the simpler broadcast patterns. Main difference between io.Connect Intents and FDC3 ones is that FDC3 Intents require context and type when raised, whereas Intents in io.Connect give full flexibility for the passed data.

### **When to Use Which?**

Both mechanisms complement each other:

* Use **Intents** when you want **loose coupling** and flexibility, especially where user choice matters — e.g., opening a client record in whichever app the user prefers.
* Use **Interop methods** when you need **well-defined services** or continuous interactions — e.g., fetching a pricing stream or executing a trade through a specific service app.

### Questions and Answers

**Question:** Is it possible to discover what Intents or Interop methods are available at runtime?

**Answer:** Yes, io.Connect lets apps query for available Intent handlers, and similarly discover and call registered Interop methods, including streaming ones.

---

**Question:** What about real-time streaming - can I stream data using both Intents and Interop methods?

**Answer:** Streaming is supported only via Interop methods. You can register a streaming-enabled method and let other apps subscribe to real-time data; Intents don’t support this feature.

---

**Question:** Is io.Connect Intents fully compatible with FDC3?

**Answer:** Yes. The io.Connect Intents API aligns fully with the FDC3 standard, supporting its built-in intents and context structures. See https://docs.interop.io/desktop/capabilities/data-sharing/fdc3-support/index.html

---

**Question:** Can multiple apps handle the same Intent?

**Answer:** Yes. Multiple apps can register for the same Intent, and if more than one is available, the Intents Resolver UI will let the user choose which to use.

---

**Question:** What happens if no app is available to handle a raised Intent?

**Answer:** If no handler is found, the platform reports that the Intent couldn't be resolved, and no action occurs. See https://docs.interop.io/desktop/capabilities/data-sharing/intents/overview/index.html

---

**Question:** Is there a typical purpose of **Interop methods** and **Intents**?

**Answer:**

* **Interop methods** are used for direct function calls or data exchange — for example, *get the latest price quotes.*
* **Intents** are used to raise an action — for example *view customer’s portfolio in another app.* They often involve a UI handoff or user selection.

---

**Question:** What is the difference in **targeting**?

**Answer:**

**Interop methods require explicit targeting if you want to control which app handles the call.** You can direct a call to a specific instance, a set of apps, or all available apps. **However, targeting is not mandatory** — if no target is provided, the platform automatically selects the “best” app that offers the requested method. This makes Interop flexible: you can either tightly control the call or let the platform choose the most suitable provider.

**Intents** abstract the target. Multiple apps can declare the same intent, but only one handler is chosen. If more than one is available, the **Intents Resolver UI** lets the user decide.

---

**Question:** What is the **Interaction Style** in **Interop methods** vs. **Intents**

**Answer:**

* **Interop methods** support both one-off calls and continuous interactions. Crucially, they allow **streaming**, enabling real-time data updates to flow across apps — something intents do not support.
* **Intents** are one-off requests: raise an action, pass context, optionally get a result.

---

### Related resources

**Intents**

* [**Intents API Reference**](https://docs.interop.io/desktop/reference/javascript/intents/index.html) – JavaScript guide to raising, finding, and registering Intents
* [**Intents Overview**](https://docs.interop.io/desktop/capabilities/data-sharing/intents/overview/index.html) – explains registration, discovery, filtering, and use of the Intents Resolver UI
* [**Intents > JavaScript**](https://docs.interop.io/desktop/capabilities/data-sharing/intents/javascript/index.html) – code samples for io.intents.raise(), find(), getIntents(), including handler filtering

**Interop Methods**

* [**Interop API Reference**](https://docs.interop.io/desktop/reference/javascript/interop/index.html) – JavaScript documentation for registering methods, streaming, discovery, and calling via io.interop
* [**Interop > JavaScript**](https://docs.interop.io/desktop/capabilities/data-sharing/interop/javascript/index.html) – step‑by‑step details on registering methods, method definitions, invoke(), discovery, and streaming support

**Other Related Resources**

* [**io.Connect Desktop Main API Reference**](https://docs.interop.io/desktop/reference/javascript/io.connect%20desktop/index.html) – the full overview of io.Connect Desktop capabilities, including both io.interop and io.intents
* [**JavaScript Tutorial**](https://docs.interop.io/desktop/tutorials/javascript/index.html) – end-to-end guide showing how to init the @interopio/desktop library and enable interop, intents, shared contexts, window management, etc.
