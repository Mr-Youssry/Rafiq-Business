# Rafiq Product Design Meeting - Full Transcript (Clean)
**Date:** February 1, 2026
**Duration:** 02:25:39 | **Language:** Arabic/English (mixed, translated to English)
**Participants:** Ahmed Youssry, Shereen Sayed, Nermeen Ahmed

---

## Part 1: Opening & Cohort Design Discussion
**[00:00:00 - 00:24:00]**

---

[00:00:00] **Shereen Sayed:** Let me see what comes up. No, I'm not going to share it -- I said I'd just put it up front like this. I'll record and keep the recording so I can use it later. Go ahead. Okay.

[00:00:16] **Ahmed Youssry:** We're all still getting started. I want to go over some terminology with you related to coaches, cohorts, and so on. If we say a cohort is a group of people who joined together with a coach -- is it possible for a cohort to have more than one coach? Or do we define it so that a cohort is a single journey from one coach to a group of coachees? Is there a need for multi-coach communication? Or should the design be that there's one coach with a group of coachees who complete the journey? Or should the journey have a group of coachees and a group of coaches -- many-to-many -- where they complete the journey together and all communicate with each other? The second model is complex, but it can be done. So the question is: do we need this to reflect what's happening all the time? Or should it be a defined relationship where a cohort consists of a group with a specific coach?

[00:01:50] **Shereen Sayed:** That question is -- right? It's a very layered question you're asking. But it is...

[00:01:57] **Ahmed Youssry:** The question is: many-to-many or one-to-many? Let me walk you through the scenario.

[00:02:02] **Shereen Sayed:** Karen's scenario. Okay. She's the user we're working with right now. Karen's scenario is that she's responsible for -- let me think... about fifteen, right? Yes, they were roughly fifteen tutors. She has a cohort, and there's no defined start point or end point. The start and end are determined by the semester -- we all start at the same time because the semester started, and we all close at the same time because the semester ended or the school year ended. So time is what determines it, not specific coaching criteria. There's nothing specific that says the coaching is finished. She's there -- it's her job, full-time. And if she leaves, they'll hire someone else. It's an existing role with no defined end. And she's not starting with all six at the same time; she works with them on a rolling basis, so to speak.

[00:03:31] **Nermeen Ahmed:** I'm raising my hand because I want to say something after you finish. Go ahead, finish up.

[00:03:36] **Shereen Sayed:** Feel free to jump in, that's fine -- I'm just thinking out loud.

[00:03:43] **Nermeen Ahmed:** I wanted to say we should also build on our own experience. We have our own experience as a group of coaches -- and the coaches are teachers. As Shereen said -- and I want to build on what you were saying -- we start because the school year started, and we stop when school stops. What brings us together is that we share experiences, like a community of practice. That's what unites us as coaches. Shereen used to hold a coaches' meeting every two weeks, and there were two important things in that meeting. First, we'd track where everyone was -- for example, each coach was supposed to do six sessions per semester. So who's done one? Who's done ten? We'd track that. And second, we'd share experiences -- what's challenging for whom? What worked for someone else? It's like what we call a learning journey for the coach -- they're developing too, and they have a community of coaches. But there's nothing that makes the teachers a cohort specifically. There's nothing connecting them that way. I could be a coach working with a teacher completely on my own -- not talking to anyone, not doing anything with anyone else. I'd just go through my individual journey with the teacher, like a coach does with any other coachee.

[00:05:23] **Shereen Sayed:** Exactly, it can be varied as you say. Whether from our experience or from Karen's experience -- I keep referencing Karen because her situation is quite similar to ours. They're three coaches at their level. They talk to each other if they get stuck, if there's a problem, and they track together. And I also wanted to say that, for example, among the six teachers I work with, one might be in her first year -- so this is the first time we're coaching together. Another might be in her second or third year, so we've worked together multiple times. It's not the first time I'm her coach. Just like we had the same situation -- we could stay with a teacher for two years, so someone might be in their second year with a teacher and another in their first year. That's the thing about the cohort -- there's actually no single start point, even from a timeline perspective, and no single start point from a coaching perspective.

[00:06:35] **Nermeen Ahmed:** It's meeting the teachers where they are. Each one of them is at a different point -- developing different skills and different knowledge. Each one is at their own stage.

[00:06:44] **Shereen Sayed:** The only common factor among these teachers is that they have the same coach. That's what makes them a group. But I don't think we can really call them a cohort in the conventional sense. We need to find another term for them. They are a group because they share the same coach. The coach might want to send them all a message on the app. She might want to send them all specific materials. They might want to talk to each other because they feel a certain bond from being with the same coach. Like how Cali set up levels -- there's the group with my coach, then the elementary school group, then the grade group. These communities of teachers could be communities, not necessarily a cohort.

[00:07:58] **Nermeen Ahmed:** Maybe what drives this is -- the word could lead to that. And the features, or whatever they end up being, could turn out to be something completely different. Those are separate user stories, right? But for now, with me...

[00:08:29] **Ahmed Youssry:** So this is actually the same thing. What I mean is, it's all part of the same concept. Here's the journey. Now let me ask you about the design.

[00:08:54] **Shereen Sayed:** I'll do it. Okay, second one then. Tell me. Okay, go ahead.

[00:09:00] **Ahmed Youssry:** Tell me if you have something -- I'm here to listen to you. I'm listening so we can make decisions. I'll just give you some structure. I was going to say something.

[00:09:08] **Shereen Sayed:** Let me add something. Let's think about something different -- for example, JoTogether [?]. Each of us does something different in the coaching, completely different. We don't even have the same structure or anything. There's nothing connecting us other than -- maybe we need to look at the definition if we really want to define it, or think about it together. But they're calling us a cohort because we share a sequence, but there's nothing else that's common. They are meeting us where we are. And we don't even all get coached by the same person or anything. But we still are called a cohort. And we're not thrilled about it. But because...

[00:10:12] **Nermeen Ahmed:** It's time-bound. But for the sake of what? Of networking. We start and finish together. The program has a set duration and it will end. And what does the cohort do together? Every so often they bring us together -- let's talk to each other, or meet a previous cohort, share experiences, that kind of thing.

[00:10:30] **Shereen Sayed:** Hmm.

[00:10:31] **Nermeen Ahmed:** But we're not on the same journey. With our coach, we're each on our own journey. Everyone else is on their own journey too -- we don't know what they're doing.

[00:10:40] **Shereen Sayed:** But at every start, at each stage...

[00:10:44] **Nermeen Ahmed:** We don't know what they're doing, and we don't talk to each other about where we've reached in coaching, or what our coach is doing, or anything like that. From a high level, we just talk about the field -- who's doing what, contacts, networking, and so on. That's what we do as a cohort.

[00:11:01] **Shereen Sayed:** Right, so the need to be a group has to have a purpose -- that's what you're saying, Nermeen, correct?

[00:11:10] **Nermeen Ahmed:** Exactly. It could be for the feature we were discussing -- you're a group and you might end up sharing resources with each other, or whatever. Same thing applies to being together. That's it.

[00:11:26] **Ahmed Youssry:** We've actually discussed this before. For example, I know that Gwinnett County has a new program for AI teaching -- how teachers can truly understand different aspects of it. A program like that -- we know that the people entering it will want to work on that subject. So it's not...

[00:12:18] **Shereen Sayed:** Something that brings them together. That's why, roughly from Karen's experience, the three of them -- okay? What unites them is that they're coaching something other than math. So that could be what connects them. But how would that manifest in the app?

[00:12:48] **Ahmed Youssry:** And does it need to manifest? Does it need to manifest? Let me give you my take on the decision now -- it could be nice if we keep it simple. Simple but allows for complexity. What I mean is: our core unit is a journey. Right? How long is the journey? It can be extendable. At the end of the journey, I can add more objectives to it.

[00:13:32] **Nermeen Ahmed:** Got it.

[00:13:34] **Ahmed Youssry:** And the journey connects two people together -- a coach and a coachee. That's it. So that's the unit, and within it the coach sets... sorry, the teacher sets lesson plans.

[00:13:50] **Nermeen Ahmed:** And the coach...

[00:13:54] **Ahmed Youssry:** ...provides feedback on them. The teacher submits observations and the coach provides review sessions -- whether asynchronous or synchronous. The coach reports and the coachee reports on the journey's progress. So all together this is called a coaching journey unit. This unit can later -- if we find the need -- be placed inside larger structures: a coaching community structure with coach, coachee, journey, and so on. But the coaching journey should be clear: the journey contains lessons and lesson planning cycles and observations -- you can call them whatever you want. But there's a clear flow from first to last. The coach, like Karen, can update the journey each semester. She can define it as beginning at the start of the year or the semester. Now, the journey part -- this was actually a separate question, but I'll address it here because the timing is right. I'm currently dividing the journey into three parts, regardless of what we end up calling them -- just to keep things clear for now. There's a contracting phase, repeating cycles, and a closing cycle. So the journey must...

[00:16:08] **Shereen Sayed:** The journey...

