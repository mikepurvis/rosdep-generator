rosdep-generator
================

If you use [bloom](http://wiki.ros.org/bloom) to release catkin packages into a rosdistro other than the public
one, you've probably come across a situation
[like this one](http://answers.ros.org/question/135976/bloom-could-not-resolve-rosdep-key-message_runtime/),
where the package you want to build depends on a package in the public rosdistro, and bloom doesn't know how to
resolve the rosdep key, since you've changed `ROSDISTRO_INDEX_URL`.

Enter *rosdep-generator*, which parses the rosdistro file, and dumps a rosdep-formatted yaml into the current
directory:

```
$ ./rosdep-generator --distro indigo
Downloading: https://github.com/ros/rosdistro/raw/master/indigo/distribution.yaml
Parsing 250264 bytes of yaml.
Extracting package names.
Assembling output yaml structure.
Outputting: ros-rosdistro-indigo.yaml
Complete. Suggested next steps:

    sudo sh -c 'echo "yaml file:///Users/mikepurvis/rosdep-generator/ros-rosdistro-indigo.yaml indigo" > /etc/ros/rosdep/sources.list.d/90-ros-rosdistro-indigo.list'
    rosdep update
```

Execute the next steps to make rosdep aware of the public packages, then run:

```
$ export ROSDISTRO_INDEX_URL=https://github.com/me/my-secret-rosdistro/raw/master/index.yaml
$ bloom-release -r indigo -t indigo my_secrete_package
```

Bloom will find your dependencies now.
