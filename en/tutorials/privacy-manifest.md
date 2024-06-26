If you use User Defaults or collect user data, you need to fill out a manifest. Everything you specify will appear on the application page.

> The frameowkr's developers also add a Manifest. But if they didn’t do this, then the developer himself adds it inside the project

If the framework has a manifest, it doesn't need to be duplicated into your manifest. When you archive a project, all manifests are merged into one.

# Adding Manifest

Press `⌘+N` and select `App Privacy`-file.

![Create an `App Privacy`-file](https://cdn.sparrowcode.io/tutorials/privacy-manifest/app-privacy.png?v=2)

Each target has its own Manifest, so be careful to checkmark the right target. If the Manifest is the same for all targets, you can specify several targets at once.

![Specifying the target for the Manifest](https://cdn.sparrowcode.io/tutorials/privacy-manifest/enable-target.png?v=2)

# Structure of Manifest

The Manifest is a plist-file with the `.xcprivacy` extension.

![Example of a completed Privacy Manifest](https://cdn.sparrowcode.io/tutorials/privacy-manifest/base-app-manifest.png?v=2)

The manifest contains three fields. The first is about tracking — you fill it out when you collect mail or name. The second one for system API, for example, User Defaults. The third one for `IDFA`.

Let's break down each field in more detail.

## User Tracking

The `Privacy Nutrition Label Types` field describes what data collect about the user. Anything specify in the manifest will be visible in the App Privacy field on the application page:

![Information about what data we collect on the App Store page](https://cdn.sparrowcode.io/tutorials/privacy-manifest/nutrition-label-app-store.png?v=2)

**Collected Data Type** — is the type of data collect about the user. For example, contacts or payment information. All types are on the [official website](https://developer.apple.com/documentation/bundleresources/privacy_manifest_files/describing_data_use_in_privacy_manifests#4250555), you cannot add your own. Add a line from `Data type` to the plist-file.

![Contact data types for Manifest](https://cdn.sparrowcode.io/tutorials/privacy-manifest/collected-data-type.png?v=2)

For each data type, create a new Item. The fields below must be specified for each data type:

**Linked to User** — if you collect data related to the user's identity, put `YES`.

**Used for Tracking** — if the data is used for tracking, put `YES`.

**Collection Purposes** — here specify the reasons why are collecting the data. For example, analytics, advertising or authentication. Choose from the available [list of reasons](https://developer.apple.com/documentation/bundleresources/privacy_manifest_files/describing_data_use_in_privacy_manifests#4250556), you can't list your own.

![Reasons in Manifest why we collect data](https://cdn.sparrowcode.io/tutorials/privacy-manifest/collection-purposes.png?v=2)

## System API

There is `Privacy Accessed API Types` field for APIs. You recive email with error descriptions exactly about this field. Here we specify which API we are using and reason for it.

![The type of API and the reason for its use](https://cdn.sparrowcode.io/tutorials/privacy-manifest/privacy-accessed-api-reasons-en.png?v=2)

These are the system APIs that need to be specified in the Manifest:

[File Timestamp](https://developer.apple.com/documentation/bundleresources/privacy_manifest_files/describing_use_of_required_reason_api#4278393): Get the time when the file was created or modified
[System Boot Time](https://developer.apple.com/documentation/bundleresources/privacy_manifest_files/describing_use_of_required_reason_api#4278394): Information about application startup and OS runtime
[Disk Space](https://developer.apple.com/documentation/bundleresources/privacy_manifest_files/describing_use_of_required_reason_api#4278397): Available storage space on the device
[Active Keyboard](https://developer.apple.com/documentation/bundleresources/privacy_manifest_files/describing_use_of_required_reason_api#4278400): Access to the list of active keypads
[User Defaults](https://developer.apple.com/documentation/bundleresources/privacy_manifest_files/describing_use_of_required_reason_api#4278401): If used User Defaults

For each API by link you get a list of availalbe reasons. You can't specify your own reasons.

> If more than one reason is correct, fill all of them

## IDFA

If you are using IDFA, add the **Privacy Tracking Enabled** field and set `YES`. Add the **Privacy Tracking Domains** field as well, here you need to specify all domains that work in IDFA.

![Fields for IDFA in Manifest](https://cdn.sparrowcode.io/tutorials/privacy-manifest/tracking-enabled-tracking-domains.png?v=2)

> If you set `Privacy Tracking Enabled`, be sure to specify at least one domain.

To get which domains are used for IDFA, open the `Product` → `Profile` profiler. Now select Network in the window:

![Profiler window](https://cdn.sparrowcode.io/tutorials/privacy-manifest/profile-network.png?v=2)

In the upper left corner, click Start Recording. Select the **Points of Interest** tab, this will list all the domains. The **Start Message** column shows the domain and indicates that it has not been added to the Manifest.

![How to collect IDFA domains](https://cdn.sparrowcode.io/tutorials/privacy-manifest/points-of-interest.png?v=2)

The profile sometimes fails. If **Points of Interest** doesn't show anything or disappears altogether, here's the second way. Select your application tab, and can see all domains in the sessions.

![All domains in application sessions](https://cdn.sparrowcode.io/tutorials/privacy-manifest/app-sessions.png?v=2)

Now you will have to check each domain to see if it participates in IDFA. You will have to do it yourself.

# Manifest in Frameworks

> Framework developer adds the manifest too. But if they haven't done so, the developer adds it internally

If the framework developer has not added a Manifest, you must fill in the Manifest themselves.

If there is a Manifest in the framework, and it is complete, there is no need to duplicate to your manifest. All Manifests are merged into one when we collect the archive.

If there are errors in the Manifest, the developer will have to complete the Manifest himself within the project. For example, Firebase Crashlytics uses the domain **firebase-settings.crashlytics.com**. They didn't specify this in their manifest:

![Firebase manifest error](https://cdn.sparrowcode.io/tutorials/privacy-manifest/firebase-manifest.png?v=2)

We found it with the help of a [profiler](https://sparrowcode.io/ru/tutorials/privacy-manifest#idfa). In this case add the domain to your Manifest, this will override the problem field in the Firebase Manifest.

Framework Manifests make mistakes — be sure to double-check.

# If the error in Manifest

> Errors will come to mail only when send the application for review. If you just upload the project, there will be no errors

Only errors about the system API will come to the mail:

![A email with errors in the Manifest](https://cdn.sparrowcode.io/tutorials/privacy-manifest/privacy-manifest-email.png?v=2)

To quickly find the keys, type `NS` in the search. These are the ones that are missing from your Manifest. Even if you don't use this API, it can be used by frameworks that you have added to your project.

Here are the NS keys, and links to the key and the reason on Apple's website:

- [NSPrivacyAccessedAPICategoryFileTimestamp](https://developer.apple.com/documentation/bundleresources/privacy_manifest_files/describing_use_of_required_reason_api#4278393)
- [NSPrivacyAccessedAPICategorySystemBootTime](https://developer.apple.com/documentation/bundleresources/privacy_manifest_files/describing_use_of_required_reason_api#4278394)
- [NSPrivacyAccessedAPICategoryDiskSpace](https://developer.apple.com/documentation/bundleresources/privacy_manifest_files/describing_use_of_required_reason_api#4278397)
- [NSPrivacyAccessedAPICategoryActiveKeyboards](https://developer.apple.com/documentation/bundleresources/privacy_manifest_files/describing_use_of_required_reason_api#4278400)
- [NSPrivacyAccessedAPICategoryUserDefaults](https://developer.apple.com/documentation/bundleresources/privacy_manifest_files/describing_use_of_required_reason_api#4278401)

# Final Manifest

Collect the archive Product -> Archive. Right-click on the archive, select Generate Privacy Report.

![Exporting the final manifest](https://cdn.sparrowcode.io/tutorials/privacy-manifest/generate-privacy-report.png?v=2)

In the export PDF-file. All manifests merged into the final one:

![PDF report with all manifests](https://cdn.sparrowcode.io/tutorials/privacy-manifest/pdf-report.png?v=2)

All fields with `.app` extension are from your Manifest. Other fields are third-party frameworks in your project.