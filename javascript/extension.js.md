``` javascript
//日期格式化
Date.prototype.format = function (format) {
    var o = {
        "M+": this.getMonth() + 1,
        "d+": this.getDate(),
        "h+": this.getHours() % 12 == 0 ? 12 : this.getHours() % 12,
        "H+": this.getHours(),
        "m+": this.getMinutes(),
        "s+": this.getSeconds(),
        "q+": Math.floor((this.getMonth() + 3) / 3),
        "S": this.getMilliseconds()
    };
    var week = {
        "0": "\u65e5",
        "1": "\u4e00",
        "2": "\u4e8c",
        "3": "\u4e09",
        "4": "\u56db",
        "5": "\u4e94",
        "6": "\u516d"
    };
    if (/(y+)/.test(format)) {
        format = format.replace(RegExp.$1, (this.getFullYear() + "").substr(4 - RegExp.$1.length));
    }
    if (/(E+)/.test(format)) {
        format = format.replace(RegExp.$1, ((RegExp.$1.length > 1) ? (RegExp.$1.length > 2 ? "\u661f\u671f" : "\u5468") : "") + week[this.getDay() + ""]);
    }
    for (var k in o) {
        if (new RegExp("(" + k + ")").test(format)) {
            format = format.replace(RegExp.$1, (RegExp.$1.length == 1) ? (o[k]) : (("00" + o[k]).substr(("" + o[k]).length)));
        }
    }
    return format;
}
//日期累加
Date.prototype.addSeconds = function (Seconds){
    var date = this;
    return new Date(date.getTime() + Seconds * 1000);
};
Date.prototype.addMinutes = function (Minutes) {
    var date = this;
    return new Date(date.getTime() + Minutes * 60 * 1000);
};
Date.prototype.addHours = function (Hours) {
    var date = this;
    return new Date(date.getTime() + Hours * 60 * 60 * 1000);
};
Date.prototype.addDays = function (Days) {
    var date = this;
    return new Date(date.getTime() + Days * 24 * 60 * 60 * 1000);
};

//获取地址栏参数对象
location.getParams = function (url) {
    var search;
    if(url) {
        if(typeof url != "string") throw new Error("参数类型错误");
        if(url.indexOf("?") > -1) search = url.split("?")[1];
        else return null;
    } else search = location.search.slice(1);
    if(search) {
        var obj = {};
        var arr = search.split("&");
        for(var i = 0; i < arr.length; i++) {
            var item = arr[i].split("=");
            obj[item[0]] = item[1];
        }
        return obj;
    } else return null;
};

//生成search参数
location.getUrlString = function(url, obj) {
    if(typeof url != "string" || typeof obj != "object") throw new Error("参数类型错误");
    var arr = [];
    for (var key in obj) {
        if (obj.hasOwnProperty(key)) {
            var param = obj[key];
            arr.push(key + "=" + obj[key]);
        }
    }
    if(arr.length > 0) url += ("?" + arr.join("&"));
    return url;
}

//保留有效数字
Math.effective = function (d, n) {
    if (d == 0) return 0;
    if (d > 1 || d < -1)
        n = n - parseInt(Math.log10(Math.abs(d))) - 1;
    else
        n = n + parseInt(Math.log10(1 / Math.abs(d)));
    if (n < 0) {
        d = parseInt(d / Math.pow(10, 0 - n)) * Math.pow(10, 0 - n);
        n = 0;
    }
    return parseFloat(d.toFixed(n));
}

//数组最大值
Array.prototype.max = function() {
    var arr = this.filter(function(item) { return isNaN(item) == false; });
    return arr.length ? Math.max.apply(null, arr) : undefined
}

//数组最小值
Array.prototype.min = function() {
    var arr = this.filter(function(item) { return isNaN(item) == false; });
    return arr.length ? Math.min.apply(null, arr) : undefined;
}

//类数组转数组
Array.fromArrayLike = function(obj) {
    return [].slice.call(obj);
}

//克隆数组
Array.clone = function(arr) {
    return [].concat(arr);
}

//打乱数组
Array.prototype.shuffle = function() {
    var array = this;
    var m = array.length,
        t, i;
    while (m) {
        i = Math.floor(Math.random() * m--);
        t = array[m];
        array[m] = array[i];
        array[i] = t;
    }
    return array;
}

//返回值传递两者之间的随机整数
Number.random = function(v, i) {
    return Math.floor(Math.random() * (i - v + 1) + v);
}

//其他类型转数字
Number.from = function(v) {
    var i = parseFloat(v);
    return isFinite(i) ? i : null;
}

```

