
# lodash 메서드

## _.isEmpty(value) : value가 Object 또는 복수데이터인지 확인

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

## _.get : object를 리턴받는 메서드
```javascript
function get(object, path, defaultValue) {
  var result = object == null ? undefined : baseGet(object, path);
  return result === undefined ? defaultValue : result;
}
```

## _.concat : array 래핑 메서드
```javascript
function concat() {
  var length = arguments.length;
  if (!length) {
    return [];
  }
  var args = Array(length - 1),
  array = arguments[0],
  index = length;

  while (index--) {
    args[index - 1] = arguments[index];
  }
  return arrayPush(isArray(array) ? copyArray(array) : [array], baseFlatten(args, 1));
}
```

