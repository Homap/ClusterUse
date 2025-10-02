# Useful notes and tips on general use of cluster use and in particular, Dardel

Computer project: *naiss2025-5-325* <br>
Storage project: */cfs/klemming/projects/supr/naiss2025-6-220*

Useful tips:

In your macOS `~/.zshrc`, you can add the following line: <br>
`alias dardel='ssh -i ~/.ssh/id-ed25519-pdc homapy@dardel.pdc.kth.se'`

In your dardel `~/.bashrc`, you can add the following lines: <br>
`alias rm='rm -i'` <br>
`alias mv='mv -i'`

In this way, you are always warned before deleting or overwriting another file. <br>
You could also create an alias for your project directories to make you remember the path more easily, for example: <br>
`alias cpdna='cd /cfs/klemming/projects/supr/naiss2025-6-220/members/homa/projects/organellar_DNA_volvocine'`

I cannot imagine a terminal without colors, you can get that by adding the following lines to your `~/.bash_profile`

`alias ls="ls --color=auto"`

`export PS1="\[\033[36m\]\u\[\033[m\]@\[\033[32m\]\h:\[\033[33;1m\]\w\[\033[m\]\$ "`

This changes your shell prompt (PS1). <br>
- \u = username <br>
- \h = hostname <br>
- \w = current working directory <br>
The `\[\033[…m\]` parts are ANSI color codes: <br> 
- 36m = cyan <br>
- 32m = green <br>
- 33;1m = bold yellow <br>
- m = reset

You need to refresh `~/.bashrc` and `~/.bash_profile` after adding these lines by: <br>
`source ~/.bashrc`

`source ~/.bash_profile`

I also want to write codes in VsCode open in my computer but save it to the cluster. This is easy in Linux computers but in MacOS, we need to mount the server to the Finder. An amazing tool for this is MountainDuck (Price per user 49$).

