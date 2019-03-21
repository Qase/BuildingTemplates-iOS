[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Maintainer: luba](https://img.shields.io/badge/Maintainer-David2XN-blue.svg)](mailto:david.nemec@quanti.cz)
[![Qase: KotlinLogger](https://img.shields.io/badge/Qase-BuildingTemplates-ff69b4.svg)](https://github.com/Qase/BuildingTemplates-iOS)

## BuildingTemplates-iOS

Basic iOS templates to easy integrate and run fastlate at your iOS project.

## Features
* Lot of possible commands
* Lightweight - just one file
* Configurable using `env variables` or `lane options`
* Project is constantly developed
* Dependent on [DOTENV](https://docs.fastlane.tools/best-practices/keys/#dotenv)


## Commands

| Category | Lane name | Description | Parameter name | Parameter value |
|--------------------------|-------------------------|---------------------------------------------------------------------------------|----------------|------------------------------|
| **General** | clean | Clean all files before build (deletes derived data) |  |  |
|  | register | Register new development device to provisioning profile (prompts name and UUID) |  |  |
| **Version control** | version_set | Clean all files before build (deletes derived data) |  |  |
| **Certificates** | certificates | Update certificates (readonly) | scheme | development, appstore, adhoc |
|  | refresh_profiles | Update certificates for all schemes (development, appstore, adhoc) | readonly | boolean |
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

## Example
#### Fastfile example

```
import_from_git(
  url: "https://github.com/Qase/BuildingTemplates-iOS.git", # The URL of the repository to import the Fastfile from.
  branch: "HEAD", # The branch to checkout on the repository
  path: "fastlane/Fastfile_Base", # The path of the Fastfile in the repository
)
```

#### .env.default example
```
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
#AUTO_REFRESH_PROFILE = true

#Crashlytics
CRASHLYTICS_FRAMEWORK_PATH = "./Frameworks/Crashlytics.framework"
CRASHLYTICS_API_TOKEN = "123456"
CRASHLYTICS_BUILD_SECRET = "123456789"
CRASHLYTICS_IPA_PATH = "${IPA_PATH}"
CRASHLYTICS_GROUPS = "ios-testers"

#Gym
GYM_OUTPUT_DIRECTORY = "${BUILD_DIR}"
GYM_CLEAN = true

#Scan
SCAN_SCHEME = "${XCODE_SCHEME}"

#Deliver
DELIVER_TEAM_ID = "${TEAM_ID}"
DELIVER_IPA_PATH = "${IPA_PATH}"

#Get Info
FL_GET_INFO_PLIST_PATH = ${PLIST_PATH}
FL_NUMBER_OF_COMMITS_ALL = false
FL_VERSION_NUMBER_SCHEME = ${XCODE_SCHEME}
```

## License
[MIT](https://github.com/nishanths/license/blob/master/LICENSE)
