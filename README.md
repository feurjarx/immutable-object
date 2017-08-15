# immutable-variable
```js
function fabric(obj) {
    if (Array.isArray(obj)) {\
        return obj.reduce(function(result, _, i) {
            debugger;
            return Object.defineProperty(result, i, {
                writable: false,
                value: typeof obj[i] === 'object' ? fabric(obj[i]) : obj[i]
            })
        }, []);
        
    } else {
    
        return Object.keys(obj).reduce(function(result, name) {
            return Object.defineProperty(result, name, {
                writable: false,
                value: typeof obj[name] === 'object' ? fabric(obj[name]) : obj[name]
            })
        }, {});
    }
}
```
