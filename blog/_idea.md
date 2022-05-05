js 中的链式编程

匿名函数

O = function() {
    this.value = 1;
}

O.prototype.if = function(func) {
    if (func.call(this, this)) {
        return this;
    }
    else {
        return null;
    }
}

new O().if(o => this.value == 1) //这里 this 指向 window 对象
new O().if(o => o.value == 1) //正确
new O().if(function(o) {
    return this.value == 1;
}); //正确