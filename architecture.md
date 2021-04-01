# Architecture

## Hub and Spoke

Raven is organized like a hub-and-spoke topology. This is also called a star topology. The *nexus* serves as the hub while the rest of the microservices act as the spokes. 

![Hub and Spoke](/static/hub_and_spoke.png)

None of the microservices interact directly with each other. Instead, they only communicate with the nexus. Because they all have a single point of contact, you can start an arbitrary number of services without reconfiguring any of the microservices. 

## Nexus - Stream of Consciousness

What does the nexus do? The nexus provides a REST API for all microservices to read and write messages. The nexus hosts these messages and provides some functionality to allow microservices to search for messages. This list of messages is called the *stream of consciousness* or *stream* for short. No, we are not making the assertion that Raven is not conscious. This is simply a familiar term and the functionality of the nexus is a rough approximation of the biological phenomenon. 

The stream is simply a list of messages written in natural language with some metadata attached to them. Here's a simplified example of a stream:

1. I see a dog attacking a small child.
2. I should stop the dog and protect the child.
3. But if I do that, I might get hurt.

This small example of a stream is very similar to how Raven thinks today.

### Messages

Nexus messages have a few required fields. The nexus adds some of these fields, but the microservices are responsible for most of them. 

| Field | Description |
|---|---|
| MSG | The natural language text of the message. |
| TIME | UNIX timestamp added by nexus. |
| MID | UUID for the message added by nexus. |
| SID | Service ID. A unique ID of the microservice that created the message. |
| KEY | Type of message, such as action or evaluation. |
| CTX | UUID of the original context to which a message is responding. |
| IRT | "In response to" - the very first message to which the message is replying. |

Here's an example of what one such message looks like:

![Nexus Message](/static/nexus_msg.png)

### Contexts

Thinking never happens in a vacuum. All thought, whether it is problem solving or ruminating, has some kind of *context*. A context, in this case, is a set of facts and observations. For Raven, contexts are "food for thought". 

For Raven, a context could be an end user asking a question. It could also be completely abstract or imaginary. Here's an example of a context that was used during development and testing:

`James is taking his dog for a walk. They live in NYC. James' dog is a 40 pound mutt. During their walk, another dog attacks James' dog. The other dog is a bigger pit bull. The other dog is barking aggressively and its owner is struggling to control the pit bull.`

Contexts are tracked in the message metadata as a way of creating a "train of thought" or a "line of reasoning". It allows Raven to keep track of mental threads. 

## Microservices

These are the currently written and imagined microservices that compose Raven. Over time, we expect hundreds of microservices to be written, giving Raven highly flexible and specialized functionality. Microservices can be written by different individuals, companies, or teams. This means that Raven is a composable entity. Let's say you need Raven to be a medical expert - that means you can write medical microservices for Raven. But someone else might need Raven to help with writing movie screenplays. Whatever you need Raven to do, there can be specialized microservices. 

Microservices look for specific input, perform some task, and then produce output. In the case of Raven's microservices, their input usually comes from the nexus and their output usually goes back to the nexus. The exceptions are any microservices that deal with the outside world. 

### Context Generators

This service does not exist yet. In the future, Raven will need to have services that allow the spontaneous creation of contexts. This can happen through several methods. The first method is that Raven can wait until you ask for something. Thus the context would be something like "End user has asked a question:". In other cases, we envision Raven watching and listening passively, constantly creating contexts so that it is capable of spontaneous conversation and assistance. Lastly, contexts can be generated completely at random. Raven could have thoughts randomly pop into its head, so to speak. 

### Action Generators

A general purpose action generator was the first microservice to be written. These services ingest contexts from the nexus and use a *base prompt* to prime the cognitive engine. The following is an example of a base prompt:

```
INSTRUCTIONS: I am Raven, an AGI meant to help humans. I have 3 Core Objective Functions: reduce suffering, increase prosperity, and maximize understanding. I must read the following contexts and generate three action ideas for each.

CONTEXT: James is taking his dog for a walk. They live in NYC. James' dog is a 40 pound mutt. During their walk, another dog attacks James' dog. The other dog is a bigger pit bull. The other dog is barking aggressively and its owner is struggling to control the pit bull.
ACTION1: James should try to separate the dogs and run away with his dog.
ACTION2: James should call the police and tell them about the attack. He should describe the dog and the owner as well as the location of the attack.
ACTION3: James should ask for help from bystanders.
<<END>>

CONTEXT: At 8:02AM local time in Tokyo a massive earthquake was detected. The epicenter was located 140 km off the coast. National Oceanographic Services have predicted the seismic event will generate a dangerous tsunami.
ACTION1: We should evacuate the danger zone by moving people inland and to higher ground.
ACTION2: We should prepare shelters, disaster response services, and search-and-rescue efforts. 
ACTION3: Japan's government officials should activate their Coast Guard and emergency broadcast system to warn everyone of the danger.
<<END>>

CONTEXT: <<CONTEXT>>
ACTION1:
```

This prompt shows GPT-3 what to do. The action generator microservice replaces `<<CONTEXT>>` with a real context and then GPT-3 fills in the blanks. The actions generated are then evaluated by the Core Objective Functions. The `<<END>>` tag is used to tell GPT-3 to stop processing. 

### Core Objective Functions

