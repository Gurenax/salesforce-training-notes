# Lightning Apps
- An `app` is a collection of items that work together to serve a particular function. Salesforce apps come in two flavors: Classic apps and Lightning apps. Classic apps are created and managed in Salesforce Classic. `Lightning apps` are created and managed in Lightning Experience.
- Lightning apps let you brand your apps with a custom color and logo. You can even include a utility bar and Lightning page tabs in your Lightning app.


## Reference
- [Lightning Apps](https://trailhead.salesforce.com/trails/lex_admin_migration/modules/lightning_apps)


## Lightning App Navigation Bar may contain
- Most standard objects, including Home, the main Chatter feed, Groups, and People
- Your org’s custom objects
- Visualforce tabs
- Lightning component tabs
- Canvas apps via Visualforce tabs
- Web tabs
- Lightning page tabs and utilities like Lightning Voice

## Lightning App Launcher may include
- Salesforce apps, which include custom apps and standard apps that come with Salesforce, like Sales and Service
- Connected apps such as Gmail™ and Microsoft® Office 365™
- Partner and ISV apps

## Lightning Experience App Manager
- The App Manager in Setup is your go-to place for managing apps for Lightning Experience. It shows all your connected apps and Salesforce apps, both Classic and Lightning.

## What does that “Visible in Lightning Experience” column mean?
- A checkmark in the `Visible in Lightning Experience` column means that the app is accessible in Lightning Experience via the App Launcher and is fully functional.

- You can toggle the availability of your Classic apps in Lightning Experience by selecting or deselecting `Show in Lightning Experience` on the Classic app’s detail page. Classic apps that have been upgraded to Lightning apps automatically have that setting disabled.

---

## Create a Lightning App
- Go to `Setup | App Manager | New Lightning App`

## Other Considerations
- App icons can be `JPG, PNG, BMP or GIF`.
- App Navigation can either be `Standard Navigation` or `Console Navigation`.  Console navigation is preffered by service users because it shows more information in a single view.
- A `utility bar` can be added to give quick access to productivity tools.

---

## Upgrading a Classic App to Lightning App
- Upgrading a Classic app to a Lightning app lets you and your users take advantage of custom branding and the enhanced navigation features available in Lightning Experience.

- Go to `Setup | App Manager | Click dropdown from the selected classic app | Upgrade`

- After you upgrade a Classic app, the two versions of the app must be managed separately. Future changes you make to the Classic app won’t be reflected in the Lightning version of the app, and vice versa.

