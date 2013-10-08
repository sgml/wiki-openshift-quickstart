Wiki on OpenShift
=================
This git repository helps you to set-up the node.js version of the Smallest Federated Wiki on OpenShift. 

Running on OpenShift
--------------------

It is simple to create a Federated Wiki site using either the OpenShift command line tools, or the Web 
Interface.

### Creating using the command line

Create an account at http://openshift.redhat.com/ and set up your local machine with the client tools.

Create a node-0.10 application (you can call your application whatever you want)
```cmd
  rhc app create wiki nodejs-0.10
```

Add this upstream Wiki Quickstart repo
```cmd
  cd wiki
  git remote add upstream -m master https://github.com/paul90/wiki-openshift-quickstart.git
  git pull -s recursive -X theirs upstream master
```

#### Configuration

##### Storetype

By default flat files will be used store any edits. The alternatives are: -
* LevelDB - to enable this you need to uncomment the line, show below, in ```server.js```

```js
        // self.database = '{"type": "./leveldb"}';
```
* MongoDB - details to be added later...
* Redis - details to be added later...


Then push the repo
```cmd
  git add .
  git commit -m "wiki configured"
  git push
```

N.B. The first ```git push``` will take a while to complete as it also uses npm to install the wiki package 
and its dependencies.

That's it, you can now checkout your application at:
```
  http://wiki-$yournamespace.rhcloud.com
```
One of the first things you will want to do is to claim your site.


Developer Notes
---------------

Further information about the Federated Wiki node package can be found at:

https://github.com/WardCunningham/wiki

The OpenShift `nodejs` cartridge documentation can be found at:

http://openshift.github.io/documentation/oo_cartridge_guide.html#nodejs
