# Lightning Design System
- The Design System makes it easy for you to build applications that comply with the new Salesforce Lightning look and feel without reverse engineering the UI as custom CSS. 
- the new Design System markup results in pages which have the Lightning look and feel without writing any CSS.
- The CSS is fully namespaced with the `slds-` prefix and scoped with the `slds-scope` class to avoid CSS conflicts.
- Modern versions of Chrome, Safari, and Firefox are fully tested. For Microsoft Internet Explorer (MSIE), the Design System supports only version 11 as well as Microsoft Edge. Users of earlier versions of MSIE might encounter issues such as missing icons.

## References
- [Lightning Design System](https://trailhead.salesforce.com/modules/lightning_design_system)
- [Lightning Design System classes](https://www.lightningdesignsystem.com/)

## Four types of resources
1. `CSS framework` — Defines the UI components, such as page headers, labels, and form elements, a grid layout system, and a single-purpose helper classes, to assist with spacing, sizing, and other visual adjustments.

2. `Icons` — Includes PNG and SVG (both individual and spritemap) versions of our action, custom, doctype, standard, and utility icons.

3. `Font` — Salesforce Sans font

4. `Design Tokens` — These design variables allow you to tailor aspects of the visual design to match your brand. Customizable variables include colors, fonts, spacing, and sizing.

## Core Design Principles
- `Clarity` — Eliminate ambiguity.
- `Efficiency` — Streamline and optimize workflows.
- `Consistency` — Create familiarity and strengthen intuition by applying the same solution to the same problem.
- `Beauty` — Demonstrate respect for people’s time and attention through thoughtful and elegant craftsmanship.

## Where You Can Use the Design System
- `Visualforce pages`
- `Lightning pages and components`
- `Mobile apps` - accessing Salesforce through the Mobile SDK or another API
- `Standalone web apps` - served by Heroku or a similar platform

---

## Create a First Page
- Setup | Visualforce Pages
```html
<apex:page showHeader="false" standardStylesheets="false" sidebar="false" applyHtmlTag="false" applyBodyTag="false" docType="html-5.0">
<html xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" lang="en">
<head>
  <meta charset="utf-8" />
  <meta http-equiv="x-ua-compatible" content="ie=edge" />
  <title>Salesforce Lightning Design System Trailhead Module</title>
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <!-- Import the Design System style sheet -->
  <apex:slds />
</head>
<body>
  <!-- REQUIRED SLDS WRAPPER -->
  <div class="slds-scope">
    <!-- MASTHEAD -->
    <p class="slds-text-heading--label slds-m-bottom--small">
      Salesforce Lightning Design System Trailhead Module
    </p>
    <!-- / MASTHEAD -->
    <!-- PRIMARY CONTENT WRAPPER -->
    <div class="myapp">
      <!-- SECTION - BADGE COMPONENTS -->
      <section aria-labelledby="badges">
        <h2 id="badges" class="slds-text-heading--large slds-m-vertical--large">Badges</h2>
        <div>
          <span class="slds-badge">Badge</span>
          <span class="slds-badge slds-theme--inverse">Badge</span>
        </div>
      </section>
      <!-- / SECTION - BADGE COMPONENTS -->
    </div>
    <!-- / PRIMARY CONTENT WRAPPER -->
  </div>
  <!-- / REQUIRED SLDS WRAPPER -->
</body>
</html>
</apex:page>
```
- Like all Visualforce pages, the outer wrapper for your markup is the `<apex:page>` element.
- On your html tag, be sure to include the `xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink"` attributes. This is important to enable support for the SVG icon sprite maps within Visualforce.
- In our `<head>` section, we’re importing the design system using `<apex:slds />`, which will load the CSS in the document.
- Every time you use Design System markup in Visualforce, it should be placed inside an outer wrapper `<div>` with the `slds-scope` scoping class.
- Margins and paddings need to be explicitly defined in the classes (e.g. `slds-m-vertical--large` adds top and bottom padding)

### SLDS Class Naming
- Our CSS uses a standard class naming convention called `Block-Element-Modifier syntax (BEM)` to make the class names less ambiguous:
  - A `block` represents a high-level component (e.g. .`car`)
  - An `element` represents a descendant of a component (e.g. `.car__door`)
  - A `modifier` represents a particular state or variant of a block or element (e.g. `.car__door--red`)
- Example: `slds-button__icon--x-small`

---

## Grid System
- Similar to Bootstrap Grid System
- a grid allows you to divide your page into rows and columns.
- The Design System grid is based on `CSS Flexbox` and provides a flexible, mobile-first, device-agnostic scaffolding system.
- Breakpoints
  - Small - 480px
  - Medium - 768px
  - Large - 1024px respectively

- Basic Example
```html
<div class="slds-grid">
  <div class="slds-col">Column 1</div>
  <div class="slds-col">Column 2</div>
  <div class="slds-col">Column 3</div>
</div>
```
![](https://res.cloudinary.com/hy4kyit2a/image/upload/v1487803599/doc/trailhead/staging/team-trailhead_lightning-design-system_en-us_images_unit3-grid_9c36f649beef9bbc6237cfc37552de0e.png)

- Example with sizing helper classes
- rations can be of `2, 3, 4, 5, 6, or 12.`
```html
<!-- BASIC GRID EXAMPLE -->
<div class="slds-grid">
  <div class="slds-col slds-size--4-of-6">Column 1</div>
  <div class="slds-col slds-size--1-of-6">Column 2</div>
  <div class="slds-col slds-size--1-of-6">Column 3</div>
</div>
```
![](https://res.cloudinary.com/hy4kyit2a/image/upload/v1487803600/doc/trailhead/staging/team-trailhead_lightning-design-system_en-us_images_unit3-grid2_362796027f0874a2015b4a4885a65b8c.png)

- Mobile first example
```html
<!-- RESPONSIVE GRID EXAMPLE -->
<div class="slds-grid slds-wrap">
  <div class="slds-col slds-size--1-of-1 slds-small-size--1-of-2 slds-medium-size--3-of-4">A</div>
  <div class="slds-col slds-size--1-of-1 slds-small-size--1-of-2 slds-medium-size--1-of-4">B</div>
</div>
```
![](https://res.cloudinary.com/hy4kyit2a/image/upload/v1487803600/doc/trailhead/staging/team-trailhead_lightning-design-system_en-us_images_unit3-responsive_338c9df019ce667b8abff5b099007eee.png)

## Creating a Page Header
```html
<apex:page showHeader="false" standardStylesheets="false" sidebar="false" applyHtmlTag="false" applyBodyTag="false" docType="html-5.0">
<html xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" lang="en">
<head>
  <meta charset="utf-8" />
  <meta http-equiv="x-ua-compatible" content="ie=edge" />
  <title>Salesforce Lightning Design System Trailhead Module</title>
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <!-- Import the Design System style sheet -->
  <apex:slds />
</head>
<body>
  <!-- REQUIRED SLDS WRAPPER -->
  <div class="slds-scope">
    <!-- MASTHEAD -->
    <p class="slds-text-heading--label slds-m-bottom--small">
      Salesforce Lightning Design System Trailhead Module
    </p>
    <!-- / MASTHEAD -->
    <!-- PAGE HEADER -->
    <div class="slds-page-header" role="banner">
      <div class="slds-grid">
        <div class="slds-col slds-has-flexi-truncate">
          <!-- HEADING AREA -->
          <p class="slds-text-title--caps slds-line-height--reset">Accounts</p>
          <h1 class="slds-page-header__title slds-truncate" title="My Accounts">My Accounts</h1>
          <!-- / HEADING AREA -->
        </div>
        <div class="slds-col slds-no-flex slds-grid slds-align-top">
          <button class="slds-button slds-button--neutral">New Account</button>
        </div>
      </div>
      <div class="slds-grid">
        <div class="slds-col slds-align-bottom slds-p-top--small">
          <p class="slds-text-body--small page-header__info">COUNT items</p>
        </div>
      </div>
    </div>
    <!-- / PAGE HEADER -->

    <!-- PRIMARY CONTENT WRAPPER -->
    <div class="myapp">
    </div>
    <!-- / PRIMARY CONTENT WRAPPER -->
    <!-- FOOTER -->
    <!-- / FOOTER -->
  </div>
  <!-- / REQUIRED SLDS WRAPPER -->
  <!-- JAVASCRIPT -->
  <!-- / JAVASCRIPT -->
</body>
</html>
</apex:page>
```
- `slds-no-flex` is one of the Design System layout utility classes, and prevents the column from automatically resizing by removing its flex property. 
- `slds-align-top` is an alignment utility class which adjusts the vertical placement of the column contents so they align to the top of it.

## Add the main content
```html
<!-- PRIMARY CONTENT WRAPPER -->
<div class="myapp">
  <ul class="slds-list--dotted slds-m-top--large">
    <li>Account 1</li>
    <li>Account 2</li>
    <li>Account 3</li>
    <li>Account 4</li>
    <li>Account 5</li>
    <li>Account 6</li>
    <li>Account 7</li>
    <li>Account 8</li>
    <li>Account 9</li>
    <li>Account 10</li>
  </ul>
</div>
<!-- / PRIMARY CONTENT WRAPPER -->
```

## Add the footer
```html
<!-- FOOTER -->
<footer role="contentinfo" class="slds-p-around--large">
  <!-- LAYOUT GRID -->
  <div class="slds-grid slds-grid--align-spread">
    <p class="slds-col">Salesforce Lightning Design System Example</p>
    <p class="slds-col">&copy; Your Name Here</p>
  </div>
  <!-- / LAYOUT GRID -->
</footer>
<!-- / FOOTER -->
```