---
layout: default
---

# Introduction

Learning from interaction is a foundational idea underlying nearly all theories of learning and intelligence. In this book we explore a computational approach to learning from interaction. Rather than directly theorizing about how people or animals learn, we primarily explore idealized learning situations and evaluate the effectiveness of various learning methods.

## Reinforcement Learning

Reinforcement learning is learning what to do—how to map situations to actions—so as to maximize a numerical reward signal. The learner is not told which actions to take, but instead must discover which actions yield the most reward by trying them. These two characteristics—trial-and-error search and delayed reward—are the two most important distinguishing features of reinforcement learning.

We formalize the problem of reinforcement learning using ideas from dynamical systems theory, speciﬁcally, as the optimal control of incompletely-known Markov decision processes. The basic idea is simply to capture the most important aspects of the real problem facing a learning agent interacting over time with its environment to achieve a goal. A learning agent must be able to sense the state of its environment to some extent and must be able to take actions that affect the state. The agent also must have a goal or goals relating to the state of the environment.

Reinforcement learning is di↵erent from supervised learning. Supervised learning is learning from a training set of labeled examples provided by a knowledgable external supervisor. This is an important kind of learning, but alone it is not adequate for learning from interaction. In interactive problems it is often impractical to obtain examples of desired behavior that are both correct and representative of all the situations in which the agent has to act. In uncharted territory—where one would expect learning to be most beneﬁcial—an agent must be able to learn from its own experience.

One of the challenges that arise in reinforcement learning, and not in other kinds of learning, is the trade-off between exploration and exploitation. To obtain a lot of reward, a reinforcement learning agent must prefer actions that it has tried in the past and found to be effective in producing reward. The agent has to exploit what it has already experienced in order to obtain reward, but it also has to explore in order to make better action selections in the future. The dilemma is that neither exploration nor exploitation can be pursued exclusively without failing at the task. The agent must try a variety of actions and progressively favor those that appear to be best.

Another key feature of reinforcement learning is that it explicitly considers the whole problem of a goal-directed agent interacting with an uncertain environment. This is in contrast to many approaches that consider subproblems without addressing how they might ﬁt into a larger picture.

Reinforcement learning takes the opposite tack, starting with a complete, interactive, goal-seeking agent. All reinforcement learning agents have explicit goals, can sense aspects of their environments, and can choose actions to inﬂuence their environments. Moreover, it is usually assumed from the beginning that the agent has to operate despite signiﬁcant uncertainty about the environment it faces.

One of the most exciting aspects of modern reinforcement learning is its substantive and fruitful interactions with other engineering and scientiﬁc disciplines. For example, the ability of some reinforcement learning methods to learn with parameterized approximators addresses the classical “curse of dimensionality” in operations research and control theory. More distinctively, reinforcement learning has also interacted strongly with psychology and neuroscience, with substantial beneﬁts going both ways. Of all the forms of machine learning, reinforcement learning is the closest to the kind of learning that humans and other animals do, and many of the core algorithms of reinforcement learning were originally inspired by biological learning systems.

## Examples

A good way to understand reinforcement learning is to consider some of the examples and possible applications that have guided its development :

1. A master chess player makes a move. The choice is informed both by planning anticipating possible replies and counterreplies—and by immediate, intuitive judgments of the desirability of particular positions and moves.
2. An adaptive controller adjusts parameters of a petroleum reﬁnery’s operation in real time. The controller optimizes the yield/cost/quality trade-off on the basis of speciﬁed marginal costs without sticking strictly to the set points originally suggested by engineers.
3. A gazelle calf struggles to its feet minutes after being born. Half an hour later it is running at 20 miles per hour.
4. A mobile robot decides whether it should enter a new room in search of more trash to collect or start trying to ﬁnd its way back to its battery recharging station. It makes its decision based on the current charge level of its battery and how quickly and easily it has been able to ﬁnd the recharger in the past.

These examples share features that are so basic that they are easy to overlook. All involve interaction between an active decision-making agent and its environment, within which the agent seeks to achieve a goal despite uncertainty about its environment. At the same time, in all of these examples the e↵ects of actions cannot be fully predicted; thus the agent must monitor its environment frequently and react appropriately. In all of these examples the agent can use its experience to improve its performance over time. The knowledge the agent brings to the task at the start—either from previous experience with related tasks or built into it by design or evolution—inﬂuences what is useful or easy to learn, but interaction with the environment is essential for adjusting behavior to exploit speciﬁc features of the task.

## Elements of Reinforcement Learning

Beyond the agent and the environment, one can identify four main subelements of a reinforcement learning system: a policy, a reward signal, a value function, and, optionally, a model of the environment.

A policy deﬁnes the learning agent’s way of behaving at a given time. Roughly speaking, a policy is a mapping from perceived states of the environment to actions to be taken when in those states. In some cases the policy may be a simple function or lookup table, whereas in others it may involve extensive computation such as a search process. The policy is the core of a reinforcement learning agent in the sense that it alone is sufficient to determine behavior.

A reward signal deﬁnes the goal of a reinforcement learning problem. On each time step, the environment sends to the reinforcement learning agent a single number called the reward. The agent’s sole objective is to maximize the total reward it receives over the long run. The reward signal thus deﬁnes what are the good and bad events for the agent. The reward signal is the primary basis for altering the policy; if an action selected by the policy is followed by low reward, then the policy may be changed to select some other action in that situation in the future.

Whereas the reward signal indicates what is good in an immediate sense, a value function speciﬁes what is good in the long run. Roughly speaking, the value of a state is the total amount of reward an agent can expect to accumulate over the future, starting from that state. Whereas rewards determine the immediate, intrinsic desirability of environmental states, values indicate the long-term desirability of states after taking into account the states that are likely to follow and the rewards available in those states. For example, a state might always yield a low immediate reward but still have a high value because it is regularly followed by other states that yield high rewards. It is values with which we are most concerned when making and evaluating decisions.  Rewards are basically given directly by the environment, but values must be estimated and re-estimated from the sequences of observations an agent makes over its entire lifetime. The central role of value estimation is arguably the most important thing that has been learned about reinforcement learning over the last six decades.

The fourth and ﬁnal element of some reinforcement learning systems is a model of the environment. This is something that mimics the behavior of the environment, or more generally, that allows inferences to be made about how the environment will behave. For example, given a state and action, the model might predict the resultant next state and next reward. Methods for solving reinforcement learning problems that use models and planning are called model-based methods, as opposed to simpler model-free methods that are explicitly trial-and-error learners—viewed as almost the opposite of planning.

## Limitations and Scope

