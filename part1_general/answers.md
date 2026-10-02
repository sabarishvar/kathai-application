# Part 1 — General Questionnaire

## 1.1 Essentials

### 1. Tell us something about yourself.

Hi! I'm Sabarishvar (Sab), a second-year Metallurgical and Materials Engineering student.

My hobbies are basically nerdy things: watching anime and reading about Spider-Man and superhero stuff. I also love hanging out with my friends, and I go to the gym for some physical exercise.

Something interesting about me: in first year, I was basically useless, like literally. While everyone else was exploring their opportunities, I was doing nothing but slacking off in my room. But when I moved to second year, I started to explore. I chose cybersecurity, and I was lucky enough to get a project under a professor in it. More than that, it introduced me to Linux, which I still think is pretty great. Now I'm exploring AI/ML and also some quant stuff. So yeah, that's the vague intro about me!

### 2. What does RAG really do? What is semantic meaning according to your computer?

RAG (retrieval-augmented generation) is basically an open-book exam for a language model. Instead of answering only from what it memorised during training, the system first searches a collection of documents for the pieces most related to the question, and then gives those pieces to the model so it answers from them. So RAG has two jobs which re finding the right text, and then writing an answer based on it. If the first job fails, the second can't fix it.

For the finding part, the computer needs some idea of meaning. For a computer, meaning is a position in space. An embedding model turns each sentence into a long list of numbers (384 numbers for the MiniLM model I used), and sentences used in similar ways end up close to each other. 


### 3. Why is dealing with noisy data important?

Because every later step trusts the earlier one, and noise doesn't stay where it starts. It travels through the whole pipeline and usually gets harder to notice on the way.

Noisy data also makes evaluation tricky. In 2.1, one overall score said a recording with loud hum was more damaged than a hard-clipped one, when the hum was actually fully removable and the clipping had permanently destroyed information. And "cleaning" isn't automatically good: my spectral gate made recordings sound cleaner but made Whisper's transcription worse in four out of five runs.

For KathAI this matters even more. Field recordings of Toda and Bhili speakers won't be studio quality, Toda has no standard script so the same word may be spelled differently by different transcribers, and there is very little data to begin with, so we can't afford to lose or corrupt any of it. Dealing with noise properly means knowing what kind of noise it is, what can actually be recovered, and when to flag something for a human instead of trusting it.

### 4. Why do we need RAG? Can't LLMs solve all our NLP problems?
An LLM only knows what was in its training data, and it knows it in a blurry, compressed way. That causes three problems that RAG helps with.

First, the data we care about usually isn't in the training set. An archive of Toda and Bhili recordings collected by the LC Lab is not on the internet, and these languages have very little written text anywhere, so no LLM has really learned them. RAG lets the model work with data it has never seen, without retraining it.

Second, LLMs fill gaps fluently, even when they're wrong. In 2.3, the translation model turned "Thiruvalluvar wrote the Thirukkural" into "Thiruvananthapuram wrote the Thiruvananthapuram", and in 2.1 Whisper wrote "Saturday" instead of "seven" in noisy audio. Both read naturally. With RAG, an answer can be traced back to the exact chunk, and in our case to the exact moment in a recording, so a person can check it. For an archive, that bck trcking matters more than fluency.

Third, you simply can't give a model everything. 40,000 hours of lectures is hundreds of millions of words, which no model can read for every question.

But RAG doesn't magically fix things either. The answer is only as good as what we get bck, and in the 2.2 experiments retrieval failed when chunks cut an explanation in half, when ASR misspelled the key word, or when a chunk was so long that the embedding model only read its beginning. And for languages like Toda and Bhili, even basic steps are hard: the tokenizer already split 7 Tamil words into 16 pieces, and a model may not support these languages at all. So LLMs can't solve all NLP problems, especially for low-resource languages, and RAG is one important part of the solution, not the whole of it.

## 1.2 Project Goals and Expectations

### 1. Describe the goals of the project in as much detail as possible. What do you expect working on it to look like?

As I understand it from my team lead, KathAI is about not letting two Indian languages, Bhili and Toda, disappear. The LC Lab has already recorded speakers and transcribed the recordings, and KathAI's job is to make that material usable: a retrieval system to search through it, and translation so that people who don't speak these languages can understand it.

The two languages are in very different situations. Toda is a Dravidian language spoken by about 1,600 people in the Nilgiris, closely related to Tamil, with no script of its own. Bhili is an Indo-Aryan language related to Gujarati and Rajasthani with around 3.2 million speakers, but it has no official status and very little written text. Both are "low-resource": almost no AI model has been trained on them.

