---
title: DeepSeek-R1, Providing a New Meta of AI Model
description: "After reading \"DeepSeek-R1: Incentivizing Reasoning Capability in LLMs via Reinforcement Learning\""
slug: review-deepseek-r1
date: 2025-10-25 00:00:00+0000
image: # deepseek.png
categories:
    - all-posts
tags:
    - Paper
weight: 1       # You can add weight to some posts to override the default sorting (date descending)
---

![DeepSeek-AI Logo](deepseek.png)  

This post discusses about DeepSeek-AI's paper which reveals their iconic reasoning models. The paper introduces a novel training scheme that makes AI language models (LLMs) learn 'reasoning' abilities (e.g., solving complex mathematical problems, coding) on their own. 

The core model is DeepSeek-R1, and the main training method is Reinforcement Learning (RL). Reinforcement learning is a trial-and-error learning method that allows AI to learn better solutions on its own, by giving 'reward' for correct answers and 'penalty' for wrong answers when AI solves a problem. The question is that "How does AI make 'thinking' and 'inferring' complex problems?" It refers to the ability to logically find answers across multiple steps, not just by memorizing knowledge. OpenAI demonstrated this reasoning ability with a model called 'o1', but it kept it a secret how it made it. DeepSeek-R1 is a paper that presents DeepSeek-AI's own answer to the secret. The research team created and tested two versions of the model.
  
## First try: DeepSeek-R1-Zero (taming wild horses)
The Idea is like this.
> Should we not show any examples of correct answers (SFT) to AI, just let them solve the questions (mathematics, coding) and then reward them with 'Answer!' or 'Deng!' to learn reasoning on their own?
  
Only Reinforcement Learning (RL) was applied to this basic model (DeepSeek-V3-Base) that taught nothing. In other words, they did not show examples of correct answers (SFT data) in advance, and they let them solve problems (mathematics, coding, etc.) on their own and only rewarded them according to the correct answers. The metaphor is to give a baby a block, say, "Build the tower," and then give him a candy only when the tower is finished. He never taught me how to stack it.
  
The model has started to increase its own 'Thinking Time' (CoT) to increase the percentage of correct answers. At first, they answered briefly, but the more they repeated RL, the longer and more complex they created a solving process. During training, it solved a problem and said to itself, "Wait, wait. "Wait. I found the 'Aha Moment' here. Let's evaluate it again," he said, showing the 'Reflection' behavior of looking back and modifying the solution process. This is an 'inventive ability' that emerged purely through rewards, not taught.
  
Surprisingly, the model has developed its own reasoning skills. For example, the score for a math competition called AIME 2024 went up from 15.6 percent to 71.0 percent. While solving a problem, it even showed an "Aha Moment" in which it corrected itself by saying, "Wait, let's think again."
Results (disadvantages): The inference process is very readable and has a problem of mixing multiple languages
  
The 'wild horse' model proved its reasoning ability itself, with its AIME math score rising from 15.6 percent to 71.0 percent. However, the process of solving was very difficult for humans to read and very confusing, with several languages mixed up.
  

## Second try: DeepSeek-R1 (Creating Elite Students)
Based on R1-Zero's success (RL works) and limitations (bad readability), we're going to create a much more organized and powerful "elite" model. That's DeepSeek-R1. By way of comparison, this time we're going to enroll your baby in a systematic four-step "elite course."
  
The training stage is 4 steps of pipeline.
> **Step one (Cold Start, SFT)** - "Now, this is how blocks stack up one by one."  
It does not start like a 'wild horse.' First, it trains a small, easy-to-read, high-quality example of inference ("Cold Start" data).  
**Step 2 (Integration Reinforcement Learning, RL)** - "Now build a tower by yourself. I'll give you candy if you stack it up well."  
The model who has mastered the basics is asked to solve math/coding problems and gets a 'correct answer' reward. Like in the case of 'R1-Zero', it also adds a 'verbal consistency reward' that gives penalty points for mixing languages.  
**Step 3 (Data Buffet, SFT)**  
Now the model is pretty smart. Let this model solve the problem and only pick the 'answer' solution (Rejection Sampling). With 600,000 top-notch "reasoning answer sheets" and 200,000 "general conversation" data collected in this way, a total of 800,000 "elite textbooks" will be created, and the model will be retrained with these textbooks (SFT).  
**Step 4 (Final Trim, RL)** - "You have to build a tower well and answer questions kindly to really rank first."  
Finally, we tune the final RL by comprehensively evaluating not only the reasoning ability (rules-based reward), but also "how useful" and "how safe" (using reward models) in the general conversation.  
    
Through these four steps, DeepSeek-R1 achieved the highest level of inference performance, equivalent to OpenAI-01-1217. Specifically, it scored a high Elo score of top 96.3% of human participants on CodeForces, a coding competition site, and 90.8% on the MMLU benchmark.
  
## Key findings: "Distilling Knowledge"
In this paper, the 'Distillation' part is as important as the DeepSeek-R1 model itself. The initial problem is that DeepSeek-R1 is incredibly smart, but it's that big and heavy (expensive) that it's hard for anyone to use. So the idea had come up: "What if we let 800,000 'elite textbooks (SFT data)' made in step 3 above be trained on small public models (Small LLM) like Llama or Qwen?"
  
Just like that, they took a high-quality 800k dataset made in steps 2-3 above, and trained small public models. The result was a huge success. For example, "DeepSeek-R1-Distill-Qwen-32B" (32 billion parameter model) achieved 72.6% AIME and 94.3% MATH-500, which far outperforms most large models.
  