[00:16:11] **Ahmed Youssry:** A journey is what a coach goes through with a teacher. And within this journey, it must start with a contracting cycle. We have to do that as part of the journey. Okay. I'm giving you the definition I'm working with now. And we have -- three, five, eight -- however many repeating cycles. Number one: I want this -- and this is what we'll come back to refine later, even if we take it and implement it. You understand? You want it to have a beginning and an end, and to define what happens inside it, the known elements. Then at the start of the next semester, if they want to continue with the same coach and coachee, there's a behavior contract agreement...

[00:17:32] **Shereen Sayed:** Okay, but can you clarify -- because I can't distinguish between... what is the contracting versus the coaching?

[00:17:39] **Ahmed Youssry:** Okay. What I've written -- which is what you're referring to -- contains the file that you've been working on. Let me share it with you.

[00:17:55] **Shereen Sayed:** Just so I can confirm I understand you correctly, because you mentioned something earlier and said...

[00:18:00] **Nermeen Ahmed:** I'm not sure either. It's a file called... Yes. That's the foundation of what I...

[00:18:09] **Ahmed Youssry:** ...am calling -- I'll finalize the naming later. From now on I'll use the word "contract" so it's clear.

[00:18:16] **Nermeen Ahmed:** So it's with the school, meaning with whoever we're working with.

[00:18:21] **Ahmed Youssry:** Right. This is the one that contains -- like the coaching plan template you've been working on. You know this one, right? Yes, yes. Number one: are there other things that could be added to it?

[00:18:38] **Nermeen Ahmed:** We still need to finalize it -- it's not done yet.

[00:18:42] **Ahmed Youssry:** Okay, but can this serve as something we work from and allow them to add to? Like articles -- any contract is essentially a set of clauses, right? So this is a contract with fixed clauses that we agree on. It's a contracting process between...

[00:19:08] **Shereen Sayed:** And Nermeen and I needed to revisit this. We want to work on it and bring it back so it becomes the baseline we start with.

[00:19:23] **Ahmed Youssry:** Fine. I won't hold myself up on it -- there will be room to adjust anyway. We can revisit and refine it for the final version. Okay? Good. So that was the first part.

[00:19:37] **Shereen Sayed:** That's the content of the contracting phase for now. But can we take a break and note that this needs to be revisited -- just so nothing gets built on top of it prematurely.

[00:19:51] **Nermeen Ahmed:** So nothing is built on something incomplete, right? Is it open for us to work on? Is it urgent or not?

[00:20:04] **Ahmed Youssry:** Think of them as articles. The articles would be between the coach and the coachee. So there's an agreement between them, and when they renew it, we'd be doing it again.

[00:20:22] **Nermeen Ahmed:** No, no, I understand that. What you need from us now is to finalize that document.

[00:20:27] **Ahmed Youssry:** To have it finalized. And when? Ideally soon. But it doesn't have to be fully complete -- I'll break it down into units. I'll take what's already done, build on it, and then there'll be room to adjust later. It's not all-or-nothing. I've structured it from what we already have so...

[00:20:50] **Shereen Sayed:** So what you're saying is: if we get it to you in time, great; if not, you'll work with what you have.

[00:20:56] **Ahmed Youssry:** Okay, fine. Let's go back to our question. First thing: the concept that the journey has three phases -- are we agreed on that? I'll build it that way?

[00:21:20] **Shereen Sayed:** I think so, yes. Because there has to be a report at the end of the year, even if we're continuing together. It doesn't have to be final -- it's just a closing of that journey. Even at the end of the semester or end of the year. And I think everyone will need it for their records and so on. One thing I wanted to ask you: does defining the duration of the cycle matter to the user? Because all of this seems like the user isn't really...

[00:22:18] **Ahmed Youssry:** Let me think about how to put it. Look, for example, the journey -- any journey -- has its own duration. The question is: should I let someone control it? And who gets to control it? Anything shorter than that would mean something is off.

[00:22:44] **Shereen Sayed:** The minimum would realistically be a semester. A semester, that makes sense.

[00:22:49] **Ahmed Youssry:** Yes. Exactly. So I'll build it that way, and the school admin will have control over the settings.

[00:22:55] **Nermeen Ahmed:** Your default would be one semester. But you could also make it a full academic year.

[00:23:03] **Shereen Sayed:** The difference will also be in the definition of the semester itself, because semesters vary by context. Yes, but we can set the semester for the US. Even then, I think we'll discover differences -- from county to county the semester might differ. Not just from county to county, but from state to state. In Georgia, for example, the public schools roughly start and end at the same time. But private schools in Georgia are different from New York, from Boston -- every area has its own calendar.

[00:23:56] **Nermeen Ahmed:** But the difference between public and private schools...

[00:24:00] **Ahmed Youssry:** So this is good that you brought this up, Shereen. I'm learning something I didn't even know about. But this means the contracting should be in the school's hands, and there would be like a preset template that comes back -- is it set up correctly for you, yes or no? Something like that. And if someone thinks it needs to change, there'd be an action item telling the admin to update it, since they have access. Or tell the principal. Okay. So what happens in the repeating cycles? And the reporting? I have a model for the contract, and you'll review it.

---

## Part 2: Reporting, Privacy & Assessment
**[00:24:51 - 00:51:42]**

---

[00:24:51] **Shereen Sayed:** Two things. There are things catching up with us. But before we get into what we need to do -- I want to make sure that the rest of what you mentioned is hidden. Meaning, will someone enter it and then nobody will say "you're close to such-and-such"? You understand what I mean? It's just so she understands the content, but what exactly will be visible?

[00:25:27] **Ahmed Youssry:** Agreed, but why wouldn't you want to show that you've completed, say, eighty percent as part of the reporting? This brings up the question: what will the reporting show? You understand? Because in any system that works like metrics -- whether it's the beginning of holacracy or something -- reporting takes up a chunk. Right? So the question here is: could there be a "what did the student do?" section -- even retroactively? Even if it's days later. Because "what did the student do" could be something people fill in themselves. So how? They could put in something like a status update on how the student is doing, their performance, and so on. And now, their performance--

[00:26:21] **Nermeen Ahmed:** At the end of the day -- we're here with you. Don't stress yourselves. I'm not--

[00:26:28] **Ahmed Youssry:** I'm not stressing you out. I'm asking you questions because you might see these as barriers. Yes?

[00:26:32] **Nermeen Ahmed:** I mean, someday I want us to do this. This could be a line item that gets placed in--

[00:26:37] **Shereen Sayed:** How? And it will be what? No, this isn't just a line item -- this is a big topic. Because the school tracks things -- let me go back to the business side. In our relationship with the school, the school tracks things in a certain way. Let me talk about America specifically. So this topic is big and deep and touches everything. If we even get a whiff of this -- what it is -- no. There's history and context loaded with incredibly negative emotions. Let me tell you, there are so many people working on this issue of data -- they've become data, they've become numbers, and all of that. This isn't something new. What we want to do is something positive toward the teachers. But it won't be received as positive until it's actually done right. And I can't guarantee it's done right from self-reporting. In the end, they'll just put in the numbers from the LMS -- which have no real base. So to do this properly, it's not just research -- it requires effort, it requires a second app.

[00:28:39] **Nermeen Ahmed:** Okay, that's probably true actually.

[00:28:44] **Shereen Sayed:** At that point, we'd actually need to build a second app that does assessment in a different way.

[00:28:50] **Nermeen Ahmed:** This is the point where we'd be bridging between research and practice. Currently there's a big gap.

[00:28:58] **Shereen Sayed:** People say so many things about how assessment should happen, but what's actually being measured is garbage. That's it.

[00:29:08] **Ahmed Youssry:** Okay, I understand now. So we're not going to include the student performance piece in any form right now. Currently -- you meant something, and that was the question coming up? Okay. Per teacher -- what counts? And what constitutes the relationship between them? Because keep in mind, in this system, what is it? Let me explain something. You have two levels in the district or the larger organization. There's the admin who sets the district-level settings -- meaning the group of schools. So if, for example, we contracted with a Palestinian company, someone in that company configures all the schools. Right? So there's that level, and there's someone who takes responsibility at the next level -- that's the director or whoever. But there are two roles at the district level and two at the school level. At the individual school level, there are two similar roles: there's the admin who manages things under the school, and there's the school leader -- the principal. So it works like this, right? Or one of them, or the head supervisor, or whoever -- any number of people. They all take on roles -- one person might take a role -- but they are the ones. You understand? And there's a level below that. The relationship between these levels -- some things will be reported at the school level, some things will be reported at the district level, and some things are not allowed to be reported -- they're private. So privacy is important. But what's the breakdown of these things? I know this question is big, but I want to think about it at the principle level. When we build the data models, you'll see the details and can say yes or no. But right now I'm thinking at the principle level. Understood? So for example, I now understand from you that any conversation between--

[00:32:01] **Shereen Sayed:** --is not going to be reported. But for us to put numbers and say "because of this, that happened" -- that's not an easy leap and needs to be thought through carefully.

