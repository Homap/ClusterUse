# Useful notes and tips on general use of cluster use and in particular, Dardel

Computer project: *naiss2025-5-325* <br>
Storage project: */cfs/klemming/projects/supr/naiss2025-6-220*

Useful tips:
In your macOS ~/.zshrc, you can add the following line:
alias dardel='ssh -i ~/.ssh/id-ed25519-pdc homapy@dardel.pdc.kth.se'

In your dardel ~/.bashrc, you can add the following lines:
alias rm='rm -i'
alias mv='mv -i'

In this way, you are always warned before deleting or overwriting another file.
You could also create an alias for your project directories to make you remember the path more easily, for example:
alias cpdna='cd /cfs/klemming/projects/supr/naiss2025-6-220/members/homa/projects/organellar_DNA_volvocine'

I cannot imagine a terminal without colors, you can get that by adding the following lines to your ~/.bash_profile
alias ls="ls --color=auto"
export PS1="\[\033[36m\]\u\[\033[m\]@\[\033[32m\]\h:\[\033[33;1m\]\w\[\033[m\]\$ "
. "$HOME/.cargo/env"

You need to refresh ~/.bashrc and ~/.bash_profile after adding these lines by:
source ~/.bashrc 
source ~/.bash_profile

An amazing tool for mounting cluster to the Finder of macOS is MountainDuck. Price 49$ for one user.

