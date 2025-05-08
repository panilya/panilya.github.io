+++
author = "Illia Pantsyr"
title = "The great AI prompt arms race"
date = "2025-05-08"
tags = [ "llm", "ai" ]
+++

Remember when talking to AI was simple? You asked a question, it gave an answer. Those days vanished sometime in early 2023, replaced by an arms race of increasingly elaborate prompt engineering techniques. Today's LinkedIn is flooded with self-proclaimed "prompt engineers" sharing their proprietary formulas - meticulously crafted instructions wrapped in specific XML tags, prefaced with elaborate personas, and peppered with carefully positioned examples.

The irony is palpable: as prompt engineering has grown more complex, it has simultaneously become both more important and less special. When OpenAI's Sam Altman [described prompt engineering as "an amazingly high-leverage skill",](https://x.com/sama/status/1627796054040285184?lang=en) he wasn't wrong - but what he didn't mention was how overcrowded the field would become.

We've reached the paradoxical moment where everyone claims prompt mastery, yet results remain wildly inconsistent. Companies hire dedicated prompt engineers at eye-watering salaries—with positions at firms like [Anthropic reportedly offering](https://www.yahoo.com/news/ai-prompt-engineer-jobs-pay-201001072.html) between \$280,000 and \$375,000—while Twitter gurus insist their 🧵THREAD: ULTIMATE PROMPT TEMPLATE🧵 is the only one you'll ever need. Meanwhile, models improve rapidly, often rendering yesterday's elaborate prompting techniques unnecessary or even counterproductive.

