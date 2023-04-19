<img src="./files/Mac-Dev-Playbook-Logo.png" width="250" height="156" alt="Mac Dev Playbook Logo" />

# Mac Development Ansible Playbook

This playbook is based on Jeff Geerling's [mac-dev-playbook](https://github.com/geerlingguy/mac-dev-playbook). His original `README.md` is preserved in this repository as [README-Geerling.md](./README-Geerling.md). I have started with a more stripped-down selection of essentials.

Python 3 was not available in the specified location, so I changed the approach to installing Ansible.

## Installation

  1. Ensure Apple's command line tools are installed (`xcode-select --install` to launch the installer).
  2. Install Python 3.

     The [preferred method](https://opensource.com/article/19/5/python-3-default-mac) is to use `pyenv`, which may be installed via Homebrew. However, Homebrew is installed via this Ansible playbook, creating a chicken-or-egg scenario.

     I resolved this by installing Homebrew, then using that to install `pyenv` as described [here](https://londonappdeveloper.com/installing-python-on-macos-using-pyenv/), then using `pyenv` to install the latest Python 3 and set that as the global version.

     Note that if later on, you install (or your Ansible playbook installs) Homebrew formulae which depend on Brew-managed Python versions, you may need to clean up your `pyenv` by symlinking to the Brew-managed Python as described [here](https://thecesrom.dev/2021/06/28/how-to-add-python-installed-via-homebrew-to-pyenv-versions/).
 
  3. [Install Ansible](https://docs.ansible.com/ansible/latest/installation_guide/index.html):

     _The following steps could easily be automated further by using a shell script. This could also be a good opportunity to take care of `git config --global` options such as `user.name` and `user.email`._

     1. Install Python 3 (required for Ansible).

        1. Install [Homebrew](http://brew.sh/):

        ```sh
        /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
        ```

        2. Use homebrew to install `pyenv`:

        ```sh
        brew install pyenv
        ```

        Follow the onscreen instructions to modify your `~/.zshrc` (or `~/.bashrc`, `~/.profile`, etc.) and restart your shell (or, e.g., `source ~/.zshrc`).

        3. Use `pyenv` to install the latest Python 3 and set it as the global default:

        ```sh
        pyenv install 3.11 && pyenv global 3.11
        ```
         
     2. Upgrade Pip: `python -m pip install --upgrade pip`
     3. Install Ansible: `python -m pip install ansible`

  4. Clone or download this repository to your local drive.
  5. Run `ansible-galaxy install -r requirements.yml` inside this directory to install required Ansible roles.
  6. Copy `default.config.yml` to `config.yml`:

     ```sh
     cp default.config.yml config.yml
     ```
  
  7. Uncomment any packages you wish to install, or edit to your preference, using your favorite text editor or IDE.

     For example, to use `vim`:

     ```sh
     vim config.yml
     ```

  8. Run `ansible-playbook main.yml --ask-become-pass` inside this directory. Enter your macOS account password when prompted for the 'BECOME' password.

> Note: If some Homebrew commands fail, you might need to agree to Xcode's license or fix some other Brew issue. Run `brew doctor` to see if this is the case.

For further instructions, refer to [Jeff Geerling's original project README](./README-Geerling.md) or visit the [upstream repository on GitHub](https://github.com/geerlingguy/mac-dev-playbook).