Rather than training a small model hard with RL from scratch, it was much more efficient and performing to "distill" the inference method of a large model that had already become smart.
Training the Qwen-32B (32 billion) model with this data, "DeepSeek-R1-Distill-Qwen-32B" achieved 72.6% AIME and 94.3% MATH-500. This is comparable to or better than OpenAI-01-mini.
Even the smaller 14B (14 billion) model outperformed the existing 32B model (QwQ-32B).
  
In conclusion, it is very difficult for a small model to acquire elite-level reasoning skills through RL on its own. However, simply imitating (learning) the "answer-solving process" created by an already smart big model (R1) can quickly and effectively transfer that knowledge.
  
## Real Word Usage
Now let’s discuss the specialy regarding this model in real world. The paper was released in January 2025, and DeepSeek-AI released the model almost simultaneously with the publication of the paper. The most powerful full-version DeepSeek-R1 model can be used by companies or developers at a cost via API. Besides, the 'knowledge distillation' models described above (e.g., DeepSeek-R1-Distill-Qwen-32B, DeepSeek-R1-Distill-Llama-70B, etc.) are ompletely free (Open Source) that developers worldwide have downloaded and used these models.
  
### 'Blackbox' vs 'Glassbox'
Unlike OpenAI, which kept the 'o1' model's training secret ("Blackbox"), DeepSeek transparently revealed their successful training pipelines as well as failed attempts such as 'Process Reward Model (PRM)' and 'Monte Carlo Tree Exploration (MCTS) through this paper. This is considered an approach that contributes significantly to the development of the AI community as a whole.
  
### Consideration of 'Inventive Ability'
The 'Aha Moment' of 'R1-Zero' has become a big topic for AI researchers. This is because simply the reward of 'correct/misanswer' has shown the possibility that AI can learn the complex and high-order thinking ability of 'self-reflection'.
  
People often say that DeepSeek is a much lighter model than ChatGPT. This part is the most interesting point of DeepSeek. When we say "ChatGPT," it usually means giant models like GPT-3.5 and GPT-4/4o, which are the opposite of "lightweight." "DeepSeek has become lighter" refers to open-source models created by "Distilling." Let's elaborate on what this means and why it's efficient.
  
> What does it mean by "lightweight model"?

In AI models, 'weight' is determined by the number of 'parameters (parameters)'.
  
> What are parameters?

This is where the model's 'knowledge' is stored, similar to the **'s 'synapse' of the human brain. The more parameters, the more complex the model is, the more knowledge it can store, and the potential to be smarter.
  
### "Lightweight" = Fewer parameters
ChatGPT (GPT-4/4o), although private, experts estimate that the parameters will be well over a trillion (1 Trillion). This is a 'super' model. In the case of DeepSeek, 7 billion (7B), 14 billion (14B), 32 billion (32B), and 70 billion (70B) lightweight models were unveiled in the DeepSeek-R1 paper.
Just by looking at the numbers, there's a huge difference between 1,000,000,000+ and 7,000,000,000,000+, right? That's why DeepSeek's public models are called 'lightweighted'.
  
> From what perspective do you mean it's efficient?  

Fewer parameters (lightweight) imply tremendous "efficiency" from many perspectives. First, it means cost-effectiveness (the biggest reason). Running AI requires huge amounts of electricity and expensive GPUs (graphic cards). ChatGPT (GPT-4) has to calculate all of the over a trillion parameters to give one answer. It requires hundreds or thousands of top-end GPUs, and it consumes a lot of electricity. On the other hand, DeepSeek only needs to calculate 7 billion. Works with far fewer GPUs, even GPUs for regular high-performance PCs. 
  
The second perspective can be accessibility. This is the core of the open-source model. ChatGPT only works on OpenAI's giant servers. We can only 'borrow' via API. However, DeepSeek, the model file itself is open. Companies and individuals can download it and install it directly on their PC or server.
  
The third criterion is speed. The amount (parameters) to calculate is small, so of course, the response speed (inference speed) is faster. Lastly, the customizing issue can be discussed. ChatGPT should use the completed model as it is. After downloading the model, DeepSeek can learn only our internal documents or specific area (e.g., law, healthcare) data to **easy fine-tuning to 'our company-only AI' and 'law-specialized AI'. The lightweight model is much cheaper to modify, too.
  
## The Secret of "The Costly Rain"
This is where the core of the DeepSeek paper comes in.

> If DeepSeek is a lightweight model, isn't it going to be less efficient?

That's right. Usually, 7 billion (7B) parameter models cannot keep up with the intelligence of 1 trillion (1T) models. 
However, as mentioned above, DeepSeek used the trick of "knowledge distillation." First, it creates a super-sized "teacher" model called "DeepSeek-R1," which may be smarter than GPT-4. (It is not lightweight) Then, 800,000 high-quality "answer courses" (elite textbooks) created by the "teacher" model. It trains the textbook on seven billion "student" models. As a result, the "student" model learns by compressing the reasoning method and knowledge of a trillion "teachers" even though it has seven billion small brains.
  
That's why DeepSeek-R1-Distill models, despite their smaller sizes like 7B, 14B, and 32B, have the "cost-to-cost" performance equivalent to even larger models, outperforming other lightweight models in their class.
  
## Conclusion  
To conclude, DeepSeek showed that AI's "reasoning ability" can be significantly improved with only reinforcement learning (RL) without examples of correct answers (SFT). However, the outputs become messy when only RL is applied, so training on "easy-to-read" high-quality example data ("cold start") first and then applying RL was the best way to capture both readability and performance. The knowledge of this smart large model (DeepSeek-R1) proved that it can "distillate" small models to create small but very powerful inference models.
