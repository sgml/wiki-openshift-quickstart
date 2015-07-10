# Wiki on OpenShift

This git repository helps you to set-up the node.js version of the Smallest Federated Wiki on OpenShift.

## Running on OpenShift

It is simple to create a Federated Wiki site using either the OpenShift command line tools, or the Web Interface.

### Creating using the OpenShift Web Interface

Create an account at http://openshift.redhat.com/, and (optionaly) set up your local machine with the client tools. *The client tools will be needed to maintain your install of Federated Wiki, so they could be installed later.*

[OpenShift web creation](https://openshift.redhat.com/app/console/application_types/custom?cartridges[]=nodejs-0.10&initial_git_url=https%3A%2F%2Fgithub.com/paul90/wiki-openshift-quickstart.git&name=wiki) will start the creation process.

You will need to:-

* choose an initial public domain name, this uses an application and user part. You can add your own domain name later.
* depending on your account choose a gear size and region. A small gear is plenty to get started.
* press `Create Application`. This may take a while. The next page will give you details of the on `Making Code Changes` and `Manage your app`.

### Creating using the command line

Create an account at http://openshift.redhat.com/ and set up your local machine with the client tools.

Create a node-0.10 application (you can call your application whatever you want, we use wiki in this example)
```cmd
  rhc app create wiki nodejs-0.10
```

Add this upstream Wiki Quickstart repo
```cmd
  cd wiki
  git remote add upstream -m master https://github.com/paul90/wiki-openshift-quickstart.git
  git pull -s recursive -X theirs upstream master
```

This can then be pushed to Openshift
```cmd
  git push
```

N.B. This first `git push` will take a while to complete as it has to install all
the wiki packages, and their dependencies.

That's it, you can now checkout your application at:
```
  http://wiki-$yournamespace.rhcloud.com
```
One of the first things you will want to do is to claim your site.

## Adding Wiki sites to the Farm

Wiki sites are added to the farm by adding a custom domain, see [using a custom domain](https://developers.openshift.com/en/managing-domains-ssl.html#using-a-custom-domain).

## Keeping up to date

### Updating the Federated Wiki modules

The Federated Wiki package updates, that don't require a change to the versions specified in `package.json`,
can be achieved by adding an `update` marker, adding it to the repository, and pushing the update to OpenShift.
The presence of the `update` marker is detected by the `build` action hook, and `npm update` is run.

```cmd
  touch .openshift/markers/update
  git add .
  git commit -m "create update marker"
  git push
```

### Updating the version of Node.js

To select the version of Node.js that you want to run, just edit or add a version to the .openshift/markers/NODEJS_VERSION file.

Example: To install Node.js version x.y.z, you would run (replacing x.y.z with version you want to use):

```cmd
  echo x.y.z >> .openshift/markers/NODEJS_VERSION
  git add .
  git commit -m "updated to use node.js x.y.z"
  git push
```

N.B. Currently this only works with Node.js version.


### Pulling in changes to wiki-openshift-quickstart

The upstream repository will be updated when there is a change to either OpenShift, or Federated Wiki that requires the quickstart to be modified.

To update, you will need to pull in the upstream changes, and merge them. You will also want to review any local
changes, and review to ensure no extra steps are required. See [Changes]()

At its simplest all that is required is, though if you have made any configuration changes, e.g. configured a storetype,
you will want to check these are still in effect before doing the `git push` to update OpenShift:
```cmd
  git fetch upstream
  git merge upstream/master
  git push
```
Though if you have made any local changes, you may need to checkout the master branch, and handle any conflicts
in the merge.


## Configuration

### Storetype

By default flat files will be used store any edits. An alternative is LevelDB: -
* LevelDB - modify `package.json` to include `    "wiki-storage-leveldb": "*",` in the dependencies, and
uncomment the line, shown below, in `server.js`

```js
        /*
        self.database = '{"type": "leveldb"}';
        */
```


### Farm Mode

Farm mode is now enabled by default, see below how to disable if required.

#### How to Disable Farm Mode

To disable Farm Mode you will need to:-

1. Backup your sites pages and status, see below.
2. In `server.js` disable farm mode by changing `self.farm = TRUE;` to `self.farm = FALSE;`.
3. If you have already created content within the farm, move the page and status directories to the expected location. In farm mode each wiki will have a `pages` and `status` directory in the `$OPENSHIFT_DATA_DIR/{FQDN}` directory. You will need to move these up one level to `$OPENSHIFT_DATA_DIR`.

If you had created pages in multiple wikis within the farm and are not merging the sites you can create those wikis elsewhere by restoring the backup files in their new location.

### Pushing configuration changes to OpenShift

Then push the repo
```cmd
  git add .
  git commit -m "wiki configured"
  git push
```

N.B. The first `git push` will take a while to complete as it also uses npm to install
the wiki package with all its dependencies.

That's it, you can now checkout your application at:
```
  http://wiki-$yournamespace.rhcloud.com
```
One of the first things you will want to do is to claim your site.

## Backing Up Your Data

There are a number of strategies for backing up your data:-

1. OpenShift applications can be backed up using the snapshot command in their command line tool, see [Application Backup and Restore](https://developers.openshift.com/en/managing-backing-up-applications.html).
2. The Federated Wiki software will create an export file containing all the pages within a site, downloaded by visiting `/system/export.json`, see [Save Your Wiki](forage.ward.fed.wiki.org/save-your-wiki.html).
3. The directories containing the site's data can be copied `sftp`. See below for data locations.

## Data Locations

Below `{FQDN}` is the alias used to access the wiki.

### Flat Files

Your data is stored in `$OPENSHIFT_DATA_DIR/{FQDN}/pages` and `$OPENSHIFT_DATA_DIR/{FQDN}/status`.

### LevelDB

Your data is stored in `$OPENSHIFT_DATA_DIR/{FQDN}/leveldb` and `$OPENSHIFT_DATA_DIR/{FQDN}/status`. If you created any
pages before starting to use LevelDB, you will also want to save `$OPENSHIFT_DATA_DIR/{FQDN}/pages` as old pages will
only get saved to LevelDB when they are modified.


### If you disable Farm Mode

If you disable Farm Mode, you will only have a single wiki, remove `/{FQDN}` from the locations above for the locations of the sites data.


## Developer Notes

Further information about the Federated Wiki node package can be found at:

https://github.com/fedwiki/wiki-node

The OpenShift `nodejs` cartridge documentation can be found at:

http://openshift.github.io/documentation/oo_cartridge_guide.html#nodejs

We use [https://github.com/ramr/nodejs-custom-version-openshift](custom Node.js version quickstart) to control the version of node.js, so we can control the version being used.
