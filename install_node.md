# Install Node using nvm

To install node, we can install it using NVM (node version manager). </br>
Follow the steps below or follow the [documentation](https://www.digitalocean.com/community/tutorials/how-to-install-node-js-on-ubuntu-20-04)

- Download: `curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.3/install.sh | bash`
- Source (you can use your shell, here using bash): `source ~/.bashrc`
- Check node remote versions: `nvm list-remote`
- Install specific version as your requirement: `nvm install v18.18.2`
- All done!! now check the version: `node -v`

NB: We can install multiple versions then use specific version `nvm use v18.10.0`
