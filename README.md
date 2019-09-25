# How to exit vim
Below are some simple methods for exiting vim.

## The simple way
Credit: @tomnomnom

```
:!ps axuw | grep vim | grep -v grep | awk '{print $2}' | xargs kill -n
```

## The ps-less way
Credit: @tomnomnom

```
:!kill -9 $(find /proc -name "cmdline" 2>/dev/null | while read procfile; do if grep -Pa '^vim\x00' "$procfile" &>/dev/null; then echo $procfile; fi; done | awk -F'/' '{print $3}' | sort -u)
```
