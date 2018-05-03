# git (shallow)

Want to make sure `go get ...` always does a shallow git pull/clone? This is a little hack that will do it. Basically its just a wrapper for git, and if "pull/clone" comes up it makes sure to make it shallow. To make it work you have to install this as `$GOPATH/bin/git` and then *prepend* that to the PATH so that the wrapper will call the hardcoded git path.

```
$ go get github.com/schollz/git
$ export PATH=$GOPATH/bin:$PATH
```

# Benchmarks

Here's a benchmark showing a 50% reduction in disk usage. 

## without shallow git


```
% docker run -it golang:1.10 /bin/bash
root@d9208178f1fa:/go# time go get github.com/juju/juju/...
real    7m35.631s
user    1m40.059s
sys     0m45.436s
root@d9208178f1fa:/go# du -sh .
1.1G
```

## with shallow git

```
% docker run -it golang:1.10 /bin/bash
root@68135fb64a3e:/go# go get github.com/schollz/git
root@68135fb64a3e:/go# export PATH=$GOPATH/bin:$PATH
root@68135fb64a3e:/go# time go get github.com/juju/juju/...
real    3m0.335s
user    0m29.192s
sys     0m17.253s
root@d9208178f1fa:/go# du -sh .
499M    .
```
