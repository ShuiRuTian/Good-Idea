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
It is not correct. It just passes all cases!
Q1. messy
it is not clear how to handle the undefined type, so this PR handle it everywhere. Hard to believe someone else could understand why code is written here and like this, because I could not either.
Here are some details: it depends the origional mechemism to narrow away undefined type. But when decide whether to narrow, it use new code.

## How to improve?
we could only check once for the whole expression rather than each OptionalChain symbol expression. For example, the expression is `a.b?.c?.d`, now use a.b, a.b.c, a.b.c.d to narrow a type. But we could only use the whole expression.
I think should give more info to path and return value, such as whether this is root-undefined, whether it contains undefined in the path.
