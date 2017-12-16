## [install kubenetes completion](https://medium.com/merapar/fixing-bash-autocompletion-on-macos-for-kubectl-and-kops-e87f019652e8)

```bash
brew install bash-completion@2 # for bash 4.1

## then update your ~/.bash_profile or ~/.bashrc follow the caveats section

source <(kubectl completion bash)

if [ -f $(brew --prefix)/etc/bash_completion ]; then 
. $(brew --prefix)/etc/bash_completion
fi
```
