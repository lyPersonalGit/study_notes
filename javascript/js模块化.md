``` javascript
//立即执行函数
var module1 = (function(){

    var _count = 0;

    var m1 = function(){
    //...
    };

    var m2 = function(){
    //...
    };

    return {
    m1 : m1,
    m2 : m2
    };

})();
//放大模式 如果一个模块很大，必须分成几个部分，就需要采用“放大模式”
var module1 = (function (mod){

    mod.m3 = function () {
    //...
    };

    return mod;
    
})(window.module1 || {});

```

