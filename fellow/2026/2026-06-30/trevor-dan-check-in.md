---
title: "Trevor <> Dan Check In"
date: 2026-06-30
time: "09:30-10:00"
participants: [Dan Duling, Trevor Anderson]
fellow_url: https://fluentco.fellow.app/meetings/040000008200E00074C5B7101A82E00807EA061E7B841FADA3EFDC010000000000000000100000003513ADEC51113F42AAF05262021E63EB/
captured_at: 2026-07-01T12:10:51Z
---

## AI Notes

Talking Points
The things to talk about
( ) 

Action Items
What came out of this meeting? What are your next steps?
[ ] 

Notepad
Anything else to write down?
 •  

---

Trevor Anderson and Dan Duling held their regular check-in to discuss team management, upcoming time off, and Q3 planning. Trevor has successfully onboarded most of his data engineering team to **Lattice for performance tracking**, with most team members submitting their first weekly updates. The discussion covered Fluent's historical challenges with performance reviews due to managers lacking formal management experience. They addressed **Trevor's vacation plans for the week of July 6** to attend his brother-in-law's Green Beret graduation, with coverage arrangements through Dan and Bharat. Trevor presented his **phased technical objectives for Q3**, covering five key areas including data governance, compute optimization, and platform consolidation. Dan initially suggested scaling back but ultimately approved keeping the comprehensive documentation approach since product work documentation may be incomplete.

### Lattice rollout and performance management

- Trevor introduced the data engineering team to **Lattice last week**, with most team members successfully submitting their first weekly updates, though a few still need to complete theirs. [08:09]
- Fluent implemented a **first-half-year review cycle** in Lattice, representing a shift from their previous quarterly goal system used in ADP. The review cadence changes frequently based on the platform in use. [08:43]
- The company faces challenges with performance reviews because many senior staff members with **15+ years tenure** have never managed people, leading to inconsistent review quality across departments (some reviews are just quick yes/no responses with auto-approval). [09:30]
- HR is working to improve the review process using Lattice, as the current inconsistency has resulted in technical staff being embedded in owned-and-operated teams without proper engineering oversight. [11:01]

### Career development and team retention

- Trevor plans to discuss **career goals and aspirations** with each team member to understand what they want to achieve at Fluent over the next 1-5 years and potentially align them with relevant projects. [11:57]
- Dan advocates for a development philosophy focused on **continuous career expansion** regardless of whether employees stay with the company, requiring a strong onboarding process to support a 'revolving door' of talent growth. [12:35]
- Data engineering is well-suited for this approach since it requires **minimal tribal knowledge** - the data itself provides the context needed for new team members to ramp up quickly, unlike complex codebases. [13:32]
- Dan expects **low turnover risk** from the team since most members are located in Canada where employees typically don't job-hop frequently, though Joseph may face immigration sponsorship issues that could affect his status. [14:40]
- Manav was highlighted as someone with a **bright future ahead**, having joined the company directly instead of attending college, which Dan views as a smart decision given the changing value of traditional education versus real-world experience. [16:20]

### Trevor's vacation coverage planning

- Trevor will be out the **entire week of July 6, 2026** visiting his brother-in-law in North Carolina for his Green Beret graduation ceremony, which he views as a significant accomplishment. [24:09]
- Trevor has already submitted his time-off request in ADP and needs to set up **Slack and email out-of-office notifications** with an escalation path directing people to Dan and Bharat (who will be back from leave). [24:36]
- The team is handling coverage well despite multiple people being out - **releases are still progressing on schedule** with team members helping each other and picking up work during absences. [25:38]
- The company is closed **Thursday and Friday** (July 3-4), giving Trevor limited time to finalize Q3 planning before his week-long vacation begins. [29:23]

### Q3 technical objectives planning

