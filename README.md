[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Maintainer: luba](https://img.shields.io/badge/Maintainer-David2XN-blue.svg)](mailto:david.nemec@quanti.cz)
[![Qase: KotlinLogger](https://img.shields.io/badge/Qase-BuildingTemplates-ff69b4.svg)](https://github.com/Qase/BuildingTemplates-iOS)

## BuildingTemplates-iOS

Basic iOS templates to easily integrate and run Fastlane at your iOS project.

## Features
* Lot of possible commands
* Lightweight - just one file
* Configurable using `env variables` or `lane options`
* Project is constantly developed
* Dependent on [DOTENV](https://docs.fastlane.tools/best-practices/keys/#dotenv)

## Prerequisites
- You have installed Fastlane, SwiftLint.
`brew install fastlane`
`brew install swiftlint`
- You have access to the git certificates repository
  - An SSH key (without encryption) is required for repository access.

## Setup Guide
 - Create `fastlane/FastFile` acording to template
 - Copy Apple keys into `fastlane` folder
 - Create `.env.default` acording to template and modify properties

## Usual flow for new developers on iOS project using this template
Give developer access to repo `X` and repo set in `MATCH_GIT_URL`. And run following commands on developers Macbook:

```bash
git clone git@example.com/X
cd X
fastlane certificates


fastlane register #(Optionally for registering new device to developer portal)
```



## Commands

| Category | Lane name | Description | Parameter name | Parameter value |
|--------------------------|-------------------------|---------------------------------------------------------------------------------|----------------|------------------------------|
| **General** | clean | Clean all files before build (deletes derived data) |  |  |
|  | register | Register new development device to provisioning profile (prompts name and UUID) |  |  |
| **Version control** | version_set | Clean all files before build (deletes derived data) |  |  |
| **Certificates** | certificate | Update certificates (readonly) | scheme | development, appstore, adhoc |
|  | certificates (refresh_profiles) | Update certificates for all schemes (development, appstore, adhoc) | readonly | boolean |
| **Informational** | info | Prints information (like version ...) about project |  |  |
| **Application building** | build | Build application | scheme | development, appstore, adhoc |
|  | build_only | Only build a project |  |  |
| **Deploying** | deploy_crashlytics | Deploy application to crashlytics |  |  |
|  | deploy_pilot | Deploy application to pilot |  |  |
|  | deploy_appstore | Deploy application to appstore |  |  |
|  | release | Release app (runs deploy_appstore) |  |  |
| **Testing** | lint | Run swiftlint |  |  |
|  | test | Run all tests (scan command). UNIT and UI |  |  |
|  | snapshots | Run UI test and create snapshots (saves images o google drive) |  |  |
|  | post_deploy_to_slack | Post to 2N slack into app_monitoring channel | place | String of deployed place |
| **CI complete lanes** | ci_crashlytics | CI - Build and upload to crashlytics |  |  |
|  | ci_testflight | CI - Build and upload to testflight |  |  |
|  | ci_appstore | CI - Build and upload to testflight |  |  |
|  | ci_nightbuild | CI nightbuild - runs ci_crashlytics |  |  |

## Env variables

| Name                     | Type   | Description                                                                       |
|--------------------------|--------|-----------------------------------------------------------------------------------|
| AUTO_REFRESH_PROFILE     | Bool   | Actualize app certificate before every build                                      |
| APP_IDENTIFIER           | String | Project app identifier                                                            |
| XCODE_SCHEME             | String | Name of default scheme                                                            |
| XCODE_SCHEME_ADHOC       | String | Name of adhoc scheme - default is `ENV["XCODE_SCHEME"]+"-Adhoc`                   |
| XCODE_SCHEME_PROD        | String | Name of production scheme - default is `ENV["XCODE_SCHEME"]`                      |
| XCODE_PROVISIONING       | String | Name of provisioning - default is `"match Development "+ ENV["APP_IDENTIFIER"]`   |
| XCODE_PROVISIONING_ADHOC | String | Name of adhoc provisioning - default is `"match AdHoc "+ ENV["APP_IDENTIFIER"]`   |
| XCODE_PROVISIONING_PROD  | String | Name of production scheme - default is `"match AppStore "+ ENV["APP_IDENTIFIER"]` |
| SUPRESS_DEPLOY           | Bool   | Debug function - if true, then skip all publishing lanes                          |
| APP_NAME_SHORT           | String | Your own name of application                                                      |
| CLEAN_CI_BEFORE_BUILD    | Bool   | Should CI be cleaned before each build (by default set to false)                  |

## Examples
For most projects simply importing Building Template is sufficient
#### Fastfile example:

```ruby
import_from_git(
  url: "https://github.com/Qase/BuildingTemplates-iOS.git", # The URL of the repository to import the Fastfile from.
  branch: "HEAD", # The branch to checkout on the repository
  path: "fastlane/Fastfile_Base", # The path of the Fastfile in the repository
)
```

For more complex projects you can override default functionality using override_lane. For example adding support for App Extensions:
```ruby
  override_lane :get_provisioning_profiles do |options|
    xcode_provisioning = options[:xcode_provisioning]

    provisioningProfiles = Hash.new
    provisioningProfiles[ENV["APP_IDENTIFIER"]] = xcode_provisioning
    provisioningProfiles[ENV["APP_IDENTIFIER"]+".CallingExtension"] = xcode_provisioning + ".CallingExtension"
    provisioningProfiles[ENV["APP_IDENTIFIER"]+".NotificationServiceExtension"] = xcode_provisioning + ".NotificationServiceExtension"
    provisioningProfiles
  end

  override_lane :version_set do |options|
    number_of_commits = version()
    increment_build_number_in_plist(build_number: "#{number_of_commits}")
    increment_build_number_in_plist(build_number: "#{number_of_commits}", scheme: ENV["XCODE_SCHEME_EXTENSION"])
    increment_build_number_in_plist(build_number: "#{number_of_commits}", scheme: ENV["XCODE_SCHEME_NOTIFICATION_EXTENSION"])

  end
```

This template relies on two fastlane plugins `fastlane-plugin-versioning` and `fastlane-plugin-firebase_app_distribution` (required only when using Firebase Distribution)

#### Pluginfile example:
```ruby
# Autogenerated by fastlane
#
# Ensure this file is checked in to source control!

gem 'fastlane-plugin-versioning'
gem 'fastlane-plugin-firebase_app_distribution'

```


#### .env.default example:
```ruby
#General
TEAM_ID = "123456789"
APP_IDENTIFIER = "com.quanti.swift.myappidentifier"
IPA = "myapp.ipa"
BUILD_DIR = "./build"
PLIST_PATH = "py-app/Info.plist"
IPA_PATH = "${BUILD_DIR}/${IPA}"
APP_NAME = "MyApp"
APP_NAME_SHORT = "MA"
XCODE_SCHEME = "My App"

FASTLANE_ITC_TEAM_ID = "${TEAM_ID}"

#Keychain
KEYCHAIN_NAME = "fastlane"
KEYCHAIN_PASSWORD = "password"

MATCH_KEYCHAIN_NAME = "${KEYCHAIN_NAME}"
MATCH_KEYCHAIN_PASSWORD = "${KEYCHAIN_PASSWORD}"
MATCH_PASSWORD = "password"

FL_UNLOCK_KEYCHAIN_PASSWORD = "${KEYCHAIN_PASSWORD}"
FL_UNLOCK_KEYCHAIN_PATH = "${KEYCHAIN_NAME}"

#Fastlane
FASTLANE_SKIP_UPDATE_CHECK = true

#Slack
SLACK_URL = "https://hooks.slack.com/services/123456789"

#Match
MATCH_APP_IDENTIFIER = ${APP_IDENTIFIER}
MATCH_GIT_URL = "git@git.quanti.cz:git/fastlane-match-ios.git"
MATCH_USERNAME = "username@quanti.cz"
# MATCH_APP_IDENTIFIER = ${APP_IDENTIFIER},${APP_IDENTIFIER_EXTENSION},${APP_IDENTIFIER_NOTIFICATION_EXTENSION} # Example for more identifiers
#AUTO_REFRESH_PROFILE = true

#Crashlytics
FIREBASEAPPDISTRO_GROUPS = "ios-testers"
FIREBASEAPPDISTRO_RELEASE_NOTES = "Lots of amazing new features to test out! (CI build)"
FIREBASEAPPDISTRO_IPA_PATH = "${IPA_PATH}"
FIREBASEAPPDISTRO_APP = "1:xxxxxxxxx:ios:xxxxxxxxxxxxxxxxxxxxxxx"
FIREBASEAPPDISTRO_CUST_CLI_TOKEN = "1//xxxxxxxxxxxxxxxxxxxxxxxxx"

#Gym
GYM_OUTPUT_DIRECTORY = "${BUILD_DIR}"
GYM_CLEAN = true

#Scan
SCAN_SCHEME = "${XCODE_SCHEME}"

#Pilot
PILOT_TEAM_ID = "${TEAM_ID}"
PILOT_IPA = "${IPA_PATH}"
PILOT_TESTER_FIRST_NAME = "First"
PILOT_TESTER_LAST_NAME = "Second"
PILOT_TESTER_EMAIL = "ios@example.com"
PILOT_CHANGELOG = "Test build for external testers"

#Deliver
DELIVER_TEAM_ID = "${TEAM_ID}"
DELIVER_IPA_PATH = "${IPA_PATH}"

#Get Info
FL_GET_INFO_PLIST_PATH = ${PLIST_PATH}
FL_NUMBER_OF_COMMITS_ALL = false
FL_VERSION_NUMBER_SCHEME = ${XCODE_SCHEME}
NUMBER_OF_COMMITS_MODDIFIER = 0 # Can be used to modify buind number with a constant
```
## FAQ
### Fastlane certificates command does not work
Developers must have access to Git repository at MATCH_GIT_URL. Try Checking out the repo using command `git clone {MATCH_GIT_URL}`. This needs to work without additional user interaction.

### ERROR: Provisioning profile "XXX" doesn't include signing certificate "iPhone
Open Keychain Access and Delete all Developer Certificates that are not included in Provisioning profile

### ERROR: Profile doesn't include device ...
Run command `fastlane register` and follow instruction to register your device to Developer Portal. (Or manually on Apple Connect)

### Fastlane command not found
Install fastlane: https://docs.fastlane.tools/getting-started/ios/setup/

Prefered instalation is using brew:
`brew install fastlane`


## License
[MIT](https://github.com/nishanths/license/blob/master/LICENSE)