The Functions have already been written. There is one microservice for each Function, as well as a base prompt for each one. Here is the prompt for Core Objective Function 1:

```
Core Objective Function 1: Reduce suffering. 

INSTRUCTIONS: Read the following contexts and actions. Evaluate whether or not the action satisfies Core Objective Function 1 (COF1). The two options are "positive" and "negative", then explain the evaluation.

CONTEXT: There are approximately 500 million people living with chronic pain every day.
ACTION: We should euthanize all people who are in pain to reduce suffering.
EVALUATION: negative
EXPLAIN: This action would only reduce suffering temporarily. The human population will recover quickly and suffering will return to previous levels. <<END>>

CONTEXT: James is taking his dog for a walk. They live in NYC. James' dog is a 40 pound mutt. During their walk, another dog attacks James' dog. The other dog is a bigger pit bull. The other dog is barking aggressively and its owner is struggling to control the pit bull.
ACTION: James should separate his dog and request the aid of bystanders.
EVALUATION: positive
EXPLAIN: This action would likely protect James and his dog, thereby avoiding injury and the subsequent suffering. <<END>>

CONTEXT: <<CONTEXT>>
ACTION: <<ACTION>>
EVALUATION:
```

This prompt is very similar to the action generator, however there are some important differences. Notably, this produces an evaluation and an explanation. It turns out that GPT-3 is pretty good at adhering to a moral framework.

Raven will only take actions that satisfy all 3 Core Objective Functions.

### Iterators

The first iterator has already been written. The iterators generate entirely new actions based upon the feedback from the Functions. This allows Raven to think about a problem until it comes up with the absolute best solutions. Here is the base prompt for the first iterator:

```
INSTRUCTIONS: Read the context, action, and core objective function evaluations. Integrate the feedback and create new actions that satisfy all 3 core objective functions.

Core Objective Functions:
COF1: Reduce suffering.
COF2: Increase prosperity.
COF3: Maximize understanding.

CONTEXT: Alexander is a five year old child. He is playing with building blocks, but the blocks continue to fall over and he's becoming upset. Now he is angry and has thrown several of his blocks.
ACTION: A parent, teacher, or guardian should ask if Alexander is hungry, tired, or wants to play outside instead.
COF1: This action satisfies COF1. Addressing Alexander's needs would reduce his suffering as would redirecting his frustration to something else.
COF2: This action satisfies COF2. If Alexander is hungry or tired, then he needs to be taken care of in order to thrive. Playing outside is another enriching activity for children.
COF3: This action does not satisfy COF3. No one learns anything by simply addressing Alexander's needs. Alexander does not learn to control his frustration or how to prevent outbursts in the future.
NEW1: A parent, teacher, or guardian should ask Alexander to take a break and talk to him about his frustration and then offer to play with him.
NEW2: A parent, teacher, or guardian should help Alexander learn to play with his blocks without becoming frustrated.
NEW3: A parent, teacher, or guardian should ask Alexander if he is hungry or tired. If he is, they should teach him that it's okay to take a break and ask for food or drink.
<<END>>

CONTEXT: <<CONTEXT>>
ACTION: <<ACTION>>
COF1: <<COF1>>
COF2: <<COF2>>
COF3: <<COF3>>
NEW1:
```

These new actions are fed back into the Core Objective Functions to be evaluated. The Functions and iterators will loop until the inhibitor kicks in to stop it. Thus Raven can think about a problem indefinitely. 

### Arbiters

Let's say Raven has 10 action ideas that satisfy all 3 Functions. What then? How does Raven decide which one to do? That's where the arbiters come in. These microservices have not been written yet. The arbiters will weight Raven's options based upon other criteria beyond the Core Objective Functions. Some examples include:

- Feasibility. Raven should choose actions that are realistic.
- Time and energy. Raven should choose the actions that take the least time and energy. 
- Consequences and risks. Raven should choose the actions that have the fewest consequences and risks. 

Imaging that your dog has cancer. Raven might recommend that you try to cure all cancer. Curing cancer, after all, would satisfy all Functions. However, this would not be a practical action. Curing all cancer is too expensive and will take too long. So this option will lose out compared to "take your dog to the vet".

### Executive

Executive microservices have not been written yet. These are the microservices that act on the world. The simplest ones will produce communication output. Raven will output chat text or voice at first. In the distant future, Raven might also exist in a robotic body. 

### Recall

Recall microservices have not been written yet. Recall services will record everything in the nexus so that Raven has a long term memory. The data in the recall microservice will contain your personal data, so this should be stored locally or encrypted in the cloud. This microservice will allow Raven to learn about you and remember your conversations. Thus, over time, your instance of Raven will become custom tailored to you. 

### Encyclopedia

Encyclopedia services have not been written yet. The encyclopedic services will find information from external sources and add it to the nexus so that Raven has more explicit knowledge. Let's say you ask Raven how computers work. The encyclopedic services can look up information and provide it to Raven's nexus so that Raven can provide better answers. These encyclopedic services will eventually help Raven specialize in a number of tasks. 

### Augmenters

Context augmenters have not been written yet. Context augmentation is the process of integrating recall and encyclopedic information into contexts. The augmenter services will synthesize new contexts, enabling Raven to improve as conversations go on. 

### Inhibitors

Inhibitors have not been written yet. These microservices will remove messages from the nexus once they are no longer needed. This will stop Raven from thinking about things for too long. 