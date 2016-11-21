---
layout: post
title: using p4merge with git on fedora 16
date: 2012-09-14 15:00:12 +0800
---

[p4merge][1] is Perforce Visual Merge and Diff Tools. download [p4v.tgz][2] for linux.

### install p4merge

```
tar zxf p4v.tgz
sudo mv p4v-2012.1.500245/lib/p4v/ /lib/
sudo mv p4v-2012.1.500245/bin/p4merge* /bin/
```

### config git

add following lines into ~/.gitconfig

```
[diff]
    tool = p4merge
[difftool "p4merge"]
    cmd = p4merge \\\"$LOCAL\\\" \\\"$REMOTE\\\"
[merge]
    tool = p4merge
[mergetool "p4merge"]
    cmd = p4merge \\\"$BASE\\\" \\\"$LOCAL\\\" \\\"$REMOTE\\\" \\\"$MERGED\\\"
```

[1]: http://www.perforce.com/product/components/perforce_visual_merge_and_diff_tools
[2]: http://www.perforce.com/downloads/complete_list
