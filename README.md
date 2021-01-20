# windefender-box

| License | Versioning | Build |
| ------- | ---------- | ----- |
| [![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT) [![FOSSA Status](https://app.fossa.com/api/projects/git%2Bgithub.com%2Fextra2000%2Fwindefender-box.svg?type=shield)](https://app.fossa.com/projects/git%2Bgithub.com%2Fextra2000%2Fwindefender-box?ref=badge_shield) | [![semantic-release](https://img.shields.io/badge/%20%20%F0%9F%93%A6%F0%9F%9A%80-semantic--release-e10079.svg)](https://github.com/semantic-release/semantic-release) | [![Build status](https://ci.appveyor.com/api/projects/status/x4089wg5edwf0h8i/branch/master?svg=true)](https://ci.appveyor.com/project/nikAizuddin/windefender-box/branch/master) |

Developer box for [Windows Defender](https://github.com/malice-plugins/windows-defender) scanner.


## Getting Started

Clone this repository and `cd`:
```
$ git clone --recursive https://github.com/extra2000/windefender-box.git
$ cd windefender-box
```


## Creating Vagrant Box

Copy vagrant file from `vagrant/examples/` and then create the vagrant box (you can change to `--provider=libvirt` if you want to use Libvirt provider):
```
$ cp -v vagrant/examples/Vagrantfile.windefender-box.fedora-33.x86_64.example vagrant/Vagrantfile.windefender-box
$ vagrant up --provider=virtualbox
```

Provision the vagrant box and build Windows Defender image:
```
$ vagrant ssh windefender-box -- sudo salt-call state.highstate
$ vagrant ssh windefender-box -- sudo salt-call state.sls windefender
```


## Examples

To test with EICAR file:
```
$ vagrant ssh windefender-box
$ curl https://secure.eicar.org/eicar.com --output eicar.com
$ podman run --init --rm -v ./eicar.com:/malware/eicar.com:ro,z --security-opt seccomp=/opt/windefender/src/seccomp.json malice-plugins/windefender eicar.com
```

To test with malware file from https://capesandbox.com/analysis/110788/:
```
$ vagrant ssh windefender-box
$ curl https://capesandbox.com/file/sample/110788/50e2c6aac34de9ed4e1b3fcfcd5aaa34892696f2681aa5e8c45a5dbe0915a43c/ --output samplefile
$ podman run --init --rm -v ./samplefile:/malware/samplefile:ro,z --security-opt seccomp=/opt/windefender/src/seccomp.json malice-plugins/windefender samplefile
```


## License

[![FOSSA Status](https://app.fossa.com/api/projects/git%2Bgithub.com%2Fextra2000%2Fwindefender-box.svg?type=large)](https://app.fossa.com/projects/git%2Bgithub.com%2Fextra2000%2Fwindefender-box?ref=badge_large)
