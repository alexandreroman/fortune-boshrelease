# Fortune BOSH addon/release

This is a BOSH addon which adds output from the famous CLI
[fortune](https://linux.die.net/man/6/fortune) to the login banner of your VMs.

Next time you go SSH to your VM, you will see a quote coming from `fortune`.

## How to use it?

Upload this release to your BOSH environment:
```shell
$ bosh -e <env> upload-release git+https://github.com/alexandreroman/fortune-boshrelease
```

Create an addon deployment. You may reuse
[this template](https://raw.githubusercontent.com/alexandreroman/fortune-boshrelease/master/templates/fortune.yml):
```yaml
---
releases:
- name: fortune
  version: 1
  url: git+https://github.com/alexandreroman/fortune-boshrelease

addons:
- name: add-fortune-banner
  jobs:
  - name: fortune-banner
    release: fortune
```

Apply this addon to your target deployment:
```shell
$ bosh -e <env> -d <dep> update-runtime-config fortune.yml
```

This addon will be applied on the next deploy:
```shell
$ bosh -e <env> -d <dep> deploy <opsfile>
```

Check everything is OK by connecting to one of your VM:
```shell
$ bosh -e <env> -d <dep> ssh <vm>
Unauthorized use is strictly prohibited. All access and activity
is subject to logging and monitoring.
Welcome to Ubuntu 16.04.5 LTS (GNU/Linux 4.15.0-38-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

---
He who minds his own business is never unemployed.
---

The programs included with the Ubuntu system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by
applicable law.

Last login: Mon Nov  5 01:27:05 2018 from 192.168.101.5
To run a command as administrator (user "root"), use "sudo <command>".
See "man sudo_root" for details.

dummy/0266d354-8ef8-4275-a5fc-84b459a91500:~$
```

Notice the new banner between `---`:
you can now enjoy quotes from `fortune`!

## Contribute

Contributions are always welcome!

Feel free to open issues & send PR.

## License

Copyright &copy; 2018 Pivotal Software, Inc.

This project is licensed under the [Apache Software License version 2.0](https://www.apache.org/licenses/LICENSE-2.0).
