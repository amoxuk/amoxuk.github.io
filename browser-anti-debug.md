# 浏览器反调试代码

使用定时器和debugger以及时间对比实现的反调试功能

```js

setTimeout(function() {
    var s = new Date().valueOf();
    try {
        (function z() {
            debugger ;z();
        }
        )();
    } catch (e) {}
    if ((new Date().valueOf() < s) || (new Date().valueOf() - s) > 1094) {
        var H2eeee = 2;
        while (H2eeee !== 1) {
            switch (H2eeee) {
            case 2:
                H2eeee = 1;
                break;
            }
        }
    }
}, 0);

```

---

> [跳转到目录](menu.md)
