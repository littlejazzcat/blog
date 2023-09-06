## Javascript的一些疑问解答记录


    // 判断输入是否为数字
    function isNumeric(n) {
        return !isNaN(parseFloat(n)) && isFinite(n);
    }
这段代码是用来检查变量`n`是否是一个有限的数字。它首先尝试将`n`转换为浮点数（通过`parseFloat(n)`），然后检查转换后的值是否是NaN（not a number:不是一个数字）。如果`n`不是数字，`parseFloat(n)`将返回NaN，因此`isNaN(parseFloat(n))`将返回true。如果`n`是数字，`parseFloat(n)`将返回一个数字，所以`isNaN(parseFloat(n))`将返回false。

然后，它检查这个转换后的数字是否是有限的（通过`isFinite(n)`）。如果这个数字是有限的，那么这个函数将返回true，否则返回false。

所以，这个表达式的结果是true，如果`n`是一个有限的数字，否则是false。

- myArray.constructor.toString().indexOf("Array") > -1;这里>-1的作用是什么

这段代码是在检查一个变量 `myArray` 是否是一个数组。它使用了以下步骤：

1. `myArray.constructor`：这会得到 `myArray` 的构造函数。在大多数情况下，如果 `myArray` 是一个数组，那么它的构造函数就是 `Array`。
2. `myArray.constructor.toString()`：这会将构造函数的源代码转化为字符串。在浏览器或 Node.js 的环境中，`Array.toString()` 通常会返回如下的字符串：`function Array() { [native code] }`。
3. `myArray.constructor.toString().indexOf("Array") > -1`：这会检查构造函数的源代码字符串中是否包含 "Array" 这个子串。`indexOf()` 方法会返回子串在字符串中第一次出现的位置（从0开始计数），如果没有找到，就返回 -1。因此，`myArray.constructor.toString().indexOf("Array") > -1` 的意思是，检查 "Array" 是否在构造函数源代码的开始部分。

所以，这段代码的目的是检查 `myArray` 是否是一个数组，如果是，那么 `myArray.constructor.toString()` 的结果应该以 "Array" 开头。如果这个表达式的结果为 `true`，那么 `myArray` 就是一个数组。如果为 `false`，那么 `myArray` 就不是数组。

这个方法虽然可以用来检测一个对象是否是数组，但在现代 JavaScript 中，更推荐使用 `Array.isArray()` 函数，例如 `Array.isArray(myArray)`，因为这样更直接、更清晰。