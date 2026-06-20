# basic terminology of language models

# 1. wdym by token?

a token is a piece of text that the model processes.

humans read words.

llms read tokens.

## why do we even need them?

computers can't directly work with text.

so text first gets converted into numbers.

```text
text
 ↓
tokenizer
 ↓
tokens
 ↓
token ids
 ↓
neural network
```

example:

```text
"cat sat"
```

might become:

```text
[4912, 18874]
```

the model has no idea what "cat" means.

all it sees is:

```text
4912, 18874
```

## common confusion

token != word

sometimes:

```text
1 word = 1 token
```

sometimes:

```text
1 word = multiple tokens
```

example:

```text
"iphone"
```

might be 1 token.

```text
"extraordinary"
```

might become multiple tokens.

## tl;dr

```text
token = chunk of text

llms don't read words

llms read token ids
```

---

# 2. wdym by parameter?

parameters are where the model stores what it learned during training.

think of them as the model's learned knowledge.

suppose i teach you:

```text
2 + 2 = 4
```

after seeing it enough times, you remember the pattern.

llms learn similarly, except instead of a few rules, they learn billions of patterns.

those patterns get stored inside parameters.

eventually the model learns things like:

* grammar
* facts
* coding styles
* language structure
* some reasoning patterns

## tokens vs parameters

people mix these up all the time.

### tokens

tokens are the input/output.

example:

```text
"hello"

"swift"

"iphone"
```

these get converted into tokens and fed into the model.

### parameters

parameters are the learned numbers inside the model.

something like:

```text
0.1834

-0.9273

1.4482

...
```

users never see these.

## mental model

```text
book = tokens

brain = parameters
```

when you read:

```text
the sky is blue
```

the sentence is the input.

your brain's knowledge is what lets you understand it.

same idea here:

```text
tokens = input

parameters = learned knowledge
```

## why is training so expensive?

training means updating parameters.

for every batch of tokens:

```text
tokens
  ↓
prediction
  ↓
error
  ↓
adjust parameters
```

this happens billions or trillions of times.

example:

```text
70b parameter model

trained on

10 trillion tokens
```

that's an insane amount of computation.

that's why training modern llms costs millions of dollars.

## tl;dr

```text
tokens = text

parameters = learned knowledge

training = updating parameters
```

---

# 3. wdym by context window?

context window = how much text the model can currently see.

think of it as the model's working memory.

## example

suppose a model has a context window of 8 tokens.

you give:

```text
i am learning about large language models today
```

pretend it becomes:

```text
[i] [am] [learning] [about] [large] [language] [models] [today]
```

that's 8 tokens.

the model can see all of them.

now you add:

```text
and transformers
```

the sequence becomes:

```text
[i] [am] [learning] [about] [large] [language] [models] [today] [and] [transformers]
```

10 tokens.

if the context window is only 8, some older tokens may get dropped:

```text
[learning] [about] [large] [language] [models] [today] [and] [transformers]
```

the model can no longer see:

```text
i am
```

## another example

you say:

```text
my dog's name is bruno
```

200 messages later:

```text
what's my dog's name?
```

if the original message is still inside the context window:

```text
bruno
```

easy.

if it's no longer visible:

the model can't use it.

## context window vs parameters

another thing people confuse.

### parameters

the model's learned knowledge.

example:

```text
70b parameters
```

### context window

the text currently visible.

example:

```text
128k context window
```

## mental model

```text
parameters = your brain

context window = what's currently on your desk
```

you may know:

* pythin
* swift
* system design

that's knowledge already stored in your brain.

but if i give you a 500-page document and only allow 10 pages on your desk at once, your working space is limited.

that's basically a context window.

## why do larger context windows matter?

small context:

```text
2k tokens
```

good for:

* short chats
* small code snippets

medium context:

```text
32k tokens
```

good for:

* large documents
* multiple files
* bigger conversations

large context:

```text
128k+ tokens
```

good for:

* entire codebases
* long technical discussions
* large pdfs

the bigger the context window, the more information the model can reason about at the same time.

## common confusion

large model != large context window

these are completely different things.

example:

```text
small model + huge context
```

is possible.

and:

```text
huge model + small context
```

is also possible.

## why does attention become expensive?

the transformer uses something called attention.

don't worry about the details yet.

just know this:

every token checks how relevant other tokens are.

suppose we have:

```text
i love swift programming
```

label them:

```text
t1 = i

t2 = love

t3 = swift

t4 = programming
```

when processing:

```text
programming
```

the model asks:

```text
how relevant is "i"?

how relevant is "love"?

how relevant is "swift"?

how relevant is "programming"?
```

and every other token does the same thing.

so:

```text
t1 looks at 4 tokens

t2 looks at 4 tokens

t3 looks at 4 tokens

t4 looks at 4 tokens
```

total:

```text
4 × 4 = 16
```

## why is it called n²?

let:

```text
n = number of tokens
```

each token compares itself against:

```text
n tokens
```

and there are:

```text
n tokens
```

doing this.

so:

```text
n × n = n²
```

this is called quadratic scaling.

## mental model

imagine a meeting.

with:

```text
10 people
```

everyone talks to everyone.

```text
10 × 10 = 100 interactions
```

now:

```text
100 people
```

```text
100 × 100 = 10,000 interactions
```

now:

```text
10,000 people
```

```text
10,000 × 10,000 = 100,000,000 interactions
```

things get out of hand very quickly.

that's exactly the problem with attention.

## why should i care?

because context size is one of the biggest bottlenecks in llms.

going from:

```text
1k context
```

to:

```text
10k context
```

isn't 10x more work.

it's closer to:

```text
(10,000²) / (1,000²)

= 100x
```

more attention work.

that's one of the biggest challenges in building llms that can understand entire codebases and huge documents.

## tl;dr

```text
context window = text currently visible

parameters = learned knowledge

bigger context = more memory

more context = more expensive attention
```
