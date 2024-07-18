# mongodb_on_k3s_vagrant_libvirt_ansible

Vagrant-libvirt setup that creates a VM with [k3s](https://k3s.io/), the minimal
lightweight Kubernetes distribution.

On top of k3s, this setup installs a MongoDB using the [Bitnami
chart](https://github.com/bitnami/charts/tree/main/bitnami/mongodb).

Default OS is openSUSE Leap 15.6, but that can be changed in the Vagrantfile.
Please be aware, that this might break the Ansible provisioning.

## Vagrant

1. You need `vagrant`, obviously. And `git`. And Ansible...
1. Fetch the box, per default this is `opensuse/Leap-15.6.x86_64`, using
   `vagrant box add opensuse/Leap-15.6.x86_64`.
1. Make sure the git submodules are fully working by issuing
   `git submodule init && git submodule update`
1. Run `vagrant up`
1. Connect to the URL that Ansible printed out at the end of the provisioning.
   You can either use a locally installed `mongosh` or run a container with
   podman or docker, e.g.
   `podman container run --rm -ti --entrypoint bash docker.io/library/mongo`.

   ```
   $ mongosh --username root mongodb://mongodb.192.0.2.13.sslip.io:80
   Enter password: *************
   Current Mongosh Log ID: 6698f11e951bc2f385149f47
   Connecting to:          mongodb://<credentials>@mongodb.192.0.2.13.sslip.io:80/?directConnection=true&appName=mongosh+2.2.10
   [...]

   test>
   ```

   To connect as the unprivileged user `vagrant-libvirt` to the database of the
   same name, use the following command:

   ```
   $ mongosh --username vagrant-libvirt mongodb://mongodb.192.0.2.13.sslip.io:80/vagrant-libvirt
   Enter password: *************
   Current Mongosh Log ID: 6698f1376d240778f7149f47
   Connecting to:          mongodb://<credentials>@mongodb.192.0.2.13.sslip.io:80/vagrant-libvirt?directConnection=true&appName=mongosh+2.2.10
   [...]

   vagrant-libvirt>
   ```

1. Party!

## Cleaning up

The VMs can be torn down after playing around using `vagrant destroy`. This will
also remove the kubeconfig file `ansible/k3s-kubeconfig`.

## License

BSD-3-Clause

## Author Information

I am Johannes Kastl, reachable via git@johannes-kastl.de
