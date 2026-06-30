---
title: "Weekly Audience Solutions Jira Prioritization"
date: 2026-06-29
time: "10:00-10:30"
participants: [Virginia Marsh, Trevor Anderson]
fellow_url: https://fluentco.fellow.app/meetings/040000008200E00074C5B7101A82E00807EA061DB071A2F2B3FCDC0100000000000000001000000023F4BDC371730F47B2B5AF3C490F0D4C/
captured_at: 2026-06-30T12:02:44Z
---

## AI Notes

The team addressed critical workflow issues around **Jira ticket assignments and response times**, with Virginia raising concerns about tickets sitting unassigned for 1-2 weeks despite their revenue impact. To improve visibility, the team agreed JC should be added as a watcher on all new tickets. The discussion covered **high-priority CMI H2 audiences** including WeGovy (AS-206) and Ozempic (AS-194), with the latter requiring clarification on seed size discrepancies between Virginia's analysis and Manav's implementation.

A significant debate emerged around **data upload strategy for TransUnion modeling**, where conflicting guidance led to uploading only 25K records versus entire matched datasets. Virginia clarified that TU's 25K recommendation applies to post-match counts, not upload limits, and that TU already receives full data feeds separately, addressing Brian's concerns about data sharing. All work leveraging **Playful data remains on hold** pending legal review of contract permissions, with Don coordinating between legal and Steve Green to resolve access questions.

### Ticket Workflow & Communication Issues

- Virginia raised concerns about **ticket AS-206 sitting unassigned for 12 days** (submitted June 17, untouched until June 29) despite being marked highest priority for H2 revenue generation. This is the second instance of extended ticket delays she's encountered.
- The team identified that **no notifications are sent to JC when tickets are created** in the AS Jira project, only to a Slack channel. Virginia agreed to add JC as a watcher on all future tickets to ensure visibility and faster assignment.
- A second example emerged with **ticket AS-194 (Ozempic audiences) submitted June 2 but not touched until June 16**, taking two weeks to get initial attention. Virginia emphasized the need for better cadence on CMI-related tickets worth millions in revenue.
- JC acknowledged the alignment issues and emphasized that when engineers receive conflicting direction from different stakeholders (like Virginia vs. Brian), they should **ask for clarification rather than override** the original guidance without explanation.

### High-Priority CMI Audience Work

- **Ticket AS-206 contains WeGovy H2 audiences** critical for driving CMI revenue in the second half of 2026. The ticket was created June 17 at highest priority but remained unassigned, prompting Virginia's escalation about workflow problems.
- Virginia flagged that **Meta is now blocking audience data** containing certain age groups and income levels. She requested a comprehensive review of all LiveRamp audiences to identify which segments might be in jeopardy under Meta's new content policy.
- **Ticket AS-194 (Ozempic audiences)** showed seed size discrepancies between Virginia's code analysis and Manav's implementation. Virginia questioned why Manav's seeds were significantly lower when both were supposedly pulling 'all time' data, requesting a walkthrough to understand the methodology.
- For ticket AS-192, **39 declared audiences** are ready and waiting for deployment to production workspace. Joseph is coordinating with Monisha to push multiple PRs together, including around 50 modeled audiences that Monisha is handling due to capacity availability.
- Ticket AS-189 (started May 26) has **declared audience PR raised and modeled audiences nearly complete**, with one segment blocked due to a file upload issue affecting a 600K record file that won't load to TransUnion TAP platform despite being under the 350MB size limit.

### Data Upload Strategy Discussion

- Virginia clarified that **TransUnion's 25K recommendation refers to post-match counts** for representative modeling samples, not upload limits. With TU's 15% match rate on Reddit, uploading only 25K pre-match records would yield insufficient matched users for modeling.
- The team had been limiting uploads to 25K based on Brian's direction, but Virginia emphasized they should **upload entire seed files up to the actual file size limit** to maximize match potential, especially given low match rates on certain platforms like Reddit.
- Granth explained Brian's hesitation stemmed from concerns that **uploading entire datasets would allow TU to train their models on Fluentco's data**. However, JC and Virginia noted TU already receives monthly data feeds and live sign-up feeds, making this concern moot.
- Virginia clarified the **contract with TU on TAP precludes them from using uploaded data** for their own training purposes, and even if they violated this, Fluentco is already selling them data separately through other feeds, so withholding seed uploads provides no protection.
- Virginia will bring the upload strategy issue to **tomorrow's call with Adrian and his team** to get product, legal, and engineering aligned on whether cost, product, or legal concerns justify limiting uploads, or if full seed files should be uploaded going forward.

### Playful Data Legal Hold

