CtoB
====

CtoB is a compiler which CoffeeScript to Babel friendly JavaScript.

# Syntax

## Object and Arrays

```coffee
a = [1, 2, 3]

obj = {a: 1, b: 2}

b = [
  1
  2
  3
]

obj2 =
  hoge:
    fuga: 'piyo'
  some: 'thing'

c = [
  a: 1
  b: 2
,
  a: 3
  b: 4
]
```

compiles

```js
var a, b, c, obj, obj2;

a = [1, 2, 3];

obj = {
  a: 1,
  b: 2
};

b = [1, 2, 3];

obj2 = {
  hoge: {
    fuga: 'piyo'
  },
  some: 'thing'
};

c = [
  {
    a: 1,
    b: 2
  }, {
    a: 3,
    b: 4
  }
];
```

## Variables

```coffee
var a = 1
let b = 2
const c = 3
```

compiles

```js
var a = 1;
let b = 2;
const c = 3;
```

## If-Else

```coffee
if x
  hoge()
else if y
  fuga()
else
  piyo()
```

compiles

```js
if (x) {
  hoge();
} else if (y) {
  fuga();
} else {
  piyo();
}
```

`if-else` return values

```coffee
z = if x
  1
else if y
  2
else
  3
```

compiles

```js
var z;

z = x ? 1 : y ? 2 : 3;
```

also

```coffee
z = if x then 1 else if y then 2 else 3
```

is same result

## Loops

*TODO*

## Operators and Aliases

| CoffeeScript | JavaScript |
| ------------ | ---------- |
| is           | ===        |
| isnt         | !==        |
| not          | !          |
| and          | &&         |
| or           | ||         |
| true, yes, on | true      |
| false, no, off | false    |
| @, this      | this       |

## The Existential Operator

```coffee
y = if x? then x else null
```

compiles

```js
var y;

y = typeof x !== "undefined" && x !== null ? x : null;
```

## Functions

```coffee
->
```

compiles

```js
(function() {});
```

### Default

```coffee
f = (x, y=5)-> x + y
```

#### CoffeeScript

```js
var f;

f = function(x, y) {
  if (y == null) {
    y = 5;
  }
  return x + y;
};
```

#### CtoB

```js
var f;
f = function(x, y=5) {
  return x + y;
};
```

### Rest

```coffee
f = (x, y...)-> x + y.length
```

#### CoffeeScript

```js
var f,
  slice = [].slice;

f = function() {
  var x, y;
  x = arguments[0], y = 2 <= arguments.length ? slice.call(arguments, 1) : [];
  return x + y.length;
};
```

#### CtoB

```js
var f;
f = function(x, ...y) {
  return x + y.length;
};
```

### Spread

```coffee
f = (x, y, z)-> x + y + z

f ...[
  1
  2
  3
]
```

#### CoffeeScript

```js
// unexpected ...
```

#### CtoB

```js
var f;
f = function(x, y, z) {
  return x + y + z;
}

f(...[1, 2, 3]);
```

## Arrows

```coffee
nums.forEach (v)=> @hoge v
```

### CoffeeScript

```js
nums.forEach((function(_this) {
  return function(v) {
    return _this.hoge(v);
  };
})(this));
```

### CtoB

```js
nums.forEach((v)=> {
  this.hoge(v);
});
```

## Classes

```coffee
class B extends A
  constructor: (x, y)->
    super x, y

  some: (x)->
    super.hoge x
```

### CoffeeScript

```js
var B,
  extend = function(child, parent) { for (var key in parent) { if (hasProp.call(parent, key)) child[key] = parent[key]; } function ctor() { this.constructor = child; } ctor.prototype = parent.prototype; child.prototype = new ctor(); child.__super__ = parent.prototype; return child; },
  hasProp = {}.hasOwnProperty;

B = (function(superClass) {
  extend(B, superClass);

  function B(x, y) {
    B.__super__.constructor.call(this, x, y);
  }

  B.prototype.some = function(x) {
    return B.__super__.some.apply(this, arguments).hoge(x);
  };

  return B;

})(A);
```

### CtoB

```js
class B extends A {
  constructor(x, y) {
    super(x, y);
  }

  some(x) {
    super.hoge(x);
  }
}
```

## Template Strings

```coffee
hoge = 'hoge'
fuga = 'fuga'
"hoge: #{hoge}, fuga: #{fuga}"
```

### CoffeeScript

```js
var fuga, hoge;

hoge = 'hoge';

fuga = 'fuga';

"hoge: " + hoge + ", fuga: " + fuga;
```

### CtoB

```js
var fuga, hoge;
hoge = 'hoge';
fuga = 'fuga';
`hoge: ${hoge}, fuga: ${fuga}`
```

## Destructuring

*TODO*
