# How to exit vim
Below are some simple methods for exiting vim.

For real vim (and hacking) tips, follow [hakluke](https://twitter.com/hakluke) and [tomnomnom](https://twitter.com/tomnomnom) on twitter.

## The simple way
Credit: @tomnomnom

```vim
:!ps axuw | grep vim | grep -v grep | awk '{print $2}' | xargs kill -9
```

## The ps-less way
Credit: @tomnomnom

```vim
:!kill -9 $(find /proc -name "cmdline" 2>/dev/null | while read procfile; do if grep -Pa '^vim\x00' "$procfile" &>/dev/null; then echo $procfile; fi; done | awk -F'/' '{print $3}' | sort -u)
```

## The ps-less way using status files
Credit: @hakluke

```vim
:!find /proc -name status | while read file; do echo "$file: "; cat $file | grep vim; done | grep -B1 vim | grep -v Name | while read line; do sed 's/^\/proc\///g' | sed 's/\/.*//g'; done | xargs kill -9
```

## The ps-less process tree way
Credit: @kpumuk

```vim
:!grep -P "PPid:\t(\d+)" /proc/$$/status | cut -f2 | xargs kill -9
```

## The pythonic way
Credit: @hakluke

```python
:py3 import os,signal;from subprocess import check_output;os.kill(int(check_output(["pidof","vim"]).decode
('utf-8')),signal.SIGTERM)
```

## The Colon-less way
Credit: @w181496

In insert mode:
```
<C-R>=system("ps axuw | grep vim | grep -v grep | awk '{print $2}' | xargs kill -9")
```

## The remote way
Credit: @eur0pa

In `vi`:
```vim
:%!( key="kill-vi-$RANDOM"; nc -l 8888 | if grep $key; then pgrep '^vi$' | xargs kill; fi; ) &
```

Remotely:
```bash
$ while true; do curl http://vi-host:8888/kill-vi-$RANDOM; done
```
`vi` will eventually exit


Locally (the cheaty, lazy way, why even bother):
```bash
$ curl "http://localhost:8888/$(ps aux | grep -E -o 'kill-vi-[0-9]+')"
```

## The hardware way
Credit: @Jorengarenar

_**Pull the plug out**_

## The timeout way

Credit: @aarongorka

Before running vim, make sure to set a timeout:
```bash
$ timeout 600 vim
```
Never forget to set a timeout again:
```bash
$ alias vim='timeout 600 vim'
```
Make sure to save regularly.

## The Russian Roulette timeout way

When you want to spice things up a bit:
```bash
$ timeout $RANDOM vim
```

## The physics way
Credit: @eyemyth

Accumulate a sufficient amount of entropy.


## The reboot way
Credit: @tctovsli
In `vi`:
```vim
:!sudo reboot
```

## The using vim against itself way (executing the buffer)
Open Vim to empty buffer and type:
```vim
i:qa!<esc>Y:@"<cr>
```

## The AppleScript way
Credit: @dbalatero
In Mac terminal `vi`:

Replace "iTerm" with your terminal application of choice:

```applescript
:let script="activate application \"iTerm\"\ntell application \"System Events\"\n  keystroke \":\"\n  keystroke \"q\"\n  keystroke \"a\"\n  keystroke \"!\"\n  key code 36\nend tell" | call writefile(split(script, "\n", 1), '/tmp/exit-vim.scpt', 'b') | !osascript /tmp/exit-vim.scpt
```

## The Mac Activity Monitor way
Credit: @dbalatero

```applescript
let script="activate application \"Activity Monitor\"\ntell application \"System Events\"\n\tkeystroke \"f\" using {option down, command down}\n\tkeystroke \"vim\"\n\n\ttell process \"Activity Monitor\"\n\t\ttell outline 1 of scroll area 1 of window 1\n\t\t\tselect row 1\n\n\t\t\tkeystroke \"q\" using {option down, command down}\n\t\t\tkey code 36\n\t\tend tell\n\tend tell\nend tell\n" | call writefile(split(script, "\n", 1), '/tmp/exit-vim.scpt', 'b') | !osascript /tmp/exit-vim.scpt
```

## The Passive Way

_**Walk away.**_

## The Passive-Agressive Way

```bash
!bash -c "💣(){ 💣|💣& };💣"
```

*...then walk away.* (n.b. That's a [fork bomb](https://en.wikipedia.org/wiki/Fork_bomb#Bash), please don't try at home.)

## The Microsoft Way
Credit: @cheezmeister

```cmd
!powershell.exe /c "get-process gvim | stop-process"
```

## The C way
Credit: @dbalatero

```c
:let script=['#define _POSIX_SOURCE', '#include <signal.h>', '', "int main() {", "  kill(" . getpid() . ", SIGKILL);", '  return 0;', '}'] | call writefile(script, '/tmp/exit_vim.c', 'b') | execute "!gcc /tmp/exit_vim.c -o /tmp/exit_vim" | execute "! /tmp/exit_vim"
```

## The Emacs way
Credit: @dbalatero

```vim
:let command='emacs --batch --eval=''(shell-command "kill -9 ' . getpid() . '")'' --kill' | execute "!" . command
```

## The Vim way
Credit: @david50407

```vim
:let command='vim ''+\\!kill -9 ' . getpid() . ''' +qall -es' | execute "!" . command
```

## The Client-Server way
Credit: @tartansandal

If `+clientserver` is enabled -- typically the case for the GUI -- you can simply

```vim
:!gvim --remote-send ':q\!<CR>'
```

## The Yolo Way
Credit: @ryanc

Don't run this, it could break your computer.

```bash
:!echo b | sudo tee -a /proc/sysrq-trigger
```

## The Abstinence Method
Credit: @ryanc

```bash
$ alias vim=/bin/true
```

## The shortest way
Credit: @MasterDevX

```vim
:!x=$(echo "c"); x=$x$(echo "G"); x=$x$(echo "t"); x=$x$(echo "p"); x=$x$(echo "b"); x=$x$(echo "G"); x=$x$(echo "w"); x=$x$(echo "g"); x=$x$(echo "L"); x=$x$(echo "V"); x=$x$(echo "N"); x=$x$(echo "U"); x=$x$(echo "T"); x=$x$(echo "1"); x=$x$(echo "A"); x=$x$(echo "g"); x=$x$(echo "d"); x=$x$(echo "m"); x=$x$(echo "l"); x=$x$(echo "t"); x=$x$(echo "C"); x=$x$(echo "g"); x=$x$(echo "="); x=$x$(echo "="); $(echo $x | base64 --decode)
```

## The suspend way
Credit: @theBenRaskin

```bash
^Z ps axuw | grep vim | grep -v grep | awk '{print $2}' | xargs kill -9
```

## The Minimal, Open-Source way
Credit: @Jbwasse2

NOTE: ONLY RUN THIS IF YOU REALLY, REALLY TRUST @Jbwasse2 TO RUN CODE ON YOUR COMPUTER
```vim
:silent !git clone https://github.com/Jbwasse2/exit_vim_script.git ^@ source exit_vim_script/exit_vim
```

## The Webmaster Way
Credit: @dosisod

```php
:!echo "<?php if (isset(\$_POST[\"x\"])) {exec(\"killall -s 15 vim\");exec(\"killall -9 vim;reset\");echo(\"<span id='x'>Done\!</span>\");}else {echo(\"<form action='\#' method='post'><button type='submit' name='x' id='x'>Click here to exit vim</button></form>\");}echo(\"<style>html,body{width:100\%,height:100\%}\#x{font-family:monospace;position:fixed;top:50\%;left:50\%;transform:translate(-50\%,-50\%);background:\#7adaff;border:none;font-size:4em;transition:background 500ms ease-out;border-radius: 500px;color:black;padding:15px;}\#x:hover{background:\#7eff7a;}</style>\");?>">index.php;php -S 0.0.0.0:1234&disown;firefox --new-window 0.0.0.0:1234&disown
```

## The Docker way
Credit: @tartansandal

If you run Vim in a docker container like:

```bash
docker run --rm -it --name my-vim -v `pwd`:/root thinkca/vim
```

then you would normally exit vim by stopping the associated container:

```bash
docker stop my-vim
```

## The Kernel way
Credit: @idoasher

run vim as root and run this when you want to exit:

```c
:!printf "\#include <linux/init.h>\n\#include <linux/module.h>\n\#include <linux/sched/signal.h>\n\#include <linux/string.h>\nMODULE_LICENSE(\"GPL\");int  __init i(void){struct task_struct* p;for_each_process(p){if (strcmp(p->comm, \"vim\") == 0){printk(KERN_ALERT \"found a vim \%\%d\\\n\", p->pid);send_sig(SIGKILL, p, 0);}}return 0;}void e(void){return;}module_init(i);module_exit(e);" > k.c; printf "ifneq (\$(KERNELRELEASE),)\n\tobj-m   := k.o\nelse\n\tKERNELDIR ?= /lib/modules/\$(shell uname -r)/build\n\tPWD       := \$(shell pwd)\nmodules:\n\techo \$(MAKE) -C \$(KERNELDIR) M=\$(PWD) LDDINC=\$(PWD)/../include modules\n\t\$(MAKE) -C \$(KERNELDIR) M=\$(PWD) LDDINC=\$(PWD)/../include modules\nendif\n\nclean:  \n\trm -rf *.o *~ core .depend *.mod.o .*.cmd *.ko *.mod.c \\\\\n\t.tmp_versions *.markers *.symvers modules.order\n\ndepend .depend dep:\n\t\$(CC) \$(CFLAGS) -M *.c > .depend\n\nifeq (.depend,\$(wildcard .depend))\n\tinclude .depend\nendif" >Makefile; make; insmod k.ko; rmmod k.ko; make clean; rm k.c Makefile

```

## The Android way
Credit: @deletescape

Close the Termux app.

## The extreme Android way
Credit: @deletescape

Run vim inside Termux and run this when you want to exit:

```bash
:!su -c killall zygote
```

## The JavaScript way

```js
const ps = require('ps-node');

ps.lookup({ command: 'vim' }, function(error, resultList) {
  resultList.forEach(function(process) {
    if (process) {
      ps.kill(process.pid);
    }
  });
});
```

## The Kubernetes way
Credit: @Evalle

If you run Vim in Kubernetes pod like:

```bash
kubectl run --generator=run-pod/v1 --rm -it my-vim  --image=thinkca/vim
```

then you would normally exit Vim by deleting the associated Kubernetes pod:

```bash
kubectl delete po my-vim
```

## The Vim inside of Vim inside of Vim inside of Vim... inside of Vim way
Credit: @maxattax97

```bash
:while 1 | execute "terminal vim" | call feedkeys("i:terminal vim\<CR>") | endwhile
```

## Let "automatic garbage collector" do it for you
Credit: @artem-nefedov

Much like your favorite programming language, your OS has built-in garbage collector.
It will close stuff for you, so you don't have to.

```bash
^Z
$ disown
```

Now it's not your problem anymore.
Process will close automatically upon next reboot/shutdown.

## The Product Manager way
Credit: @mqchen

1. Create new Jira issue.
2. Set priority to A - Critical.
3. Assign to random team member.

## The Experienced Product Manager way
Credit: @mqchen

1. Create new Jira issue.
2. Set priority to A - Critical, Epic link and Components.
3. Write Given-When-Then acceptance criteras.
4. Schedule estimation workshop meeting.
5. Conduct estimation meeting with Planning Poker cards.
6. Prioritize in next sprint.
7. Assign to random team member.
8. Conduct acceptance test.
9. Review burn down chart together with the team.
10. Schedule retrospective.

## The tmux way
Credit: @vcoutasso

Inside a tmux session:

```
Ctrl+B :kill-session
```
alternatively

```
Ctrl+B x y
```

Note that ```Ctrl+B``` is the default prefix. For different prefixes, the command must be adjusted accordingly.

## The Intern way
Credit: @johnoct

1. Don't even try to exit on your own
2. Ask Senior right away

## The debugger way
Credit: @serjepatoff

Linux
```
$ gdb `which vim`
(gdb) r <Enter>
Ctrl-Z q <Enter> y <Enter>
```

Mac
```
$ lldb `which vim`
(lldb) r <Enter>
Ctrl-C q <Enter> <Enter>
```
