# Github setup

* Create an account on [github.com](github.com).
* If at an academic institution, apply for an [account upgrade](https://education.github.com/benefits).
* Follow [these instructions](https://docs.github.com/en/github/authenticating-to-github/connecting-to-github-with-ssh) to set up ssh key access to github. You can either create a new key for github, in which case name it something obvious like `id_github`, or use an existing key if you have one. I tend to create new keys for different machines and accounts. I use [bitwarden](https://bitwarden.com/) (`brew --cask install bitwarden`) to generate and store key passphrases.
* Modify your ssh config file (`vim ~/.ssh/config`) by adding the following lines: (replacing `<ID_FILE_NAME>` with the key that was just created)

        Host github.com
            Hostname github.com
            User git
            IdentityFile ~/.ssh/<ID_FILE_NAME>
            

* Install github command line tools: `brew install gh`