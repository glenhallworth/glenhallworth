Title: Telerik AppBuilder Alternatives
Published: 12/21/2017
Tags: ["Teletrik AppBuilder", "Cordova", "Phonegap", "Ionic"]

---

[Telerik AppBuilder](https://docs.telerik.com/platform/appbuilder/) is being [retired on 10th May, 2018.](https://www.telerik.com/platform-next-level) It was an easy way to compile Cordova apps for iOS and Android packages without a Mac OS machine. With the Visual Studio plugin, AppBuilder provided a low-friction way to build cross-platform apps. A new platform is required to continue creating these apps due to the impending retirement. We need a tool that allows developers to initiate a build on a windows machine and receive an iOS package in return.

## Convert to Cordova

Telerik AppBuilder based on [Cordova](https://cordova.apache.org/), the first step to moving away from AppBuilder will be converting the project into a Cordova project. There will be minimal work to move plugins and platforms over, although Cordova is strict with the [directory structure](https://cordova.apache.org/docs/en/latest/reference/cordova-cli/). This initial pain will make it easier moving forward if you want to trial different platforms for app build and release.

## Platforms

### [Adobe Phonegap](https://build.phonegap.com/)

#### Pros

- The [CLI](https://phonegap.com/products/#phonegap-cli-section) is really nice. To initiate a remote build:

```
phonegap remote install ios
```

- The paid plan is $9.99 / month.
- Supports QR Codes in the command line
  <img src="https://i.imgur.com/ajpwZlO.png" width="250">

#### Cons

- Lackluster [desktop application](https://phonegap.com/products/#desktop-app-section) without much functionality.
- The free plan is limited to 1 app.

### [Monaca](https://monaca.io/)

#### Pros

- Has support for [CLI](https://github.com/monaca/monaca-cli) or [Localkit IDE](https://monaca.io/localkit.html)
- Can deploy to [HockeyApp](https://hockeyapp.net/) or [DeployGate](https://deploygate.com/?locale=en).
- [$20 / month per developer](https://monaca.io/pricing.html)
- CLI is easy to use

```
monaca remote build ios
```

#### Cons

- Discontinued [Visual Studio 2015 plugin](https://marketplace.visualstudio.com/items?itemName=MonacaandOnsenUI.MonacaforVisualStudio2015)
- No Visual Studio 2017 plugin.
- Visual Studio Code extension in development but no release date.

### [Visual Studio Team Services](https://www.visualstudio.com/team-services/)

#### Pros

- Supports [hosted Mac OS build agents](https://blogs.msdn.microsoft.com/devops/2017/11/16/cloud-hosted-mac-agents-for-ci-cd-pipelines/)
  - If hosted agents unsuitable can use [MacinCloud](https://www.macincloud.com/) build agents or private agents on your machines
- Generic build and release support for applications other than iOS or Android apps such as a backend service.
- Full software development suite: Source Control, Project Management, Build & Release, Testing
- Free, but active development will exhaust free build minutes quickly.

#### Cons

- Need to check in to build
- Slow feedback loop
- Need to push app package to a third party for deployment

### [Cocoon](https://cocoon.io/)

#### Pros

- Website easy to use with good UX
- Essentially free for development builds

#### Cons

- Poor deployment and automation support
  - Downloads a zip file with packages.
  - No straight to phone option.
  - No QR Codes.
  - No command line tools
- Adds a "Built with Cocoon" splash screen that you can remove for a one-time fee of $500

## Other Platforms

### [Ionic](https://ionicframework.com/)

Ionic is good for building [Angular](https://angular.io/) based Cordova apps with cloud-build options. It is not suitable using with existing Telerik AppBuilder app due to being tied closely to the Ionic Framework.

### [Fastlane](https://fastlane.tools/)

Fastlane does not support windows and is out of the scope of this article.
