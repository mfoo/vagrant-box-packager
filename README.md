# vagrant-box-packager

A basic tool to export an existing Vagrant virtual machine as a new box file that can be re-used as
a Vagrant base box. Produces an output compatible with Vagrant's config.vm.box_url setting. The
produced box file and metadata.json file can be copied to a web server and used with Vagrant. For
instance:

```
$ vagrant init 'hashicorp/precise64'
# Do some work to make your new base box
$ vagrant-box-packager -v 0.0.1 -n 'mfoo/base_box' -t http://localhost:8081/files
```

The script will produce a directoy called 'mfoo' with the base_box image and a metadata.json
file inside it. These two files should then be copied to the web server such that they become available
at http://localhost:8081/files/mfoo/metadata.json and
http://localhost:8081/files/mfoo/base_box-0.0.1.box.

They can then be used in another Vagrant project:

    $ vagrant box add http://localhost:8081/files/mfoo/metadata.json
    $ vagrant init 'mfoo/base_box'

If you ship a new box with a newer version to the web store above, Vagrant will inform you that a
new version is available and the `vagrant box update` command will go and fetch it for you. To
use the new version of the box you must destroy your current Vagrant virtual machine and make a
new one.

This tool is based on https://github.com/vube/vagrant-boxer. It is a much stripped-down testless
Ruby-based version that's quite specifically tailored to my needs (making pre-provisioned Vagrant
box updates available to people). It assumes that you're going to be hosting the box somewhere
and that the Vagrantfile is in $CWD. It has no dependencies outside of the Ruby standard library.
Improvements welcome.
