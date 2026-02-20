****
Disclaimer
****
This is word vomit, it's completely raw and unpolished. I am a blunt and to the point person in real life, and I'm Australian. There is nothing here that other documents don't provide more coherently and without swearing.
****
 
 An exploration of Deterministic software analysis and ADHD word vomit.

It's started as a hobby purely out of interest, I like the concept of knowing C++... to be part of the wizard community that interacts with hardware on a super granular level (bitshifting!) all the way to the performance capabilities that correct usage of templates can unlock, abstractions and being able to use libraries? Man that's so cool!

I watched videos, I wrote some amateur code and fell down the LLM generated code rabbit hole. In doing that however, I discovered something even if I didn't fully understand memory, or pointers, or arrows in code. I can think really really well at a systems level. For a mechanic that came not just intuitively but felt like coming home. I've spent years thinking at a systems level understanding what is known, unknown, what we can't identify, what behaviours should be exhibited and what shouldn't be.

Briefly for a few days, it felt incredible being able to think at “large” scale and just tell the magic box to make this idea work. The project exploded to 55 files and 2800 LOC in a day or 2. “what does this error mean?”, “what's an lvalue?” The only thing I understood vaguely how to make it work was passing values by reference and sometimes it had to be const. So I did that.

At 2800 LOC I had to do a context reset, I uploaded everything in 1 go in a brand new chat window. After all this is interactive google right? and quality input in = quality input out. What happened though was different to what I expected. Firstly, it wasn't generating quality results anymore. Secondly I had to understand more. Somewhere along the lines the project had grown so large that I couldn't actually understand clearly and conceptually what the code was doing line by line, only an overview of what the systems are meant to do.

Now I had a real fuckin' problem, and truthfully I lost motivation for a while. After all it was just experimenting. 

Then my brain latched onto a new idea. What if right, what if, I've got a gaming PC, what if it can run an LLM at home and I can just make it run for longer on the same code? That'll fix it!

From there, I REALLY latched onto the idea. I had this little 6700XT with 13b quantised models, 7b quantised models and the results sucked. It was slow. It was DUMB. I could not get it to do what I wanted. So I did what any rational person does when they are out of their depth and they need some quality realistic information based and grounded in reality. I consulted reddit.

What happened was I learned about multiple different systems. The most appealing to me was the MIT recursive language model. I could get this baby language model to process over huge amounts of context!! I quickly used an LLM to write up some very messy python scripts and tried it on the MIT research paper itself. After some disappointments I could get the LLM to reason over the whole document! From memory it was something like 20k tokens, and that quantised model had a limit of 8k, I felt like a computer scientist!

I learned what a RAG was and started to blend the RLM and RAG systems together, as the online LLM told me this would be even better! But that was hard and boring. So I lost motivation for that concept. But what if I could get an LLM to reason in steps? Well you need an external scaffold that includes normalisation to prevent context drift and very specific prompting. Alright, now what if we get it to reason over the same step a few times and pick the “best” out of a new context window?



The quality of results actually shot way up! Hell yeah I had created a self correcting LLM! I can solve logic problems more reliably now!

Then I gave it a code generation problem and spoilers it sucked. So next was a code specific GGUF. It was slightly better but honestly it still sucked.  I wanted it to gobble up the game code and spit out stuff to add to it! It just wasn't capable of reasoning over large chunks of code. Turns out code is hard and wildly token heavy. It's closer to the machine than english, what do you mean it's hard???

Now I really went down the rabbit hole. I was researching recursive layers in LLM's, TRM's, latent reasoning space, compression of larger models to fit it onto my 6700XT (lol). Anything to make it work usefully.

I started looking at making external recursion systems more seriously. I came up with a novel and amusing tree concept, the root of the tree is all the context required and then everything is split up mostly binarily or conceptually until there's enough reduction in scope for an LLM to reason over each component and then rebuild the whole truth slowly.

This was the true roots of ProofPath, and while I didn't even start to implement the tree concept due to chasing dopamine on ChatGPT, who knew you could literally burn tonnes of coal on your keyboard deeply exploring different concepts.... it did however put me on a few grains of truth.

Rebuilding the whole truth in the tree concept, what do you need for that to have any actual valid meaning? You need a record. Now we're cookin, I can assemble facts to create one coherent picture!

But what happens when you're overloaded with context? My first thought was probably the biggest joke out of all of this: Why can't I just get the LLM to, instead of reading code, read assembly??

So yeah, did you know assembly is actually ENORMOUS? I made a bad problem worse. But IR? Also ENORMOUS. But most of that is like... libraries macro's and templates and stuff right? Why not just make an index in a database of the important stuff. The chat gippity LOVED that idea. I swear to god it said “shit son that's brilliant you should be a computer scientist”. (It actually said that converges on what multiple other large scale companies are working on).

This is how the 2 core concepts of ProofPath came to be. Once I had the database index of extracted IR and the ProofPath concept derived from the LLM tree swarm concept, I was now deeeeeep in the rabbit hole. Still am.

Now I had a solid core, and I kept poking at the idea. Do I get the LLM to reason over the IR? How do I get good answers? Does the LLM even have training to read extracted IR? 

I don't recall the exact process of stumbling onto particular logical steps but out of many sessions of interrogating ChatGPT intensively, I struck some more golden nuggets. Template fingerprinting. Not a new concept, but relatively new in bounded queries. Why wouldn't I want the search to be bounded? I only work on 1 part of the program at a time right? 

I had no clue how important bounded was, just that maybe I had a good idea here and I could finally, FINALLY get my 6700XT to write me some decent code and analyse my code structures!

What does bounded even mean?