- Virginia has **no update on Playful data access** after the team was told last week they lack permissions to use the data. Don is coordinating with Jeff, Jason (legal), and Steve Green (relationship owner) to clarify what's allowed under the contract.
- The team invested significant hours building Playful audience capabilities before discovering the **Experian data access issue**. Virginia expressed frustration that they were told to proceed with full access only to hit legal roadblocks when attempting to commercialize audiences.
- **All work leveraging Playful data for audience commercialization is on hold** until legal clarifies permissions. The team can only wait for Don's group to resolve the contract interpretation with Jason from legal.
- The team needs to understand what data they can access beyond basic information, potentially including **PII (first name, last name) and demographic data**. Once the contract is reviewed, Steve Green (relationship owner) can negotiate for additional data fields if needed.

### Open Ticket Status Review

- Joseph provided status on **AS-192 with 39 declared audiences** ready in production workspace pending final push. Monisha created a PR for declared audiences and is picking up around 50 modeled audience segments as capacity allows.
- **AS-189 is nearly complete** with declared audience PR raised. One modeled audience segment with 600K+ records cannot be uploaded to TAP despite being under 350MB limit. Monisha escalated to TU support (Kania) on June 17 but hasn't received a response.
- **AS-193 shows as live** with all segments in marketplaces as of June 19. Multiple Playful-related tickets (AS-188 and others) are blocked pending legal resolution. AS-187 is already closed and waiting on Virginia's final review.
- **Tickets AS-196 through AS-202 are test tickets** Joseph created for agent testing and should be deleted. JC will clean these up to avoid cluttering the backlog.
- JC assigned **AS-205 and AS-206 to Monisha during the meeting**. AS-204 is marked complete. The primary open items are the high-priority CMI audiences (AS-206, AS-194), Playful data tickets on hold, and AS-189 pending the TU upload resolution.

### Action Items

- [ ] JC Camacho to review ticket AS-206 (WeGovy H2 audiences) and provide Virginia with a status update on why it hasn't been assigned since June 17th.
- [ ] Shamal to review all LiveRamp audience segments to identify which ones could be affected by Meta's new policy around age group and income level restrictions.
- [ ] JC Camacho to schedule a walkthrough meeting with Virginia and Manav to explain the seed size discrepancies for the Ozempic audiences (AS-194) and review the methodology.
- [ ] Coordinate a call with Adrian, Brian, and team to resolve confusion around data upload limits and align on whether to upload entire seed files or limit to 25K records.
- [ ] Monisha to copy Virginia on the email thread with Kania regarding the 600K record upload issue that's failing due to file format problems.
- [ ] JC Camacho to delete test tickets AS-196 through AS-202 that Joseph created for testing purposes.

### Decisions

- **All work on Playful data** (commercializing audience data) is on hold until Don resolves legal permission questions with Jeff, Jason, and Steve Green regarding contract interpretation.

## Transcript

