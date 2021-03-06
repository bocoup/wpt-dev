# Web Platform Tests Development Environment

This project defines an [Ansible](https://www.ansible.com/) playbook for
configuring a Unix-like system to contribute to [the Web Platform Test
suite](https://github.com/w3c/web-platform-tests).

Features:

- the latest revision of the Web Platform Test suite is made available in the
  current working directory
- the command `chromium-latest` will be defined. This command will fetch the
  latest version of the Linux x64 build of the Chromium browser and run it
  (script courtesy [the "chromium-latest-linux"
  project](https://github.com/scheib/chromium-latest-linux))
- Firefox Nightly will be installed and available via the `firefox-nightly`
  command (this browser automatically receives updates from Mozilla)
- the `wptrunner` project will installed (see the `runtests` script for a
  simplified interface designed for the environment defined here)

## Requirements

A Unix-like system with the following software:

- Python 2.7 ([installation instructions](https://wiki.python.org/moin/BeginnersGuide/Download))
- Ansible ([installation instructions](https://docs.ansible.com/ansible/intro_installation.html))

## Installation

Run the following command from the root of this project:

    ansible-playbook provision.yml

This command will prompt for the system administrator's password because the
installation procedure necessarily modifies system-level configuration.

Tooling will be installed into the local `tools/` directory by default. This
location may be changed by specifying the `tools_dir` variable via Ansible's
`--extra-vars` option, as in:

    ansible-playbook --extra-vars "tools_dir=/opt/web-platform-tests" provision.yml

## License

Copyright 2016 Bocoup under [the GNU General Public License
v3.0](https://www.gnu.org/licenses/gpl-3.0.html)
