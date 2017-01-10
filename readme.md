# Web Platform Tests Development Environment

This project defines an [Ansible](https://www.ansible.com/) playbook for
configuring a Unix-like system to contribute to [the Web Platform Test
suite](https://github.com/w3c/web-platform-tests).

Goals:

- Automated installation of the Web Platform Test suite itself
  - Version control system (git at the time of this writing)
  - Source code repository
  - Relevant system configuration (e.g. host file modifications)
- Automated installation of the development version for various browsers

## Requirements

A Unix-like system with the following software:

- Python 2.7 ([installation instructions](https://wiki.python.org/moin/BeginnersGuide/Download))
- Ansible ([installation instructions](https://docs.ansible.com/ansible/intro_installation.html))

## Installation

Run the following command from the root of this project:

    ansible-playbook provision.yml

This command will prompt for the system administrator's password because the
installation procedure necessarily modifies system-level configuration.

## License

Copyright 2016 Bocoup under [the GNU General Public License
v3.0](https://www.gnu.org/licenses/gpl-3.0.html)
