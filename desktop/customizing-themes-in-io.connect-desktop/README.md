# **Customize Themes in io.Connect Desktop**

_Author: [Georgi Georgiev](https://github.com/ggeorgievx)_

Many users ask what are the best practices and how they can customize the look and feel of their io.Connect platform. Even though a lot of examples are already covered in the io.Connect Desktop docs here https://docs.interop.io/desktop/developers/configuration/themes/index.html and the io.Connect Browser docs here https://docs.interop.io/browser/developers/platform-styles/index.html there’s much more you can do to completely redesign both to the smallest piece and comply with your brand’s style. Below, we’ll explore some tips and examples on how to achieve that.

## Defining the Available Themes

### A.  Default themes

You can define the available Themes in the `themes.json` file located in the `config` directory of your io.Connect Desktop installation. By default, io.Connect Desktop includes two themes: a "dark" theme (Night) and a "light" theme (Day).  

<img width="608" height="690" alt="image" src="https://github.com/user-attachments/assets/fccbaa2b-172c-4a71-97da-8ccbbc68d241" />

### B.  Theme persistence

By default the selected theme is automatically persisted between io.Connect Desktop restarts.

It's saved in a `userTheme.json` file located in the io.Connect Desktop's `UserData`:

<img width="671" height="169" alt="image" src="https://github.com/user-attachments/assets/0d507fb4-a372-485d-a34f-517aa19a4ae0" />

`userTheme.json` can also be used to Pre-select a theme programatically (but keep in mind the file is rewritten when the user swithes the theme via UI).

### C.  Hide the theme selector.

If we don’t want to allow users to select different themes and keep it simple, we can:

1.  Hide selector in the Tray by setting `"themes": false` under `"tray"` in `system.json`:

<img width="586" height="247" alt="image" src="https://github.com/user-attachments/assets/de58920e-0c80-45ab-b128-91248ae9871f" />

which will remove the selector from the menu in the screenshot below:

<img width="595" height="715" alt="image" src="https://github.com/user-attachments/assets/535140a6-123a-406c-b25f-4089a9750ff3" />

The schema definition is `trayConfig.themes` - you can explore the `system.json` properties and definitions here https://docs.interop.io/desktop/assets/configuration/system.json

2.  Hide the selector in the Launchpad -> Platform Personalization.
   *Launcher customization will be covered in a separate article.*

## Theme Selector UI

### A. Default 
Below is the io.Connect Desktop out-of-the-box Theme Selector UI

<img width="638" height="286" alt="image" src="https://github.com/user-attachments/assets/4ba3cb94-0a1c-4e2e-b898-181d6755254a" />


### B. Customization 
If we want to build a custom Theme Selector UI we need to:

1.  Discover the available Themes using [the \`list()\` method of the Themes API](https://docs.interop.io/desktop/reference/javascript/themes/index.html#API-list)

<img width="1028" height="503" alt="image" src="https://github.com/user-attachments/assets/b7bda5c6-b813-48a3-8695-9efe9914fe2a" />

2.  Programmatically set the Theme on click using [the \`select()\` method of the Themes API](https://docs.interop.io/desktop/reference/javascript/themes/index.html#API-select)

<img width="1028" height="503" alt="image" src="https://github.com/user-attachments/assets/25904d3f-4bd6-4e08-973d-2d7deb0d94ce" />

### C. Sync with OS theme changes. 
If you’d like io.Connect’s theme to automatically respond to changes in the operating system's light or dark mode preference, simply update [the \`syncWithDefaultOSAppMode\` system setting:](https://docs.interop.io/desktop/developers/configuration/themes/index.html#theme_properties)

<img width="1035" height="365" alt="image" src="https://github.com/user-attachments/assets/dcd798bf-c3a6-486e-8282-b87c0474c87f" />

And then test changing your theme in your Windows personalization settings:

<img width="1097" height="580" alt="image" src="https://github.com/user-attachments/assets/c7b2601e-efc7-461a-8887-b5315fcc6ac1" />

## Styling

A. Change the container design (**using classic groups only**)

  
Inside the `themes.json` file within the `properties`, you can specify the styling of the window chrome.  
You can find details about the available options [here](https://docs.interop.io/desktop/developers/configuration/themes/index.html#theme_properties).

<img width="1004" height="579" alt="image" src="https://github.com/user-attachments/assets/2909ae46-26cc-4c19-89be-eb747b93c39f" />

Changes to `themes.json` take effect immediately - no need to restart io.Connect Desktop.
B. Applications look and feel

  
Applications are responsible for updating their styling in response to events emitted by the Themes API.

1. Web apps

You can get the currently selected Theme on startup using [the getCurrent() method of the Themes API](https://docs.interop.io/desktop/reference/javascript/themes/index.html#API-getCurrent)

<img width="981" height="558" alt="image" src="https://github.com/user-attachments/assets/0989dac5-bdf2-4170-99bf-f326d7067406" />

And subscribe for Theme changes using [the \`onChanged()\` method of the Themes API](https://docs.interop.io/desktop/reference/javascript/themes/index.html#API-onChanged)

<img width="1099" height="593" alt="image" src="https://github.com/user-attachments/assets/7f3bcb09-2b19-472f-9ee5-4e42e08bbaa6" />

2. Other apps

- The Themes API is currently supported only in interop.io's JavaScript SDK. Under the hood, it uses the Shared Contexts API to propagate theme changes across the platform. If you're working with other technologies - such as Java, .NET, or anything else - let us know your use case. We're happy to offer guidance or explore potential support options.

# **Themes in io.Connect Browser**

## Available Themes

io.Connect Browser includes two themes: a dark theme and a light theme.

## Build a Custom Theme Selector UI

A. You can discover the available Themes using [the \`list()\` method of the Themes API](https://docs.interop.io/browser/reference/javascript/themes/index.html#API-list)

B. Programmatically set the Theme on click using [the \`select()\` method of the Themes API](https://docs.interop.io/browser/reference/javascript/themes/index.html#API-select)

## Styling Applications

The applications are responsible for updating their styling in response to events emitted by the Themes API.

A. Get the currently selected Theme on startup using [the `getCurrent()` method of the Themes API](https://docs.interop.io/browser/reference/javascript/themes/index.html#API-getCurrent)

B. Subscribe for Theme changes using [the `onChanged()` method of the Themes API](https://docs.interop.io/browser/reference/javascript/themes/index.html#API-onChanged)

## Persisting the Selected Theme

The selected theme is automatically persisted between io.Connect Browser restarts. It's saved in the browser's local storage.

## **More to read**

This guide just scratches the surface of what’s possible with theming in io.Connect. From `themes.json` and `system.json` config changes to live updates in your web apps, you have the tools to deliver fast, consistent UI updates and fully customize your platform. Find more in the docs below and bookmark the repository for more quick tips.

**io.Connect Desktop**

https://docs.interop.io/desktop/developers/configuration/themes/index.html

https://docs.interop.io/desktop/assets/configuration/themes.json

https://docs.interop.io/desktop/reference/javascript/themes/index.html

**io.Connect Browser**

https://docs.interop.io/browser/capabilities/windows/themes/index.html

https://docs.interop.io/browser/reference/javascript/themes/index.html

https://docs.interop.io/browser/developers/platform-styles/index.html
