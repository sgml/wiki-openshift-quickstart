# Federated Wiki OpenShift Quickstart Changes

## 0.6.0

* Update to the latest version of the Federated Wiki software (wiki @ 0.10.0)
* Update default node version to 6.5.0, 6.x will soon be the LTS version...
* Include the new Passport security module, which requires configuration - by default we fall back to serving in read-only mode.

## 0.5.0

* Update to the latest version of the Federated Wiki software (wiki @ 0.7.0).
* **Farm Mode is now the default**, if you are upgrading an existing server you will need to move your site's `pages` and `status` directories into the sub-directory for the FQDN you are using to access the site. See [ReadMe](README.md) for locations of the directories.
* New plugins added (audio, flagmatic, grep, markdown, roster), other plugins have seen some significant updates.
* Add support for installing io.js
* Update to use Node.js 0.10.39 as the default. Later versions, including io.js, can be used.
