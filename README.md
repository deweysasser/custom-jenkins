Custom Jenkins
==============

An example of a custom Jenkins image

Usage
=====

```
docker build -t my-jenkins .
docker run -d --name my-jenkins -p 8080:8080 my-jenkins
```

Now open your docker machine (probably `localhost`) in your browser
at:

http://localhost:8080

You can log in with username `admin` and password `admin`.

You should see a single Job, called "hello" at 

http://localhost:8080/job/Hello

If you click "Build Now" it should run.


Updating the plugin list
========================

With the plugins installed on a local jenkins container run as above,
run

```
curl -s -k "http://admin:admin@localhost:8080/pluginManager/api/json?depth=1" \
  | jq -r '.plugins[].shortName' | tee plugins.txt
```

and rebuild your image

Updating the jobs
=================

To update the state of all jobs, run

```
docker cp my-jenkins:/var/jenkins_home/jobs - | tar x
```

This will get all jobs configure *and* all builds previously run.  You
may remove the builds directories of jobs if desired.

Also, to make sure they start from build #1, remove the
`nextBuildNumber` flies.
