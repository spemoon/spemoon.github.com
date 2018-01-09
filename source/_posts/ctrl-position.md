---
title:  获取光标的位置
date: 2011-11-10
category: javascript
tags:
 - javascript
categories: 
 - javascript
 - position
---

前段时间写一个[MOMO](http://momo.im)的表情组件，遇到一个问题，即确定当前光标的位置，主要用于textarea跟input中，下面将相关代码罗列如下。

``` javascript
(function(){

    var textareaHelper = {
        /**
         * 获取光标的位置
         */
        getPostion : function(node, iePos){
            if(node.setSelectionRange) {
                return node.selectionStart;
            } else if (document.selection) {
                if( iePos === true ){
                    i = node.pos || 0;
                } else {
                    var i, sc = document.selection.createRange(), bc = document.body.createTextRange();
                    bc.moveToElementText(node);
                    for(i = 0; bc.compareEndPoints('StartToStart', sc) < 0 && sc.moveStart('character', -1) !== 0; i++){
                        if(node.value.charAt(i) == '\n'){
                            i++;
                        }
                    }
                }
                return i;
            }
        },
        /**
         * 设置光标的位置
         */
        setPosition : function(textarea, p){
            p = p == 'end' ? textarea.value.length : p;
            if(textarea.setSelectionRange){
                textarea.focus();
                textarea.setSelectionRange(p, p);
            } else if (textarea.createTextRange) {
                var range = textarea.createTextRange();
                range.moveEnd('character', -textarea.value.length);
                range.moveEnd('character', p);
                range.moveEnd('character', p);
                range.select();
            }
        }
    }    

})();
```
