---
layout: post
title: nodejs save遇到的一个坑
---

```
混合类型因为没有特定约束，因此可以任意修改，一旦修改了原型，则必须
调用markModified()
>>>
person.anything = {x:[3,4,{y:'change'}]}            
person.markModified('anything');//传入  anything，表示该属性类型发生变化     
person.save();

```