[00:32:24] **Ahmed Youssry:** Let's set aside that second piece. I'm talking right now about the contents of coaching. Meaning, today -- will I report to the school that the teachers completed coaching sessions? That the teachers gave a certain rating for the coaching journey?

[00:32:42] **Nermeen Ahmed:** I think this question needs to be asked to Karen. We need to ask: what do you all report on?

[00:32:51] **Shereen Sayed:** Tessa suggested that we do a survey or something.

[00:32:55] **Nermeen Ahmed:** We actually don't know. Of course there are things we've left aside, but this is a question that should be asked to the person doing this work. You're the coach -- what do you report to the school on? And what does the district get from the school? We need that person to answer us. But it's good -- I feel like this opened up something. When we say we want engagement and we're going to ask them about what else, and so on -- these are things we should ask about.

[00:33:24] **Shereen Sayed:** Yeah, I'm thinking that instead of just asking Karen, we could create a questionnaire.

[00:33:31] **Nermeen Ahmed:** A set of questions.

[00:33:35] **Shereen Sayed:** That we want to ask, because she could also help us circulate it among other coaches working in different areas. We also don't want to limit ourselves to just math.

[00:33:47] **Nermeen Ahmed:** No, of course. I understand from her that she'd also connect you with others.

[00:33:52] **Shereen Sayed:** Exactly. So if she brought together the people at that school -- if we focused just on the school she works at -- that could be nice. We could put together diverse questions covering the fundamental things we need to understand. About how they work, how they report. For example, in our experience, we tied coaching to assessment. But to tie coaching to assessment, we had to create high feasibility or transparency around what's in the assessment. So we would break down, for example, an evaluation -- just as an example. Say in the school's evaluation form there was "active learning" and within active learning there are various behaviors. We'd take that behavior into coaching and break it down further and further, so we could tell her, for instance -- so that in the end we could say "you have the skill of using diverse resources." Let's break that down in coaching and say this means, for example, you use and create things -- you produce diverse resources consistently, you make resources that are connected to the objective, and you feel the difference it makes with students. Then we'd develop observation tools to help us see this in different forms. We'd keep breaking things down so that when we finally say coaching helped her grow, she'd have progress on that assessment line item. This takes effort. And I won't pretend we didn't try different paths, but it's a lot of work, it's a lot of breakdown to actually get there. And in the end, some of them still felt they were treated unfairly -- that they were wronged.

[00:36:20] **Nermeen Ahmed:** I want to remind you of something here. Do you remember the types of coaching? The "comprehensive communication" [?] that goes to the school administration?

[00:36:29] **Shereen Sayed:** I remember -- it was called "comprehensive communication" [?].

[00:36:30] **Nermeen Ahmed:** They had names like that in Arabic. There were indeed things -- but what we did in the revised version, we're the ones who put in what gets reported. But ultimately, we defined it for the school. So we're the ones saying: some things will go to the administration and will be part of your evaluation, and the principal will know about them. And some things the principal won't know about. And they used to agree on this upfront, right? There was that section at the beginning -- the coach and coachee had to agree on what gets reported and what doesn't. We were responsible for the school. I'm just saying -- the approach matters.

[00:37:34] **Ahmed Youssry:** Today, I've generated -- right? -- from the interactions between the coach and coachee. So there's the fact that they met. This could show whether someone attended or not. Right? Attended or didn't attend. But could there also be something about what happened? And whether they're happy or not? Whether their relationship is working or not? In other words -- let me give you a clear example. Is the coach-coachee relationship like a patient-therapist relationship, or like an athlete-coach relationship where the coach is preparing them for the national team? Where the coach says "this one is ready to play"--

[00:38:30] **Shereen Sayed:** --or not ready to play. It varies, yes. But this is something -- as Nermeen was saying -- look, you're asking complex questions, so I'm going step by step.

[00:38:43] **Ahmed Youssry:** I'm sorry, but I'm building on the work you've already done, and the problems in it are complex problems. Since they're genuinely complex, they need to be asked together.

[00:38:56] **Nermeen Ahmed:** I'm thinking about this: transferring the experience from reality to the app -- that's exactly the challenge. We put everything from our real-world experience into it. Now translating that to the app brings out--

[00:39:07] **Shereen Sayed:** --things. Yes. So the first part about what they report: if I understand correctly from Karen, they basically don't report much. They report attendance, like you're saying, because even in the interview sheet -- if the school claims it's not receiving support, they go back to the sheet and see that she did, say, a certain number of coaching sessions or whatever. Now, what we're doing -- that's why I was telling you this needs further breakdown. I'm not sure how we'll do it. But the idea is: yes, we've separated out the student piece. But now, am I going to report on the teacher's performance? Connecting teacher performance -- that's a big shift. And that shift is connected, as you say, to privacy. Because privacy can either help or hurt. What's the benefit of keeping everything private when I'm making progress? Or even if it's not about making progress but about putting in effort -- showing up, doing things, trying -- why hide it? That's a strange context, you know what I mean? So even when we designed the types, it was related to the fact that we -- the teachers -- it's very much related to the context. These teachers -- because we were outsiders, they saw us as -- you know, like people in a village seeing people from Cairo as strangers. So there was this dynamic of "you're watching us" and so on. We needed to create spaces for the coaches to protect the teachers. Now, coming back to America -- they don't have this issue. The teacher is actually supervised -- the context is different. The teacher cares about their own interest, and their interest is showing the effort they're putting in. Maybe they're not even difficult with Karen because this dynamic doesn't exist. Because in the end, she's coaching me, I'm improving. But then there's another supervisor who sees me and gives me feedback, and that's disconnected from the coaching. So maybe -- God knows -- I'm dealing with this person and trying, but in the end, others come and tell me different things, and she's not defending me and nobody asks her. So then what? So I'm not--

[00:42:27] **Nermeen Ahmed:** I'm thinking -- if there's information we'll learn from this, that's one thing. But this is part of the agreement with the school itself. Excuse me -- when we work with the school, what's their system? I remember the woman in Saudi Arabia from that company who was talking to me. They -- in the end -- for example, they might have something. The school has people coming in -- not coaching exactly, but ultimately we need to measure whether progress happened or not. So it could be that the tests they complete in their courses say something. But she tells me these are just information -- for us, they don't assess anything. Because in the end, someone could just get the answers from the internet. And there's another thing -- there could be observations. They do another layer of observation that's not the coach's. They go and actually observe, right? They're not necessarily observing--

[00:43:20] **Shereen Sayed:** Of course, at that point they might not even be observing--

[00:43:22] **Nermeen Ahmed:** --the skill that was worked on in coaching. They might be looking at random things, right?

[00:43:28] **Shereen Sayed:** Exactly.

[00:43:29] **Nermeen Ahmed:** So that could be causing the problem. The third thing -- when we were thinking about reporting, the entire goal was that the report goes to the teacher. The teacher gets something that tells them what their journey concluded with, or what their progress was each month or each semester. And ultimately, that's a report. We actually didn't think at all about -- I mean, when you said "report," you meant reporting to the administration, right? Or to administration in general?

[00:44:03] **Ahmed Youssry:** I'm telling you, there are several types--

[00:44:05] **Nermeen Ahmed:** Of reports.

[00:44:08] **Ahmed Youssry:** My personal one--

[00:44:09] **Nermeen Ahmed:** --goes to the individual. It doesn't go to anyone else. So that's something that might need to be in the contract or in the agreement with the school. We don't know if the school will want to see this or not.

[00:44:20] **Ahmed Youssry:** What do you think? What if -- small thing -- I'll make a suggestion and tell me what you think. In every cycle, there could be a prompt--

[00:44:50] **Shereen Sayed:** But this won't be trusted. Unlike a situation with no intervention -- where there's no full report. That's different.

[00:45:00] **Ahmed Youssry:** Meaning what? I don't follow.

[00:45:02] **Shereen Sayed:** The prompt itself -- having it there implies mistrust. Like, as a school principal, I won't trust what they wrote together. I'll trust what comes from the app. Even if the app pulls from what they wrote together. You understand? I don't want what they exchanged and discussed with each other. I want what comes from the app -- the objective part. What they discussed together is subjective. They think very much this way, even if it's an approach we want to move away from. We want to help them -- facilitate -- without touching this sensitive area too much. I don't have a complete picture.

[00:45:56] **Ahmed Youssry:** Okay, I'm forming my understanding as you talk. So let me reconcile things for you.

[00:46:01] **Shereen Sayed:** What you're saying is very important, you know?

[00:46:06] **Ahmed Youssry:** Because what we want today -- this is a relationship between three parties. Three: administration, coach, and coachee [?]. How does the administration trust the coach? That's the question. If the two of them have an interest in covering for each other, as you're saying, then it's real. We could have transparency with one party, but maybe it's not as transparent with the other. And there could be an element where -- you're in administration, and the administration might have a technique for validating progress. Meaning, how does the administration validate that progress is happening? We're not interested in the middle details. But we're saying: this person wasn't succeeding in a certain area and now they are. How does that happen? In the school, this could be a question that gets asked. You understand? But if we included best practice evidence--

