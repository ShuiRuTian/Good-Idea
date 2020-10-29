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

## How to improve?
we could only check once for the whole expression rather than each OptionalChain symbol expression. For example, the expression is `a.b?.c?.d`, now use a.b, a.b.c, a.b.c.d to narrow a type. But we could only use the whole expression.
