# Create and Edit Lightning Components 
- Lightning components are created in the `Developer Console`.
- But because lightning resources are available via the `Tooling API` we can also use Salesforce SX, Force.com ID, Sublime Lightning or VS Code.

## Create Lightning Components in the Developer Console
- `File | New | Lightning Component`
- Sample Hello World App (e.g. helloWorld)
```html
<aura:component >
	<p>Hello Lightning!</p>
</aura:component>
```

## Container App
- Components (e.g. helloWorld) cannot be ran directly. It needs to run inside a `container` app.
- `File | New | Lightning Application`
- Sample container app (e.g. harnessApp)
```html
<c:helloWorld/>
```
- Then click `Preview` from the right sidebar
- The format of the URL is the following: `https://<yourDomain>.lightning.force.com/<yourNamespace>/<yourAppName>.app`.
  - `<yourAppName>` represents the name of your app bundle
  - `<yourNamespace>` represents the namespace (default is `c`)


## Components
- A `component` is a bundle that includes a definition resource, written in markup, and may include additional, optional resources like a controller, stylesheet, and so on
- A `resource` is sort of like a file, but stored in Salesforce rather than on a file system.
- A `bundle` is sort of like a folder. It groups the related resources for a single component. 
- `Auto-wiring` just means that a component definition can reference its controller, helper, etc., and those resources can reference the component definition. They are hooked up to each other (mostly) automatically.

### Styling
- e.g. helloWorld.css
```css
.THIS {
}
p.THIS {
  font-size: 24px;
}
```
- `.THIS` refers to the current component


### Folder structure
- `helloWorld` — the component bundle
    - `helloWorld.cmp` — the component’s definition
    - `helloWorldController.js` — the component’s controller, or main JavaScript file
    - `helloWorldHelper.js` — the component’s helper, or secondary JavaScript file
    - `helloWorld.css` — the component’s styles


## App
- an app is just a special kind of component
- An app uses `<aura:application>` tags instead of `<aura:component>` tags.
- Only an app has a Preview button in the Developer Console.
- When writing markup, you can add a component to an app, but you can’t add an app to another app, or an app to a component.
- An app has a standalone URL that you can access while testing, and which you can publish to your users. We often refer to these standalone apps as `my.app.`
- You can’t add apps to Lightning Experience or the Salesforce app—you can only add components. 
- What you add to App Launcher is a Salesforce app, which wraps up a Lightning component, something defined in a `<aura:component>`. A Lightning Components app—that is, something defined in a `<aura:application>` —can’t be used to create Salesforce apps. A bit weird, but there it is.

### Adding a child component
- helloHeading component
```html
<aura:component >
    <h1>W E L C O M E</h1>
</aura:component>
```

- helloWorld component with child component helloHeading
```html
<aura:component >
    <c:helloHeading/>
	<p>Hello Lightning!</p>
</aura:component>
```

## Solution to Camping List challenge
- campingList.cmp
```html
<aura:component >
	<ol>
        <li>Bug Spray</li>
        <li>Bear Repellant</li>
        <li>Goat Food</li>
    </ol>
</aura:component>
```
- campingHeader.cmp
```html
<aura:component >
	<h1>Camping List</h1>
</aura:component>
```
- campingHeader.css
```css
.THIS {
}
h1.THIS {
    font-size: 18px;
}
```
- camping.cmp
```html
<aura:component >
    <c:campingHeader />
    <c:campingList />
</aura:component>
```
- Bonus: campingApp.app
```html
<aura:application >
    <c:camping/>
</aura:application>
```