This isn't just academic—it's a genuine problem. When everyone is an "expert," how do you separate signal from noise? When every YouTuber has "THE ONLY PROMPT YOU NEED," who do you believe? And as models become more capable, is the entire premise of prompt expertise slowly evaporating beneath our feet? Ironically, even Sam Altman himself expressed skepticism about the longevity of prompt engineering, [stating in a 2022 interview that "I don't think we'll still be doing prompt engineering in five years"](https://www.thealgorithmicbridge.com/p/prompt-engineering-is-probably-more).

Let's explore how we got here, why the current prompt engineering landscape resembles the Wild West, and what might actually matter in the future of human-AI interaction beyond crafting the perfect system message.

## The Rise of the Prompt Engineer

The term "prompt engineer" barely existed before 2023. Now it's plastered across job boards, Twitter bios, and LinkedIn profiles with the same frequency as "blockchain expert" circa 2018. But how did we get here?

In the early days of ChatGPT, the barrier to entry was refreshingly low. People discovered that simply asking "explain this like I'm five" or "write this in Shakespeare's style" yielded surprisingly good results. These basic techniques spread organically, creating a moment of collective discovery as millions experimented with this new technology.

Then came the watershed moment: leaked prompts from advanced ChatGPT users revealed the power of elaborate system instructions. Suddenly, everyone learned you could tell the AI to "think step by step" or "you are an expert in X" to dramatically improve outputs. What followed was inevitable - an explosion of increasingly specific persona-based prompts:

"You are a world-class Solidity developer with 15 years of experience who specializes in gas optimization and always includes detailed comments..."

The internet did what it does best: it commodified and commercialized. Prompt marketplaces emerged overnight. [PromptBase](https://promptbase.com), now featuring over 190,000 curated AI prompts, began selling "premium prompts" for $5-50 each. Notion templates promising "500+ Ultimate ChatGPT Prompts" proliferated. YouTube thumbnails screamed about "SECRET PROMPT TECHNIQUES OPENAI DOESN'T WANT YOU TO KNOW!"

Meanwhile, corporate America caught prompt fever. According to [Coursera's 2025 guide]( https://www.coursera.org/articles/prompt-engineering-salary), prompt engineers can earn salaries as high as six figures. Job listings for "AI Prompt Engineers" appeared with salaries ranging from \$80,000 to over \$300,000. Consulting firms rebranded existing employees as "prompt specialists" and charged premium rates for their services.

The gold rush reached its peak with the emergence of prompt formatting techniques. First came the "I'll tip $200" trend, where users discovered that promising (fictional) monetary rewards sometimes improved responses. As noted in [research by SuperAnnotate](https://www.superannotate.com/blog/llm-prompting-tricks), "a \$200 tip will most probably motivate the model more than a \$20 tip, according to statistical observations". Then came the XML tag explosion - <thinking>, <reasoning>, <confidence=high> - as users tried to control AI outputs with increasingly elaborate syntax that sometimes worked and sometimes didn't. [Anthropic's own documentation recommends](https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/use-xml-tags) using tags like \<instructions\>, \<example\>, and \<formatting\> to "clearly separate different parts of your prompt".

What started as a genuine skill exploration had transformed into something between an art form and snake oil sales. Everyone became an expert overnight, and the internet became flooded with contradictory advice. The prompt engineer had risen, but as with any rapidly emerging field, the signal-to-noise ratio was plummeting fast.

## When Everyone Has the Secret Recipe...

The democratization of prompt techniques has created a peculiar problem: when everyone knows the "secret sauce," it stops being secret and starts being... sauce.

Remember when adding "let's think step by step" to your prompts felt like discovering fire? That technique spread so rapidly that model providers actually incorporated it into their base models. The same happened with Chain of Thought prompting, few-shot examples, and most recently, the explosion of XML tags. What was once the domain of AI researchers became common knowledge virtually overnight.

This commoditization has led to diminishing returns. When Claude or GPT-4o receives its thousandth prompt beginning with "You are an expert in..." or "I will tip $50 if you do a good job," these signals likely lose their effectiveness. The models either adapt or their creators explicitly tune them to ignore such manipulation attempts. The half-life of a truly novel prompting technique has shortened from months to mere weeks.

More frustrating still is how model updates regularly invalidate what prompt engineers spend weeks perfecting. A carefully crafted prompt working flawlessly on GPT-4o might suddenly break after a quiet backend update. The prompt that coaxed perfect code from GPT-4o might fall flat on GPT-4.1. This constant instability means today's prompt engineering expertise has the shelf life of fresh sushi.

Perhaps most troubling is the widening gulf between simple and complex prompts. We've created a bizarre landscape where sometimes a straightforward question yields better results than an elaborately engineered prompt. As noted in [research on prompt complexity](https://saasprompts.com/simple-vs-complex-prompt-engineering), "simple prompts often lead to clearer responses, making it easier for you to guide the conversation effectively". Users report spending hours fine-tuning a complex prompt only to discover that asking directly works better. This unpredictability leads many to wonder: are we overthinking this? Is elaborate prompt engineering sometimes just superstition dressed as expertise?

When everyone has the secret recipe, it seems we're all just making slightly different versions of the same dish—and sometimes discovering that a simpler recipe tastes better after all.

## The Signal-to-Noise Problem

If you've ever searched for "best ChatGPT prompts" or "how to write better prompts," you've encountered the prompt engineering ecosystem's biggest challenge: information overload. What began as helpful knowledge sharing has became a cacophony of contradictory advice, template collections, and "ultimate guides" that leave users more confused than enlightened.

Open any social media platform and you'll find an overwhelming array of content: LinkedIn influencers sharing their "10x prompt frameworks," Twitter threads promising "the ChatGPT secret they don't teach you," and countless YouTube videos with increasingly hyperbolic titles. The problem isn't just volume—it's that much of this content contradicts itself. One expert insists on third-person instructions, while another swears by first-person. Someone claims XML tags are essential, while others dismiss them as placebo.

This environment breeds what I call "cargo cult prompt engineering"—copying techniques without understanding why they work (or if they work at all). This parallels the established concept of ["cargo cult programming," which Wikipedia defines](https://en.wikipedia.org/wiki/Cargo_cult_programming) as programming "characterized by the ritual inclusion of code or program structures that serve no real purpose". Users religiously include phrases like "you will be penalized if you don't follow these instructions" or "answer in the style of a Pulitzer Prize winner" without testing whether these actually improve outputs. They faithfully reproduce templates with specific formatting, believing the magic lies in the structure rather than the clarity of instruction.

The noise drowns out genuinely useful innovations in prompt techniques. When everyone claims their method is revolutionary, users become skeptical of all methods—even the ones with legitimate research backing. Like the boy who cried wolf, the prompt engineering community has shouted "breakthrough!" so many times that true advancements struggle to be heard above the din.

As one person put it: "I spent more time reading about prompt engineering than actually using the AI, only to discover that half the techniques I learned were obsolete and the other half were just common sense dressed up in jargon."

## The Real Expertise No One Talks About

While everyone's busy debating whether to use angle brackets or curly braces in their prompts, a different kind of expertise is quietly proving far more valuable—and it's not what the YouTube thumbnails are selling.

The uncomfortable truth is that domain knowledge still trumps prompt wizardry every time. [Research confirms that](https://blog.andovar.com/the-role-of-domain-knowledge-in-effective-prompt-engineering) "domain knowledge is essential for ensuring that AI systems generate accurate outputs". The financial analyst who understands balance sheets will always get better AI-generated financial insights than the prompt engineer who doesn't know EBITDA from EBIT. The most elaborate legal prompt can't compensate for not understanding the difference between tort and contract law. This reality contradicts the tempting narrative that clever prompting can substitute for years of specialized knowledge.

The most sophisticated AI users I know rarely talk about their prompts. Instead, they focus on building systems around AI capabilities. They're creating evaluation frameworks to test output quality. They're implementing human-in-the-loop processes where AI suggestions get expert review. They're designing fallback mechanisms for when AI inevitably fails. This systems thinking—not prompt crafting—is where the real intellectual challenge lies.

The often-ignored reality is that the best prompt in the world can't fix fundamental limitations in what AI can reliably do. Understanding these limitations—and building guardrails around them—is the real expertise that delivers value. It's less glamorous than claiming you've discovered the perfect prompt template, but it's infinitely more valuable.

## Conclusion

The prompt engineering bubble isn't just leaking air—it's actively deflating. As models become more capable and interfaces more intuitive, the obsessive focus on prompt construction feels increasingly like optimizing horse carriages at the dawn of automobiles. This deflation is healthy and necessary.

The future isn't about crafting the perfect prompt in isolation—it's about thoughtfully embedding AI capabilities into workflows where they genuinely add value. It's about designing systems that combine machine strengths with human expertise rather than treating AI as a magical oracle that needs the right incantation.

The most forward-thinking organizations are already pivoting. They're investing less in prompt libraries and more in comprehensive evaluation frameworks. They're moving from "how do we get the best prompt?" to "how do we measure when AI is helping versus hindering?" They're building guardrails, feedback mechanisms, and human-AI collaboration patterns that acknowledge both the power and limitations of these systems.

As for individual practitioners, the advice becomes simpler yet more demanding: develop deep knowledge in your domain. The lawyer who understands both law and AI limitations will outperform the prompt engineer who only knows AI. The programmer who can code both traditionally and with AI assistance will outpace those who can only do one. The writer who understands narrative structure will craft better AI writing prompts than someone who only knows prompt patterns.

In the near future, we'll likely see advanced AI interfaces that eliminate much of what we now call prompt engineering. They'll interpret natural requests more effectively, ask clarifying questions when needed, and maintain useful context without elaborate instructions. The current complexity of prompting may soon seem as outdated as having to know DOS commands to use a computer.

The real question isn't "what's the perfect prompt?" but rather "what's the right relationship between humans and AI?" And that question requires more than clever text engineering—it demands thoughtful consideration of how these technologies best serve human needs and augment human capabilities.

Perhaps the most valuable prompt of all is the one we should be asking ourselves: Are we using AI to solve real problems, or are we just fascinated by our own cleverness in talking to machines?