[00:00:00] JC Camacho: Been a rainy June, man. Hopefully it's. Hopefully it gets better. We'll see.
[00:00:07] Trevor Anderson: Hopefully stops soon as we get to July.
[00:00:10] Granth S: Just.
[00:00:10] JC Camacho: Yeah, I mean, it's sunny today, so we're good.
[00:00:14] Granth S: Nice. Wow.
[00:00:16] JC Camacho: All right, cool. Is Virginia back?
[00:00:25] Monisha BL: It.
[00:01:12] JC Camacho: Actually, I wanted Virginia to join. Oh, there she is. Hey, how are you?
[00:01:16] Virginia Marsh: Hi. My apologies. I was trying to catch up and look down and realized it was five minutes late.
[00:01:21] Virginia Marsh: So sorry. Thank you for reminder.
[00:01:23] JC Camacho: No, no worries. I want you. I wanted to kick it off with that Playful data. Do we have any.
[00:01:32] JC Camacho: Any update, Virginia?
[00:01:35] Virginia Marsh: I don't know.
[00:01:36] Virginia Marsh: Honestly, I don't even know why they asked us to do something if we didn't have the permissions in the first place.
[00:01:42] Virginia Marsh: So I have no update other than we just have to wait till Don is working on it with Jeff and he's working on it with Jason and Steve Green.
[00:01:54] Virginia Marsh: So all we have to do now is just wait for them to figure out what are the next steps.
[00:01:59] Virginia Marsh: We were told we had full access and go.
[00:02:02] Virginia Marsh: And then we ran into the glitch when we asked questions about using the Experian data.
[00:02:11] JC Camacho: It kind of sucks because, I mean, if you. If you put, like, man hours to all that stuff, it's just.
[00:02:17] JC Camacho: It sucks. But whatever. Hopefully. Hopefully we get somewhere positive with this.
[00:02:21] Virginia Marsh: No, I hope so.
[00:02:23] Virginia Marsh: I mean, I don't know why they would tell us and do all this work to give us access to it if we didn't have access to it.
[00:02:29] Virginia Marsh: But I'm sure that'll get resolved.
[00:02:31] JC Camacho: Yep. All right, sounds good. So for now, anything that to do with Playful is put on hold. Correct.
[00:02:40] Virginia Marsh: Anything to do with leveraging the Playful data to put it on audiences and commercialize it, that is on hold.
[00:02:48] Virginia Marsh: There's nothing we can do but wait to see what exactly is going on.
[00:02:54] JC Camacho: All right, Joseph, you. You. You caught that? Yep. All right, cool.
[00:03:04] Joseph: What's this question about, like, what are we waiting for? Which. What are we waiting from them?
[00:03:12] Virginia Marsh: Like, any confirmation last permissions to actually use the data?
[00:03:18] JC Camacho: Yeah, last week we were told that we didn't have permission to use their data.
[00:03:23] Joseph: Okay.
[00:03:24] JC Camacho: So. Yeah, so, yeah, so that. So again, we're waiting for final confirmation from legal from Don.
[00:03:34] JC Camacho: Virginia has Don involved as well.
[00:03:37] JC Camacho: So we're hoping to get somewhere sooner than later so we can continue this.
[00:03:43] Joseph: Okay,
[00:03:46] JC Camacho: so.
[00:03:47] Joseph: And do we. Do we even have a plan to get the PI from them from Playful? Like first name, last name?
[00:03:58] Joseph: And also demographically?
[00:04:00] Virginia Marsh: Well, once we. We get a better look at the contract, then we can go back.
[00:04:04] Virginia Marsh: Is Steve Green owns The relationship.
[00:04:07] Virginia Marsh: And Steve, see what other data we can use to supplement because it sounds like not everything is coming through okay.
[00:04:17] Joseph: Makes sense.
[00:04:19] Virginia Marsh: Yeah. So, unfortunately, I apologize. I was under the impression it was a go.
[00:04:24] Virginia Marsh: And as soon as we got that note from Jason from legal, I went right to Don, and Don went right into having the conversation, so.
[00:04:34] Virginia Marsh: So all we can do now is just wait for them to sort out what can and can't be done.
[00:04:40] Virginia Marsh: Because I think even under Don's understanding was we have full access and rights to the data.
[00:04:46] Virginia Marsh: It's just not how it's written in the contract that Jason is interpreting.
[00:04:50] Virginia Marsh: So that's all we can do is just wait for them to figure that part out. So I do have a cut line.
[00:05:02] Virginia Marsh: I don't want to. I want to make sure we're following what.
[00:05:05] Virginia Marsh: You guys, I'm sorry I wasn't here last week, but I have some open ticket.
[00:05:12] Virginia Marsh: So on June 17th at 9:59, I put in WeGovy H2 audiences.
[00:05:20] Virginia Marsh: And as you know, this is a big deal to try to get as much revenue in as possible, but nobody has even picked this ticket up.
[00:05:28] Virginia Marsh: So I'm just trying to understand, like, now it's the 29th. This was put in on the 17th.
[00:05:34] Virginia Marsh: How does this really. How does it work?
[00:05:36] Virginia Marsh: Like, do I need to put a note out there to say, hey, there's a ticket? Like, how does this work?
[00:05:43] Virginia Marsh: I just want to make sure I understand because now I have to go back and make sure that leadership understands what we're working on and revenue that's tied to it.
[00:05:52] Virginia Marsh: So I'm just. I want to understand which.
[00:05:55] JC Camacho: Do you have a. Do you have a ticket number?
[00:05:57] Virginia Marsh: Sorry, yeah, it's as 206.
[00:06:10] Virginia Marsh: Like, how do we get into.
[00:06:11] Virginia Marsh: And I put this as the highest because obviously we're trying to get as much revenue as possible and just trying to understand, like, how.
[00:06:19] Virginia Marsh: How does this. How do we work in a better cadence?
[00:06:23] JC Camacho: So one of the things that I would suggest can.
[00:06:26] JC Camacho: Like, every time you create a ticket, can you either assign it to me or add me as a watcher, because I'm not getting, like, notification that a ticket is being created.
[00:06:39] Virginia Marsh: Well, that. That in itself is. Is very concerning, but I get it.
[00:06:43] Virginia Marsh: If that's the workaround, I'm happy to, but I just.
[00:06:47] Virginia Marsh: It takes us so long to get these tickets out and to be competitive in the market.
[00:06:51] Virginia Marsh: And when we are trying really hard to drive revenue for this, I mean, that's a lot of time that that's kind of been sitting open.
[00:07:01] Virginia Marsh: So I mean, I get it. You guys are super busy, and if that is the.
[00:07:05] Virginia Marsh: The workaround, then I'm happy to do that.
[00:07:09] Virginia Marsh: But it's not the first time that I've had to ask, like, this hasn't
[00:07:12] JC Camacho: been assigned, but hold on. So just correct me if I'm wrong. Right? You don't.
[00:07:17] JC Camacho: You don't get any notification when she create. When a ticket is created in as, Right?
[00:07:22] Joseph: I think we get one in the Slack channel.
[00:07:26] JC Camacho: Oh, on the Slack channel.
[00:07:27] Virginia Marsh: Yeah.
[00:07:29] JC Camacho: Okay.
[00:07:30] Virginia Marsh: Because I was told not to assign somebody, but I'm happy to assign. I just, I'm just.
[00:07:36] Virginia Marsh: I would make sure that there is a good cadence here, especially as we get the new account manager starting, that she understands that too.
[00:07:42] Virginia Marsh: Like how to best work through this.
[00:07:45] JC Camacho: Maybe not assigned them, but you do have options to add watchers, right? Can you add me as a watcher?
[00:07:50] JC Camacho: I think I get. At least I'll get a notification there.
[00:07:54] Virginia Marsh: Okay. So just anytime I create one, just assign you.
[00:07:59] JC Camacho: Well, add me as a watcher.
[00:08:01] Virginia Marsh: Okay. All right, So I added you as a watcher there.
[00:08:04] JC Camacho: Okay, Let me just review this and I'll have an update to you, like, after this meeting.
[00:08:11] Virginia Marsh: Okay.
[00:08:12] JC Camacho: Okay.
[00:08:12] Virginia Marsh: I mean, I know you picked it up too, because I put. I put a.
[00:08:16] Virginia Marsh: A note in the channel because we ran into a big problem where Meta is blocking our data.
[00:08:22] JC Camacho: Oh, wait, is this that ticket?
[00:08:24] Virginia Marsh: No, no, you already took care of that one.
[00:08:26] JC Camacho: Okay. Yeah, yeah, yeah. Sorry. Sorry. Yeah.
[00:08:29] Virginia Marsh: So how does that.
[00:08:30] Virginia Marsh: You know, because if I put a note in here and then obviously I went and put it in the ticket and you picked it up, but you only picked up the one for that one audience.
[00:08:38] Virginia Marsh: Is there someone else that can go in and look at every single one of our audiences that might now be in trouble because of Meta's new policy around that type of language?
[00:08:58] JC Camacho: I think.
[00:08:59] Joseph: Well,
[00:09:02] JC Camacho: so you're asking if we can go in and see if we're being blocked by any audiences of being blocked.
[00:09:10] Virginia Marsh: No, I mean, I'm asking is.
[00:09:11] Virginia Marsh: And even if it's Shamal, that goes through all of the audiences that we have listed on Live Ramp and take a look at the ones now, given that we can't put that age group and that.
[00:09:25] Virginia Marsh: That revenue or their income level in, we need to go about and highlight all of those audiences that might be jeopardy with Meta.
[00:09:34] JC Camacho: Okay. Yeah, I believe. Jamal, can you take that on?
[00:09:42] JC Camacho: It's pretty much going into Live Ramp and identifying. Identifying which one of the.
[00:09:48] JC Camacho: How many segments are being affected, could be affected by what Virginia is saying.
[00:10:00] JC Camacho: I Caught the ask, Virginia. And I'll touch base with Jamal.
[00:10:05] Virginia Marsh: Okay.
[00:10:06] Virginia Marsh: And then there were some other audiences that I put out there, and I'm trying to figure out which ones they were, but.
[00:10:16] Virginia Marsh: Oh, for the Ozempic. So I do have a question on the Ozempic one.
[00:10:21] Virginia Marsh: So I put in, and I actually had pulled some code myself, and the numbers looked different.
[00:10:30] Virginia Marsh: So I'm hoping somebody could walk me through the seed sizes and why your seed size is different and if the code that I put together was wrong.
[00:10:40] Virginia Marsh: But I'm just trying to understand these seed sizes and what is the actual seed size once you combine it all.
[00:10:48] Virginia Marsh: And the audience is as 194.
[00:11:02] JC Camacho: That was Manav. He's here. He is not here. All right, I will. I will touch base with him so we.
[00:11:14] JC Camacho: We can set something else so he can give you a walk through.
[00:11:17] Virginia Marsh: Okay. I really appreciate it.
[00:11:19] Joseph: Okay.
[00:11:20] Virginia Marsh: Because these. These ones that I'm mentioning right now, these are all part of H2 budget for CMI, so.
[00:11:26] Virginia Marsh: So it's really important that we get these audiences accurate because it's a big portion of the revenue that we're trying to bring in.
[00:11:34] Virginia Marsh: So I just want to understand why his seeds.
[00:11:38] Virginia Marsh: Seeds were so low versus when I pulled it, there was a lot more people in the pile in the pool.
[00:11:45] Virginia Marsh: And when I asked him if he was looking at all time, he said yes.
[00:11:49] Virginia Marsh: But I'm just trying to understand how that possible. So we can take that one offline.
[00:11:54] JC Camacho: You're. You're. You're fully back these. These couple days, right?
[00:11:58] Virginia Marsh: I'm back.
[00:11:59] JC Camacho: Okay. All right. I'm gonna. I'm just gonna find some time for. For him and. And you. And, I mean, I'll.
[00:12:04] JC Camacho: I'll get beyond it as well. So that. And. And put. Put it on your calendar.
[00:12:08] Virginia Marsh: Okay, thank you.
[00:12:09] Joseph: And sorry to interrupt. J.C. manava's off this week.
[00:12:13] JC Camacho: Oh, he's off this week.
[00:12:16] Joseph: And just a question. So, Virginia, is it something.
[00:12:20] Joseph: Are you looking for the sample size that we upload to the tap?
[00:12:25] Virginia Marsh: I'm trying to understand that.
[00:12:27] Virginia Marsh: And this is another one where I put the ticket in on June 2, and it wasn't touched until June 16.
[00:12:36] Virginia Marsh: And so it's another one where I'm really trying to understand, like, I mean, I.
[00:12:41] Virginia Marsh: I was under the impression that it could take up to five days for you guys to.
[00:12:45] Virginia Marsh: To pick up a ticket, but this is another one where, again, I'm just trying to understand why it's taking so long to get picked up, and then there's so much ambiguity around it
[00:12:59] Monisha BL: and
[00:12:59] Virginia Marsh: now he's out for a week and I'm still waiting to understand. So I completely understand. I'm just.
[00:13:07] Virginia Marsh: This is just another one where I'm trying to figure out like, how do we, we better work together so that we're not taking weeks, especially for ones that are around CMI, that are highly important, it's worth millions of dollars.
[00:13:21] Virginia Marsh: How do we get into a better cadence?
[00:13:25] JC Camacho: Let's start with the adding me as a watcher and let's go from there.
[00:13:31] Virginia Marsh: No, I appreciate it. I just added you on the other one.
[00:13:33] Virginia Marsh: But again, not to hammer it, it's so important.
[00:13:37] JC Camacho: No, no, I get it and it makes sense. Like I see it too. I'm seeing the comments and I get it.
[00:13:43] Virginia Marsh: Okay. No, totally appreciate it.
[00:13:47] Joseph: And Virginia, do you want us to upload the entire seed?
[00:13:51] Joseph: Like if it's less than a million for the model audience?
[00:13:55] Virginia Marsh: I really do.
[00:13:56] Virginia Marsh: Especially since we're, I'm working with tu, especially on the Reddit portion where TU is going to try to do something better with Reddit because keep in mind, you load in certain amount and then we create that audience.
[00:14:11] Virginia Marsh: But at the end of the day, tu only has a 15 match rate with Reddit.
[00:14:18] Virginia Marsh: So no matter how big you make that audience, we're still losing a ton of of users because of the situation that Reddit has with tu.
[00:14:29] Virginia Marsh: It's not something that we can control other than trying to make that seat side as good as possible.
[00:14:35] Virginia Marsh: And that's why we were talking back and forth about 25, 000 is not the upload limit.
[00:14:40] Virginia Marsh: That's the amount of matched users that TU feels is a good sample size for modeling.
[00:14:47] Virginia Marsh: But there's no limits other than the megabytes that they had set on the email, which I don't know off the top of my, my head of the size of how much you can load into the platform.
[00:14:59] Joseph: Yeah, but JC, I believe that we agreed with the 25,000 representative sample, right?
[00:15:05] Joseph: Rather than uploading, the entire matched is
[00:15:09] Virginia Marsh: a representative sample and once it's matched to tu.
[00:15:14] Virginia Marsh: But TU is saying that the size that can be modeled from that's representative is 20, 25,000 after it's matched.
[00:15:23] Virginia Marsh: Not once you load it and wait for the match, it's once it's matched.
[00:15:26] Virginia Marsh: So the actual size of the data or the amount of users that could be modeled off of 25k is a good sample size.
[00:15:37] Virginia Marsh: It's not the limit of what we can do.
[00:15:39] Virginia Marsh: But they're saying in their professional opinion, 25,000 is a representation representative Sample size for us.
[00:15:47] Joseph: Okay.
[00:15:47] Virginia Marsh: Off of not so for us to load up.
[00:15:51] Joseph: I understand. So for example, if the match rate is 50%, so we'll have to upload like 50,000.
[00:15:56] Joseph: So at least we'll get 25,000 matched. That's what you're saying.
[00:16:00] Virginia Marsh: I agree, but we shouldn't just limit it to that unless there's an astronomical cost that I don't know about.
[00:16:08] Virginia Marsh: We should go to the actual limit of the, the size of the file to make sure that we're getting the best sample.
[00:16:16] Joseph: Okay.
[00:16:17] Virginia Marsh: We don't know what 25,000 did match.
[00:16:21] Joseph: Okay.
[00:16:21] Virginia Marsh: Is that. That's how many did. That's how. That is what they're saying is a representative sample.
[00:16:28] Joseph: I understand, but the question is they, they do not have any limit.
[00:16:32] Joseph: We can send up up to like 1 million. That's what we decided.
[00:16:35] Joseph: But then Brian and like other team, they didn't want to upload the entire.
[00:16:40] JC Camacho: See,
[00:16:42] Joseph: I'm not sure. But that was when.
[00:16:45] JC Camacho: When we get, when we get direction that differs from what Virginia or from the original direction that we get, we should ask why we can't just go and just say, you know, somebody else said something.
[00:16:55] JC Camacho: So we're going to trump what Virginia said versus what Brian is saying. Right.
[00:16:59] JC Camacho: We have to ask the wise and come back and say, well, okay, Virginia, Brian is saying this because of this, this and this.
[00:17:06] JC Camacho: Right.
[00:17:07] JC Camacho: And now Virginia, I, I get what you're saying, but I, I had interpreted it another way, which is where we would.
[00:17:13] JC Camacho: And that's why I gave the direction just to load the 25, 000 as long as it was representative.
[00:17:17] JC Camacho: But now you're saying that out of the match.
[00:17:19] JC Camacho: So after, obviously we can load 25, 000 and if only two records match, then we're screwed.
[00:17:23] JC Camacho: So that's why you preferred us to load everything and let them Dutch it.
[00:17:27] Granth S: Okay, can I just interject here?
[00:17:31] Granth S: The reason why Brian said no is because he believes that if we upload the entire thing because TU can use our data to train their model and we don't want that.
[00:17:43] Granth S: So that's the reason why there was a hesitation on Brian's side that we, he.
[00:17:47] Granth S: He didn't want to upload the entire thing. And again, this is a product decision.
[00:17:53] Granth S: Us engineers, we're just here to solve and that's fine.
[00:17:57] JC Camacho: But again, Joseph answer was I don't know. Right.
[00:18:01] JC Camacho: So then that's, that's why we were saying, well, if there is a reason and it's legitimate, then fine.
[00:18:07] JC Camacho: But does Brian know that we send to you monthly data from everything that, you know, we already send them all our data on a separate feed on a monthly.
[00:18:16] Virginia Marsh: And they get it every day as they. People log in. People are. As people sign up.
[00:18:20] Virginia Marsh: They're getting a live feed and they're getting a monthly feed.
[00:18:23] JC Camacho: So. Yeah. So it's not like we're giving them anything that they probably already don't have.
[00:18:27] Virginia Marsh: Exactly.
[00:18:29] Granth S: Then. Then in that case, yeah, that. I guess we'll just have to reiterate this to Brian.
[00:18:35] Virginia Marsh: I just sent him a note too. And I'll. I'll have a call with.
[00:18:39] Virginia Marsh: With Adrian and Brian and team and just try to figure out. Because it's just way too confusing.
[00:18:45] Virginia Marsh: It's product is product. And I get that.
[00:18:47] Virginia Marsh: And if there's limitations where there's costs and all that associated, I completely get it.
[00:18:53] Virginia Marsh: I understand where you. You're saying he feels like they're stealing our data.
[00:18:56] Virginia Marsh: That makes no sense because we're literally selling it to him. They're.
[00:19:00] Virginia Marsh: They're getting, like I let JC said, a monthly file that comes from JC's team and they're getting an active as people sign up file.
[00:19:08] Virginia Marsh: So it's not. I don't think so.
[00:19:11] Virginia Marsh: And the contract that we have with them on tap precludes them from being able to do that.
[00:19:15] Virginia Marsh: So if they're doing it, shame on them. And if we ever had to do an audit it.
[00:19:19] Virginia Marsh: But even if we did, we'd still find our data because we're literally selling it to them separately.
[00:19:26] Granth S: Sounds good.
[00:19:27] Joseph: Once.
[00:19:28] Granth S: Yeah. I mean, in that case, then, yeah, whatever. If you could. If once every.
[00:19:34] Granth S: Once the team's on board, everyone's aligned, if you could just shoot a note to us, then we can move accordingly.
[00:19:43] Granth S: But I do understand what you're saying.
[00:19:46] JC Camacho: And Grant. Grant and Joseph, this is really. It's not on you guys. Right. We totally get it. Right.
[00:19:51] JC Camacho: So don't, you know, don't. Don't take any offense. It's more about.
[00:19:53] Granth S: I know, I know.
[00:19:54] JC Camacho: You know, we just want to make sure that everyone is aligned because I think unfortunately fluent as a whole, we're not aligned.
[00:20:01] Granth S: Yeah.
[00:20:01] JC Camacho: So we got to just get aligned. And I think, you know, Virginia reaching out to Brian is good.
[00:20:06] Virginia Marsh: And we have our call with Adrian and Adrian's team tomorrow. So I'll bring it up and ask.
[00:20:12] Virginia Marsh: And if it's obviously, if it's a product issue, completely under understand.
[00:20:17] Virginia Marsh: If it's a legal issue, then we just bring in, you know, Jason and team.
[00:20:20] Virginia Marsh: But at the end of the day, we.
[00:20:23] Virginia Marsh: We can't expect for our clients to pay millions of dollars if we can't put our best foot forward and we can't limit ourselves because we feel like if we do this than that.
[00:20:35] Virginia Marsh: Completely agree with jc. This is completely not on you guys.
[00:20:38] Virginia Marsh: It's just we just need to figure out how do we move forward.
[00:20:42] Virginia Marsh: And there's just a lot of different things that I think we're just running into.
[00:20:46] Joseph: For sure.
[00:20:47] Granth S: Yeah.
[00:20:47] Granth S: Like I said, just if once everyone's on board, if you could just let us know, that'd be really great.
[00:20:53] Granth S: Thank you.
[00:20:57] Virginia Marsh: So could you.
[00:20:58] Virginia Marsh: I know there's a ton of audiences and so is there any chance that somebody can just walk through where we are and what's pending, what's open?
[00:21:10] Joseph: Yeah, I can go faster for 192. I think we have put up the declared audience.
[00:21:16] Joseph: I'm just waiting for on the production workspace and the audience solution so we can push it.
[00:21:21] Joseph: I think Monisha also created a PR for the declared audience for her ticket so we can push all together in one shot.
[00:21:29] Joseph: And apart from that, in my ticket I have around like 50 audiences which are modeled that I have asked Monisha to pick up because I'm working on the other task in the E board and even Monisha has the capacity as she has completed the other model audience tickets.
[00:21:47] Virginia Marsh: So 192. It's showing 38.
[00:21:52] Joseph: 39 declared audience.
[00:21:54] Virginia Marsh: 39 declare. Okay, the last note was on June 15th and it was showing 38 and 47.
[00:22:06] Joseph: Okay.
[00:22:08] Monisha BL: So. So the ticket.
[00:22:11] Monisha BL: In ticket 189, the declared audience have raised the PR and the model audience is almost completed.
[00:22:16] Monisha BL: I have just one model audience segment which I couldn't pull the.
[00:22:20] Monisha BL: I can couldn't upload the seed even if it's less than 350. MBA.
[00:22:25] Monisha BL: I have mailed the TU support team regarding that and I didn't get any reply.
[00:22:29] Monisha BL: I did the follow up also. I'll it again.
[00:22:32] Virginia Marsh: Well, hold on. So we were on 192 and you. You were talking about 189. I just want to make sure.
[00:22:39] Monisha BL: No, no, no. I thought you were talking about the other one. That's the 189.
[00:22:45] Virginia Marsh: Okay, so the 189. That one started May 26th and the last update was June 2nd.
[00:22:57] Monisha BL: Actually, yeah, the declared audience.
[00:22:59] Monisha BL: I have raised the PR and for the model audience, it's like almost completed.
[00:23:05] Monisha BL: I just have one segment for which I couldn't upload the seed because of the size limit.
[00:23:12] Monisha BL: Size limit was below one. I mean 350mb. I'm just working with TU on that.
[00:23:17] Virginia Marsh: So the one audience. When you say so it was less than a thousand people. So you couldn't model it.
[00:23:24] Monisha BL: No, that was a different ticket. So this is a different ticket.
[00:23:28] Monisha BL: So it has like more than 6 lakhs numbers but I couldn't upload it in the tab. I.
[00:23:35] Monisha BL: I don't know what's the reason. I am just getting in touch with the support team for that.
[00:23:41] Virginia Marsh: And you put in the ticket and raised it with Brian or do you need to send me the ticket and I raise it with Brian?
[00:23:49] Monisha BL: I actually raised it with Kania. I think so. So I'll just mail Brian if it's necessary.
[00:23:57] Monisha BL: And I also put the update in the jira.
[00:24:08] Virginia Marsh: So you're saying that that 600k would not load up? It was too.
[00:24:14] Monisha BL: No, it's not too big. I think there is some issue with the files format.
[00:24:18] Monisha BL: But I just loaded other all segments with the same file format.
[00:24:24] Monisha BL: I don't know what's the issue with this same file format.
[00:24:28] Virginia Marsh: Okay, and then when did you put this tip ticket in and has Kanaya said anything?
[00:24:36] Monisha BL: I just put the tick.
[00:24:37] Monisha BL: I just put the Mail on 17th of June and then she just replied me regarding the error message.
[00:24:48] Monisha BL: And then I think she was out of office when I gave the follow up message last week and I didn't get any reply as of now.
[00:24:56] Virginia Marsh: Okay. Can you copy me on that email?
[00:24:58] Monisha BL: Yeah, sure, I'll do it.
[00:25:01] Virginia Marsh: Okay. All right. And then what other ones do we have? So that we talked about? 189, 192. What about 193?
[00:25:19] Virginia Marsh: That one's just waiting on me. Is that completely closed?
[00:25:23] JC Camacho: I thought that was.
[00:25:30] Virginia Marsh: Okay.
[00:25:31] JC Camacho: Yeah, Yeah. The segments. It says the Last on the 19th.
[00:25:38] JC Camacho: The segments are live and all in the marketplaces.
[00:25:41] Virginia Marsh: Okay, I will go triple check that. Playful. We can't do anything. 192, 189, 188.
[00:25:56] JC Camacho: I think that's also. No, it's not. Is that playful? That's also playful data.
[00:26:05] Virginia Marsh: Yep. So we can't do anything. There. Is 187 also playful. Nope. That one's just waiting for me.
[00:26:17] Virginia Marsh: It's already closed on. So never mind.
[00:26:19] JC Camacho: Right, Sorry.
[00:26:24] Virginia Marsh: I'm just trying to go through the list.
[00:26:25] JC Camacho: No, yeah. So. So.
[00:26:31] Virginia Marsh: Okay. So it's saying here 196. Is 196 something that Joseph put in and nobody's picked up.
[00:26:47] Virginia Marsh: It's from June 12th.
[00:26:49] Joseph: Sorry, that, that was. I was testing with an agent. So I. I tried to delete it, but I couldn't. J.C.
[00:26:56] Joseph: if you can, can you please delete that? Yeah.
[00:27:01] Virginia Marsh: Okay. Is that the same for one? So that's 197, 196. Those are all the same overdraft.
[00:27:12] Virginia Marsh: So, yeah, it would be 196, 97, 98, 99, 200, 201, and 202. Those are all tests?
[00:27:54] JC Camacho: Yeah. 205 and 206. I just said I just assigned to. To Monisha. Okay.
[00:27:58] Virginia Marsh: Okay, I see.
[00:27:59] JC Camacho: 200. That one's done.
[00:28:11] Virginia Marsh: Okay, so which ones are actually open?
[00:28:17] Virginia Marsh: Other than the ones that I brought up this morning and this one that I know that Monisha is working on because she's waiting on live ramp.
[00:28:26] Virginia Marsh: I mean, tuition, I believe.
[00:28:33] JC Camacho: I think that that is it. The ones that are staying, I guess, pending are the ones that are.
[00:28:42] JC Camacho: Another one for playful.
[00:28:43] JC Camacho: And then obviously the 194, which will stay open until we can confirm with you and Manav as far as the seed logic.
[00:28:53] Virginia Marsh: Okay.
[00:28:54] JC Camacho: All right. And then, yeah, I'll work on getting rid of the ones that are. That. That Joseph created.
[00:29:01] Virginia Marsh: All right. I really appreciate it. Thank you.
[00:29:03] JC Camacho: All right, guys. Thank you so much.
[00:29:05] Virginia Marsh: All right. Bye.
[00:29:06] Granth S: Bye.
