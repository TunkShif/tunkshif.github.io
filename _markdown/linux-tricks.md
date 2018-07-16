TITLE: Linux Tricks
TAG: linux
# Linux Tricks
### Merge PDF
**Requirement**: `ghostscript`
``` bash
gs -dNOPAUSE -sDEVICE=pdfwrite -sOUTPUTFILE=output.pdf -dBATCH first.pdf second.pdf
```

### Parse JSON
**Requirement**: `jq`
``` bash
curl -s https://v1.hitokoto.cn/ | jq '.hitokoto'
```

### Clipboard
**Requirement**: `xclip`
``` bash
pwd | xclip -selection 'clipboard' # copy current path
```