[00:47:27] **Shereen Sayed:** You're overcomplicating it. Let's not add roles. We should work within the existing system.

[00:47:33] **Nermeen Ahmed:** What does the system do? And what does it want from us? That's what I'm trying to ask.

[00:47:39] **Shereen Sayed:** I agree with you. That's why I'm asking. And I started envisioning that they come from--

[00:47:47] **Ahmed Youssry:** They come, or could also -- look at it from their perspective, using their terminology. What is it -- external? This is something I've been thinking about. The idea that someone -- I mean, within the system, I need someone to represent the administration alongside me. You're not going to--

[00:48:08] **Shereen Sayed:** You can't tell them "do this for me so I can do that." They won't, and you'd be imposing on the existing structure.

[00:48:20] **Ahmed Youssry:** What I understand from the existing structure is that the assistant principal sometimes comes in to observe.

[00:48:28] **Shereen Sayed:** And as Karen mentioned, the district math people come in with their own forms, go in, use them, leave, and write the report on the teacher. And this usually happens -- if I understand correctly -- twice a year. And the general impression of the teacher comes from these reports, which happen in basically nothing. What we're offering is a more consistent approach. We could give the district observer a specific form or give them a history they can build on. Meaning, we could improve the observation process, but we can't dictate it. That person comes in on their own schedule, with their own process -- we can't control that.

[00:49:19] **Nermeen Ahmed:** I have a thought, but we won't need it right now -- I actually need to go pray. But here's where we are: we're building the coaching report for the coach and coachee -- that's our application. But in our application, are we responsible for reporting to the administration and the district? How would that work? From what Shereen is saying -- and I was thinking about this before we asked -- they have their own way. They come in to observe, and they write things up. Our app isn't solving their problem. We're not saving them time on their reporting or their observation process, right? Those are their processes that get done anyway. Yes, but now you're entering the territory of solving their problem -- saying "I'll save you time" or "I'll give you an easier or faster way to generate the report." But the application doesn't do that. So let me leave this as a small question for reflection, because I need to step out.

[00:50:19] **Ahmed Youssry:** Okay, let me add a question to yours. If there's a form that the district or senior administration uses for evaluation -- could that form also be used by the coach in the contract? So that every time we fill out this form, it's as if it walks us through what they're thinking about, and we build on that? Meaning, it could work alongside us. If it--

[00:50:56] **Nermeen Ahmed:** Actually, they should already know it -- they do know it -- it comes to them.

[00:51:00] **Ahmed Youssry:** Right, but it's not part of coaching. So we could have the coach fill it out on their own. Maybe that's something the coach does -- filling it in and validating their own development while building on what comes from the district. So it's as if a journey is happening. You understand? And everything builds on what the administration uses. It doesn't have to be something they actively do, but it reflects their development on each metric. So when the administration comes in to do their thing, they could validate the final result if the teacher reached it.

[00:51:42] **Nermeen Ahmed:** I'll just say this -- this is something additional in Rafiq that solves a second problem. Okay--

---

## Part 3: Platform Architecture & Data Models
**[00:51:52 - 01:16:47]**

---

[00:51:52] **Ahmed Youssry:** Alright. This topic -- it will reach you. A few minutes and I'll be back, God willing.

[00:51:58] **Shereen Sayed:** Okay, fine. Let me just -- give it a few minutes so we can wrap it up.

> *[00:52:00 - 01:03:30] -- Break / Audio artifacts / Participants away from microphones*

[01:03:32] **Shereen Sayed:** Yes, we can hear you, but we're still cooking.

[01:03:37] **Nermeen Ahmed:** I'm also cooking, so it's okay. When they're done, I'll get up and use the oven.

[01:03:50] **Shereen Sayed:** The oven? So you didn't cook?

[01:03:53] **Nermeen Ahmed:** No, I'm just cooking now.

[01:03:56] **Shereen Sayed:** Who's cooking?

[01:03:59] **Nermeen Ahmed:** Mom.

[01:04:01] **Shereen Sayed:** Mom -- on a whole different level.

[01:04:05] **Nermeen Ahmed:** Mom already has food ready, so it's fine. We just say no, right?

[01:04:11] **Shereen Sayed:** No, of course. We take everything and then say no, we made it ourselves.

[01:04:15] **Nermeen Ahmed:** Exactly, it's nice, and then she stays for a few days after that.

> *[01:04:23 - 01:06:39] -- Personal conversation (family, moving, adjusting to new living situation)*

[01:06:39] **Nermeen Ahmed:** Alright, should we go back to the topic or what are we doing?

[01:06:49] **Shereen Sayed:** Yeah, we can go back. But hold on -- you're not recording, are you? Okay, okay, let's continue the conversation. I was parsing the map.

[01:07:01] **Nermeen Ahmed:** Very saturated. Tell me, I can see you.

[01:07:06] **Shereen Sayed:** Okay, the point he was making raised something I think we haven't thought about: what's in it for the school or the district? What is it? We have to figure it out. And the coaching is great, but there's nothing they would benefit from. And if everything is private, they'll tell us, "Go ahead, play somewhere else."

[01:07:38] **Nermeen Ahmed:** That's exactly what I was saying.

[01:07:43] **Shereen Sayed:** Right.

[01:07:44] **Nermeen Ahmed:** Yes, say it again -- say it again. The app benefits the administration and the district how?

[01:07:50] **Shereen Sayed:** Exactly. So this makes us think that there are several reports depending on the map I'm thinking about -- or trying to work through.

[01:08:06] **Nermeen Ahmed:** There's a progress report.

[01:08:08] **Shereen Sayed:** For the teacher and the coach about the coachee -- that's what we already thought about. But that's all we thought about -- exactly. But what we still need to think about is the performance report. The performance report -- is it, for example, possible to go and do the -- you're on mute. Are you on mute? Yeah. Stop the audio or stop the video. I can hear myself echoing. So is this about teacher performance, coach performance, or both? So for each of them -- or I don't know right now how to draw the line between the two of them going back and forth. Let's look at the school and the district, right? So do they get the performance report, or do they want additional reports? I mean, the district -- I've placed it in -- and what report do they want about whom? And the same thing for the school. The school will definitely want the performance report, but do they have other needs? So this has become -- okay, the question came up about -- yes, we need to validate this. Of course. But until we do -- is it worth thinking about? I mean, it's not necessarily that we will validate, but it's already -- I mean, we just need to think about what they want. What's blocking this?

[01:10:12] **Nermeen Ahmed:** Do you have more questions, or is this the last one? Actually, this could be the last question.

[01:10:15] **Shereen Sayed:** I mean, all of them. Honestly, if you figure out this piece, you've pretty much answered about twenty-nine percent of my questions. There will be remaining things, yes, but honestly those are -- simple things. But this is the core of what we'll produce. I know what I need to input. But I don't know what we're going to output. So I don't know based on what will come out.

[01:10:50] **Nermeen Ahmed:** I have some preparations and I still need to talk to Jed [?] and so on. So let me just check on the time.

[01:10:55] **Shereen Sayed:** You need to talk to Jed today -- not necessarily today. You could send him a message and say let's do it another day. What do you think? Actually, I still have two questions for you before -- after we finish this. Not too many questions, but we need to finalize the scheduling topic, and we need to agree on a meeting time because Cali sent me stuff.

[01:11:24] **Nermeen Ahmed:** We need a post on Meety [?]. Okay, those are things I can handle.

[01:11:38] **Shereen Sayed:** Another hour. Is that okay with you? Just so that --

[01:11:41] **Nermeen Ahmed:** All of us. Yeah, one more hour. But actually, I've been working since this morning today.

[01:11:45] **Shereen Sayed:** Oh, okay. We need a quick recap of your points.

[01:11:56] **Nermeen Ahmed:** Since this morning. So by now I'm pretty drained. Yeah.

[01:12:13] **Shereen Sayed:** We haven't discussed our scheduling topic. I felt like we keep leaving this topic open. But let's talk to each other one-on-one about our schedule.

[01:12:34] **Nermeen Ahmed:** Have something we need to prepare for the meetings, right?

[01:12:43] **Shereen Sayed:** The notes. Sorry -- because Nermeen asked that question, it made me feel like we need to think about all the things besides those. We said this is what we were supposed to do today. And we need to set aside time. The refuel plan -- I was supposed to do that.

[01:13:39] **Nermeen Ahmed:** Where exactly? And that's why the task from last week, when he asked us -- we said there's just teacher and coach, that's it. But the topic turned out to be deeper and more complex.

[01:14:09] **Shereen Sayed:** Wait, but we have in the alpha -- tell me. Show me in the thing you created. You wrote some details -- like what you told me you put in. What you had was like -- there should be -- I'm supposed to have done two things: active learning --

[01:14:50] **Nermeen Ahmed:** But measuring progress itself is a different thing.

[01:14:57] **Shereen Sayed:** That initially, it's like this -- how many did I do? We can break it down a bit. How many sessions with the coach, and how many sessions solo? And in the end, how many total sessions are supposed to be -- like if there are ten per month. Yes, exactly.

