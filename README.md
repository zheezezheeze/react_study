# react_study

```javascript
let a = 10;
console.log(a);
```

## lodash 메서드

```javascript
function isEmpty(value) {
if (value == null) {
  return true;
}
if (isArrayLike(value) &&
    (isArray(value) || typeof value == 'string' || typeof value.splice == 'function' ||
      isBuffer(value) || isTypedArray(value) || isArguments(value))) {
  return !value.length;
}
var tag = getTag(value);
if (tag == mapTag || tag == setTag) {
  return !value.size;
}
if (isPrototype(value)) {
  return !baseKeys(value).length;
}
for (var key in value) {
  if (hasOwnProperty.call(value, key)) {
    return false;
  }
}
return true;
}
```
