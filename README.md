[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Maintainer: luba](https://img.shields.io/badge/Maintainer-David-blue.svg)](mailto:david.nemec@quanti.cz)
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

| Category             | Lane name               | Description                                                                     | Parameter name | Parameter value              |
|----------------------|-------------------------|---------------------------------------------------------------------------------|----------------|------------------------------|
| **General**          | clean                   | Clean all files before build (deletes derived data)                             |                |                              |
|                      | register                | Register new development device to provisioning profile (prompts name and UUID) |                |                              |
| **Version control**  | version_set             | Clean all files before build (deletes derived data)                             |                |                              |
| Certificates         | create_default_keychain | ?????                                                                           |                |                              |
|                      | unlock_default_keychain |                                                                                 |                |                              |
|                      | delete_default_keychain |                                                                                 |                |                              |
|                      | certificates            | Update certificates (readonly)                                                  | scheme         | development, appstore, adhoc |
|                      | refresh_profiles        | Update certificates for all schemes (development, appstore, adhoc)              | readonly       | boolean                      |
| Informational        | info                    | Prints information (like version ...) about project                             |                |                              |
| Application building | build                   | Build application                                                               | scheme         | development, appstore, adhoc |
|                      | build_only              | Only build a project                                                            |                |                              |
| Deploying            | deploy_crashlytics      | Deploy application to crashlytics                                               |                |                              |
|                      | deploy_pilot            | Deploy application to pilot                                                     |                |                              |
|                      | deploy_appstore         | Deploy application to appstore                                                  |                |                              |
|                      | release                 | Release app (runs deploy_appstore)                                              |                |                              |
| Testing              | lint                    | Run swiftlint                                                                   |                |                              |
|                      | test                    | Run all tests (scan command). UNIT and UI                                       |                |                              |
|                      | snapshots               | Run UI test and create snapshots (saves images o google drive)                  |                |                              |
|                      | post_deploy_to_slack    | Post to 2N slack into app_monitoring channel                                    | place          | ???                          |
| CI complete lanes    | ci_crashlytics          | CI - Build and upload to crashlytics                                            |                |                              |
|                      | ci_testflight           | CI - Build and upload to testflight                                             |                |                              |
|                      | ci_appstore             | CI - Build and upload to testflight                                             |                |                              |
|                      | ci_nightbuild           | CI nightbuild - runs ci_crashlytics                                             |                |                              |

## Env variables

## Example

## License
[MIT](https://github.com/nishanths/license/blob/master/LICENSE)
