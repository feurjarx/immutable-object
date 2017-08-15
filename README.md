# immutable-object
```js
function fabric(obj) {
    obj = Object.assign({}, obj);

    return Object.keys(obj).reduce(function(result, name) {
        debugger;
        return Object.defineProperty(result, name, {
            writable: false,
            value: typeof obj[name] === 'object' ? fabric(obj[name]) : obj[name]
        })
    }, {});
}
```
