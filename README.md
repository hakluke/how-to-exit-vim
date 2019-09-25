# How to exit vim
Below are some simple methods for exiting vim.

## The simple way

```
:!ps axuw | grep vim | grep -v grep | awk '{print $2}' | xargs kill -n
```