[01:15:24] **Nermeen Ahmed:** And put my focus area. And maybe in the progress report -- I worked on --

[01:15:30] **Shereen Sayed:** If that's the focus, then it becomes "I worked on this area." Right? So that's that. Should we also put how many -- I was thinking it doesn't all have to be about observation.

[01:16:04] **Nermeen Ahmed:** Yeah, but that other part -- what is it measuring? I mean, the observation part, they have their whole thing, so that could be something. But that's a whole document on its own, you know?

[01:16:21] **Shereen Sayed:** This is too broad. What is this measuring exactly?

> *[01:16:41 - 01:17:56] -- Meal break / casual conversation*

---

## Part 4: Session Structure & Observation Tools
**[01:17:56 - 01:40:24]**

---

[01:17:56] **Nermeen Ahmed:** Okay, so he's doing reviewing for them. Keep in mind, this is something we set up -- if I want feedback, if I want edits and suggestions for edits and things like that, I submit it to be reviewed according to the goal I'm working on.

[01:18:14] **Shereen Sayed:** Exactly.

[01:18:15] **Nermeen Ahmed:** This is something else. It's not a bad thing and it's not a big deal -- it's just a different thing.

[01:18:22] **Shereen Sayed:** I'm trying to think what else there is.

[01:18:25] **Nermeen Ahmed:** What we opened up for them, or what we were saying in the lesson plan scenario -- they submit their lesson plans back in a general way, not tied to the specific goal they're currently working on for their development.

[01:18:38] **Shereen Sayed:** Wait, wait -- because people forget, of course. You were right about that.

[01:18:47] **Nermeen Ahmed:** You were right about that. People do forget.

[01:18:50] **Shereen Sayed:** Validation based on the lesson plan validation. So we have a template -- an AI chatbot that provides feedback using a sandwich approach. Feedback is provided. We haven't yet added lesson plan validation to the document. We gave them an action item to skip our feedback and everything. So that means they could do it that way. So the data -- should the focus of the bot be on lesson plan writing quality or learning experience design quality? For learning experience design quality, we've built something, but we could pull options from it. Like, do you want to review the lesson plan overall, or do you want to focus on, for example, teaching strategies, or whether the activities are learner-centered, and so on. We have criteria for everything -- there are a lot of things. We could make them options, or we could have it give automatic feedback on all the criteria.

[01:20:24] **Nermeen Ahmed:** Okay, so when we break this down in terms of performance, is it based on the criteria we set for the lesson plan, or is it what you were saying earlier -- that I'm focused on the goal I'm working on, and I want to see whether my lesson plan is actually aligned with the goal I'm developing toward? That's a nice idea. I'm not objecting to it -- I'm processing. I'm thinking.

[01:20:50] **Shereen Sayed:** Think about your question. I'm also thinking this through. Not objecting.

[01:20:59] **Nermeen Ahmed:** I'm on board. I'm focusing on the goal. I really want to develop. So I'm focused. I mean, I'm flagging something -- but it's related. The question is --

[01:21:10] **Shereen Sayed:** Your question is what's making me go back and forth between whether we keep the full criteria or hand it off [?].

> *[01:21:21 - 01:22:23] -- Meal break / casual conversation about food*

[01:22:41] **Shereen Sayed:** Your question is making me think about the idea of -- well, let them choose. They're coming back to the lesson plan based on what, exactly? But also, at the same time, if they keep coming back, that's an indicator of engagement. But we could also add the idea that they come back based on specific criteria -- like, I'm interested in my goal, or I'm generally interested in developing my lesson plan.

[01:23:30] **Ahmed Youssry:** I mean, develop it -- for example, if a teacher is trying a new technique, right? We want to generate feedback so they can improve and try again next time. Right? If the administration itself is trying a new curriculum and wants to coach and also get insights on what else can be developed, then that could be --

[01:24:05] **Shereen Sayed:** Where does that go?

[01:24:12] **Ahmed Youssry:** All of these details could be -- good point, you were about to say something, Shereen. These details could all live in the initial agreement, not just the agreement itself. And in what we bring in to train people at the school on a new technique --

[01:25:03] **Shereen Sayed:** A new whatever -- do you understand that? This is something I haven't heard about before here. What do you mean? How? Is this something you feel is happening, or something you've seen? Because I haven't heard about it here.

[01:25:24] **Ahmed Youssry:** What I understand here is that anything new goes to one of the existing things --

[01:25:30] **Shereen Sayed:** They don't assign new people to it. Anything from the existing things -- they don't assign new people. Whatever comes down goes to the language people. So the language coaches get involved in the work. No one gets added. They don't add people unless it's a brand new subject.

[01:26:17] **Ahmed Youssry:** An addition on top of math and science and whatever else, as a way of thinking. So how do you use that to develop a deeper understanding of AI? It applies to the same --

[01:26:30] **Shereen Sayed:** Subjects. But using new techniques. So that means there's no one --

[01:26:39] **Ahmed Youssry:** To assign to this. Yeah, but what do they do, Shereen?

[01:26:56] **Shereen Sayed:** Don't infer. If you're thinking from a logical standpoint, not everything you'll find applicable to the structures of this type. I haven't heard about anything like that. All the teachers I meet in the classes and trips -- I haven't heard anything about it.

[01:27:32] **Ahmed Youssry:** How are they using it to develop? If schools are using it --

[01:27:43] **Nermeen Ahmed:** That's a mistake. But I feel like, in the end, the information we were talking about earlier -- the progress that you were telling me about -- this would be the information they want. Basically, how many sessions you attended, what you worked on, and so on. These are things -- information we can easily pull from the app, and there's nothing problematic about it.

[01:28:06] **Ahmed Youssry:** But I think there's a poverty of imagination here in educational administrations around technology. And that's what we could be offering. Why are they tracking this? Because they don't know how to track anything --

[01:28:26] **Nermeen Ahmed:** Else. That's all. My technical question now is: is this something for after the MVP, maybe even after people start using the app? Or is this something we do now? Because this would track them. Like what you arrived at earlier -- they're the ones paying, so they'll pay. So where does this fit in the stage we're at now? This is a bit aggressive -- we're solving a problem and we know how to solve it. It'll save time, save effort, save the coach's time, the coach can take on more sessions -- all of that is great. And then there's the data that comes out. The app will generate data for you -- what kind of data? And how will that data help you? And here we cite what you said, Shereen, about when we sell in Egypt -- you're solving a problem and at the same time you're changing mindsets, making them think about coaching as something important. That's a separate thing. So here too, you're trying to make them think, "This app will generate data for you, and you need this data." I'm going to change things for you. I'm convinced. But I'm just trying to organize our thinking here.

[01:29:47] **Ahmed Youssry:** What do we need to do? The idea of data -- a company comes and does data work. Let's set that aside since -- but it could be something we say we want to reach toward.

[01:30:41] **Shereen Sayed:** About coaching. They're already hiring coaches anyway.

[01:30:46] **Ahmed Youssry:** Are hiring coaches. But as we can see, in the world there are people who are convinced and many who still aren't. So are you convincing the rest of the world -- look, if you're making life easier -- all the talk becomes the main thing. And our human mission isn't just to make money. It's to improve the process of education. So we want it to be fundamental -- for what purpose? Are we building a tool just to save time? Or so the coach can actually be effective? How do we make sure the coaches who are hired are actually effective? Right now, we're putting a lot of weight on the coach.

[01:31:41] **Shereen Sayed:** The kids. After all this -- they worked on it, worked on how many things, worked on which skill, came back -- all of that also plays into it. To see the performance, this means that if we take the teacher's performance report, there needs to be something related to teaching -- teaching skills.

[01:32:58] **Nermeen Ahmed:** As practiced.

[01:33:01] **Shereen Sayed:** In the classroom. Not as reported. So there's teaching skills as practiced and teaching skills as reported. Meaning, the teacher says "I've gotten better at doing X," but there's someone else -- what you were talking about -- that someone else --

[01:33:20] **Ahmed Youssry:** Which is --

[01:33:22] **Shereen Sayed:** Doesn't have to be external. It could be the coach.

[01:33:26] **Ahmed Youssry:** External to the journey. Yeah, it could be the coach themselves.

[01:33:31] **Shereen Sayed:** Not just the coach. We could take the cues from observation reports. Observation reports are performance reports, right? But we don't typically use them in a certain way. If we took them and extracted other things from them, that would be great.

[01:34:02] **Ahmed Youssry:** A vision. Okay, let me share a vision with you. It works for the MVP -- not final, but this is a very important point. First, I want to comment: it's clear that we need to understand the method better, because we're not able to articulate it easily. The things we understand, we can write down and express. The things we don't understand, we get stuck on. So we even need a deep dive into everything they do. What I want to say is that the agreement should include something like self-reported pre-coaching standard. At the beginning, when we're setting up the agreement between coach and coachee, we say, "Where am I at? And where do I want to see myself? What am I doing well?" And then every cycle, we report in the progress report for the administration, for the teacher, and the coach reports the whole semantic report. And at the end, when we say "share a success milestone" -- they share, but it's not really a report. They're showcasing their work for something positive. So we encourage sharing. But there's a distinction between the observation cycle where you're developing in the formal way, and --

