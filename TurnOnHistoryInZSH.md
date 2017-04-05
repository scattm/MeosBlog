![](https://68.media.tumblr.com/1078f0f08704cbb296ca67a65975ec0a/tumblr_inline_onx45x6wRA1ursgxc_540.png)

After using ZSH for a long time, one day I can not view my commands history anymore. Frequently updating using Home Brew do fuck me up. But it gave me a chance to look into ZSH and know how to set up things `setopt` command. Before begin (or after finish), please spend your time to read [this man page](https://linux.die.net/man/1/zshoptions) to know more about ZSH options.

So how to turn on history in ZSH? Open your ~/.zshrc file and add following lines at the end:

    # Enable history saving.
    setopt append_history
    # Ignore duplicate with previous command, so if you 
    # execute two `ls` command ZSH will only store one 
    # into your history file.
    setopt hist_ignore_dups