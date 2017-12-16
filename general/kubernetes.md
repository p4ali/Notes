## [install kubenetes completion](Fixing Bash autocompletion on MacOS for kubectl)

```bash
brew install bash-completion@2 # for bash 4.1

## then update your ~/.bash_profile or ~/.bashrc follow the caveats section

source <(kubectl completion bash)

if [ -f $(brew --prefix)/etc/bash_completion ]; then 
. $(brew --prefix)/etc/bash_completion
fi
```