[01:37:46] **Shereen Sayed:** I'm not sure we can control that. But let's unpack what you're saying. You're saying that we'll do pre and post, right?

[01:38:02] **Ahmed Youssry:** The progress report -- but the report at its own time. So there's a report that says, "Where are we?" It will be based on -- what are you trying to develop? So where are you at with that thing right now? And then they share their perspective on it.

[01:38:47] **Shereen Sayed:** You're basically saying that we'll get input about --

[01:38:55] **Ahmed Youssry:** The teacher -- not about both. About the teacher from the teacher's perspective and from the coach's perspective.

[01:39:07] **Shereen Sayed:** That won't be pleasant because it'll create tension in the relationship. It's not great to have the coach come in and evaluate like that from the very first moment. Maybe we skip the coach's part and just have the teacher say --

[01:39:28] **Ahmed Youssry:** "Where am I?" Something will come down, regardless of its level. To facilitate that.

[01:39:36] **Shereen Sayed:** And that assumes we're doing something, and we're not doing something [like formal evaluation].

[01:39:42] **Ahmed Youssry:** The progress. You said people want to receive it.

[01:39:50] **Shereen Sayed:** We don't have anyone doing that. We don't even have that process. Things need to be aligned together. That's why I'm saying the link -- what you have in mind is the ideal --

[01:40:02] **Ahmed Youssry:** I'm saying at an MVP level.

[01:40:03] **Shereen Sayed:** And I'm not saying --

[01:40:07] **Ahmed Youssry:** That you're wrong. And I said first that this is innovation we need to work on. At the MVP stage, having even a subjective assessment is better than nothing at all.

[01:40:24] **Shereen Sayed:** The pre and post is a quantitative approach. Fine. If we're going to do something else, we don't need pre and post. We do something else. Alright, let's discuss --

---

## Part 5: Coaching Cycles & Progress Tracking
**[01:40:38 - 02:06:23]**

---

[01:40:38] **Ahmed Youssry:** What else is left? One more thing. Look, I'm now blending two parts together. The sales part with the product itself. Ultimately, for me to convince anyone to buy this because it's better -- a teacher versus the alternative reality -- I need to be able to tell them "you've improved." The part about where your level was and where you've gotten to -- that's a way of thinking.

[01:41:11] **Shereen Sayed:** If you'll allow me, that's the same line of thinking. It's still following the same logic that there has to be a starting point. But that's not the only thing --

[01:41:26] **Ahmed Youssry:** That you're measuring. Well, we just ran it. Yes, I understand. I didn't get it before.

[01:41:34] **Shereen Sayed:** OK. What the app will show is basically: what were you supposed to do, and how much of it did you complete? Straightforward -- did you finish the tasks, did you commit to X, Y, Z. That's what we'll call being "on plan." That gets set in --

[01:41:53] **Ahmed Youssry:** The beginning. Exactly. I just want to ask a real question now. Does the observation cycle plan get updated?

[01:42:13] **Shereen Sayed:** Usually these things -- I don't know if Nermeen agrees -- but these things don't really change at the semester level. They don't change drastically mid-semester unless something extreme happens. Changes happen at the semester level.

[01:42:28] **Nermeen Ahmed:** I feel like changes don't happen in less than a year. From experience, I mean.

[01:42:33] **Shereen Sayed:** Yeah. Like changing the number or deciding we now need to do something different.

[01:42:43] **Nermeen Ahmed:** Who would decide that? Who would say "we want to do this"?

[01:42:46] **Ahmed Youssry:** I don't know. A teacher, or a teacher says "I want more, I need more." Let me try to explain where I'm going with this. You have someone from outside. Now we have several things to measure. I'll put them into two categories. There's an administrative bucket -- how many cycles we'll do, or how many sessions enter the system. So that's administrative. The question is: who determines this? Is it determined at the school level based on resources, or at the coach level based on their view of the teacher's needs? So I can say: you had X planned and you made Y percent progress on them.

[01:43:53] **Shereen Sayed:** OK, that could work. I mean, we're thinking conceptually. The details around this -- it could be a setting for the administrator, or for the coach, or something else.

[01:44:09] **Nermeen Ahmed:** But this is also for the EA [Educational Agency], right? Karen told us that she ends up chasing after teachers because she can't keep up with meeting all of them. So there isn't a fixed number -- from what I understood from her, there's no set number of sessions required. If we set one ourselves --

[01:44:28] **Ahmed Youssry:** That means we'd be giving them something of their own.

[01:44:35] **Shereen Sayed:** Let's default to conceptual for now, and leave the details.

[01:44:52] **Ahmed Youssry:** My goal is to figure out the details now so I can start building. My purpose here is to figure out the details and make decisions --

[01:45:00] **Shereen Sayed:** But the details are making us lose focus. We've already gotten sidetracked. But anyways, if we say as an assumption that we'll set a number of sessions, and we'll validate that assumption by going and talking to people, and we put the setting either with the administrator or with the coach in the agreement -- we can put it in different places. So there would be a number to enable a progress report. We can't force this behavior -- whether it's in the teacher-and-coach onboarding or the administrator onboarding depends on the data we'll collect.

[01:46:01] **Ahmed Youssry:** So what I can do now is put something like a placeholder. Even without specifying, so it becomes something that must be answered. I'm just looking for the data model here. I need to know: is this data you'd include or not? So yes, this data will be included. I'll create a data model for the required number of sessions. And I'll give it this action: either you, the coach, or the admin set it, or if you leave it blank, or if the decision is made, it gets passed to the coachee. So the coachee says "you have this budget, what do you say?" Or it gets passed to the coachee and the coach and they agree on the number of sessions.

[01:46:58] **Shereen Sayed:** Yes.

[01:47:00] **Ahmed Youssry:** Good. So that's something that will be set and will land in the contract. Now the second question --

[01:47:08] **Nermeen Ahmed:** OK.

[01:47:11] **Ahmed Youssry:** This one is about behavioral progress. You enter the contract, you have a starting point, and you've developed from there -- or so we claim. And this could be something for the teacher to choose what to share. Either that, or when there's a requirement for it to be shared -- meaning by default it's shared with the school.

[01:48:22] **Shereen Sayed:** What you're proposing here has several -- did you hear me earlier?

[01:48:28] **Ahmed Youssry:** What did you understand from what I said? I want to check if I explained correctly or not. This topic is fundamental.

[01:48:51] **Shereen Sayed:** I understand. But let me ask -- is time an important factor? We talked about two things and you didn't hear me. Let me explain. This approach is problematic because it doesn't happen the way you're imagining. There is no linearity in a school system. In schools, the assistant principal has time, she goes in to observe and that's it. The district has their own schedule -- they have a million schools to visit, they go whenever they want, and suddenly nobody knows -- they walk in, observe what they want, and leave. There's no linearity of "wait until we give you a signal that the teacher is ready for formal observation." That's the first problem. And the second problem is that a single formal observation doesn't determine anything. The teacher might be sick that day, the students might be in a terrible mood -- it doesn't actually determine teacher performance. This is actually one of the things we want to kill -- not just reduce, kill completely -- the idea that one observation could make it or break it. Kill that idea entirely. It's like a test where you examine someone and that's it -- your life is over or you've become a person. Keep in mind, evaluations affect teachers' livelihoods -- it's a big deal, it's a big conversation. I don't know how to express this strongly enough. This is one of the things that causes strikes and problems because their salaries are determined by these evaluations. So for us to go down that path and insert ourselves into the evaluation process is a premature step. We need to embed more in the culture and enter from the angle that we facilitate something that contributes to the evaluation, not that we get into the evaluation itself. We cannot facilitate this process at all. I advise against it completely, at least for the US. This will fail -- the product will fail from the very beginning because we'd be stepping into politics we don't understand. What I'm proposing -- what Nermeen and I were discussing while you were cooking -- is that we create a performance report. Simple, easy, with no high stakes, because high stakes are problematic. This performance report can contribute to the teacher's evaluation, but it doesn't evaluate. It doesn't say pre/post. You understand what I mean? It doesn't have those restrictive parameters. So it will simply be -- I still want to explain -- I want us to start from here for a bit and come back to this point. This is what could be in the performance report without giving the edge of "we are evaluating."

[01:51:48] **Nermeen Ahmed:** And I think this relates to what we were discussing about coaching right now at this stage.

[01:51:54] **Shereen Sayed:** Exactly. It gives sufficient information. It gives sufficient information about performance but without saying "this is --"

[01:52:14] **Ahmed Youssry:** What do you think of what I'm saying? OK, that makes sense. Initially I'm convinced now about the guardrail that we shouldn't get into evaluation. This is an important discovery. So I'm convinced. I will revise a lot of what I've suggested based on this specific point. We can help by reducing the hassle of administrative reporting -- which has nothing controversial in it. This is what you were already doing with an Excel sheet; we'll give them a better Excel sheet. So that's harmless. And the progress part becomes something for the teacher. Put your own stuff and assess yourself on it. It's for the teacher, not for the coach. And let's be clear. This gives us data on what teachers want to track. So we gather around self-observed progress. We're not getting into any of that evaluation stuff, but we're preparing ourselves as a starting point for it. This becomes a space between the teacher and themselves. "I'm trying to develop myself in this area."

