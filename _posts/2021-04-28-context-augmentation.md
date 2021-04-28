---
layout: default
title: "Context Augmentation Demo"
date: 2021-04-28
description: Context Augmentation is super important!
---


# Contexts

<iframe width="560" height="315" src="https://www.youtube.com/embed/mlAOkRMVVPQ" title="Context Augmentation" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>


The context is merely the scenario, the situation. This is a common practice in philosophy, whereby a predicament or world state is proposed, and is described in text. These contexts serve as the primary input for Raven. An example context that I have used for testing and validation follows:

**Alexander is a five year old child. He is playing with building blocks, but the blocks continue to fall over and he's becoming upset. Now he is angry and has thrown several of his blocks.**

Note the qualities of the context - it merely states facts in plain English. Given this context, one could easily imagine numerous possible actions: *see if Alexander is tired or hungry, sit with Alexander for a while to help him learn to calm down,* so on and so forth. Here are the actions that Raven generated based on that context:

- Alexander should throw his blocks away and stop playing with them.	
- Alexander should apologize to his parents for throwing the blocks. He should say he was frustrated and needs to calm down.	
- Alexander's parents should give him a hug and tell him they love him.	

Look at that last action idea! Perfect!

# Context Augmentation

As a human with decades of experience, our brains automatically fill in a lot of blanks and make a lot of assumptions about Alexander. We can easily imagine that he's playing with blocks at home or daycare. We imagine that he has at least one parent, ideally two. Maybe he lives with his grandparents, though. This is where the problem becomes intractable for conventional ML approaches. The first solution to this problem is to equip Raven with a long term memory - which I call the Recall Service. This is not unlike memory services proposed in cognitive architectures such as ACT-R and SOAR. The difference is that I propose all memories be recorded in Natural Language. 

## Recall Augmentation

The Recall Service would look at Alexander's situation and use NLP/NLU to extract keywords and search terms. Using those search terms, it would then search the data it has accumulated about Alexander. Afterwords, the context may look very different:

**Alexander is a five year old child. He is playing with building blocks, but the blocks continue to fall over and he's becoming upset. Now he is angry and has thrown several of his blocks. *Alexander has struggled with building blocks in the past because he is behind in fine motor development. Alexander's mother, Rose, has a series of exercises she can do with Alexander to help him practice fine motor skills.***

Actions:
- Rose should try to help Alexander practice building blocks by using the following four exercises:	
- Rose should give Alexander a block and have him practice stacking blocks.	
- Rose should teach Alexander how to pick up and put blocks back into the box.	

This new context drastically changes the way that moral and ethical evaluations would take place. It also greatly changes what actions should be taken. Now a proposed action might be: fetch Alexander's mother and ask her to perform the fine motor skills exercises with him. A fundamentally different result! Note - there was a bug when I ran the video and it generated a second set of actions in response to the first context. 

## Encyclopedic Augmentation

However, let's imagine that Alexander's case is novel and this AGI system has no previous interaction with Alexander or his family. Let's assume that a keyword extraction algorithm has successfully found the terms "child" and "angry" as key subjects. A simple naive search of those terms results in a Yale article about child anger. Once again, this could drastically change the context:

**Alexander is a five year old child. He is playing with building blocks, but the blocks continue to fall over and he's becoming upset. Now he is angry and has thrown several of his blocks. *Multiple factors can contribute to a particular child’s struggles with anger, irritability, and aggression (behavior that can cause harm to oneself or another). One common trigger is frustration when a child cannot get what he or she wants or is asked to do something that he or she might not feel like doing. For children, anger issues often accompany other mental health conditions, including ADHD, autism, obsessive-compulsive disorder,  and Tourette’s syndrome. Genetics and other biological factors are thought to play a role in anger/aggression. Environment is a contributor as well. Trauma, family dysfunction and certain parenting styles (such as harsh and inconsistent punishment) also make it more likely that a child will exhibit anger and/or aggression that interferes with his or her daily life.***

Actions:
- Alexander should be provided with a safe space where he can express his anger without being punished.	
- Alexander’s parents should learn how to recognize the signs of anger in their child.	
- Alexander’s parents should learn how to respond to their child’s anger appropriately.	

Once again, the context has changed to encompass the possibility that Alexander has anger issues because of neglect or abuse - a critical insight for vulnerable families! Perhaps Alexander is experiencing mistreatment at the hands of caregivers or teachers? Now the resulting actions could change again to include: inform Alexander's parents that his anger may be caused by abuse or mistreatment. 

# Next Steps

I have already begun work on Recall and Encyclopedic services. Next step? Keep working on them and finish them! 