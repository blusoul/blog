# JavaScript 中的深拷贝和浅拷贝

## JavaScript 对象的存储

JavaScript 中对象的创建保存在堆中，栈中的变量只保存堆中对象的地址。

```javascript
var a = {
    name: 'John',
    age: 14
};
var b = a;
b.name = 'Tom';
console.log(a.name); // Tom
```

## JavaScript 中的数组和对象的拷贝都是浅拷贝。比如和 

### 数组的 Array.prototype.slice 方法

```javascript
var originArr1 = [1,2,3];
var originArr2 = [[1,2],[2,3],[3,4]];
var targetArr1 = originArr1.slice(0);
var targetArr2 = originArr2.slice(0);

targetArr1[0] = 'a';
targetArr2[0][0] = 'a';

console.log(originArr1);  // [1, 2, 3]
console.log(targetArr1);  // ["a", 2, 3]
console.log(originArr2);  // [["a",2],[2,3],[3,4]]
console.log(targetArr2);  // [["a",2],[2,3],[3,4]]
```

### Object.assign 静态方法

```javascript

```

## 对象深拷贝的处理

### 安全的 JSON 数据可以通过 JSON 的方法转换，JSON.stringify 方法会过滤 undefined, function, symbol 值

```javascript
var originObj, targetObj;
originObj = {
    name: 'John',
    age: 18,
    favoriteColor: ['black','blur','red']
};

targetObj = JSON.parse(JSON.stringify(originObj));
targetObj.favoriteColor.push('grey');

console.log(originObj, targetObj);
```

### jQuery 中的 extend 方法摘录

```javascript
var objectCopy = (function () {
    var class2type = {};
    var hasOwn = class2type.hasOwnProperty;
    var toString = class2type.toString;
    var fnToString = hasOwn.toString;
    var ObjectFunctionString = fnToString.call(Object);

    function objectCopy() {
        var options, name, src, copy, copyIsArray, clone,
            target = arguments[0] || {},
            i = 1,
            length = arguments.length,
            deep = false;

        // Handle a deep copy situation
        if (typeof target === "boolean") {
            deep = target;

            // Skip the boolean and the target
            target = arguments[i] || {};
            i++;
        }

        // Handle case when target is a string or something (possible in deep copy)
        if (typeof target !== "object" && !isFunction(target)) {
            target = {};
        }

        // Extend jQuery itself if only one argument is passed
        if (i === length) {
            target = this;
            i--;
        }

        for (; i < length; i++) {

            // Only deal with non-null/undefined values
            if ((options = arguments[i]) != null) {

                // Extend the base object
                for (name in options) {
                    src = target[name];
                    copy = options[name];

                    // Prevent never-ending loop
                    if (target === copy) {
                        continue;
                    }

                    // Recursive if we're merging plain objects or arrays
                    if (deep && copy && (isPlainObject(copy) ||
                            (copyIsArray = Array.isArray(copy)))) {

                        if (copyIsArray) {
                            copyIsArray = false;
                            clone = src && Array.isArray(src) ? src : [];

                        } else {
                            clone = src && isPlainObject(src) ? src : {};
                        }

                        // Never move original objects, clone them
                        target[name] = objectCopy(deep, clone, copy);

                        // Don't bring in undefined values
                    } else if (copy !== undefined) {
                        target[name] = copy;
                    }
                }
            }
        }

        // Return the modified object
        return target;
    }

    function isFunction(obj) {
        return toString.call(obj) == '[object Function]';
    }

    function isPlainObject(obj) {
        var proto, Ctor;

        // Detect obvious negatives
        // Use toString instead of jQuery.type to catch host objects
        if (!obj || toString.call(obj) !== "[object Object]") {
            return false;
        }

        proto = Object.getPrototypeOf(obj);

        // Objects with no prototype (e.g., `Object.create( null )`) are plain
        if (!proto) {
            return true;
        }

        // Objects with prototype are plain iff they were constructed by a global Object function
        Ctor = hasOwn.call(proto, "constructor") && proto.constructor;
        return typeof Ctor === "function" && fnToString.call(Ctor) === ObjectFunctionString;
    }

    return objectCopy;
})();
```

### immutable 不可变数据，比如 Facebook 的 immutable.js 库
