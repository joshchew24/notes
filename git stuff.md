# git stuff
- if git is slow in wsl
	- https://github.com/microsoft/WSL/issues/4401#issuecomment-670080585
	- in `root`: `vim /etc/profile`
	- add:
```bash
# checks to see if we are in a windows or linux dir
function isWinDir {
  case $PWD/ in
    /mnt/*) return $(true);;
    *) return $(false);;
  esac
}
# wrap the git command to either run windows git or linux
function git {
  if isWinDir
  then
    git.exe "$@"
  else
    /usr/bin/git "$@"
  fi
}
```