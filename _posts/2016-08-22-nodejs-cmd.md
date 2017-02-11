---
layout: post
title: nodejs cmd
---

引入系统模块

```
var exec = require('child_process').exec;
```

 cmd.js

```
var exec = require('child_process').exec;
 
var commandline={
    get:getString,
    run:runCommand
};
 
function runCommand(command){
    exec(
        command
    );
}
 
function getString(command,callback){
    exec(
        command,
        (
            function(){
                return function(err,data,stderr){
                    if(!callback)
                        return;
 
                    callback(data);
                }
            }
        )(callback)
    );
}
 
module.exports=commandline;
```

test.js

```
var cmd=require('../cmd.js');
 
cmd.get(
    'pwd',
    function(data){
        console.log('the current working dir is : ',data)
    }
);
 
cmd.run('touch example.created.file');
 
cmd.get(
    'ls',
    function(data){
        console.log('the current dir contains these files :\n\n',data)
    }
);
 
// cmd.get(
//     `
//         git clone https://github.com/RIAEvangelist/node-cmd.git
//         cd node-cmd
//         ls
//     `,
//     function(data){
//         console.log('the node-cmd clone dir contains these files :\n\n',data)
//     }
// );
```

