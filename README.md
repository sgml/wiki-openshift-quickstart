Wiki on OpenShift
=================
This git repository helps you to set-up the node.js version of the Smallest Federated Wiki on OpenShift. 

Running on OpenShift
--------------------

It is simple to create a Federated Wiki site using either the OpenShift command line tools, or the Web 
Interface.

### Creating using OpenShift site

Create an account at http://openshift.redhat.com/ and, optionally, set up your local machine with the 
client tools.

Log into //My Apps//, and go to the //Create an Application// tab




### Creating using the command line

Create an account at http://openshift.redhat.com/ and set up your local machine with the client tools.

Create a node-0.10 application (you can call your application whatever you want)
```
  rhc app create wiki nodejs-0.10
```

Add this upstream Wiki Quickstart repo
```
  cd wiki
  git remote add upstream -m master https://github.com/paul90/wiki-openshift-quickstart.git
  git pull -s recursive -X theirs upstream master
```

#### Configuration

Configure <strong>server.js</strong> file with your information:

Then push the repo
```
  git add .
  git commit -m "my first commmit"
  git push
```

N.B. ```git push``` will take a while to complete as it also runs the build process which installs wiki and its dependencies.

That's it, you can now checkout your application at:
<pre>
  http://wiki-$yournamespace.rhcloud.com
</pre>



The OpenShift `nodejs` cartridge documentation can be found at:

http://openshift.github.io/documentation/oo_cartridge_guide.html#nodejs