- **Q2 is wrapping up** with only a few stragglers remaining, and Brian is leading discussions about product goals for Q3 with the team positioned and ready to execute. [26:32]
- Trevor developed **five phase-one technical objectives** for Q3 covering: data object classification, governance, compute governance and optimization, lake/database infrastructure, and platform tech debt consolidation. [27:00]
- The objectives are scoped as **phase one of longer initiatives** expected to continue through Q4 2026 and into Q1 2027, with specific deliverables targeted for Q3 given time constraints. [27:23]
- Dan initially suggested focusing on top priorities as an MVP approach to avoid overcommitment, but then approved keeping the **comprehensive documentation** since product may not have all their work fully documented. [28:20]
- Trevor structured the objectives as a **running list of gated epics** that can flow between quarters if not completed in Q3, creating a clear pathway for technical foundation work over multiple quarters. [30:00]

### Action Items

- [ ] Ensure Brian has everything needed in good working order before vacation (week of July 6) [24:27] (Trevor Anderson)
- [ ] Set up out-of-office notifications on Slack and email with escalation path to Dan and Bharat for vacation coverage [24:56] (Trevor Anderson)
- [ ] Review Trevor's technical objectives document for Q3 and provide feedback [29:05] (Dan Duling)

### Decisions

- Trevor will take vacation the **week of July 6, 2026** to attend his brother-in-law's Green Beret graduation in North Carolina. [24:09]
- The team will proceed with Trevor's **comprehensive phased technical objectives** for Q3 (covering all five initiatives) rather than scaling back, since product documentation may be incomplete. [29:08]
- The company is closed **Thursday and Friday** (July 3-4, 2026) for the holiday. [29:23]

## Transcript

