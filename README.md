# TalkTerminal

TalkTerminal (tt) is a small script that uses the feature of [ dtach ]( https://github.com/crigler/dtach ) to automatically input command from one dtach session to another.

Here is a [simple screencast](./talkTerminal.gif) showing the functionalities of tt.

The software is more a proof of concept in the current state and might need some adjustement for it to work like you'd like.

## Typical use case :

First create your `dtach` sessions using the `dt` wrapper :

```
terminalA:$ tt dt -c main
-------------------------
terminalB:$ tt dt -c two
```

Now modify your `$PROMPT_COMMAND` variable so that it filters your input and executes the content of the file `$HOME/.config/tt/actions/<current tt session name>`

An example is as follow :

```
terminalA:$ PROMPT_COMMAND=history 1 | sed -n 's/^ *[0-9]* *\([[:alnum:]]*\).*/\1/p' |grep -E "\<(cd|cdp)\>" >/dev/null && source ~/.config/tt/actions/main
```

A short explanation of the command above :
* we first get the last command in the history
* we then filter the command name usig sed
* finally, we source the file if the command name correspond to the grep regexp. Here it will be sourced every time we input a `cd` or a `cdp` as command

Now we can 'synchronize' our two dtach session :

```
terminalB:$ tt sync main
```

Now every time terminalA change directory, it will also change the current working directory of terminalB.
If you want to customize the command being executed you can input something like :

```
terminalB:$ tt sync -c 'cd $(pwd)|clear|ls' main
```
Note that you can add any number of terminal to sync with the 'main' session.

When your done, you can either unsync :

```
terminalB:$ tt sync -u main
```

Or unsync all terminal connected to the session :

```
terminalXYZ:$ tt clr main
```