so as far as the project is concerned:

 **Getting the data into shape:** Transcriptions of an unwritten language may spell the same word differently, so they need to be made consistent, linked to their audio with timestamps, and documented.
 **A retrieval system:** where someone can ask a question and get the exact passage and the moment in the recording it comes from. Oral storytelling has long digressions, so chunking matters a lot here, and search has to handle spelling variation, which plain keyword search can't.
 **Translation:**, probably through a related language that models already handle better (Tamil for Toda, Gujarati or Hindi for Bhili), with special care for names, places and cultural words, since that is exactly where I saw models fail in 2.3.
 **when to not trut the output:** Since there is no correct English to compare against, the system should flag uncertain translations for a linguist to check.

I expect the work to be less of building a large model and more careful experiments: understanding the data first, building small pieces, measuring them honestly, and working closely with the trncript given, because they know the languages and we know the tools.

### 2. Imagine yourself a year later. What achievements or progress would make the experience a tremendous success?

 A working search system that a linguist or someone from the community has actually used: they ask a question and get the right passage and the exact place in the recording, and we have tested how often it gets this right.
 A translation pipeline with measured reliability: we know which kinds of sentences it handles well, which it gets wrong, and it flags the risky ones for review instead of hiding them.
 A clean, consistent and documented version of the data that future students and researchers can build on, so the work doesn't end with our team.
 understanding speech and NLP for low-resource languages properly, not just using libraries, and having contributed parts of the system that others rely on.


### 3. How much effort do you think it would take to reach that level?

A lot of steady work rather than short bursts. I'd expect to work  lot but also its just an estimation that id be working on this for 1.5 to 2 hrs a day.

The first few months would go into understanding the data and the languages and reading about low-resource speech and translation, before building much. I also expect data cleaning and evaluation to take longer than the modelling itself, and working with linguists means waiting for feedback and adjusting, so progress won't always be fast.

This application gave me a small taste of it: in a few days I built and tested pieces for audio, retrieval and translation, and most of the time went into checking whether results were real, not into writing code. I know a year-long project needs that same effort kept up consistently, and I'm prepared for that.

## 1.3 Motivation and Drive

### 1. What is your primary motivation for joining this project? What excites you the most?

Most AI projects I've seen build something that would exist anyway, just slightly better. KathAI is different because if the work isn't done, something is actually lost. Toda has around 1,600 speakers and no script of its own, and a language like that can disappear within a generation. Helping keep it searchable and understandable feels like work that matters beyond a leaderboard.

What excites me most is that the project covers the whole chain, from noisy field audio to transcripts, retrieval and translation, and each step is hard in its own way. Doing the three technical questions, the most fun parts were the surprises, like finding that "Pourbaix" never appeared in a lecture about Pourbaix diagrams. I want to spend a year finding and fixing problems like that on real data that matters.

An further i want to go deep into the ai ml stuff in general since i started it late but still managed to join the guild and all but the thing is that this kinda problem solving tasks exites me the most 

### 2. Is there another opportunity that is more aligned with this motivation than KathAI PM?

Not that I know of. Most other projects I could join are general ML problems, or work on English data where good tools already exist. KathAI is the only one I've come across that combines speech, retrieval and translation for real Indian languages that are actually at risk, and that works directly with linguists who know the languages. That combination is exactly what I'm looking for.And further since this works on the RAG in depth so it will be an oppurtunity for me to learn about it even furhter than the nkowledge which i have right now

### 3. What could dampen your enthusiasm or make you disengage?

If  work turned into only data cleaning for months with nothing being built or tested, or if what we build never reaches the people it's meant for. I'd rather know early that something isn't working than keep going quietly, so if I notice any of this happening, I'd raise it with the team instead of slowly drifting away.

### 4. Is this a good opportunity for you in early UG? Do the positives outweigh the challenges?

Yes. As a Metallurgy student moving towards AI/ML, I already get breadth from courses, online material and small projects, but none of that shows I can take one real problem from messy data to a working, tested system. That only comes from staying with one project long enough to hit its real difficulties.



## 1.4 Commitments/PoRs

### 1. Other commitments this year, their peaks, and how you'll handle clashes.
Currently my only position of responsibility is Events Coordinator in Team MITR, IIT Madras' peer support body. Earlier I was an Events Coordinator for Shaastra Ignite, and I worked as a Backend and AI/ML intern at Hertzworks × CIFIL Lab, where I built parts of a FastAPI/PostgreSQL ERP and an AI/ML chatbot. MITR's workload peaks around its events, which are planned well in advance, so I can schedule KathAI work around them and keep my weekly hours steady; if something does clash, I'll tell both teams early, finish whichever deadline can't move first, and make up the KathAI hours the following week.

