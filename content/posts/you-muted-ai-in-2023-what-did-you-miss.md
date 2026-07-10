---
title: "You Muted AI in 2023. What Did You Miss?"
date: 2026-07-10T19:40:00+12:00
draft: false
description: "A practical catch-up guide for anyone who muted AI in early 2023 and has just returned to a world of reasoning models, coding agents, AI search and synthetic video."
slug: "you-muted-ai-in-2023-what-did-you-miss"
tags:
  - AI
  - ChatGPT
  - Claude
  - software development
  - technology
categories:
  - Artificial intelligence
keywords:
  - AI developments since 2023
  - what happened in AI 2023 to 2026
  - AI catch-up guide
  - AI agents explained
  - AI coding tools
  - what did I miss in AI
---

Earlier today, [Kent C. Dodds posted a thought on X](https://x.com/kentcdodds/status/2075412225536504259):

> Imagine being someone who muted "AI" early on when some people thought it was an annoying "topic of the week" that would pass, but they forgot to unmute it. They must live in a completely different world 😅

He is right that they would. The thought stuck with me, because I could not immediately work out what I would tell that person if they asked.

So imagine you muted the word "AI" in early 2023. ChatGPT had become popular, but you assumed it was another tech-industry obsession that would settle down after a few months. You then avoided every model launch, benchmark graph, job-loss prediction, generated image and argument about whether a chatbot was alive.

Today you open your muted-words settings by accident and turn AI back on.

What did you miss?

## The extremely short version

In 2023, most people understood generative AI as a website where you typed a question and received a suspiciously confident block of text.

By 2026, the same broad technology can inspect images, listen and speak in real time, read large collections of documents, search the web, operate software, edit files, run terminal commands, write and test code, generate convincing images and video, and work through multi-step tasks with less supervision.

The important change is not that the chatbot became a better conversationalist. It is that the chat box grew eyes, ears, memory and hands.

It still makes things up, still misunderstands instructions, and still needs human judgment when the consequences matter. What changed is that it can now be wrong while doing something, rather than merely wrong in a paragraph. That is most of the last three years in one mildly alarming sentence.

## What AI looked like when you muted it

ChatGPT arrived in November 2022 and turned a field of machine-learning research into a product that almost anyone could understand. The interface required no programming, specialist vocabulary or expensive hardware. You wrote something and it wrote back.

The results were startling, then quickly annoying. ChatGPT could explain code, draft emails, imitate styles and produce school essays. It could also fabricate books, court cases, quotations and historical events while sounding completely relaxed about it.

[GPT-4 arrived in March 2023](https://openai.com/index/gpt-4-research/). It followed complicated instructions more reliably than GPT-3.5 and was designed to accept images as well as text, although its visual input was initially restricted. OpenAI's own announcement placed the model around the top ten percent on a simulated bar exam while warning that it still hallucinated facts and made reasoning errors.

That was the tone of 2023: extraordinary benchmark results paired with a model that might invent the source it had just recommended.

The year also established that this would not be one company's product. Anthropic released Claude. Google launched Bard, which it would later rename and rebuild around Gemini. Meta released [Llama 2 for research and commercial use](https://about.fb.com/news/2023/07/llama-2/), helping create a large ecosystem of models that people could download, modify and run outside the biggest AI services.

If you muted AI at that point, your mental picture is probably a text generator with an impressive demo and questionable reliability.

That picture is now badly out of date.

## 2024: the chatbot acquired senses and a longer memory

Several changes arrived at once in 2024.

### Multimodal stopped being a demo

AI systems became much better at moving between text, images and sound.

[GPT-4o launched in May 2024](https://openai.com/index/hello-gpt-4o/) with text, vision and audio handled within one model. It could respond to speech at roughly conversational speed, interpret a camera feed and talk with changes in tone. Similar multimodal abilities spread through competing products.

Instead of transcribing audio into text, sending that text through a language model and converting the answer back to speech, a single model could reason across the signals directly. The interaction felt less like sending a message to a web form and more like talking to something present in the room.

The limitations were still obvious. It could misread an image, interrupt at the wrong time or confidently describe an object that was not there. Yet voice and vision changed which tasks felt natural. You could point a phone at a machine, diagram, menu or broken appliance and ask about what it saw.

### Context windows became enormous

The context window is the amount of information a model can consider during one interaction. Early ChatGPT conversations could lose track of details after a modest amount of text.

In February 2024, [Google announced Gemini 1.5 Pro](https://blog.google/innovation-and-ai/products/google-gemini-next-generation-model-february-2024/) with a standard context window of 128,000 tokens, and up to one million tokens for a restricted group of developers in private preview. Google demonstrated it processing long documents, hours of audio, video and large codebases. The million-token headline was a preview, not something you could go and use that week, which is worth remembering whenever a context-window record gets announced.

A large context window is not human memory, and stuffing more information into a prompt does not guarantee understanding. It did, however, change the scale of material people could hand to an AI. A contract, book, meeting archive or repository no longer had to be squeezed into a few selected paragraphs before the model could begin.

This helped move AI from party trick to work tool. The valuable prompt became less like "write an essay about contracts" and more like "compare these six contracts, identify every difference in the termination clauses and link each finding to the source text."

### Models learned to spend more time on hard problems

Most early chatbots generated answers immediately, one token after another. If the problem needed planning, checking or several attempts, they often committed to the first plausible route and continued confidently toward the wrong answer.

[OpenAI's o1 preview in September 2024](https://openai.com/index/introducing-openai-o1-preview/) popularised a different approach. The model was trained to spend additional computation working through difficult problems before returning an answer. These systems became known as reasoning models.

The label should not be mistaken for a claim of human thought or consciousness. In practical terms, the model uses more time and computation to explore and refine a solution. This made a noticeable difference in mathematics, science and programming, while often being unnecessary for simple questions.

The old question was, "Which model is smartest?" The new question became, "How much time and computation should this task receive?"

### AI gained a common plug shape

Language models are much less useful when trapped inside an empty chat window. They need controlled access to files, databases, search engines and software.

In November 2024, Anthropic introduced the [Model Context Protocol](https://www.anthropic.com/news/model-context-protocol), usually called MCP. It is an open standard for connecting AI applications to tools and data sources. People often describe it as USB-C for AI, which is not technically complete but conveys the idea.

Before MCP, every assistant and data source needed a custom integration. A common protocol made it easier for different clients to connect to GitHub, Slack, Google Drive, databases and local tools. The acronym began appearing everywhere in 2025, so you should learn it now and save yourself several confused searches later.

## 2025: the chatbot acquired tools and started doing the task

The fashionable word of 2025 was "agent."

An agent is an AI system allowed to take a sequence of actions toward a goal. It might search the web, open several pages, change its plan, run code, inspect the result and continue until it has produced something useful.

This was not a completely new idea. Developers had been building tool-using loops around language models for years. What changed was reliability, product design and access. Agentic features moved into mainstream products.

### Research became a task you could delegate

[Deep research tools](https://openai.com/index/introducing-deep-research/) began taking a broad question, searching many sources and returning a cited report after several minutes. Similar systems appeared across the industry.

The experience differs from asking an ordinary chatbot what it already knows. A research agent can decide which information it needs, look for it, inspect PDFs and web pages, notice a missing detail and search again.

This is helpful, but the citations still need checking. A report with 40 links can contain a claim that is not supported by the link beside it. When I went looking for [the base plate dimensions of my router]({{< ref "bosch-pof-1200-ae-dimensions-and-specs.md" >}}), I got two confidently cited answers that contradicted each other, and one of them contradicted itself three ways inside a single bullet list. Automation made serious research faster without making source criticism optional.

### Coding moved from autocomplete to delegation

Software development became one of the clearest demonstrations of the change.

The first generation of AI coding products completed the next line or generated a function inside an editor. By 2025, coding agents could inspect a repository, edit many files, run the tests, read the failures, try again and present a patch for review.

[Claude Code](https://claude.com/product/claude-code) brought an agent into the terminal and code editor. [OpenAI's Codex](https://openai.com/index/introducing-codex/) offered cloud environments where separate agents could work on tasks in parallel. Other products pursued the same idea with different interfaces and levels of autonomy.

This did not eliminate software engineering. It changed where some of the effort went. Developers spent less time typing routine code and more time defining the task, supplying the right context, reviewing changes, designing tests and noticing when the machine solved the wrong problem elegantly.

The new failure mode was not just bad generated code. It was a large, internally consistent change that passed weak tests and quietly misunderstood the product.

### Open models got much more competitive

Open-weight models continued improving. These are models whose trained weights can be downloaded, although their training data and complete development process may not be open.

The release of [DeepSeek-R1 in January 2025](https://arxiv.org/abs/2501.12948) drew attention because it offered strong reasoning performance and published downloadable models and distilled variants. It reinforced a point already visible through Llama, Mistral, Qwen and others: advanced AI was not going to exist only behind a handful of American subscription websites.

This mattered for cost, research, local use, national strategy and privacy. A company could run a capable model on its own infrastructure. An enthusiast could run a smaller one on a laptop. Developers could fine-tune models for narrow tasks without sending every request to the same provider.

"Open source" remained a contested label because licences, training data and disclosure varied. "Open weights" is usually the safer description.

### Images stopped looking like melted dreams

AI image generators existed well before 2023, but they improved rapidly. Hands became less monstrous. Text inside images became more reliable. Editing an existing image through conversation became easier than endlessly rewriting a prompt.

By March 2025, [native image generation inside GPT-4o](https://openai.com/index/introducing-4o-image-generation/) could follow detailed instructions, render useful text and revise an image over several turns. Competing systems made similar progress.

Video advanced too. Models could generate increasingly coherent clips, extend scenes and follow camera directions. [Google's 2025 generative-media models](https://blog.google/innovation-and-ai/products/generative-media-models-io-2025/) added video with sound effects and dialogue. A person with no camera crew, actors or animation software could create a plausible advertisement or imaginary news clip.

This was exciting for small creators and extremely convenient for scammers. By 2026, "I saw the video" was no longer strong evidence that an event happened.

### Search began answering instead of listing

Search engines had already displayed snippets and knowledge panels, but generative AI changed the centre of the page.

Google expanded AI Overviews and introduced [AI Mode in 2025](https://blog.google/products-and-platforms/products/search/ai-mode-search/), allowing multi-part questions and follow-ups within Search. ChatGPT, Perplexity, Claude and other products offered their own combinations of web search and generated answers.

This changed behaviour for users and economics for publishers. A person could ask one complicated question instead of opening ten blue links. The answer might still cite those ten sites, but fewer people needed to visit them.

The web did not disappear. The route through it changed.

## By 2026, the interface is shifting from answers to outcomes

The most useful way to understand AI in 2026 is to stop thinking about a model as an oracle.

It is closer to an unreliable but extremely fast computer operator. It can read what is on the screen, use tools, manipulate files and produce a result. You can ask for a spreadsheet, presentation, website, research report, edited image or pull request rather than asking for instructions to make one yourself.

This is still uneven. A ten-minute task may complete perfectly. A two-minute task may spiral into an absurd detour. Giving an agent broader access makes it more helpful and increases the damage it can cause through a misunderstanding.

The mature workflow therefore looks less like blind automation and more like supervised delegation:

1. Define the outcome and constraints.
2. Give the system the necessary context and tools.
3. Let it work in a limited environment.
4. Inspect its evidence, changes and output.
5. Correct it or approve the result.

The best users are not necessarily the people with the cleverest prompts. They are the ones who can describe good work, recognise bad work and design a process in which mistakes are visible and reversible.

## Prompt engineering did not become a permanent profession for everyone

In 2023, social media filled with magic phrases that supposedly unlocked extraordinary results. People sold collections of prompts containing commands such as "act as a world-class expert" and "take a deep breath."

Clear instructions still matter. Examples still help. Asking for a specific format still produces a better result than asking the model to "make it good."

The emphasis has moved toward context engineering. That means giving the system the right documents, tools, permissions, examples, constraints and feedback. A beautiful prompt cannot compensate for missing source material or a test suite that checks the wrong behaviour.

The model also does more of the prompt work itself. Modern products can ask clarifying questions, plan a task and choose tools. The valuable human skill is increasingly judgment rather than incantation.

## The models became dramatically cheaper

Frontier models remained expensive to train, requiring huge clusters of specialised chips and substantial energy. Using capable models became much cheaper.

The [Stanford 2025 AI Index](https://hai.stanford.edu/ai-index/2025-ai-index-report) estimated that the inference cost of a system performing at GPT-3.5 level fell more than 280-fold between November 2022 and October 2024. Smaller models became better, hardware improved and competition pushed prices down.

This is one reason AI appeared in every product. A feature that was financially ridiculous in 2023 could be cheap enough to give away a year later.

Falling cost also encouraged waste. Companies added AI summaries where a sentence written by a human would have been clearer, and products generated material nobody wanted because generation itself was cheap. "Can we add AI?" was answered much more often than "Should we?" I have written about [what it takes to use this stuff without producing sludge]({{< ref "building-a-content-farm-in-2026.md" >}}), and the short version is that the cost of making something has collapsed while the cost of it being worth reading has not moved.

## Work adopted AI faster than work understood it

The same Stanford report said 78 percent of surveyed organisations used AI in 2024, up from 55 percent in 2023. Adoption did not mean every deployment was successful or even well considered. It meant AI had moved from a research project to a normal budget item.

Some employees used approved enterprise systems. Others quietly pasted company material into consumer chatbots because it saved time. Organisations rushed to write policies after discovering that their unofficial policy was "everyone is already doing it."

The productivity effects were real for some tasks and occupations, especially drafting, translation, support, analysis and software work. They were inconsistent across people and workflows. A skilled user with a well-defined task could save hours. An inexperienced user could save ten minutes and create two hours of review work for somebody else.

The important divide was not simply between companies with AI and companies without it. It was between those that redesigned a process around the technology and those that bought licences while keeping every old process intact.

## Governments noticed

In 2023, arguments about AI regulation often sounded theoretical. By 2026, laws, standards and court cases were affecting product decisions.

The [European Union's AI Act](https://digital-strategy.ec.europa.eu/en/policies/regulatory-framework-ai) entered into force on 1 August 2024, and then arrived in stages. The bans on prohibited practices and the AI literacy obligations applied from 2 February 2025. Obligations for general-purpose AI models followed on 2 August 2025. The regulation becomes fully applicable on 2 August 2026, which is about three weeks after I am writing this, while some high-risk categories run on until 2027 and 2028.

Other jurisdictions developed their own mixtures of sector rules, executive action, safety requirements and disclosure laws. Copyright cases continued testing whether training on protected works was lawful and when generated outputs infringed existing material.

Companies also faced a practical authenticity problem. Synthetic voices, cloned faces and generated video made fraud easier. Schools, newsrooms, employers and courts had to decide what evidence to trust and whether AI use needed to be disclosed.

The technology moved faster than the shared rules around it. That has not changed.

## What did not get fixed

Three years of progress did not turn language models into reliable databases or independent adults.

### They still hallucinate

Models are better at factual work when they can search, cite sources and check tools. They can still invent a detail, misunderstand a source or attach a real citation to a claim the page does not support.

Fluency remains easy to mistake for knowledge. The more polished the answer, the more important it is to remember that confidence is part of the writing style, not a calibrated guarantee.

### Agents make ordinary mistakes at computer speed

An agent can delete the wrong file, send the wrong message, alter unrelated code or follow malicious instructions hidden inside a web page. This last problem is called prompt injection. Giving an AI access to tools exposes a new security boundary because untrusted content can try to instruct the model controlling those tools.

Permissions, sandboxes, logs, backups and human approval are boring. They are also what separates a useful agent from a fast-moving incident.

### Long context is not perfect memory

A model may accept a whole book and still miss the paragraph that matters. Large context windows reduce the need to chop up information, but they do not guarantee accurate retrieval or reasoning.

Persistent memory features create another problem: the system can remember something wrong, private or no longer relevant.

### Benchmarks are not your job

A new model can lead a coding benchmark and perform worse on the old spreadsheet you actually need fixed. Vendors optimise against public tests, tests become saturated and small scoring differences may have no practical meaning.

The useful evaluation is a set of representative tasks from your own work, checked with your own quality standards.

### Nobody agreed what it all means for employment

AI automated parts of jobs, changed expectations and reduced the value of some routine output. It also created new work and made ambitious projects possible for smaller teams.

Predictions ranged from almost no economic effect to the immediate disappearance of most office jobs. Reality moved unevenly. Tasks changed faster than whole occupations, and organisational adoption remained messier than product demonstrations suggested.

Anyone claiming certainty was selling something, including certainty.

## A tiny 2026 AI glossary

If you have just unmuted the topic, these are the terms most likely to appear before lunch:

- **LLM:** Large language model. The type of model behind systems such as ChatGPT and Claude.
- **Multimodal:** Able to work across text, images, audio or video rather than text alone.
- **Token:** A unit used to process text and other inputs. Cost and context limits are often measured in tokens.
- **Context window:** The information a model can consider during a request or conversation.
- **Reasoning model:** A model that spends extra computation working through difficult tasks before answering.
- **Agent:** A model operating in a loop with tools so it can take several actions toward a goal.
- **RAG:** Retrieval-augmented generation. Supplying a model with relevant external information before it answers.
- **MCP:** Model Context Protocol, an open standard for connecting AI applications to tools and data.
- **Open weights:** Model parameters that can be downloaded and run by others, subject to a licence.
- **Vibe coding:** Building software by describing the desired result to AI and steering the generated implementation, sometimes without understanding much of the code.

You now know enough vocabulary to misunderstand a conference panel at roughly the same level as everybody else.

## How to catch up without turning your feed into AI soup

You do not need to read three years of launch posts. Model names and benchmark leaders change too quickly for memorising them to be useful.

Try a few current capabilities with real tasks instead:

1. Give an assistant a long document you understand. Ask it to extract something specific and cite the relevant passages. Check the result.
2. Ask a research tool a current question that requires several sources. Follow the citations and see where it cut corners.
3. Use voice and a camera to discuss an object, diagram or room. Notice both the speed and the mistakes.
4. Let a coding agent make a small change in a disposable repository with good tests. Review every file it touches.
5. Generate an image containing several precise elements and readable text, then revise it conversationally.
6. Connect an agent to one low-risk tool and watch what it does before granting wider access.

Do not begin with your medical history, production database, private company strategy or the only copy of a family photo archive.

Pick one recurring annoyance from your actual life and test whether AI improves it. Summarising every meeting may be useful. Generating 40 versions of every email may not be. The goal is not to become an AI person. It is to learn where the tools are worth the trouble.

## Were the people who kept AI muted left behind?

Less than you might expect. Following every AI release for three years did not make a person wise. It may only have taught them the names of 70 discontinued models and given them strong opinions about benchmark contamination.

What the people who kept watching actually gained was not intelligence. It was the loss of surprise. They used the tools, so they learned which tasks worked, when to distrust an answer, and how quickly yesterday's limitation might disappear. That intuition cannot be reconstructed by scrolling one timeline, and it is the part of the last three years that was genuinely worth accumulating.

The tools also got much easier to use, which works in your favour. There was no secret three-year training course, and you can open a current assistant today and start with a real problem.

The world on the other side of the mute button is stranger, noisier and more useful than it was in 2023, and it is also full of exaggerated claims, avoidable mistakes, and products whose only feature is that somebody added a sparkle icon. You missed a great deal. Most of it was noise, and the part that was not is still sitting there, waiting for you to try it.

## Sources and further reading

- [Kent C. Dodds' original X post](https://x.com/kentcdodds/status/2075412225536504259), 10 July 2026.
- [GPT-4](https://openai.com/index/gpt-4-research/), OpenAI, March 2023.
- [Llama 2](https://about.fb.com/news/2023/07/llama-2/), Meta, July 2023.
- [Gemini 1.5 and long-context models](https://blog.google/innovation-and-ai/products/google-gemini-next-generation-model-february-2024/), Google, February 2024.
- [GPT-4o and real-time multimodal interaction](https://openai.com/index/hello-gpt-4o/), OpenAI, May 2024.
- [OpenAI o1 reasoning models](https://openai.com/index/introducing-openai-o1-preview/), OpenAI, September 2024.
- [Model Context Protocol](https://www.anthropic.com/news/model-context-protocol), Anthropic, November 2024.
- [DeepSeek-R1 paper](https://arxiv.org/abs/2501.12948), DeepSeek-AI, January 2025.
- [Deep research](https://openai.com/index/introducing-deep-research/), OpenAI, February 2025.
- [Native image generation in GPT-4o](https://openai.com/index/introducing-4o-image-generation/), OpenAI, March 2025.
- [Generative video, image and music models](https://blog.google/innovation-and-ai/products/generative-media-models-io-2025/), Google, May 2025.
- [Claude Code](https://claude.com/product/claude-code), Anthropic.
- [Codex](https://openai.com/index/introducing-codex/), OpenAI, May 2025.
- [AI Overviews and AI Mode](https://blog.google/products-and-platforms/products/search/ai-mode-search/), Google, March 2025.
- [The 2025 AI Index Report](https://hai.stanford.edu/ai-index/2025-ai-index-report), Stanford Institute for Human-Centered AI.
- [EU AI Act overview and implementation timeline](https://digital-strategy.ec.europa.eu/en/policies/regulatory-framework-ai), European Commission.
