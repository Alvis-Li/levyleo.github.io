---
layout: post
title: Error when running Swift3 in REPL
---

```
Traceback (most recent call last):<br>
  File "", line 1, in <br>
NameError: name 'run_one_line' is not defined<br>
Traceback (most recent call last):<br>
  File "", line 1, in <br>
NameError: name 'run_one_line' is not defined
```



 fix:

```
pip install six
```



[ link](https://github.com/Homebrew/homebrew-core/issues/2712)