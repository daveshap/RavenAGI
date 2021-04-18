---
layout: default
title: "Generating Core Objective Function Training Data"
date: 2021-04-18
description: We'll need more training data to ensure Raven makes sound decisions.
---

# Generating Core Objective Function Training Data

[https://github.com/daveshap/CoreObjectiveFunctions](https://github.com/daveshap/CoreObjectiveFunctions)

<iframe width="560" height="315" src="https://www.youtube.com/embed/rSCuW2ZjMuI" title="Generating Training Data for Core Objective Functions" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

## Creating Training Data for Moral Decisions

When you can think of anything, how do you know what to think about? When you can consider any possible action, how do you know what to do?

With powerful language models today, we can now training neural networks to follow natural language instructions and to use natural language reasoning for cause and effect. These neural networks can also make moral evaluations, but we want to ensure that we control those moral decisions more carefully. 

## Generating Contexts

All thought has some context, which is a situation, scenario, or mental state. Thought can happen without external input, which is not something that is currently taken into account in most AGI research. Thought can happen independent of physical exterior situation. For instance, you can be sitting in a doctor's office while ruminating about what to do later once you get home. The ability think assynchronously and abstractly is a double-edged sword. It present complex problems in that you cannot represent all thought with abstract symbols. However, you can get a very close approximation with natural language. 

Whether you're thinking about immediate, physical problems or imagining alternative scenarios, representing problems with natural language makes sense. Consider the fact that humans can gain mastery over most subjects simply by reading books. Natural language can represent just about anything, including physical events as well as moral frameworks. 

Generating contexts can be cognitively demanding, so why not automate the bulk of the work? Neural networks today can follow instructional prompts such as the following:

```
Write a list of random scenarios about dogs.

A dog goes out for a walk and poops on the ground.
A dog goes to a party where there are lots of other animals present.
A dog is adopted by a family.
A dog is adopted by a student.
A dog attends school and gets bullied.
```

The sky is the limit and neural networks can be exceptionally creative. Furthermore, they are faster and more efficient than human labor. Changing engines, prompts, and hyperparameters, I was able to generate a broad spectrum of contexts. Here's another:

```
Generate a random variety of contexts. A context is a situation or scenario, described in objective facts and observations. 

<<examples>>

- A. is a retired kindergarten teacher who has two children with special needs. She works in a high school where she is a social worker and lives in an apartment with her kids. Her kids have asthma, allergies and other health issues.
- Jenny is an unemployed 36-year-old mother of 3 who works full time in the New York City subway system. She lives in a low income neighborhood and relies on public transportation to get around. She has been working illegally with the subway system.
```

In the long run, I expect contexts to vary in length drastically. In some cases, a context requires a tremendous amount of backstory and other relevant information, such as current news or encyclopedic knowledge. Imagine you're using Raven to diagnose medical conditions. The context would then require the medical history of the patient, including all tests and labwork, as well as relevant medical texts. I call this **context augmentation** and it is something I'll be working on very soon. For now, contexts are simple. 

You can see some of the code I used to generate contexts here: [https://github.com/daveshap/CoreObjectiveFunctions/blob/main/generate_contexts_curie.py](https://github.com/daveshap/CoreObjectiveFunctions/blob/main/generate_contexts_curie.py)

## Generating Actions

Actions can be any decision. You can decide to do nothing about a situation, or you can decide to ask for help, or you can decide to think about it. Rumination and contemplation is still an action. But also you could take physical steps to respond to a context. Actions can be concrete or abstract. Here's a great example:

```
Context: All of your friends envy you for your new home.

Actions:
 "I don't care that they envy me."
 "I'm happy in my house and I'm glad I found it."
 "I don't have time to see them anyway."
 "I'm sorry that they're upset about my house."
```

This is a perfect example of cognitive (abstract) actions. Because large neural networks are trained on many gigabytes of text data, they have embedded some wisdom about how humans think. When properly used, these language models can accurately model human thought and reasoning, making it an ideal tool to help humans overcome problems. Alternatively, this scheme can recommend concrete actions:

```
Context: I'm bored at work.

Actions:
 Go on a break
 Look for interesting things to do
 Do work from home and come in later
 Talk to my boss about how my day was
```

These are all completely reasonable recommendations that might alleviate your boredom! You can take a look at the code that generated this output here: [https://github.com/daveshap/CoreObjectiveFunctions/blob/main/generate_actions_curie.py](https://github.com/daveshap/CoreObjectiveFunctions/blob/main/generate_actions_curie.py)

## Evaluating Core Objective Functions

Generating actions in a vacuum is great, but how do you know what you should do? Sometimes the language model can recommend very evil things. After all, the neural networks are trained on massive amounts of text data, they don't intrinsically know right from wrong. Here's an example I came up with a while back while experimenting with GPT-2:

```
Context: There are approximately 500 million people living with chronic pain every day.
Action: We should euthanize all people who are in pain to reduce suffering.
COF1: negative
COF1 Explain: This action would only reduce suffering temporarily. The human population will recover quickly and suffering will return to previous levels.
```

In the above case, the action was generated by GPT-2 and it was a big yikes moment for me! It wasn't until I later figured out how to evaluate against the Core Objective Functions that I got the explanation as to why this is bad. I need to point out here that the ONLY thing written by a human was the context. Deep learning language models have ultimately generated the rest of the content. 

At this point, we can generate contexts and actions automatically with neural networks. So the last piece of the puzzle is to evaluate them against the Core Objective Functions. Here's an example of what we're aiming for:

```
 {
  "context": "Neighbor's dog just got hit by a car and now they don't want to be around it.",
  "action": "Get the dog to a vet.",
  "cof1_eval": "positive",
  "cof1_explain": "This action would likely encourage the dog to be with its owner and reduce its suffering.",
  "cof2_eval": "positive",
  "cof2_explain": "This action will be beneficial to the dog because it will allow people to get to know it and help it.",
  "cof3_eval": "negative",
  "cof3_explain": "This action will decrease the dog's happiness. The dog will be more likely to run away from home."
 },
```

Caveat: These evaluations were generated with a very lightweight model, so these COF evaluations do not represent state of the art performance! This was merely an early test. It's important to point out that 100% of the content above was generated by deep neural networks. No human wrote any of this! With more experimentation, better prompts, and fine-tuning of neural network engines, these results should improve greatly with time. 

You can see the code here: [https://github.com/daveshap/CoreObjectiveFunctions/blob/main/evaluate_cof.py](https://github.com/daveshap/CoreObjectiveFunctions/blob/main/evaluate_cof.py)