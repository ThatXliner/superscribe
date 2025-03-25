<script lang="ts">
	import MermaidGraph from '$lib/MermaidGraph.svelte';
	import Record from '$lib/Record.svelte';
	import OpenAI from 'openai';
	import Input from '$lib/components/ui/input/input.svelte';
	let apiKey = $state(null);
	let graphCode = $state(``);
	const openai = $derived(apiKey ? new OpenAI({ apiKey, dangerouslyAllowBrowser: true }) : null);
	function createPrompt(transcript: string) {
		return (
			// biome-ignore lint/style/useTemplate: Hurts readability
			'Generate Mermaid.js diagram code that visually represents ' +
			'the structure and logic of the following transcript. ' +
			'The diagram should only include the content explicitly ' +
			'mentioned in the speech and exclude any extraneous information ' +
			'such as greetings. Ensure that the diagram accurately reflects ' +
			"the content provided in the transcript, even if it's incomplete. " +
			'Your code should only link nodes using the simple "A[Content] --> B" syntax. ' +
			'Here is the transcript:\n```\n' +
			transcript +
			'\n```'
		);
	}
	function extractMermaidCode(text: string) {
		const code = text.match(/```mermaid\n([\s\S]+)\n```/);
		if (!code) {
			throw new Error('No Mermaid code found in response');
		}
		return code[1];
	}
	function blobToDataURL(blob: Blob): Promise<string> {
		return new Promise<string>((resolve, reject) => {
			const reader = new FileReader();
			reader.onload = (_e) => resolve(reader.result as string);
			reader.onerror = (_e) => reject(reader.error);
			reader.onabort = (_e) => reject(new Error('Read aborted'));
			reader.readAsDataURL(blob);
		});
	}
</script>

