---
layout: post
title: 使用ESLint检测node.js代码格式（SublimeText插件）
---
链接：[ESLint中文网](http://eslint.cn)

1、使用npm安装eslint:

`npm install -g eslint`

2、使用`eslint --init`初始文件，或者手动插件`.eslintrc.js`文件

因为我使用的Mac，使用希望全局都能使用，所以将`.eslintrc.js`文件放在了根目录下`~` [查看完整规则](http://eslint.cn/docs/rules/)

```
module.exports = {
    "env": {
        "node": true,
        "browser": true,
        "commonjs": true,
        "es6": true
    },
    "extends": "eslint:recommended",
    "parserOptions": {
        "sourceType": "module"
    },
    "rules": {
        "indent": [ //强制使用一致的缩进
            "error",
            "tab"
        ],
        "linebreak-style": [ //强制使用一致的换行风格
            "error",
            "unix"
        ],
        "quotes": "off", //强制使用一致的反勾号、双引号或单引号
        "semi": [ //要求或禁止使用分号而不是 ASI
            "error",
            "always"
        ],
        "comma-dangle": ["error", "only-multiline"], //要求或禁止末尾逗号
        "no-cond-assign": ["error", "always"], //禁止条件表达式中出现赋值操作符
        "no-console": "off", //禁用 console
        "eqeqeq": ["error", "allow-null"], //要求使用 === 和 !==
        "no-eval": "error", //禁用 eval()
        "no-const-assign": "error", //禁止修改 const 声明的变量
        "no-class-assign": "error", //禁止修改类声明的变量
        'no-dupe-class-members': "error", //禁止类成员中出现重复的名称
        "handle-callback-err": "error", //要求回调函数中有容错处理
        "no-extra-semi": "error", //禁止不必要的分号
        "no-extra-parens": "error", //禁止不必要的括号
        "no-unused-vars": "error", //禁止出现未使用过的变量
        "no-redeclare": "error", //禁止多次声明同一变量
        "no-loop-func": "error", //禁止在循环中出现 function 声明和表达式
        "max-depth": ["warn", 5], //强制可嵌套的块的最大深度
        "max-nested-callbacks": ["error", 4], //强制回调函数最大嵌套深度
        "max-len": ["warn", 200], //强制一行的最大长度
        "comma-style": ["error", "last"], //强制使用一致的逗号风格
        "no-multi-spaces": "error", //禁止使用多个空格
        "no-self-assign": "error", //禁止自我赋值
        "no-self-compare": "error", //禁止自身比较
        "no-unmodified-loop-condition": "error", //禁用一成不变的循环条件 (死循环)
        "no-useless-concat": "error", //禁止不必要的字符串字面量或模板字面量的连接
        "no-sync": "error", //禁用同步方法
        "space-infix-ops": ["error", {
            "int32Hint": false
        }], // 要求中缀操作符周围有空格 
        "space-unary-ops": "error",
        "no-dupe-keys": "error",//禁止在对象字面量中出现重复的键
    }
};
```

3、在`package.json`文件中添加：
```
{
    "eslintConfig": {
        "env": {
            "browser": true,
            "node": true
        }
    }
}
```  

4、使用[`Package Control`](https://sublime.wbond.net/installation)安装Sublime插件`SublimeLinter`和`SublimeLinter-contrib-eslint(只支持Sublime text 3)`

5、安装好之后就可以在`Tools->SublimeLinter`看到菜单