Turns out, it has a specific definition, I just didn't realise I'd struck another gold nugget for ProofPath. Bounded searching means you constrain your build, your compiler version, your compiler flags, your OS. It means a lot. I skipped right over that, and was just like yeah obviously?? Like, the binary is different on different systems??

This was the start of ProofPath becoming a stateful system that searches in bound, constrained searches. I was just solving a template problem conceptually. Macro expansion was another one that I had a little bit of trouble wrapping my head around, but eventually conceptually I did.

At this point it was talking about architecture and reasoning engines and I was so confused by this. I want the LLM to do the reasoning! It had likely been weeks of challenging idea's and drilling deeper, and the LLM had been telling me essentially from the start that I needed a reasoning engine. Eventually I noticed it's language use, that there was a reasoning engine in there and I questioned it. Why do we have this thing when we have the LLM? And the LLM was like, well your LLM just pulls the levers it doesn't actually do reasoning. “yeah but why not? I don't wanna write heaps of code”. “Because then it's non deterministic” which I took to be incorrect.

I was wrong. While LLM's can and are correct in the right use case, its almost like a happy accident. (heuristics) not actually because they reason about the specific problem. I was honestly resistant to getting rid of the LLM. I wanted the LLM think for me! Do you know how much code this is going to be????

Throughout all of this I still haven't learned to code anything more than mucking around with the game engine and some tutorials mind you.

Unfortunately or fortunately this became a brainbug for me somewhere along the way that I could not get rid of.... so I had another bright idea. What if this database has layers right? So you've got your index of IR, and then you reason over it and to produce some “does x call y” stuff yeah? My brain went “well it's going to annoy the fuck out of me if I don't store the results and reuse the useful stuff, instead of throwing away the lot”. But the LLM told me that wouldn't really work because the results need to be bounded to a particular build fingerprint etc etc. I didn't have a name for it, but I was all, “I gotchu fam.” and sent it some stuff I'd copy and pasted about the ProofPath concept and the gippity was all “sheeeiiitt das good”. It actually said it was a complimentary system and rare conceptually.

I didn't stop at that, I had vyvanse, dexies and a mission. Get Dopamine. Why cant this system reason about pointers and stuff? “because it's hard mate”. It actually said combinatorial explosion. This struck me as odd, because I'd been reducing the search space for everything else. Why can't it work conceptually for pointers and references and god knows what else? So I asked the gippity.

It said y'know what, that's actually pretty solid. If you bound your search on previously derived structures and calls, and then reason over it combining with the IR that right at the bottom layer you actually genuinely reduce the possibilities to an approximately logarithmic scale. Well that was reassuring. Anyway computers now have bulk processing power and incredible IPC. Who cares.(brave words from the ignorant)







I still hadn't realised the greatest strength of the derived facts “cache” at that point. I kept on challenging with new idea's until the gippity itself pointed out, one of your greatest strengths is that your system learns the code base. “listen here you gippity trash dont tell me you've gone as useful as copilot on me”. And it was basically “yeah dude, you store that data and you check the data first and you can combine reasoning processes with both the preexisting derived “cache” (which is now storage), and get factual data. I was genuinely shocked at this. I'd accidentally created software that accumulates each search result and turns it into an ever growing knowledge base of the users software.

This was accidentally 4 of 4 core pillars of ProofPath.

So now I'm at a concept where I have extracted and normalised IR, it has a retrieval engine and a reasoning engine, an evidence chain of actions and auditability and some basic idea's of what reasoning implementation I should put in, plus a database built up over time the more you use ProofPath, making it more and more useful and able to resolve questions faster and with a broader scope.

I hadn't realised the best application for this yet, until yet again gippity pointed out “hey fam, this is starting to look like it would suit mission critical systems aye”. Which was confusing because this was just a toy right? I just want some software that's going to tell me what I'm doing wrong!

But what I'd conceptually created was deterministic, auditable, repeatable, had an exportable proof chain, AND an exportable knowledge base provided the build parameters stay the same, it's able to be used in airgapped environments. It doesn't prove software in the traditional mission critical sense, it provides exact details on what is true in the specific scope of the query. I literally tripped and fell on all of this. 

There's a lot of conceptually in here. That's not by accident. I still have to learn C++, and not to a competent level. To write this system so that it becomes useful to the mission critical sphere, I have to be a weapons grade C++ writer. I need to have a mastery of what exactly I'm doing with the code, and the hardware. I need to have logs indicating design intent and evaluations of what worked and what didnt, a record of failures or learning experiences however you look at it, and tradeoffs. 

I can't just trip my way into this like I did the basic concepts.

But my idea's didn't and don't stop there. I have a vision of where this can go, and while the market IS small, it's also valuable monetarily (how else can it exist?) and morally to society. Mission critical software is no joke, it's invisible but it runs everything in our society. Right now, I'm a few steps away from starting to learn C++ to a level most people will never require, learning multiple compiler pipelines to a degree that most people don't even think of let alone see, going DEEP into C++ build system hell, learning the systems in detail... multiples of them! The outcome of all that is most people won't give a shit, it will be “does this solve our problem? Cool. Stop talking nerd, how much do you want?”

The gravity of it isn't lost on me. This is something that can potentially develop into a company that has international clients if driven correctly by not just myself but future business partners and implementation is strong. Me, a fuck up for most of my life has accidentally stumbled onto something oustandingly valuable. Realistically it's not google or facebook or netflix, not even close. It has a small client base and the problem is HARD. It's probably going to take 10 years to go from now to a mature product with scope large enough that it needs a team to drive it. It honestly scares me. I have zero people in my “team” that even understand this concept or are pushing me. Let's just throw in a little bit of zero formal education.

In the words of Linus the Linux developer “How hard can it be?”.

ChatGPT: timeline from chat records is ~ mid-2025 → early-2026 (≈ 6–9 months evolution).
