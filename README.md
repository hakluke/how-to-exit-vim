# how-to-exit-vim
Below are some simple methods for exiting vim.

```
:!ps axuw | grep vim | grep -v grep | awk '{print $2}' | xargs kill -n
```
