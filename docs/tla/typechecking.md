---
title: TypeChecking 
layout: default
parent: Tla+
grand_parent: Model Based Testing
---
# Typechecking

As a model grows it becomes difficult to ensure that the TLA+ code in the models is doing what you think it is. There are techniques to help ensure there are no bugs in your model. The best way to make sure your model is high quality is to use types and the Apalache type checker.

Apalache comes with a type checker. The [docs](https://apalache.informal.systems/docs/HOWTOs/howto-write-type-annotations.html) contain all the details. In this tutorial we will type the model of Alice and Bob's interactions in hello_world.tla.

## Typechecking

Our model of Alice and Bob's interaction is simple: the state variables are simple data structures.

We should type the variables with a particular data structure.

```tla
VARIABLES
    \* @type: Set(Str);
    sent_by_alice,
    \* @type: Set(Str);
    network,
    \* @type: Str;
    bobs_mood,
    \* @type: Seq(Str);
    bobs_inbox
```

The Apalache type system works by annotating lines of code with special TLA+ comments

```
\* @type: ...
```

We have specified that 

1. _sent_by_alice_ is a set of strings
2. _network_ is a set of strings
3. _bobs_moods_ is a string
4. _bobs_inbox_ is a sequence of strings

We should also specify the type of operators. For example we can annotate AliceSend(m)

```tla
\* @type: (Str) => Bool;
AliceSend(m) == 
    /\ m \notin sent_by_alice
    /\ sent_by_alice' = sent_by_alice \union {m}
    /\ network' = network \union {m}
    /\ UNCHANGED <<bobs_mood, bobs_inbox>
```

The annotation says that AliceSend is an operator taking strings and returning booleans.

Finally we can typecheck the model

```bash
java -jar apalache-pkg-0.17.5-full.jar typecheck hello_world_typed.tla

# Apalache output:
# ...
# Type checker [OK]
```