[00:06:50] Dan Duling: Hey, what's going on? Trevor?
[00:06:53] Trevor Anderson: Hey, Dan. Hope you had a good vacation.
[00:07:00] Dan Duling: It was nice. Sorry, you're gonna, you're gonna take a walk with me? I've been stuck on calls since 8am
[00:07:08] Trevor Anderson: so you stretch the legs. No, no worries.
[00:07:11] Dan Duling: That's probably nothing new to you.
[00:07:13] Trevor Anderson: Yeah, yeah, yeah. I've been slowly getting.
[00:07:18] Trevor Anderson: Trying to figure out my schedule here with, you know, been piling on meetings.
[00:07:24] Trevor Anderson: But, you know, probably similarly to you, it's good to have some time to actually be able to get through stuff as well.
[00:07:34] Trevor Anderson: But the extra meetings have definitely been helpful to help build an understanding of everything that go.
[00:07:40] Trevor Anderson: That's going on here.
[00:07:42] Dan Duling: Nice. Yeah, I mean, that's.
[00:07:46] Dan Duling: I think that the right perspective is, you know, the nice thing about not needing to, you know, be an ic.
[00:07:54] Dan Duling: Right. Take the time to figure out where everything is and how we can start improving it.
[00:08:05] Trevor Anderson: Yeah, you know, that's been huge. You know, team has been good.
[00:08:09] Trevor Anderson: You know, you know, consistently been meeting with them, kind of just introduced them or I introduced the team all to Lattice last week.
[00:08:20] Dan Duling: Okay.
[00:08:21] Trevor Anderson: So, you know, most of everyone was able to give me, provide a weekly update, which was good to see.
[00:08:28] Trevor Anderson: Still a few slackers on that, but. But it's the first go around, so. So.
[00:08:38] Trevor Anderson: But yeah, it's good to see most of the team was able to do that. And I did see that the.
[00:08:43] Trevor Anderson: There was a first half of the year review. I'm not sure if that's new or if.
[00:08:50] Trevor Anderson: If there's always been a first half or it's six months, it's kind of,
[00:08:55] Dan Duling: it's kind of new.
[00:08:56] Dan Duling: I mean, they, they switch around all the time and it's based upon whatever tool they're using.
[00:09:02] Dan Duling: Like adp.
[00:09:03] Dan Duling: They just did quarterly goals that were, you know, reviewed and then they stopped that and they did, they always did the yearly reviews on top of that.
[00:09:18] Dan Duling: And then, you know, with Lattice, their whole idea is, oh, well, we'll, you know, change it so that it, it captures all this data for people to make reviews easier.
[00:09:29] Trevor Anderson: The.
[00:09:30] Dan Duling: Yeah, the big issue you have at Fluent is the fact that we have a lot of people that, you know, had been with the company for 15 years.
[00:09:42] Dan Duling: Right. So they've kind of started when it was a, you know, from the original startup days.
[00:09:48] Dan Duling: And they've never managed.
[00:09:52] Dan Duling: So we have a lot of people even on the, the senior team that, you know, have never managed anyone, but they've been put into those positions.
[00:10:03] Dan Duling: And so HR is always trying to figure out a better way to make the reviews a little bit More, you know, useful because it, it widely varies, you know, what happens on, you know, engineering reviews versus some other orgs.
[00:10:25] Dan Duling: Like their review might just be. Yes, yes, yes, yes, yes. Done.
[00:10:31] Trevor Anderson: Okay.
[00:10:32] Dan Duling: Like, you know, what, did you do this, you know, last quarter?
[00:10:36] Trevor Anderson: Yes, like, like three bullet points.
[00:10:40] Dan Duling: Not even that.
[00:10:41] Trevor Anderson: Oh, not even that. Oh, wow.
[00:10:42] Dan Duling: And then they're just kind of like auto approved because again, it's, you know, individuals that historically have never, you know, manage so they don't, you know, they don't care about, you know, career progression or anything else.
[00:10:57] Dan Duling: It's just more like, oh, I know what they're doing. And then.
[00:11:01] Dan Duling: Yeah, and then what happens is then we start getting people that are, you know, basically doing tech stuff and they're embedded two layers down on, owned and operated, running email servers and everything else with no engineering overhead.
[00:11:24] Dan Duling: So it just becomes a complete mess. So we'll see how Lattice goes.
[00:11:31] Trevor Anderson: Yeah, it seems like it's going to go well. You know, it's not something new to me.
[00:11:36] Trevor Anderson: This is, I mean, the platforms definitely haven't used before, but the idea of, you know, having a performance evaluate or a goal that you set at the beginning of a quarter or year and you try to accomplish that and track it and things like that.
[00:11:53] Trevor Anderson: So yeah, not something new and so it'll be good to see.
[00:11:57] Trevor Anderson: Also, you know, I, you know, I talked to all the guys and said, you know, I'd love to discuss your, you know, career goals and what do you want?
[00:12:05] Trevor Anderson: Affluent? What do you want?
[00:12:08] Trevor Anderson: What are you looking to get out of the next year or, you know, two years, five years, whatever it may be, and see where their head's at and curious what they want to focus on.
[00:12:17] Trevor Anderson: And you know, maybe I can help kind of give them or put them in a part of the engineering team that is more focused on or aligns with what they want to do.
[00:12:28] Trevor Anderson: I don't know how much I can help that, but, you know, it'd be good to hear from them on that side of things.
[00:12:35] Dan Duling: You know, it's always good too. Right.
[00:12:36] Dan Duling: Like, I always took an approach of I don't care if you stay with the company. Right.
[00:12:43] Dan Duling: You know, the goal should always be to continue to expand a career.
[00:12:49] Dan Duling: Right now what that means is as you're building up individuals so that they can either promote within the company or, you know, go elsewhere, you have to kind of build that revolving door.
[00:13:05] Dan Duling: Right.
[00:13:06] Dan Duling: So that, what that means is you also have to kind of improve and work on the onboarding process so that, you know, if you're expecting People to, you know, kind of grow out and, you know, move on with their careers if we don't have a place for them.
[00:13:23] Dan Duling: Right. Well, that just means that now you have to have a streamlined process to bring people on.
[00:13:32] Dan Duling: Data engineering is a great example of where that works well. Right.
[00:13:36] Dan Duling: Because end of the day you don't really need any tribal knowledge, right?
[00:13:43] Trevor Anderson: Nope.
[00:13:44] Dan Duling: I mean, like, it is one of the few areas that literally the data tells you what you need to know and you can start rolling into it.
[00:13:54] Dan Duling: You know, you'll hear the argument on engineering side that, oh, you know, you can just read the code.
[00:14:00] Dan Duling: Well, you know, then insert a code base like Minion and you know, you find out that doesn't actually always, you know, work.
[00:14:10] Dan Duling: So.
[00:14:12] Dan Duling: But yeah, I think that's a smart way to start progressing with that team, figure out, you know, what their wants are.
[00:14:21] Dan Duling: It also gives you a good understanding, right. Of when people might be looking.
[00:14:27] Trevor Anderson: Right.
[00:14:28] Dan Duling: When they might be wanting to go get either more out of our group or, you know, go elsewhere.
[00:14:35] Dan Duling: Now, I do think you're in an interesting situation.
[00:14:40] Dan Duling: I figure given where everyone's located and giving, you know, kind of how that that team works, you know, I, I don't really see a lot of people trying to jump ship.
[00:14:59] Dan Duling: I think you're going to have a lot of.
[00:15:01] Trevor Anderson: Yeah, yeah, yeah, I'll take your word on that. Because I'm new.
[00:15:06] Trevor Anderson: It seems like everyone's relatively new for the most part.
[00:15:11] Dan Duling: They are. I mean, well, the team's only existed for, you know, two years, so.
[00:15:19] Trevor Anderson: Yeah.
[00:15:19] Dan Duling: But you know, what I've seen from people that work in Canada, they do not typically jump.
[00:15:27] Dan Duling: And, you know, most of the team is all in Canada.
[00:15:31] Dan Duling: And then when you take into account other areas like that makes it even more where they are not going to, you know, jump.
[00:15:40] Dan Duling: We do have one thing that I have to discuss with HR for Joseph. I guess he needs some.
[00:15:49] Dan Duling: I don't know if they'll do it or not, but I think he needs to get sponsored.
[00:15:56] Trevor Anderson: Is he trying to come to the US or why would he?
[00:15:59] Dan Duling: I, I guess he has to be sponsored for Canada even so.
[00:16:03] Trevor Anderson: Really?
[00:16:04] Dan Duling: Yeah. So, you know, I noticed a call come up on my calendar for something.
[00:16:11] Dan Duling: So just a heads up, you know, okay.
[00:16:14] Dan Duling: Joseph may be in limbo depending on, you know, where, where that kind of goes.
[00:16:20] Dan Duling: But outside of that, you know, only person I'd worry about is enough just because, you know, we are, we're probably like, you know, he'll have a bright future ahead.
[00:16:34] Dan Duling: So.
[00:16:36] Trevor Anderson: Yeah, I learned at the at the, the collab week that he was.
[00:16:46] Trevor Anderson: I didn't realize he had joined instead of going to college. That was awesome news to hear.
[00:16:55] Trevor Anderson: I was like, really? Oh, yeah, yeah. I was like, yeah, congrats, man. That's.
[00:17:01] Trevor Anderson: That's pretty impressive, considering it was hard for me to figure out what, you know, what I wanted to do and where I wanted to work and things like that.
[00:17:09] Trevor Anderson: Yeah. Even during college. So.
[00:17:11] Dan Duling: Oh, and you know what? Like, here's the crazy thing about. I mean, end of the day, right?
[00:17:16] Dan Duling: Reality is, what is school?
[00:17:18] Dan Duling: School is to, you know, straight up just, you know, train people to go work for others. Right.
[00:17:29] Dan Duling: I mean, you know, and it's.
[00:17:32] Dan Duling: Even if you look at test scores and what's going on in, you know, these schools now, and even some of the new testing and ranking, it becomes even more apparent.
[00:17:41] Dan Duling: Like, you know, you know what? They never teach in school. They don't teach, hey, how to invest.
[00:17:52] Trevor Anderson: Right.
[00:17:53] Dan Duling: Like, what would happen if high schoolers all of a sudden had teachers that were telling them, you know, from the age of 13 on, hey, you really need to start investing, you know, instead of your chore money or, you know, little things that you're doing, you know, if you invest $100 a month over the course of your year, right.
[00:18:19] Dan Duling: You know, like, and then you do that for the rest of your life, by the age of 25, you'll have over a million and a half, you know, in your bank account.
[00:18:29] Trevor Anderson: Yeah.
[00:18:29] Dan Duling: Like, if that happened, what would happen to the economy in the States?
[00:18:35] Trevor Anderson: Right?
[00:18:36] Dan Duling: It would crash because no one would work.
[00:18:40] Dan Duling: Or maybe it wouldn't crash because they, they wouldn't have, like, they would actually just go ahead and, you know, create their own thing.
[00:18:47] Dan Duling: So for me, when Manav was talking about it, I'm like, here's the thing.
[00:18:52] Dan Duling: Unless you're going to a networking school, you're really not going to learn anything out of college.
[00:19:00] Dan Duling: No, I mean, like, you know, even. I mean, like, everything else these days
[00:19:06] Trevor Anderson: is really, just really not efficient way to learn things. It's.
[00:19:12] Dan Duling: It's not efficient. It's a great networking tool, Right?
[00:19:16] Dan Duling: Like, it's a great social aspect to, you know, jump into.
[00:19:21] Dan Duling: But, you know, you're kind of taking your best most, you know, ingenious years of your life just sitting behind a desk listening to someone teach.
[00:19:36] Dan Duling: That's teaching because they couldn't do.
[00:19:40] Trevor Anderson: Yeah.
[00:19:43] Dan Duling: And I, I'm still good friends with all my professors, but I strongly believe that sentiment still.
[00:19:51] Trevor Anderson: I mean, if it was a good place for me to find out, like, What I wanted to do career wise, you know, and figure that out.
[00:19:57] Trevor Anderson: But if you already know, you know, hats off to you, you shouldn't even go to school.
[00:20:02] Trevor Anderson: Yeah, like start, start doing the work that you want to do.
[00:20:08] Dan Duling: Oh.
[00:20:09] Dan Duling: Especially in today's society with how AI and everything else is, you know, I am so envious of, you know, individuals that, you know, have, you know, they don't have kids, they don't have a wife, they, you know, like, I don't know, I'm like, I wish I had, if I had no time in just to myself, sure, I do my, you know, 9 to 5 job, but holy crap, would it be awesome to go ahead and, you know, just really knock out new business ideas, start floating them around, you know, trying to get as many sbu, you know, loans as I can buy up all these, you know, boomers of business that, you know, aren't.
[00:21:02] Dan Duling: Have kids that don't want them and just kind of go from there.
[00:21:08] Trevor Anderson: Yeah, I know.
[00:21:09] Trevor Anderson: It's, it's definitely a great time for like any individual that has that drive to, to want to build their own future in business.
[00:21:23] Trevor Anderson: Right.
[00:21:24] Trevor Anderson: Like, there's just so much, so many resources that were not available within the past few years that make that all possible, you know.
[00:21:32] Dan Duling: Oh, yeah.
[00:21:33] Trevor Anderson: So it's quite amazing.
[00:21:35] Dan Duling: The barriers today have been basically broken down, right?
[00:21:40] Dan Duling: Like, you, you want to start something you really can, right?
[00:21:44] Dan Duling: Like even, you know, it's crazy with the, the viral marketing and affiliate marketing and how that whole industry even, you know, can work and just driving people to, you know, drive people to, you know, traffic and everything and how it actually works and, you know, where you're, I can get $500 for a, a product that someone buys, right?
[00:22:14] Dan Duling: Like, and here's the thing, it's crazy when you start looking at the statistics for how many people doom scroll.
[00:22:22] Dan Duling: Like, I think I, I forget what it was the app like, and how long.
[00:22:29] Dan Duling: Yeah, like, it's like, you know, people are just scrolling through like Instagram, Tick Tock, you know, etc.
[00:22:38] Dan Duling: Like, Tick Tock's the worst because it rewires your brain.
[00:22:42] Dan Duling: And then people just can't even, you know, hold a conversation for longer than a couple minutes because, you know, they're just looking for that dopamine hit of, you know, the next big thing.
[00:22:54] Dan Duling: It's like, well, you know what you should do? Go to the dishes, clean a cabinet.
[00:22:59] Dan Duling: It actually hits the same dopamine effect, so.
[00:23:03] Trevor Anderson: And people are making a lot of money off of that effect too.
[00:23:06] Trevor Anderson: So on the other side of Things that's, that's what you talk about like affiliate marketing.
[00:23:11] Trevor Anderson: You know, you don't even have to sell your own product.
[00:23:13] Trevor Anderson: You could just oh yeah, market someone else's thing and you'll make money off the views and the purchases that come from your page and I mean yeah, a lot of people are taking advantage of that
[00:23:33] Dan Duling: 100%. Oh crap. Let me. Any. I guess you know any.
[00:23:42] Dan Duling: I know we, I, I didn't realize I have a call at Built but I know we were a little bit late too.
[00:23:50] Dan Duling: Any, any red flags or major items right now for you that I need to be aware?
[00:23:57] Trevor Anderson: Yeah, I'm. Well, I'm planning to take off time. Not this week, next. Next. Yeah, all of next week.
[00:24:09] Trevor Anderson: Visiting my brother in law in North Carolina. He's.
[00:24:13] Trevor Anderson: He's actually graduating from the Green Beret in the army. So pretty big accomplishment.
[00:24:19] Trevor Anderson: I haven't seen him in a few years so. Or all going out with the in laws visiting him for the week.
[00:24:26] Trevor Anderson: Nice.
[00:24:27] Dan Duling: So just yet, I mean on that just make sure that everything that Brian needs is in good working order and you know, then enjoy your time.
[00:24:36] Trevor Anderson: Okay, great. Thank you. I put that on your calendar as well.
[00:24:42] Dan Duling: You might just want to throw it in 80 or wherever people request time off. I think it's ADP.
[00:24:48] Trevor Anderson: Yeah, I got it in there. So feel free to check that when you have time. But it's there.
[00:24:54] Trevor Anderson: And then I would just do on
[00:24:56] Dan Duling: your you know, on Slack throw your out of office on there.
[00:25:00] Dan Duling: You can actually even put an autoresponder I think on there if you want or just a note that you're out.
[00:25:07] Dan Duling: You know, if there's you know, pretty simple escalation path, just tell people to reach out to me.
[00:25:14] Trevor Anderson: Okay.
[00:25:15] Dan Duling: And then you know, or you know, me and Bharat even you know, Bharat will be back too.
[00:25:20] Dan Duling: So Bharat can do help out and then just set up your out of office on your email and you know, you'll be in a good spot.
[00:25:31] Trevor Anderson: Awesome. Yeah, I'll do that.
[00:25:33] Trevor Anderson: I don't tend to receive too many emails right now which is everything's like through Slack.
[00:25:38] Trevor Anderson: But yeah for email, yes, definitely still and then a kill I think no new updates on him.
[00:25:49] Trevor Anderson: Yeah, I think he's still planning on taking that paternity leave in August I believe but that's a little bit out from now.
[00:25:59] Trevor Anderson: Any. Everything else is. Everyone else is doing great.
[00:26:04] Trevor Anderson: It's a lot of folks that are kind of out right now. Even, even with that though there's.
[00:26:12] Trevor Anderson: We're still moving through our releases and everything.
[00:26:14] Trevor Anderson: So nothing's really slowed down, which is great.
[00:26:16] Trevor Anderson: Like the team is helping out each other, picking up things where people are at our office. So.
[00:26:27] Trevor Anderson: So far everything like is moving well. I think for the Most part like Q2 is wrapping up.
[00:26:32] Trevor Anderson: Like there's only a few stragglers that are being finished at this point.
[00:26:38] Trevor Anderson: So I think Brian is ready to position and I think he has other discussions going on about planning out the product goals for Q3.
[00:26:47] Trevor Anderson: I did, I don't know if you want to go through in detail the, the technical objectives, but I did comb through those and I have five.
[00:26:59] Trevor Anderson: But I, I gated them the phase one so they only include like certain things.
[00:27:07] Trevor Anderson: Like certain things I want to see accomplished for each of those goals regarding each of the tech initiatives.
[00:27:17] Trevor Anderson: And so I did phase one, you know, thinking that there is going to be more related to these items.
[00:27:23] Trevor Anderson: You know, they're kind of higher level, almost, almost greenfield type of items.
[00:27:28] Trevor Anderson: Like one's like data object classification, one's governance, one is compute governance and optimization.
[00:27:36] Trevor Anderson: Another one is like lake base and database infrastructure.
[00:27:39] Trevor Anderson: Another one's just platform tech debt and consolidation.
[00:27:43] Trevor Anderson: So there's like specific items I want to see in Q3 be completed.
[00:27:47] Trevor Anderson: But I think these things will progress into Q4 and even Q1, 2027 and things like that.
[00:27:52] Trevor Anderson: So that's kind of how I position them and try to scope them down so we can, I know we have limited time on those so we can get through the things that we want to do, but that's just at a high level.
[00:28:08] Trevor Anderson: I know we're. But we both have meetings after this so. But yeah, everything else.
[00:28:16] Trevor Anderson: No, no other red flags jumping out right now. So. Okay.
[00:28:20] Dan Duling: What I would do on that just you know, to be honest, like you know, let's put your like your top ones.
[00:28:25] Dan Duling: Right. Like don't. Let's not overwork it.
[00:28:29] Dan Duling: Like again, this is the first time we're doing it so want to use this as like, you know, our, you know, let's see, You know, kind of MVP of this.
[00:28:47] Dan Duling: So you know, I don't want to, you know, over commit to anything right now.
[00:28:52] Dan Duling: So just think through like what would be achievable because you know, we do too much too then it's probably not bad to at least have it all documented out because I have a feeling we're not going to have all the work documented out from product.
[00:29:08] Dan Duling: So never mind. Let's you. We can keep with that.
[00:29:11] Dan Duling: So I'll take a review of it and then you know, Kind of let you know, but I'll let you get going and then, you know, we can, we can also find time tomorrow or, you know, there's tomorrow, I guess we're off Thursday, so I think we're off Thursday and Friday.
[00:29:27] Dan Duling: So really, you have, you know, Thursday until, you know, Monday, so.
[00:29:35] Trevor Anderson: Yeah, pretty much. Yeah. I'm out next week too, so I will have to pick this up following that.
[00:29:42] Trevor Anderson: But, you know, it's over documented right now.
[00:29:47] Trevor Anderson: So, you know, worst case scenario, this gets cut back, some of those items get cut back, but.
[00:29:53] Trevor Anderson: Or they go into the next quarter.
[00:29:55] Trevor Anderson: So, you know, what I'm trying to do is kind of like have a list running list of things, you know, gated into certain epics that if they, if we can't get them in Q3, they go into Q4 and subsequent quarters.
[00:30:08] Trevor Anderson: So we at least have like a, you know, a pathway of technical foundation items that we want to see completed.
[00:30:15] Trevor Anderson: So. But yeah, I'll work with you on the format and structure of tracking those as well. So.
[00:30:27] Dan Duling: Perfect.
[00:30:27] Trevor Anderson: Yeah.
[00:30:28] Dan Duling: All right, I'll let you go and then I'm going to jump over to this, but awesome.
[00:30:32] Dan Duling: Good to hear that things are going well and, you know, enjoy the time off. You deserve it.
[00:30:38] Trevor Anderson: Thank you, Dan. Appreciate it. Have a good one.