<Input class="ml-3 mt-3 w-fit" type="password" bind:value={apiKey} placeholder="API Key" />
{#if openai !== null}
	<Record
		onAudioChunk={async (x) => {
			console.log('Received audio chunk', x);
			// TODO: Live streaming
		}}
		onFinishRecord={async (x) => {
			(new Audio(await blobToDataURL(x))).play();
			const audioMimeToExt = {
 'audio/aac': 'aac',
 'audio/aacp': 'aacp',
 'audio/amr': 'amr',
 'audio/basic': 'au',
 'audio/flac': 'flac',
 'audio/midi': 'midi',
 'audio/mp3': 'mp3',
 'audio/mp4': 'mp4',
 'audio/mpeg': 'mp3',
 'audio/ogg': 'ogg',
 'audio/opus': 'opus',
 'audio/vorbis': 'ogg',
 'audio/wav': 'wav',
 'audio/webm': 'webm',
 'audio/x-aac': 'aac',
 'audio/x-aiff': 'aiff',
 'audio/x-mpeg': 'mp3',
 'audio/x-mpegurl': 'm3u',
 'audio/x-ms-wax': 'wax',
 'audio/x-ms-wma': 'wma',
 'audio/x-pn-realaudio': 'ra',
 'audio/x-realaudio': 'ra',
 'audio/x-wav': 'wav',
};
// TODO: Run Whisper locally. The API quality is super ass
			const transcription = {text:`Ladies and gentlemen, esteemed guests,

Today, I stand before you to share my thoughts on a concept that is both fascinating and contentious: algorithmic government.

In this age of rapid technological advancement, the notion of algorithmic government has emerged as a potential solution to many of the challenges facing traditional governance structures. But what exactly does it entail?

At its core, algorithmic government involves the use of algorithms and data-driven decision-making processes to govern societies. Imagine a system where policy decisions are not solely reliant on human intuition or political rhetoric, but instead are informed by vast amounts of data and complex algorithms capable of analyzing this data in real-time.

Proponents of algorithmic government argue that such a system could lead to more efficient and effective governance. By harnessing the power of big data and machine learning algorithms, governments could potentially identify and address societal issues with unprecedented speed and accuracy. From optimizing public services to predicting and preventing crime, the possibilities are endless.

However, it's essential to acknowledge the potential pitfalls and ethical concerns associated with algorithmic government. One major concern is the issue of bias inherent in data sets. Algorithms are only as unbiased as the data they are trained on, and if that data reflects historical prejudices or inequalities, then the decisions made by these algorithms may perpetuate rather than alleviate existing injustices.

Moreover, there's the question of accountability. Who is responsible when an algorithm makes a faulty decision that negatively impacts citizens' lives? Can we trust algorithms to make decisions that are in the best interests of all members of society, or will they inevitably favor certain groups over others?

These are just a few of the many questions that arise when contemplating the implementation of algorithmic government. While the idea certainly has its merits, it's crucial that we proceed with caution and carefully consider the potential consequences.

In conclusion, the concept of algorithmic government represents a tantalizing vision of the future, where data and technology are harnessed to create more responsive and efficient governance systems. However, we must approach this idea with a critical eye, ensuring that our pursuit of innovation does not come at the expense of justice, equality, and human rights.

Thank you.`}
// const transcription = {text:`I have not been uploading podcast episodes for the last week because the last week has been pretty tough.

// It's not insane, it was not brutal but it's just that there is a lot of events that happened and it kind of piled up.

// I'm a little behind on some of my schoolwork like some of it is missing because of these events.

// Two weeks ago I was at my FRC Silicon Valley Regional.

// We got to the finalist round, yeah, but the thing is that took up two weekends, so Saturday and Sunday, so I had no time to study for whatever was there on Monday.

// So I asked for an extension on my bio quiz, got the move and I did it.

// Got a 9 out of 10 on it.

// I guess didn't study too much about it.

// Even more is that like there's a sheer amount of tests that was there last week.

// It was pretty crazy.

// Yeah, of course there's the regular daily bio tests, bio quizzes, but there's also, yeah, math chapter 10 test.

// That thing was insane.

// I don't need to talk much about it because everybody knows how bad it was.

// When I say I think I failed the test, this time we literally mean it.

// We literally mean solid F's failed the test.

// The class average was a 49%, the median was a 52%, the highest in my class is or that I've heard was an 86%, and the lowest I heard it was a 12% on that chapter 10 test.

// There is also no curve.

// Luckily for us there is test corrections, so that should be a life saver.

// But I also am looking forward to calculus and limits because those should be fairly easy, much easier compared to the CONIX test.

// This chapter 10 test, the problem with that is that it's really like inaccurate, poorly named.

// I was told that we have to study this, this, this, this, but then on the test there is literally one topic multiplied by five.

// There is all, most of the problems, you could even say all the problems, were graphing polar CONIX.

// You have to find these, these, these properties, or you have to find the equation given these, these, these properties, and you have to graph it on a polar form, polar graph.

// A polar graph, a polar coordinates, yeah, polar graphs are based off of polar coordinates.

// So instead of your regular XY rectangular coordinate system, we have R, which is, I guess it stands for radius, and theta.

// Theta is the current angle, and the angle of the point, and R is the length of how far that point is from the origin or the pole.

// I could go into polar coordinates, but I really did not want to.

// I'm just so glad the test is over, that I could barely do 50% of the questions on that test.

// I don't think anything could prepare me for what that was to come, because my teacher did not really go over that specific topic, polar CONIX, graphing polar CONIX, that much.

// We just had a really short lecture, because he started like 30 minutes late.

// It's always like over math classes.

// And then with the test, right, I have an English test tomorrow.

// I have not really studied that much.

// The only thing I did study was the vocabulary, but I need to review all the characters in the Frankenstein book, because this test is about the Mary Shelley's Frankenstein.

// All the characters, all the events, all the potential quotes related, and which character said this quote.

// We also have to memorize the probably details and like storyline.

// I need to review the book a little bit.

// I just finished it like last night, two nights ago.

// At least the vocabulary.

// I started that like last week, and that is a great decision.

// Using Quizlet like every day to go over the vocabulary.

// It's it's really efficient.

// It's great.

// I feel like read comprehension tests.

// They're useful.

// They should definitely be allowed, but there is one thing I want to complain about my English class right now, and that is it's way of around the AI use, right.

// We in 2024, after 2023, after that fateful day, March something in 2023, where Chachi Buti was introduced to the world, it changed the entire world, right.

// That is like one of the biggest changes that has biggest impacts on the world ever since, say, the computer itself, or I would say it's comparable to, yeah, the computer itself.

// They mentioned the computer, mentioned of Wi-Fi, ARPANET, etc.

// Chachi Buti being introduced to the world forces us to really reconsider how we do a lot of things, because we have to now like hand write essays in class.

// That is barely enough time to actually formulate, revise, and edit a whole essay.

// It would definitely lower the quality of the essays that students write, because of so little time that we have, and little time to prepare.

// They give you the prompt and you start writing the moment you're given the prompt, because they don't want you to send that prompt to an AI and just like write it, get the AI to write it.

// My problem with this is that AI is really bad at that, though.

// Like, yes, it can generate essays, but have you seen the essays that it wrote?

// They are trash.

// They are trash in the sense that they are not captivating.

// They are not interesting.

// If you read any AI-generated content, you couldn't almost immediately tell it's AI-generated.

// It is very verbose.

// It is containing a lot of like unnecessary information.

// People say that if we fully allow students to use AI for everything, they will not learn.

// I believe this is true to some extent, and I do believe that AI use should be restricted at some areas.

// Say, like in elementary school, they fundamentally learn the fundamentals, right?

// They have to learn how to write an essay for the first time, they have to know the basics, like paragraph structure, topic sentence, conclusion sentence.

// Also, we don't do that anymore.

// I got the claim data warrant, blah blah blah blah.

// That, yes, that definitely does not constitute the need for an AI.

// That definitely should, yeah, you should restrict AI use when you teach students that.

// But at that age, if they are able to figure out how to download a local LLM, that is, Large Language Model, which we, which is what we developers call all of these AI's chatbots, such as like ChatGVT, Google Bard, Anthropics Quad, Quad 3, whatever, Microsoft Copilot, Bing, Chat.

// We call those LLMs, large language models.

// Let's say, as a parent, you restrict your kid from ever accessing the site OpenAI.com.

// But if they're able to get their hands on a local model and be able to run it on their own computer, you should probably let them do that.

// Like, congrats to them for being able to figure out the instructions, like on like a GitHub repository, how to use the command line, and even like install the application without parents' permission.

// If they're able to do that at that point, you should probably just let them have it.

// Because they should have the reading comprehension ability to even read on how to install the AI locally and run it.

// But the thing that is a problem is writing.

// I want to learn how to write better.

// Sometimes, in my application essays, I use ChatGVT to help me rephrase some sentences that I think don't flow as nicely as I want them to.

// But here's the key thing here.

// While, yes, I'm fully admitting to using AI in my application essays, and I do follow them, I do have integrity and do not use AI when we are not allowed to.

// Application essays, we are they don't, unless it's specified, it'll be checked for AI.

// I use it to my advantage, but it is always to my advantage.

// What I mean by this is that I learn from how they restructure my content.

// Like, I find myself having a lot of short, simple sentences, and then I forget that I can actually combine them with conjunctions.

// Or how I should combine them?

// What's the most optimal way to place this independent clause?

// Should I split it into dependent clause in front, or this in the back, etc.?

// Oh, keep passive voice, keep active voice, stuff like that.

// Verb tense shifting.

// The thing is, you want your writing for informative essays and stuff to be as concise yet coherent as possible.

// For literature and personal essays and stuff, any artistic kind of writing, you could throw all that out the window.

// I would actually recommend you shut off Grammarly unless you are using it minimally for grammar detection and basic grammar detection.

// But for style, I don't think any kind of rewriting, unless you can't figure out how to rewrite it in the way you want it to, you should not need to rewrite your sentences in the way it's super concise and coherent.

// Because for literature, screenplay, screenwriting, I don't know what it's called, playwriting, screenwriting, whatever.

// You want to have a specific tone, a specific voice, a specific style for yourself.

// If you're writing a book for yourself or if you're writing a script, a play for your characters.

// Now, for informational writing, you should definitely learn from the tools that we have right now.

// Although, CHMT can write verbose text by default.

// If you tell it to be concise, it will be concise.

// And that really helps me lower my word count for application essays that do have a word limit.

// These word limits, I don't want to talk about them.

// These word limits are intentional and they are helpful.

// The thing is this, while yes, you should be taking advantage of the word limit as much as possible, if you're just BS-ing stuff or just yapping, or putting very verbose English just so you want to maximize the word limit, do not do that.

// The reason why they have a word limit in the first place is that you should be so passionate about the program you're applying to, that you have so much to say about it.

// It's really difficult for you to cut down on what you should expand and what you should not say.

// That's why there's a word limit.

// It's for you to boil down your point and give it to the whatever officer reading your application in the most efficient way possible.

// You should break your essay down to the most efficient way possible, boil it down to the minimum points, minimum ideas needed to deliver your coherent message.

// This should also apply to any essays you write.

// The thing is, ChachiBT is useless for personal essays.

// Well, yes, you can like force ChachiBT to like make something up.

// I think if we want English teachers to teach on how to properly structure an essay or like give them feedback on how they're wording and stuff, they should probably teach techniques directly.

// And second of all, if they want like a test or like some kind of essay for students to write, please do not do that in class.

// Okay, let's say Frankenstein, right?

// That's what we're doing right now.

// They made us write an essay related to like Frankenstein.

// Well, not really, but for the sake of explaining, I'm gonna say that.

// And then what she did was she gave us three research, three articles about what she said.

// Also, we read the book, right?

// So we use that.

// You have to cite that specifically and take advantage of citations and write your essay.

// The thing is, AI is perfect for this because it can definitely summarize, it can definitely regurgitate pretty well, and that's exactly what you're doing here.

// But the hard part is new ideas and personal insight.

// If we write a full-on analysis, okay, AI can also do full-on analysis too.

// That's the problem.

// But you want new analysis, new topics, maybe from personal experience, right?

// You know what should grade this properly?

// There should be like some like commonly shared personal experience.

// So maybe like a school activity and then you have to like write about it.

// That is useful for like as a topic for English teachers to quantify your writing capability in general.

// But then for like informational writing capability that you should have like...

// I would...

// I want to encourage the use of AI.

// English teachers should be teaching you how to use AI in the first place.

// That's the thing.

// And math?

// Hey, we're being taught how to use a calculator.

// We should be taught how to use this too.

// Although I guess you could argue that the tool is too new for anyone to be a master of it and teach others.

// But hey, who knows?

// There's already books about how to prompt engineer, probably written using AI.

// But the thing is, I want to say personal experiences, shared experiences, things that just cannot be replicated by AI.

// Oh, ideas and persuasive essays on controversial topics.

// You know, OpenAI, they have safeguards.

// At Google, they have safeguards on their LLMs just so they won't get any like backfire or some like drama.

// Oh look, China GBT is politically left-wing like blah blah blah.

// They want to avoid that so they have safeguards trying to keep the AI as neutral as possible and they're trying to like refuse to answer any like political questions.

// Those should be topics that these English teachers should like make you write about.

// Persuasive essays?

// Yes, write about controversial topics.

// For students who are truly passionate about like said controversial topic, they should be able to write this really easily too.

// And all that they need to learn is how to structure the essay, how they should present their point across, and etc. etc.

// How to, I don't know, cite quotes and stuff if that's what you're teaching in the class.

// Because they know their reasonings, they can know their ideas on this controversial topic, so they don't really need to.

// It's not useful for them to ask an AI on creating like topics or ideas because they should have their own.

// It's much easier to just write your own down rather than asking AI and try to extract information from AI that's just like probably refusing their ideas on a controversial topic in the first place.

// That being said, there are jailbroken models, open source, based off like say Mistral or like Orca.

// But as a developer, if you're also part of that community, if you're able to download and use that, go for it.

// Like I don't think this is a major problem.

// If you're smart enough to do that, you should be smart enough to pass the English class and learn what they're teaching in the first place.

// So honestly, the argument does not really apply.

// Although you could probably say it does.

// In my opinion, either way, I should be encouraged.

// Because at the end, this is inevitable.

// Our current solution of rejecting AI, banning it, making people handwrite it, that is a temporary solution.

// Let's say people actually, students actually installed Neuralink in the brain.

// Now they have an AI and an unfair advantage for everybody.

// The same can go for say Olympics and like should we allow drugs?

// Okay, there are two ways you can go with that and this is a huge topic which I do not have time for.

// And actually, I can talk about this on Friday or Saturday about Olympics and should we allow drugs?

// There is a, what is it called, enhanced Olympics by some billionaire guy.

// I really want to talk about that next podcast.

// But anyways, That's it for today`}
			console.log("TRANSCRIBED: ", transcription.text,transcription);
			const completion = await openai.chat.completions.create({
				messages: [{ role: 'user', content: createPrompt(transcription.text) }],
				model: 'gpt-3.5-turbo',
				n: 1
			});
			// message.content can't be null because we're not using any tools
			graphCode = extractMermaidCode(completion.choices[0].message.content as string);
			// TODO: Detect verbatim responses up to a threshold
			// that don't summarize the transcript
			// and display it as "more content needed for accurate diagram"
		}}
	/>
{/if}
{#if graphCode !== ''}
	<MermaidGraph graph={graphCode} />
{/if}
