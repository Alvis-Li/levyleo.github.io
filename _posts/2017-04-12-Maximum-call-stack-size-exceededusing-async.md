---
layout: post
title: 'Maximum call stack size exceeded' using async
---

'Maximum call stack size exceeded' using async.eachLimit 

```
I was doing a sync operation. I fixed it by changing from:

callback();
to

setTimeout(callback, 0);
```

