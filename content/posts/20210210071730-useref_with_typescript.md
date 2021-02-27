+++
title = "useRef with Typescript"
author = ["Matias Hernandez"]
draft = false
+++

-   tags:: [Typescript]({{< relref "20200926235811-typescript" >}})


## With crateRef {#with-crateref}

```typescript
const myRef = createRef<HTMLDivElement>()
```


## forwardref {#forwardref}

```typescript
type Ref = HTMLButtonElement
const F = forwardRef<Ref, Props>

or

type Ref = React.Ref<HTMLDivElement>
```
