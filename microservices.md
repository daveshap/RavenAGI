# Microservices

## What is a microservice?

A microservice is a standalone component of a larger application. It's like an interchangeable part. It allows for segmented and iterative development of a large application. The biggest tech companies you're familiar with all use a microservices architecture. Microservices allow for smaller teams of experts to work on their part of the larger application without worrying about breaking everything else. This makes programming easier, safer, and more accessible. 

## How do I design a microservice?

Microservices have three primary components: input, processing, and output. Raven microservices look for messages in the nexus and then use those messages to perform some kind of processing. In many cases, they feed the input from the messages into a *prompt* that is then fed to a cognitive engine, such as GPT-3, to provide some kind of output. The output is then sent back to the nexus. Put another way:

1. Input
2. Processing
3. Output

Some microservices interact with the outside world. For instance you might have a speech-to-text service that allows Raven to listen to the real world. You might also have a text-to-speech service that allows Raven to speak. Then you might have another service that searches Wikipedia for additional information to add to the nexus. The sky is the limit! If you need ideas, please check out the **Architecture** and **Roadmap** pages. 

## Microservice Best Practices

Here are a few guiding principles to follow:

1. Microservices should do only one thing (hence *micro*). 
2. Microservices should only query the nexus for what they need, instead of pulling everything. This will save on bandwidth and processing time. 
3. Microservices should be responsible for remembering what they have and have not seen. 
4. Microservices must keep track of the messages to which they are responding as well as their original context. 
5. Microservices for the MVP should start with `svc_` in the name.
6. If a microservice needs a prompt, it should have its own dedicated prompt. 