# immutable-variable
```js
function freezeDeep(obj) {
    if (Array.isArray(obj)) {
        return obj.reduce(function(result, _, i) {
            return Object.defineProperty(result, i, {
                writable: false,
                value: typeof obj[i] === 'object' ? freezeDeep(obj[i]) : obj[i]
            })
        }, []);

    } else if (obj && typeof obj === 'object') {
        return Object.keys(obj).reduce(function(result, name) {
            return Object.defineProperty(result, name, {
                writable: false,
                value: typeof obj[name] === 'object' ? freezeDeep(obj[name]) : obj[name]
            })
        }, {});

    } else{
        return obj;
    }
}
```
## use case
```js
const immutableArray = freezeDeep([
    {a: {aa: [1,2,3], bb: {ccc: 2}}},
    {a: {
        aa: null, 
        bb: undefined, 
        cc: [], 
        dd: [
            {aaa: 1, bbb: {}, ccc: 0}
        ]
    }}
]);

const immutableObject = freezeDeep({
    a: {aa: [1,2,3], bb: {ccc: 2}},
    aa: {
        aa: null,
        bb: undefined,
        cc: [],
        dd: [
            {aaa: 1, bbb: {}, ccc: 0}
        ]
    }
});
```

## advantage over Object.freeze
```js
let immutableObject1 = Object.freeze({a: {aa: 1}});
immutableObject1.a = null;
console.log(immutableObject1.a); // {aa: 1}   [+] good
immutableObject1.a.aa = null;
console.log(immutableObject1.a.aa); // null   [x] bad

const immutableObject2 = freezeDeep({a: {aa: 1}});
immutableObject2.a = null;
console.log(immutableObject2.a); // {aa: 1}   [+] good
immutableObject2.a.aa = null;
console.log(immutableObject2.a.aa); // 1   [+] good too
```