[01:54:06] **Shereen Sayed:** How is that different from what we just discussed? I was imagining that's what it was.

[01:54:26] **Ahmed Youssry:** Actually, my original question was about what data is what, and what's shared at which level. Because I have --

[01:54:53] **Shereen Sayed:** My data. It's my data and it belongs to whoever sees it.

[01:54:59] **Ahmed Youssry:** I'm saying it along with you all.

[01:55:05] **Nermeen Ahmed:** Your question is telling us that you're imagining what it would look like, but I don't have a vision. I'm coming to --

[01:55:15] **Ahmed Youssry:** Ask you: where is it? What is it? Simply put, when you're building any data model, you need to tell me what this data is for, what kind it is, who will see it, and who will edit it. So I need an understanding behind the data. What belongs to the teacher only? What belongs to the teacher and coach? What belongs to the teacher, coach, and school? What belongs to the coach and school but not the teacher? You understand the matrix? What belongs to the teacher, coach, school, and district? Which is everything -- everyone sees it. So that's a matrix. For any data point, you tell me what it is --

[01:55:54] **Nermeen Ahmed:** And what it looks like, how it gets collected, who has the right to see it, and who has the right to --

[01:56:00] **Shereen Sayed:** Edit it. And these things can't be haphazard or random --

[01:56:04] **Ahmed Youssry:** For any place. This is a table that will depend on commitment to it, and any data that gets classified within it becomes part of the data model.

[01:56:15] **Shereen Sayed:** And Nermeen, confirm for me -- the way we're currently designing the dashboard, it's shared between the teacher and the coach. The teacher sees it for themselves and the coach sees it for everyone. Even the coach's dashboard -- the coach sees: where is everyone? Where is each person? So they can see, because there's nothing private -- it's all the things that happen between them.

[01:56:40] **Ahmed Youssry:** The middle ground could be something for everyone. Well, it doesn't necessarily have to be for everyone.

[01:56:53] **Shereen Sayed:** If we assume that the progress report is only for the teacher and the coach, it could be shared or elevated, because why would anyone else want to see these details? No one else should need to see the details. But that's also a question. When problems arise -- when there's a problem at a school, the school complains they're not getting support, and then they go back and ask "how many sessions did you get?"

[01:57:50] **Nermeen Ahmed:** I actually have a thought. In a school our size, sometimes we've managed with just one person, but it's impossible for a school principal to be actually tracking the details of every teacher like that.

[01:58:08] **Ahmed Youssry:** Yes, but she can ask when there's a problem. The question is: will she reach a consensus, or does she ask the system and the system answers her? That's the point. The principal wants to know how many sessions the coach and teacher had together, and what their plan was to complete, and where they are on that. So sometimes -- if the approach is that she has to send and ask them, she gets the information from them. But if it's the approach where even if we don't display it by default -- we don't put it front and center -- but if she asks, the answer comes to her. Because we won't be telling her anything evaluative; we'll just tell her how much was completed. So there's a difference between the data model allowing this data and between this data being visible versus us not needing to show it by default. Isn't this a good question?

[01:59:22] **Shereen Sayed:** Very. I agree that it's fine for her to ask the system and the system answers her. These aren't secrets -- on the contrary, these are things -- what did you say?

[01:59:31] **Ahmed Youssry:** Don't think about how to report it. Think about whether she should have access to it or not. That's it. She should have access --

[01:59:38] **Shereen Sayed:** But she shouldn't have access to see the details. The things in the data like observation reports, lesson plans -- those are too detailed. In my opinion, those are the things that might require specifying. For example, there could be a report. But wait, let's come back to that. OK? My suggestion -- which I was actually thinking about while we were eating -- is that the performance report becomes a "teaching skills" report. So there's a self-reporting section where the teacher says what they see, what they've developed, or what they can now do. It doesn't have to be about development specifically -- not "I improved" but rather "I can now do X." And the evidence comes from the observations. It's reconciled -- we have a lot of data coming from the observations. Where are the observations? Not just skills -- we observe and the coach writes notes, so there's information we can extract from that data. The teacher can now do X, Y, Z.

[02:01:16] **Ahmed Youssry:** What? That's really good. I like this part. Because we could make this an assistant for writing the report. Rafiq could say to her: "OK, this is your midterm review. This is your final review. What do you want to report?" And give her sentence starters and she completes them. Done? So after she completes it, it takes that and researches what she selects from the things available. So there's a way for it to be present so you can choose what to include, and it generates the teacher's report. Like "this teacher can now do X, Y, Z." So what do we call it in the end? Performance report. Does the performance report also include a "now I can do" section? So there's a section for "now I can do" and a section for "next I want to explore." Possibly --

[02:03:24] **Shereen Sayed:** We could also add a coach testimonial section. Then it becomes a testimonial. The coach says things about me or wants to recommend things -- they recommend areas and suggest next steps. But it's no longer the coach evaluating. It becomes the coach adding a testimonial to my review.

[02:03:52] **Ahmed Youssry:** That's nice. So now we have, outside of everything they're doing on our platform -- they take it and put it into their own system. Is that good? Now a question: is it possible to add --

[02:04:17] **Shereen Sayed:** And it becomes a journey or not? But let's get Nermeen's input. No, I'm OK. I'm with you. So you think this is OK? Yes, yes.

[02:04:33] **Ahmed Youssry:** "Next I will coach," "now she can" -- what you're generating now, I'm trying to put it into paragraphs or into --

[02:04:55] **Shereen Sayed:** Are you talking about the midterm or what?

[02:04:58] **Ahmed Youssry:** I'm talking about the performance report.

[02:05:00] **Shereen Sayed:** Oh, whether it's about its timing or what its timing is --

[02:05:04] **Ahmed Youssry:** It will have: "Now I can do X." "Next, in the future, I want to work on Y." And the coach's section: "Now she can do X." "Next she will work on Y." And in any of these sections, you can attach a testimonial or evidence from something that happened.

[02:05:34] **Shereen Sayed:** OK, that's fine.

[02:05:37] **Ahmed Youssry:** And this gets generated with help from the AI. "We can add this as an evidence. Allow this part."

[02:05:49] **Shereen Sayed:** That's also something we need to think about. But let me take a quick break and come back to you. We were actually supposed to be wrapping up.

[02:05:59] **Nermeen Ahmed:** You need to talk about the things you were asking me about. We won't think about school coaching. That was a topic we had. We can discuss it in the evening.

[02:06:23] **Shereen Sayed:** But it's clear he wants to be part of the discussion. His questions are opening up new areas. That's a fact. They're opening things up, but at the same time --

---

## Part 6: Action Items & Closing
**[02:06:33 - 02:25:39]**

---

[02:06:33] **Nermeen Ahmed:** Also, when I look at this, I'm trying to remind myself -- is this part of what we're doing now, or is this something they want, or is this something that would be appealing to them? I mean, do we just mention it, or do we mention it and actually build it? You know?

[02:06:48] **Shereen Sayed:** When we say the app will do this and this and this -- but we're an MVP, so we don't need everything to be there. But actually, he drew our attention to something because it's a selling point. Because it's our unique buzz -- this could actually be the selling point. Alright then. So can we -- since Nermeen has to leave us soon, and I still have several questions for you -- can we stop here? Could this be --

[02:07:47] **Ahmed Youssry:** This could be. I just wasn't sure about that. You can go ahead and tell us the other things. I'm looking at it right now. My first question was -- okay? So it was the communication, the scheduling, and matching responsibilities and boundaries. We talked about those -- you'll go back and review them, but they're fine. We broke them down. You can come back to them at any time, but that's the foundation. What else? Let me walk through this. These are the ones in the journey together. Right. And regarding the coach cohort -- we've dropped that. Now it's a community. You join a community, either with your group, or with a group at the school, or with a group on a topic. They're communities you join. Good. The cohort is gone now, so it's become the coach -- it's become the journey, right? And the journey -- its name might change, but if they want they can take the old one and modify it. But what else? The things they want. We added your part -- that was the question I had. So we didn't know what happened with this. Understand? I had named these things, so we were messaging back and forth to reach -- I was naming them. After that, there's a step to review -- go down or do round one, round two, round three, and so on. According to the agreement. And then at the end, there's a closing cycle documentation -- what happens in it? So we agreed now that it will produce a closing performance review or closing performance report. From that comes the attendance report -- which is logistics -- and the performance report, which is self-and-coach-reported.

[02:11:04] **Nermeen Ahmed:** But you don't mean that this is a session closing -- this is what happens after the last session. What happens after the last session?

[02:11:12] **Ahmed Youssry:** After the last observation cycle, this is the report that comes out. It could be done within a session. We didn't agree on that detail -- I hadn't gotten into it. But it could include that.

