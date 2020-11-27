## Question Description

Now this works:
``` ts
type T1 = {
    key: "key1"
    data: string
}

type T2 = {
    key: "key2"
    data: string
}

type T = T1|T2;
function f(t:T){
  if(t.key === "key1"){
    t // T1
  }else{
    t // T2
  }
}

```
But, this not:
``` ts
type T1 = {
    key: {k:"key1"}
    data: string
}

type T2 = {
    key: {k:"key2"}
    data: string
}

type T = T1|T2;

function f(t:T){
  if(t.key.k === "key1"){
    t // T, but should be T1
  }else{
    t // T, but should be T2
  }
}
```

## How does My PR work?

### Abstract

Narrow condition is composed by three parts `Expression`, `operator` and `expected value`.

We need these three parts together to decide the result. Here is an example "typeof a.b?.c === 'undefined'"

And to implement this feature, we need to get the final path type. Then use this type, `operator` and `expected value` to narrow origional type.

Looks easy, right?

But... it is not that easy in fact.

### implement

It is not correct. It just passes all cases!

Q1. messy

In the beta version, it is not clear how to handle the undefined type and OptionalChain operator, so this PR handle it everywhere. Hard to believe someone else could understand why code is written here and like this, because I could not either.

Here are some details: it depends the origional mechemism to narrow away undefined type. But when decide whether to narrow, it use new code.


### Some special cases

0. not same path.
``` Typescript
type TypeA = {a:{b:{c:1}}}
type TypeB = {a:{d:{e:1}}}
type T = TypeA | TypeB;
```
why is it special:
- obviously, we could not narrow it. If expression is `root.a.b.c`, we could tell that it is wrong.
    - This produce Rule_1: If some subtype could not get the path, we could just return.

1. Root Union caused by undefined/null, some type + undefined/null
2. Root Union contains at least three type and one is undefined/null, some type1 + some type2 + undefined/null

why 1 and 2 are special: 
- This break Rule_1! undefined and null could not contains any type, but it might be resonable -- through OptionalChain operator `?.`! Example: `root?.a.b.c`
- So we need to improve Rule_1. If the next path is accessed by OptionalChain operator, we need to remove all undefined/null subtype.
- But, wait, could we just return the types? How could we know which is produced by which?

- one important question is "Should I narrow it by this way or pass it to another function?"
getSubDeepPropertyTypeAccordingToExpression(expression, type(reference))

## How to improve?

we could only check once for the whole expression rather than each OptionalChain symbol expression. For example, the expression is `a.b?.c?.d`, now use a.b, a.b.c, a.b.c.d to narrow a type. But we could only use the whole expression.

I think should give more info to path and return value, such as whether this is root-undefined, whether it contains undefined in the path.

when getting the type through path, if undefined with OptionalChain, mark a flag, if undefined without OptionalChain, return undefined. When return the final path, if flag is true, try add undefined type.


GetPathInfoAccordingToExpressionAndReference

GetTypeAccordingToPath
return: {
    rootTypeId: "11"
    type: SomeType | undefined // UndefinedType is not undefined.
}

const isExpressionCallable = fasle;
const result = new Map();
path = ["p1?","p2"]; // Or {{path:"p1", optionalChain:true},{path:"p2", optionalChain:false}}
for rootsubType in rootSubTypes
{
    const id = rootsubType.id;
    if(rootsubType == UndefinedType && path.Count != 0){
        result[id] = UndefinedType;
        continue;
    }
    var t = getTypeAccordingToPath(rootsubType,path,isExpressionCallable);
    if(t == undefined){
        result = undefined;
        break;
    }
    result[id] = t;
}

