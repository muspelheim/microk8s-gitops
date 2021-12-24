# Add autocompletion to the bash

Run:
```shell
flux completion bash > ~/.fluxrc
kustomize completion bash > ~/.kustomizerc
helm completion > ~/.helmrc
```

Then add to the end of `~/.bashrc` following lines
```shell
if [ -f ~/.kustomizerc ]; then
. ~/.kustomizerc
fi

if [ -f ~/.fluxrc ]; then
. ~/.fluxrc
fi

if [ -f ~/.helmrc ]; then
. ~/.helmrc
fi
```
