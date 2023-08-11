# git-ssh-shim

When using SSH to access git repositories, especially via GitHub, clients will sometimes
require **multiple** SSH keys:

 - Users may have multiple GitHub accounts, each with different keys, for different
   GitHub organizations; this scenario is particularly common among contractors and consultants
   who work with multiple GitHub organizations at once.

 - Automated processes may use GitHub deploy keys -- which must be unique per repository -- and
   may wish to use the same SSH agent session to access multiple repositories.

Unfortunately, using multiple SSH keys with git (and GitHub) is not straightforward; it's quite
easy to use a key that is recognized by the provider but not authorized for a specific repository,
causing the git operation to fail and bypassing the client's normal key negotiation process.

A common solution is to write a _shim_ program that handles key negotiation; this repository
provides such a program.


## Usage

 1. Download the `git-ssh-shim` shell script.

    This guide will assume installation to the working directory.

 2. Configure your environment to use this script for git ssh operations.

    If you are using deploy keys, simply set `GIT_SSH_COMMAND` to the shim:

    ```sh
    export GIT_SSH_COMMAND="$(pwd)/git-ssh-shim"
    ```

    If you are using user keys, include your GitHub username in the command:

    ```sh
    export GIT_SSH_COMMAND="$(pwd)/git-ssh-shim --user <github-username>"
    ```

 3. Use `ssh-agent` (e.g. via `ssh-add`) to add keys you want to use.