[02:11:26] **Nermeen Ahmed:** And then I had -- after that was part of -- okay? And I think we --

[02:11:42] **Ahmed Youssry:** We added that. Okay? Now here's an important question: for the video uploads -- is there a limit or no limit? Why does this matter? Because this is a direct cost.

[02:12:17] **Shereen Sayed:** What I have written is -- I'd imagine there is a limit. A limit on --

[02:12:25] **Ahmed Youssry:** Per coach and a limit for self. What we haven't discussed, and one of the things --

[02:12:32] **Shereen Sayed:** But I just want you to note what we haven't discussed and what we haven't discussed.

[02:12:35] **Ahmed Youssry:** Two things. Are we including it or not? And what do they want to see? I mean, what does the district want to see? Do you understand? What does [the district] want? Exactly.

[02:13:01] **Nermeen Ahmed:** That's what was in my questions. I mean, questions for the people [stakeholders].

[02:13:08] **Ahmed Youssry:** I don't have answers to these. But still, this can be -- what data goes to them and what report goes out to the district -- we'll wait on this a bit. But it's needed.

[02:13:21] **Shereen Sayed:** Okay. And the performance report for the coach -- we also need to mention that. And that came out of --

[02:13:32] **Ahmed Youssry:** From our session today. I didn't have that before.

[02:13:36] **Shereen Sayed:** Right, okay. But I want to draw your attention to the idea that the closing session you mentioned -- it's not currently in the flow. There's no scenario for it, so --

[02:13:58] **Ahmed Youssry:** Do you want me to add a scenario for it, or should I remove it on my end?

[02:14:01] **Shereen Sayed:** That's a testing question. When we test, we'll see. But in my opinion, we don't need it right now. I'll put it in the testing --

[02:14:22] **Nermeen Ahmed:** Questions. Okay. But we do have a complete run. But that makes sense.

[02:14:30] **Ahmed Youssry:** So the teacher and the coach finish the journey -- they don't sit down and say, "What are you going to work on in the future? Are you happy with what happened?"

[02:14:52] **Shereen Sayed:** Will the app do that? I mean, will the app be doing something at that point? The app generates --

[02:14:58] **Ahmed Youssry:** Right. So the app will -- do we want them to do it, and should the transcript of that discussion contribute to it? Possibly.

[02:15:16] **Nermeen Ahmed:** I mean, this should be something between them, not just with the -- it's supposed to be --

[02:15:21] **Ahmed Youssry:** But should they do it, for example, after the report is generated and they come back to discuss it? The system could tell them, "This was the last session that happened. Here's what I envision for the performance report."

[02:15:41] **Nermeen Ahmed:** The teacher goes through it. I'm starting to share data now. I'm remembering -- because we used to do a closing session. There would be something prepared, right? They'd look back at what happened throughout the term or the year and so on.

[02:15:59] **Shereen Sayed:** I don't remember exactly, but I'm trying to connect with what they're saying --

[02:16:06] **Nermeen Ahmed:** The point is the report comes out. So we're discussing -- I'm trying to understand if there's --

[02:16:13] **Shereen Sayed:** Something the app needs, or something that needs to improve in the app, or is this just asynchronous and each one will have its own flow. And then we settle and decide what to include. But I'm trying to understand what's in the app.

[02:16:36] **Ahmed Youssry:** We need to think about this. Okay. This raised more questions. Those questions will be deferred.

[02:16:45] **Nermeen Ahmed:** Just a small one. Do we do this or this or this?

[02:16:51] **Ahmed Youssry:** The things that come out of this: do we ask them to submit the recording of the session, or does the AI tell them "I'll attend with you and process it," or the AI -- actually, that's not great. Because this doesn't exist at all yet in the features -- we've talked about it but haven't written it into anything. So the AI can't find it in the features; it's not in our current scope. So it becomes just documentation.

[02:17:44] **Shereen Sayed:** The one you pulled from -- that's from what we haven't been capturing, unfortunately. I know.

[02:17:47] **Ahmed Youssry:** What we've been doing in the rounds is that I update the base based on our conversations and on what happens with you. So there's still a task for me to update all of this. I'm doing that. I'm just asking because this is something that might have fallen through the cracks.

[02:18:03] **Shereen Sayed:** We did talk about it and you told us it was a duplicate. But I forgot. That's all. It's not in the -- I'll definitely forget.

[02:18:14] **Ahmed Youssry:** Okay. How does this happen on Zoom? Shereen -- in Arabic or English?

[02:18:22] **Shereen Sayed:** The Arabic portions come out in Arabic, and the English portions --

[02:18:30] **Ahmed Youssry:** Come out in English. Great. Send me that then. I don't want the recording. I want -- do you send it every time? No, you send the summary.

[02:18:42] **Shereen Sayed:** Right now we've recorded, and it'll produce a recording file and a summary file.

[02:18:51] **Ahmed Youssry:** Alright, I'm done on my end. Okay. I hope it was productive.

[02:18:58] **Shereen Sayed:** Very. Don't you feel it was? I feel we clarified a lot.

[02:19:03] **Nermeen Ahmed:** From your perspective -- if you needed us, I mean, we --

[02:19:07] **Ahmed Youssry:** Of course. But I was right to wait on these questions, because these are great questions that help me think through things. Imagine if I had built the data model without these answers -- I would have had to redo everything.

[02:19:21] **Shereen Sayed:** And it would have been frustrating. Now we can chat with Cali and start discussing bigger things. I'm happy. Okay, so --

[02:19:33] **Ahmed Youssry:** So I'm done, but there's one last thing I almost forgot -- it's what we discussed with [Cali] about documenting our decision. I'll document it as a decision that we're going to work [in Georgia]. Among the decisions I'll make -- but what do you want to document? We did the research, and the university came back with something different from what we chose. But between the interviews we know about -- we have someone in Qatar who can face us, and someone in Saudi Arabia who can help us, but we don't have anyone in the UAE. So even though the UAE might score higher on a factor, we chose [Georgia] as the basis because we're going into the Arab region here, and Shereen is here, so we have relationships -- or Shereen has relationships at the university. And from there she can reach the district or the schools. She has connections between her and the decision-makers for other things. So that makes entering [Georgia] easier. That's why we're starting in Georgia even though the report might tell you to start with California instead.

[02:20:57] **Shereen Sayed:** It's an influencing decision -- key for testing and gathering information. It might still need a bit more work.

[02:21:20] **Ahmed Youssry:** Access to the buyer and access to the tester. But Karen won't be able to give us access --

[02:21:25] **Shereen Sayed:** To the buyer, but I can get it through other channels. It requires a little bit of work but I can get it. But the access for testing and getting information is huge, because it impacts how we address buyers.

[02:21:41] **Ahmed Youssry:** It impacts --

[02:21:43] **Shereen Sayed:** Our development, the process.

[02:21:46] **Ahmed Youssry:** Let's have the AI do market research and district research for us. Then we look at it. It helped us -- we worked with the people we know. That influenced our final choices with a bit of the trust we can build --

[02:22:09] **Shereen Sayed:** Exactly. Where we found accessibility. It's like there's --

[02:22:18] **Ahmed Youssry:** Between [potential] and [accessibility] -- we took the things that were high in both. So, the UAE is highest overall but there's no one [there for us]. Qatar is [good] but -- okay? Saudi Arabia is slightly lower [in potential] but much higher [in accessibility]. That's why we put it alongside Qatar. Saudi Arabia also has the point about --

[02:23:08] **Shereen Sayed:** Of course, and that they're moving toward [education reform]. That's an important moment. Each [market] had its own uniqueness. The uniqueness of Saudi Arabia that makes us want to pursue it is this point, in my view, plus the fact that it's obviously a much bigger market -- the biggest market in the region. But also this part has new opportunity.

[02:23:40] **Nermeen Ahmed:** And I wanted to say -- the digitization and AI topic, it's also in their vision. They've now integrated education into everything. I want to tell you that Mariam put me in touch with a new contact. I'll speak with them. Good.

[02:23:56] **Ahmed Youssry:** In Saudi Arabia?

[02:23:57] **Nermeen Ahmed:** They work with the ministry or something like that. In Saudi Arabia. Okay, tell me what the other questions are, because I'll have to leave. You have questions -- a few things?

[02:24:36] **Shereen Sayed:** I do have a few questions. Okay, first thing is the scheduling. Let's look together -- Cali won't be able to join us. She was supposed to come on the 8th, but she can't make it. So do we move Cali's meeting to the middle of this week, or do we push it to the following week? The following week, you and I -- Nermeen -- we have a meeting after our tactical meeting. And God willing, I'll be able to work -- I mean, on the 15th, God willing, I'll manage. I also want to tell you that my availability on the 15th depends on my output over the next two weeks, because I have a submission on the 17th.

[02:25:28] **Nermeen Ahmed:** One thing reminds me of another -- you can stop the recording now. Right, yes. So when do you need me to send you the materials? I opened them today and found --

---

*[End of recording at 02:25:39]*
