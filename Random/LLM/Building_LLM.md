---
layout: default
---

# What We’ve Learned From A Year of Building with LLMs

[Original Article](https://applied-llms.org/)

Building with large language models (LLMs) has never been more exciting. In the past year, LLMs have become viable for real-world applications, improving in performance and affordability. Despite these advancements, creating effective systems remains challenging. After a year of building, we've encountered many obstacles and want to share our learnings to help you avoid pitfalls and iterate faster. Our insights are organized into three sections :

1. Tactical : Practical advice on prompting, RAG, flow engineering, evaluations, and monitoring. Ideal for practitioners and hobbyists.
2. Operational : Tips on team-building, day-to-day operations, and sustainable product deployment. Targeted at product and technical leaders.
3. Strategic : Long-term considerations like "no GPU before PMF" and focusing on system-level improvements. Geared towards founders and executives.

## Tactical

### Prompting

**Focus on getting the most out of fundamental prompting techniques**

It’s easy to both underestimate and overestimate its importance. It’s underestimated because the right prompting techniques, when used correctly, can get us very far. It’s overestimated because even prompt-based applications require significant engineering around the prompt to work well. The idea of in-context learning via n-shot prompts is to provide the LLM with examples that demonstrate the task and align outputs to our expectations. A few tips:

1. If n is too low, the model may over-anchor on those specific examples, hurting its ability to generalize. As a rule of thumb, aim for n ≥ 5. Don’t be afraid to go as high as a few dozen.
2. Examples should be representative of the prod distribution. If you’re building a movie summarizer, include samples from different genres in roughly the same proportion you’d expect to see in practice.
3. You don’t always need to provide the input-output pairs; examples of desired outputs may be sufficient.
4. If you plan for the LLM to use tools, include examples of using those tools.

In Chain-of-Thought (CoT) prompting, we encourage the LLM to explain its thought process before returning the final answer. Note that in recent times, some doubt has been cast on if this technique is as powerful as believed. Additionally, there’s significant debate as to exactly what is going on during inference when Chain-of-Thought is being used.

**Structure your inputs and outputs**

Structured input and output help models better understand the input as well as return output that can reliably integrate with downstream systems. Adding serialization formatting to your inputs can help provide more clues to the model as to the relationships between tokens in the context, additional metadata to specific tokens (like types), or relate the request to similar examples in the model’s training data.

**Have small prompts that do one thing, and only one thing, well**

A prompt typically starts simple: A few sentences of instruction, a couple of examples, and we’re good to go. But as we try to improve performance and handle more edge cases, complexity creeps in. More instructions. Multi-step reasoning. Dozens of examples. Before we know it, our initially simple prompt is now a 2,000 token Frankenstein. Just like how we strive (read: struggle) to keep our systems and code simple, so should we for our prompts.

**Craft your context tokens**

Rethink, and challenge your assumptions about how much context you actually need to send to the agent. Be like Michaelangelo, do not build up your context sculpture—chisel away the superfluous material until the sculpture is revealed. We’ve found that taking the final prompt sent to the model—with all of the context construction, and meta-prompting, and RAG results—putting it on a blank page and just reading it, really helps you rethink your context.

### Information Retrieval / RAG

Beyond prompting, another effective way to steer an LLM is by providing knowledge as part of the prompt. This grounds the LLM on the provided context which is then used for in-context learning. This is known as retrieval-augmented generation (RAG).

**RAG is only as good as the retrieved documents’ relevance, density, and detail**

The quality of your RAG’s output is dependent on the quality of retrieved documents, which in turn can be considered along a few factors

The first and most obvious metric is relevance. This is typically quantified via ranking metrics such as Mean Reciprocal Rank (MRR) or Normalized Discounted Cumulative Gain (NDCG). MRR evaluates how well a system places the first relevant result in a ranked list while NDCG considers the relevance of all the results and their positions. They measure how good the system is at ranking relevant documents higher and irrelevant documents lower.

Second, we also want to consider information density. If two documents are equally relevant, we should prefer one that’s more concise and has fewer extraneous details.

Finally, consider the level of detail provided in the document.

**Don’t forget keyword search; use it as a baseline and in hybrid search**

Nonetheless, while embeddings are undoubtedly a powerful tool, they are not the be-all and end-all. First, while they excel at capturing high-level semantic similarity, they may struggle with more specific, keyword-based queries, like when users search for names (e.g., Ilya), acronyms (e.g., RAG), or IDs (e.g., claude-3-sonnet). Second, it’s more straightforward to understand why a document was retrieved with keyword search—we can look at the keywords that match the query. In contrast, embedding-based retrieval is less interpretable.

In most cases, a hybrid will work best: keyword matching for the obvious matches, and embeddings for synonyms, hypernyms, and spelling errors, as well as multimodality.

**Prefer RAG over finetuning for new knowledge**

Recent research suggests RAG may have an edge. One study compared RAG against unsupervised finetuning (aka continued pretraining), evaluating both on a subset of MMLU and current events. They found that RAG consistently outperformed finetuning for knowledge encountered during training as well as entirely new knowledge. In another paper, they compared RAG against supervised finetuning on an agricultural dataset. Similarly, the performance boost from RAG was greater than finetuning, especially for GPT-4.

Beyond improved performance, RAG has other practical advantages. First, compared to continuous pretraining or finetuning, it’s easier—and cheaper!—to keep retrieval indices up-to-date. Second, if our retrieval indices have problematic documents that contain toxic or biased content, we can easily drop or modify the offending documents.

**Long-context models won’t make RAG obsolete**

While it’s true that long contexts will be a game-changer for use cases such as analyzing multiple documents or chatting with PDFs, the rumors of RAG’s demise are greatly exaggerated.

First, even with a context size of 10M tokens, we’d still need a way to select relevant context. Second, beyond the narrow needle-in-a-haystack eval, we’ve yet to see convincing data that models can effectively reason over large context sizes. Thus, without good retrieval (and ranking), we risk overwhelming the model with distractors, or may even fill the context window with completely irrelevant information.

Finally, there’s cost. During inference, the Transformer’s time complexity scales linearly with context length.

### Tuning and optimizing workflows

Prompting an LLM is just the beginning. To get the most juice out of them, we need to think beyond a single prompt and embrace workflows.

**Step-by-step, multi-turn “flows” can give large boosts**

It’s common knowledge that decomposing a single big prompt into multiple smaller prompts can achieve better results. Small tasks with clear objectives make for the best agent or flow prompts. It’s not required that every agent prompt requests structured output, but structured outputs help a lot to interface with whatever system is orchestrating the agent’s interactions with the environment.

**Prioritize deterministic workflows for now**

While AI agents can dynamically react to user requests and the environment, their non-deterministic nature makes them a challenge to deploy. Each step an agent takes has a chance of failing, and the chances of recovering from the error are poor. Thus, the likelihood that an agent completes a multi-step task successfully decreases exponentially as the number of steps increases.

A potential approach is to have agent systems produce deterministic plans which are then executed in a structured, reproducible way. First, given a high-level goal or prompt, the agent generates a plan. Then, the plan is executed deterministically. This allows each step to be more predictable and reliable.

**Getting more diverse outputs beyond temperature**

Briefly, increasing the temperature parameter makes LLM responses more varied. At sampling time, the probability distributions of the next token become flatter, meaning that tokens that are usually less likely get chosen more often. In other words, increasing temperature does not guarantee that the LLM will sample outputs from the probability distribution you expect (e.g., uniform random). Nonetheless, we have other tricks to increase output diversity. The simplest way is to adjust elements within the prompt. Additionally, keeping a short list of recent outputs can help prevent redundancy.

**Caching is underrated**

Caching saves cost and eliminates generation latency by removing the need to recompute responses for the same input. Furthermore, if a response has previously been guardrailed, we can serve these vetted responses and reduce the risk of serving harmful or inappropriate content.

One straightforward approach to caching is to use unique IDs for the items being processed, such as if we’re summarizing new articles or product reviews. When a request comes in, we can check to see if a summary already exists in the cache. If so, we can return it immediately; if not, we generate, guardrail, and serve it, and then store it in the cache for future requests.

For more open-ended queries, we can borrow techniques from the field of search, which also leverages caching for open-ended inputs. Features like autocomplete, spelling correction, and suggested queries also help normalize user input and thus increase the cache hit rate.

**When to finetune**

We may have some tasks where even the most cleverly designed prompts fall short. For example, even after significant prompt engineering, our system may still be a ways from returning reliable, high-quality output. If so, then it may be necessary to finetune a model for your specific task. While finetuning can be effective, it comes with significant costs. We have to annotate finetuning data, finetune and evaluate models, and eventually self-host them. Thus, consider if the higher upfront cost is worth it. If prompting gets you 90% of the way there, then finetuning may not be worth the investment.

### Evaluation & Monitoring

Evaluating LLMs is a minefield and even the biggest labs find it challenging. LLMs return open-ended outputs, and the tasks we set them to are varied.

**Create a few assertion-based unit tests from real input/output samples**

Create unit tests (i.e., assertions) consisting of samples of inputs and outputs from production, with expectations for outputs based on at least three criteria. While three criteria might seem arbitrary, it’s a practical number to start with; fewer might indicate that your task isn’t sufficiently defined or is too open-ended, like a general-purpose chatbot. These unit tests, or assertions, should be triggered by any changes to the pipeline, whether it’s editing a prompt, adding new context via RAG, or other modifications.

**LLM-as-Judge can work (somewhat), but it’s not a silver bullet**

LLM-as-Judge, where we use a strong LLM to evaluate the output of other LLMs, has been met with skepticism. Nonetheless, when implemented well, LLM-as-Judge achieves decent correlation with human judgments, and can at least help build priors about how a new prompt or technique may perform. Specifically, when doing pairwise comparisons (control vs. treatment), 

Here are some suggestions to get the most out of LLM-as-Judge:

1. Use pairwise comparisons
2. Control for position bias
3. Allow for ties
4. Use Chain-of-Thought
5. Control for response length

**The “intern test” for evaluating generations**

We like to use the following “intern test” when evaluating generations: If you took the exact input to the language model, including the context, and gave it to an average college student in the relevant major as a task, could they succeed? How long would it take?

1. If the answer is no because the LLM lacks the required knowledge, consider ways to enrich the context.
2. If the answer is no and we simply can’t improve the context to fix it, then we may have hit a task that’s too hard for contemporary LLMs.
3. If the answer is yes, but it would take a while, we can try to reduce the complexity of the task. Is it decomposable? Are there aspects of the task that can be made more templatized?
4. If the answer is yes, they would get it quickly, then it’s time to dig into the data. What’s the model doing wrong? Can we find a pattern of failures? Try asking the model to explain itself before or after it responds, to help you build a theory of mind.

**Overemphasizing certain evals can hurt overall performance**

While some models achieve near-perfect recall, it’s questionable whether NIAH truly measures the reasoning and recall abilities needed in real-world applications. Consider a more practical scenario: Given the transcript of an hour-long meeting, can the LLM summarize the key decisions and next steps, as well as correctly attribute each item to the relevant person? This task is more realistic, going beyond rote memorization, and considers the ability to parse complex discussions, identify relevant information, and synthesize summaries.

**Simplify annotation to binary tasks or pairwise comparisons**

In binary classifications, annotators are asked to make a simple yes-or-no judgment on the model’s output. They might be asked whether the generated summary is factually consistent with the source document, or whether the proposed response is relevant, or if it contains toxicity. Compared to the Likert scale, binary decisions are more precise, have higher consistency among raters, and lead to higher throughput.

In pairwise comparisons, the annotator is presented with a pair of model responses and asked which is better. Because it’s easier for humans to say “A is better than B” than to assign an individual score to either A or B individually, this leads to faster and more reliable annotations (over Likert scales).

**(Reference-free) evals and guardrails can be used interchangeably**

Guardrails help to catch inappropriate or harmful content while evals help to measure the quality and accuracy of the model’s output. And if your evals are reference-free, they can be used as guardrails too. Reference-free evals are evaluations that don’t rely on a “golden” reference, such as a human-written answer, and can assess the quality of output based solely on the input prompt and the model’s response.

**LLMs will return output even when they shouldn’t**

A key challenge when working with LLMs is that they’ll often generate output even when they shouldn’t. This can lead to harmless but nonsensical responses, or more egregious defects like toxicity or dangerous content.

While we can try to prompt the LLM to return a “not applicable” or “unknown” response, it’s not foolproof. Even when the log probabilities are available, they’re a poor indicator of output quality.

While careful prompt engineering can help to an extent, we should complement it with robust guardrails that detect and filter/regenerate undesired output.

**Hallucinations are a stubborn problem**

Unlike content safety or PII defects which have a lot of attention and thus seldom occur, factual inconsistencies are stubbornly persistent and more challenging to detect. They’re more common and occur at a baseline rate of 5 - 10%, and from what we’ve learned from LLM providers, it can be challenging to get it below 2%, even on simple tasks such as summarization.

To address this, we can combine prompt engineering (upstream of generation) and factual inconsistency guardrails (downstream of generation).

## Operational

### Data

Just as the quality of ingredients determines the taste of a dish, the quality of input data constrains the performance of machine learning systems. In addition, output data is the only way to tell whether the product is working or not.

**Check for development-prod skew**

A common source of errors in traditional machine learning pipelines is train-serve skew. This happens when the data used in training differs from what the model encounters in production.

LLM development-prod skew can be categorized into two types: structural and content-based. Structural skew includes issues like formatting discrepancies, such as differences between a JSON dictionary with a list-type value and a JSON list, inconsistent casing, and errors like typos or sentence fragments. Content-based or “semantic” skew refers to differences in the meaning or context of the data.

When testing changes, such as prompt engineering, ensure that hold-out datasets are current and reflect the most recent types of user interactions.

**Look at samples of LLM inputs and outputs every day**

LLMs are dynamic and constantly evolving. Despite their impressive zero-shot capabilities and often delightful outputs, their failure modes can be highly unpredictable. While developers can come up with some criteria upfront for evaluating LLM outputs, these predefined criteria are often incomplete. To manage this effectively, we should log LLM inputs and outputs. By examining a sample of these logs daily, we can quickly identify and adapt to new patterns or failure modes.

### Working with models

With LLM APIs, we can rely on intelligence from a handful of providers. While this is a boon, these dependencies also involve trade-offs on performance, latency, throughput, and cost. Also, as newer, better models drop (almost every month in the past year), we should be prepared to update our products as we deprecate old models and migrate to newer models.

**Generate structured output to ease downstream integration**

For most real-world use cases, the output of an LLM will be consumed by a downstream application via some machine-readable format.

**Migrating prompts across models is a pain in the ass**

Sometimes, our carefully crafted prompts work superbly with one model but fall flat with another. This can happen when we’re switching between various model providers, as well as when we upgrade across versions of the same model. 

For example, Voiceflow found that migrating from gpt-3.5-turbo-0301 to gpt-3.5-turbo-1106 led to a 10% drop in their intent classification task. Thus, if we have to migrate prompts across models, expect it to take more time than simply swapping the API endpoint. Don’t assume that plugging in the same prompt will lead to similar or better results.

**Version and pin your models**

In any machine learning pipeline, “changing anything changes everything”. This is particularly relevant as we rely on components like large language models (LLMs) that we don’t train ourselves and that can change without our knowledge.

Fortunately, many model providers offer the option to “pin” specific model versions (e.g., gpt-4-turbo-1106). This enables us to use a specific version of the model weights, ensuring they remain unchanged.

**Choose the smallest model that gets the job done**

When working on a new application, it’s tempting to use the biggest, most powerful model available. But once we’ve established that the task is technically feasible, it’s worth experimenting if a smaller model can achieve comparable results.

The benefits of a smaller model are lower latency and cost. The point is, don’t overlook smaller models. While it’s easy to throw a massive model at every problem, with some creativity and experimentation, we can often find a more efficient solution. 

### Product

While new technology offers new possibilities, the principles of building great products are timeless. Thus, even if we’re solving new problems for the first time, we don’t have to reinvent the wheel on product design.

**Involve design early and often**

Having a designer will push you to understand and think deeply about how your product can be built and presented to users. We sometimes stereotype designers as folks who take things and make them pretty. But beyond just the user interface, they also rethink how the user experience can be improved, even if it means breaking existing rules and paradigms.

**Design your UX for Human-In-The-Loop**

One way to get quality annotations is to integrate Human-in-the-Loop (HITL) into the user experience (UX). By allowing users to provide feedback and corrections easily, we can improve the immediate output and collect valuable data to improve our models.

Feedback can be explicit or implicit. Explicit feedback is information users provide in response to a request by our product; implicit feedback is information we learn from user interactions without needing users to deliberately provide feedback. Coding assistants and Midjourney are examples of implicit feedback while thumbs up and thumb downs are explicit feedback. If we design our UX well, like coding assistants and Midjourney, we can collect plenty of implicit feedback to improve our product and models.

**Prioritize your hierarchy of needs ruthlessly**

As we think about putting our demo into production, we’ll have to think about the requirements for:

1. Reliability: 99.9% uptime, adherence to structured output
2. Harmlessness: Not generate offensive, NSFW, or otherwise harmful content
3. Factual consistency: Being faithful to the context provided, not making things up
4. Usefulness: Relevant to the users’ needs and request
5. Scalability: Latency SLAs, supported throughput
6. Cost: Because we don’t have unlimited budget
7. And more: Security, privacy, fairness, GDPR, DMA, etc, etc.

If we try to tackle all these requirements at once, we’re never going to ship anything. Thus, we need to prioritize. Ruthlessly. This means being clear what is non-negotiable (e.g., reliability, harmlessness) without which our product can’t function or won’t be viable. It’s all about identifying the minimum lovable product.

**Calibrate your risk tolerance based on the use case**

When deciding on the language model and level of scrutiny of an application, consider the use case and audience. For a customer-facing chatbot offering medical or financial advice, we’ll need a very high bar for safety and accuracy. Mistakes or bad output could cause real harm and erode trust. But for less critical applications, such as a recommender system, or internal-facing applications like content classification or summarization, excessively strict requirements only slow progress without adding much value.

### Team & Roles

No job function is easy to define, but writing a job description for the work in this new space is more challenging than others.

**Focus on the process, not tools**

When faced with new paradigms, such as LLMs, software engineers tend to favor tools. As a result, we overlook the problem and process the tool was supposed to solve. In doing so, many engineers assume accidental complexity, which has negative consequences for the team’s long-term productivity. There are too many components of LLMs beyond prompt writing and evaluations to list exhaustively here.  However, it is important that AI Engineers seek to understand the processes before adopting tools.

**Always be experimenting**

ML products are deeply intertwined with experimentation. Not only the A/B, Randomized Control Trials kind, but the frequent attempts at modifying the smallest possible components of your system, and doing offline evaluation. It’s common to try different approaches to solving the same problem because experimentation is so cheap now. The high cost of collecting data and training a model is minimized—prompt engineering costs little more than human time.

**Empower everyone to use new AI technology**

As generative AI increases in adoption, we want the entire team—not just the experts—to understand and feel empowered to use this new technology. There’s no better way to develop intuition for how LLMs work (e.g., latencies, failure modes, UX) than to, well, use them. A big part of this is education. It can start as simple as the basics of prompt engineering, where techniques like n-shot prompting and CoT help condition the model towards the desired output. Folks who have the knowledge can also educate about the more technical aspects, such as how LLMs are autoregressive when generating output.

**Don’t fall into the trap of “AI Engineering is all I need”**

As new job titles are coined, there is an initial tendency to overstate the capabilities associated with these roles. This often results in a painful correction as the actual scope of these jobs becomes clear. Newcomers to the field, as well as hiring managers, might make exaggerated claims or have inflated expectations.

Initially, many assumed that data scientists alone were sufficient for data-driven projects. However, it became apparent that data scientists must collaborate with software and data engineers to develop and deploy data products effectively.

This misunderstanding has shown up again with the new role of AI Engineer, with some teams believing that AI Engineers are all you need. In reality, building machine learning or AI products requires a broad array of specialized roles.

## Strategy

Successful products require thoughtful planning and prioritization, not endless prototyping or following the latest model releases or trends.

### No GPUs before PMF

To be great, your product needs to be more than just a thin wrapper around somebody else’s API. But mistakes in the opposite direction can be even more costly.

**Training from scratch (almost) never makes sense**

For most organizations, pretraining an LLM from scratch is an impractical distraction from building products.

As exciting as it is and as much as it seems like everyone else is doing it, developing and maintaining machine learning infrastructure takes a lot of resources. This includes gathering data, training and evaluating models, and deploying them. If you’re still validating product-market fit, these efforts will divert resources from developing your core product. Even if you had the compute, data, and technical chops, the pretrained LLM may become obsolete in months.

**Don’t finetune until you’ve proven it’s necessary**

For most organizations, finetuning is driven more by FOMO than by clear strategic thinking.

Organizations invest in finetuning too early, trying to beat the “just another wrapper” allegations. In reality, finetuning is heavy machinery, to be deployed only after you’ve collected plenty of examples that convince you other approaches won’t suffice.

LLM-powered applications aren’t a science fair project. Investment in them should be commensurate with their contribution to your business’ strategic objectives and competitive differentiation.

**Start with inference APIs, but don’t be afraid of self-hosting**

With LLM APIs, it’s easier than ever for startups to adopt and integrate language modeling capabilities without training their own models from scratch. Providers like Anthropic, and OpenAI offer general APIs that can sprinkle intelligence into your product with just a few lines of code.

But, as with databases, managed services aren’t the right fit for every use case, especially as scale and requirements increase. Indeed, self-hosting may be the only way to use models without sending confidential / private data out of your network, as required in regulated industries like healthcare and finance, or by contractual obligations or confidentiality requirements.

### Iterate to something great

To sustain a competitive edge in the long run, you need to think beyond models and consider what will set your product apart. While speed of execution matters, it shouldn’t be your only advantage.

**The model isn’t the product, the system around it is**

For teams that aren’t building models, the rapid pace of innovation is a boon as they migrate from one SOTA model to the next, chasing gains in context size, reasoning capability, and price-to-value to build better and better products. This progress is as exciting as it is predictable. Taken together, this means models are likely to be the least durable component in the system.

Instead, focus your efforts on what’s going to provide lasting value, such as:

1. Evals: To reliably measure performance on your task across models
2. Guardrails: To prevent undesired outputs no matter the model
3. Caching: To reduce latency and cost by avoiding the model altogether
4. Data flywheel: To power the iterative improvement of everything above

**Build trust by starting small**

Building a product that tries to be everything to everyone is a recipe for mediocrity. To create compelling products, companies need to specialize in building sticky experiences that keep users coming back. To address this, focus on specific domains and use cases. Narrow the scope by going deep rather than wide. This will create domain-specific tools that resonate with users.

**Build LLMOps, but build it for the right reason: faster iteration**

DevOps is about shortening the feedback cycles between work and its outcomes so that improvements accumulate instead of errors. MLOps has adapted the form of DevOps to ML. We have reproducible experiments and we have all-in-one suites that empower model builders to ship.

Hearteningly, the field of LLMOps has shifted away from thinking about hobgoblins of little minds like prompt management and towards the hard problems that block iteration: production monitoring and continual improvement, linked by evaluation.

**Don’t Build LLM Features You Can Buy**

Most successful businesses are not LLM businesses. Simultaneously, most businesses have opportunities to be improved by LLMs.

This pair of observations often mislead leaders into hastily retrofitting systems with LLMs at increased cost and decreased quality and releasing them as ersatz, vanity “AI” features, complete with the now-dreaded sparkle icon. There’s a better way: focus on LLM applications that truly align with your product goals and enhance your core operations.

**AI in the loop; Humans at the center**

Right now, LLM-powered applications are brittle. They required an incredible amount of safe-guarding and defensive engineering, yet remain hard to predict. Additionally, when tightly scoped these applications can be wildly useful. This means that LLMs make excellent tools to accelerate user workflows.

While it may be tempting to imagine LLM-based applications fully replacing a workflow, or standing in for a job function, today the most effective paradigm is a human-computer centaur (Centaur chess). When capable humans are paired with LLM capabilities tuned for their rapid utilization, productivity and happiness doing tasks can be massively increased.

### Start with prompting, evals, and data collection

Over the past year, we’ve seen enough to be confident that successful LLM applications follow a consistent trajectory. We walk through this basic “getting started” playbook in this section. The core idea is to start simple and only add complexity as needed. A decent rule of thumb is that each level of sophistication typically requires at least an order of magnitude more effort than the one before it. With this in mind…

**Prompt engineering comes first**

Start with prompt engineering. Use all the techniques we discussed in the tactics section before. Chain-of-thought, n-shot examples, and structured input and output are almost always a good idea. Prototype with the most highly capable models before trying to squeeze performance out of weaker models.

Only if prompt engineering cannot achieve the desired level of performance should you consider finetuning.

**Build evals and kickstart a data flywheel**

Even teams that are just getting started need evals. Otherwise, you won’t know whether your prompt engineering is sufficient or when your finetuned model is ready to replace the base model.

Effective evals are specific to your tasks and mirror the intended use cases. The first level of evals that we recommend is unit testing. These simple assertions detect known or hypothesized failure modes and help drive early design decisions.

### The high-level trend of low-cost cognition

In the four years since the launch of OpenAI’s davinci model as an API, the cost of running a model with equivalent performance on that task at the scale of one million tokens (about one hundred copies of this document) has dropped from $20 to less than 10¢ – a halving time of just six months. Similarly, the cost to run Meta’s LLaMA 3 8B, via an API provider or on your own, is just 20¢ per million tokens as of May of 2024, and it has similar performance to OpenAI’s text-davinci-003, the model that enabled ChatGPT. That model also cost about $20 per million tokens when it was released in late November of 2023. That’s two orders of magnitude in just 18 months – the same timeframe in which Moore’s Law predicts a mere doubling.
