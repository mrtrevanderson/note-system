---
title: "Manav x Trevor Sync"
date: "2026-06-18"
start_time: "2026-06-18T11:30:00-04:00"
end_time: "2026-06-18T12:00:00-04:00"
meeting_id: "040000008200E00074C5B7101A82E00807EA0612A5440CF490DDDC01000000000000000010000000AB76D0CA659EAC419E1852FA011E20DB"
note_id: 80285280
recording_id: 14666354
fellow_url: "https://fluentco.fellow.app/meetings/040000008200E00074C5B7101A82E00807EA0612A5440CF490DDDC01000000000000000010000000AB76D0CA659EAC419E1852FA011E20DB/"
meeting_link: "https://fellow.link/WFQQT1G0941X"
---

# Manav x Trevor Sync

## Participants

| Name | Email | Organizer | RSVP |
|------|-------|-----------|------|
| Trevor Anderson | tanderson@fluentco.com | yes | accepted |
| Manav Paul | mpaul@fluentco.com | no | notResponded |

## Fellow Notes

Talking Points
The things to talk about
( ) 

Action Items
What came out of this meeting? What are your next steps?
[ ] 

Notepad
Anything else to write down?
 •  

AI-Detected Action Items
[ ] Manav Paul to post weekly updates into Lattice every Friday covering AI wins, weekly highlights, next week's focus, blockers, and other updates
[ ] Manav Paul to send the DevOps request list (10 items previously shared with David) to Trevor via Slack
[ ] Review the DevOps requests for the segment registry pipeline and follow up with the DevOps team

## AI Summary

Trevor and Manav held their weekly sync to discuss performance tracking processes and ongoing project work. The team will now **post weekly updates into Lattice** starting next week, which Dan will use to report data engineering progress to leadership and will eventually feed into annual performance reviews conducted each January. Manav is currently handling **large model audience requests manually** for CMI (the company's biggest client) while blocked on pipeline automation due to service principal access issues and pending DevOps requests. The **segment registry automation pipeline** is targeted for Q3 2026 completion, with most base code already written and ready for testing once blockers are resolved. The team also discussed the need to standardize JIRA ticket formats with required fields and validation to support the new automation pipelines and eliminate inconsistencies across different account managers.

### Chapters

#### Lattice performance tracking implementation

- [00:02:27] The company is rolling out **weekly Lattice updates** with five questions covering AI wins, weekly highlights, next week's focus, blockers, and other updates. Dan will use these to report data engineering progress to leadership.
- [00:03:13] Trevor linked the recurring 1-on-1 meeting with Lattice, though it's unclear what functionality this provides beyond a potential notes tracking feature that may need manual entry.
- [00:05:56] Fluentco conducts **annual performance evaluations in early January** using Lattice, where employees and managers both complete self-assessments and ratings on skills like relationship building, then compare responses.
- [00:08:39] Trevor plans to have ongoing conversations with Manav about career goals and aspirations to help navigate opportunities within the department, assign relevant work (like AI automation or Databricks projects), and provide guidance based on his own career experience.
- [00:11:40] The weekly Lattice updates will automatically summarize throughout the year to help employees prepare their annual performance review submissions, providing a running record of accomplishments.

#### Model audience request workload

- [00:12:04] Manav is handling a high volume of model audience requests manually this week, including some of the **biggest audience requests ever received** from CMI, the company's largest and highest-paying client.
- [00:12:19] Manual processing is still required due to the FDSO(?) situation and lack of validation testing for the production-ready automation pipeline, requiring hand processing of seed files, modeling, score ranges, custom audiences with filters, and taxonomy distribution.
- [00:21:06] Multiple requests came into the AS board recently, with contractors, Manav, and Joseph each picking up tickets. Virginia flagged that many requests are overdue, requiring additional hands beyond the contractors.
- [00:23:47] Manav will likely pick up Joseph's model audience tickets (approximately 14 modeled audiences) since he's out and clients will expect timely completion of the work.

#### Declared audience pipeline blockers

- [00:13:05] Grant has one blocker that Trevor reached out to David about, which is being worked on. The **service principal needs write access to two buckets** to properly deliver files to both vendors.
- [00:14:41] Manav is fully blocked on the automated segment registry pipeline (the new declared audience system) and has **10 additional DevOps requests** from a meeting with David two weeks ago that haven't been started yet.
- [00:15:21] The segment registry is targeted as a **Q3 2026 goal**, while Q2 was strictly focused on pipeline setup. Most of the base code is already written and ready for dry run testing once DevOps requests are fulfilled.
- [00:16:24] Manav remains busy with manual audience requests in the meantime, so the team is not behind schedule on the automation project despite the blockers.

#### Segment registry JIRA standardization

- [00:17:05] The original ticket design called for Virginia to get GitHub access and approve new audiences by granting YAML file changes, though Manav believes a **JIRA linkage solution would be better** based on working with the team.
- [00:24:47] Current audience request tickets lack standardization - the rate/cost field is just one giant text field, and **every account manager formats tickets differently** (some put CPM in description, others in comments), making it difficult to switch between people's work.
- [00:25:06] The standardization plan will separate fields like CPM and POM into distinct required fields with validation, and the AS board will need a full revamp to support the automation pipeline reading tickets directly.
- [00:29:45] The system could use conditional fields where selecting 'model audience' displays different required fields than other audience types, and Grant also wants standardization for the model audience pipeline.
- [00:29:22] Manav created a brief outline mapping every field needed and field types for the standardized format, which still needs review with Grant before implementation to ensure it covers model audience requirements.

### AI-Detected Action Items

- Manav Paul: Post weekly updates into Lattice every Friday covering AI wins, weekly highlights, next week's focus, blockers, and other updates
- Manav Paul: Send the DevOps request list (10 items previously shared with David) to Trevor via Slack
- Trevor Anderson: Review the DevOps requests for the segment registry pipeline and follow up with the DevOps team

### Decisions

- The data engineering team will **post weekly updates into Lattice** for Dan to share with leadership and to build a record for annual reviews.

## Transcript

[00:00:00] Trevor Anderson: Yeah, I'll have a nice four. It's 11:11am for me or 10:30. Oh yeah, it picks back.
[00:00:10] Trevor Anderson: Oh so an hour and a half. Yeah, it's been. I don't know what happened.
[00:00:17] Trevor Anderson: It's just like some stuff showed up and it's been back to back since the stand up.
[00:00:27] Trevor Anderson: It happens, you know. I'm excited. Happy Thursday or early Friday however you want to look at it.
[00:00:35] Trevor Anderson: Excited for the long weekend.
[00:00:37] Manav Paul: Me as well. Tomorrow is my birthday as well so it's gonna be a nice little happy early birthday.
[00:00:43] Manav Paul: Thank you. Thank you.
[00:00:44] Trevor Anderson: Exciting one. One day closer to being 21 as I learned. One year closer. Yeah that's. That's exciting.
[00:00:56] Trevor Anderson: Happy birthday. Any birthday plans or.
[00:01:00] Manav Paul: I'm having dinner with my family tomorrow night and then probably afterwards we'll spend some time with some of my buddies.
[00:01:06] Manav Paul: But it's nice that I always get off now on my birthday because of all these holidays so it's pretty convenient.
[00:01:14] Trevor Anderson: Oh that's sweet. I don't have a holiday on my birthday. My birthday is on like near. No it's not.
[00:01:22] Trevor Anderson: I was about to say it's near Halloween but it's not really. It's in October.
[00:01:28] Trevor Anderson: That's the closest holiday though that doesn't.
[00:01:31] Manav Paul: I have two cousins Halloween. I have two cousins that both their birthdays on Halloween.
[00:01:36] Manav Paul: So yeah I don't know it was that common but maybe it is for my family.
[00:01:42] Trevor Anderson: I think if you. What is that like it's Valentine's Day is conception.
[00:01:48] Trevor Anderson: I think so than nine months before. So I think October is pretty popular birthday month for people.
[00:01:58] Manav Paul: Interesting.
[00:02:03] Trevor Anderson: Yeah. Well what's going on in your world?
[00:02:09] Trevor Anderson: Oh one thing I wanted to chat about was something that Dan wants to start seeing is and it would be good helpful for me to track is looks like the company is going to start rolling out or what the company wants Dan wants.
[00:02:27] Trevor Anderson: I guess it would be helpful for me too is we're going to start wanting us to post updates into Lattice weekly updates.
[00:02:36] Manav Paul: Okay.
[00:02:37] Trevor Anderson: So I don't know if you've used this platform at all.
[00:02:42] Manav Paul: They just added it a few months ago.
[00:02:45] Manav Paul: I used it for like we had like an end of year review for 2025 so I used it then and then other than that I don't have any.
[00:02:52] Trevor Anderson: Oh yeah. I was going to ask about that too but yeah, one quick thing on the update.
[00:02:57] Trevor Anderson: So the weekly update there's like an update section on Lattice. Okay.
[00:03:02] Trevor Anderson: I don't know if you want to like go go there.
[00:03:04] Manav Paul: Yeah, I'll take it out I think I saw an email saying something.
[00:03:07] Manav Paul: I thought it was just a reminder for this thing, but I saw it through Lattice so that I guess that's like linked to this now.
[00:03:13] Trevor Anderson: Oh, I just set up. Yeah, I just set up like I linked our stand up with Lattice.
[00:03:18] Manav Paul: Okay.
[00:03:18] Trevor Anderson: Like a one on one thing. I don't know what it does. I don't know if it does.
[00:03:22] Trevor Anderson: Takes any notes or tracks anything or does. I don't think it does it.
[00:03:25] Manav Paul: That's probably. We have to put notes in there or something, I guess.
[00:03:28] Trevor Anderson: Right? Yeah. So there's this update section.
[00:03:31] Trevor Anderson: It should show my updates and there's like five questions in there and it's like a weekly update and the questions are what are your AI wins for this week?
[00:03:44] Trevor Anderson: What did you do this week and had the biggest impact or what do you want to highlight?
[00:03:51] Trevor Anderson: What will your focus be for next week? Any blockers and anything else that you would like to share.
[00:04:01] Manav Paul: Okay, so that's. If I click on this one on one thing. Is it in there or how do I get to that?
[00:04:10] Trevor Anderson: It should be. There should be a. Oh, updates, updates. Yeah, I see, I see.
[00:04:15] Manav Paul: So this is just a thing to do weekly.
[00:04:17] Trevor Anderson: Yeah. So like every Friday and then there's like a little bit of a slack integration. Not much.
[00:04:24] Trevor Anderson: But in, in the apps section at the very bottom of the slack, I have the Lattice and it will like ping you on Friday to show your
[00:04:33] Manav Paul: update like starting tomorrow or I mean, I guess starting next week or something.
[00:04:37] Trevor Anderson: Yeah, next week. Yeah.
[00:04:39] Manav Paul: Oh, you know what, it has been pinging me now that I've clicked on this. Yeah.
[00:04:46] Trevor Anderson: Yeah, don't worry about your weekly, don't worry about past ones.
[00:04:48] Trevor Anderson: But like, okay, it would be good to see you post a weekly update into Lattice here.
[00:04:55] Manav Paul: Yeah, I can do that for sure.
[00:04:57] Trevor Anderson: Yeah. That way it'll be shared with, with me and Dan.
[00:05:00] Trevor Anderson: And Dan uses these to share the leadership what, what our data engineering team is, is working on.
[00:05:08] Trevor Anderson: So really helpful for him to, you know, ladder that up.
[00:05:13] Manav Paul: What's going on.
[00:05:14] Trevor Anderson: Yeah, exactly. You know, he, as he's taking on the CTO role, he's.
[00:05:19] Trevor Anderson: It's becoming harder for him to track what each department and team is doing. So. Right.
[00:05:27] Trevor Anderson: This is helpful for him.
[00:05:29] Manav Paul: Yeah, no problem. I can do that.
[00:05:31] Trevor Anderson: It's also, yeah, helpful for me too because I was having to do this, I was doing it on my own previously like tracking everyone's updates throughout JIRA and providing that update to Dan.
[00:05:44] Trevor Anderson: And so this just helps me get a better update.
[00:05:49] Manav Paul: There's one Place for everything.
[00:05:50] Trevor Anderson: Yeah. Also, I think what they're gonna. You already went through like a.
[00:05:56] Trevor Anderson: Perform an annual performance evaluation.
[00:05:59] Manav Paul: All right.
[00:06:00] Manav Paul: It was like, yeah, it was kind of like one of these updates, but it was like a yearly update.
[00:06:04] Manav Paul: Basically. They just asked like, you know, your wins for the year, your whatever, losses.
[00:06:08] Manav Paul: I guess you can say things like that. Answered a bunch of those questions.
[00:06:11] Trevor Anderson: And yeah, so I think it uses your weekly. I think it's gonna like do a summary for you too.
[00:06:15] Manav Paul: And then I think what happened was then Dan had to like go and like, I guess rate my responses on how much he agrees.
[00:06:22] Manav Paul: I think it was something like that. I don't know. And then he had.
[00:06:26] Trevor Anderson: Do they have any metrics?
[00:06:29] Manav Paul: I just. Let me see if I can actually find it. Okay. Okay.
[00:06:37] Manav Paul: I answered the questions and then I had to score myself for certain categories.
[00:06:43] Manav Paul: So then he answered the same questions, like, what do you call it, like, in terms of how he sees for me.
[00:06:50] Manav Paul: And then he would also rate me for those same rankings.
[00:06:55] Manav Paul: And then it would kind of compare whether our rankings were like similar or whatever the case may be.
[00:07:03] Trevor Anderson: Do you know what, like some of the questions were?
[00:07:05] Manav Paul: Yeah, I can actually just show you.
[00:07:15] Manav Paul: So in what ways did you see this employee learn and grow to better execute the role over the past year?
[00:07:20] Manav Paul: Recognize their key accomplishments over the past year, key priorities and metric driven goals for the coming year.
[00:07:26] Manav Paul: Some skills you want them to learn or strengthen. Evaluate the employee against the following factor.
[00:07:35] Manav Paul: Yeah, so these are the scoring ones. So three is like you're mastered it.
[00:07:39] Manav Paul: One is foundational, two is successful build relationships. Okay, so these were like.
[00:07:46] Manav Paul: Yeah, so this is like Dan would have to score. And then I also had ca. Had the same thing.
[00:07:50] Manav Paul: And I scored myself on certain ones, I guess.
[00:07:52] Trevor Anderson: Okay, interesting.
[00:07:55] Manav Paul: Yeah, so yeah, that was the only reason I ever utilized for that thing.
[00:08:00] Trevor Anderson: Do you know what time of year this happens at?
[00:08:02] Manav Paul: Is it like this? We did in January calendar year.
[00:08:05] Trevor Anderson: Okay, so January, we.
[00:08:07] Manav Paul: Yeah, it was in like early January right after like we came back from all the holiday season.
[00:08:17] Trevor Anderson: Do you fill out anything, like prior to the year start? Like, do you set goals or anything like that?
[00:08:25] Trevor Anderson: You guys didn't do that?
[00:08:26] Manav Paul: I don't think so.
[00:08:27] Manav Paul: Also, I just started full time a few months ago, so I don't know if that changes anything, but.
[00:08:31] Manav Paul: Oh, there's nothing I had to do.
[00:08:34] Trevor Anderson: Well, I guess we're in the same boat together. I'm not sure here.
[00:08:39] Trevor Anderson: You know, I guess this brings up a good topic though, something that I've done in the Past is kind of worked with everyone on their individual career goals and aspirations, you know, nothing, I guess, that would need to be recorded if it's not required, but something like just having an open conversation about, you know, making sure that, you know, this is the right career opportunity for you and this is what you want to be doing or the type of work that you're doing is what you're looking to do.
[00:09:20] Trevor Anderson: If, if there's anywhere else in the department that you would see a better fit for yourself, you know, that would be good information to share with me.
[00:09:27] Trevor Anderson: Sure.
[00:09:28] Trevor Anderson: Because it's probably something that I could actually help with, you know, help help you navigate, you know, based on kind of the same, you know, what I've gone through in my career so far and give you any advice or guidance or even help you put you in the right spot.
[00:09:47] Trevor Anderson: Maybe, you know, your goal is to, you know, become a senior engineer here or you know, like an architect.
[00:09:57] Trevor Anderson: Right. Like, like, how can I help you accomplish that? Right.
[00:10:02] Trevor Anderson: Or maybe, you know, you want to work more with AI automation and databricks and so like I can actually help get, you know, assign you those particular items that come up.
[00:10:16] Trevor Anderson: Right. Or. Yeah, right. Or maybe you like, you know, working with a particular type of data or. Right.
[00:10:25] Trevor Anderson: Stuff like that. So. Right.
[00:10:26] Manav Paul: Yeah, that makes sense.
[00:10:32] Trevor Anderson: But yeah, that's. I, I don't think they're like the company's really looking to see.
[00:10:37] Manav Paul: Yeah. Just from the questions, they're pretty like just, I don't know, performance based.
[00:10:42] Manav Paul: How you feel. Like, yeah, how you, how you did or anything. Just a review on yourself and your year.
[00:10:47] Manav Paul: But I actually think those kinds of questions are just as important, if not more important, just, you know, actually seeing you.
[00:10:54] Manav Paul: How do you feel about what you're doing this year and how do you want to proceed next year?
[00:10:58] Trevor Anderson: Yeah, yeah.
[00:11:00] Trevor Anderson: But yeah, other than that, just, I think just having those weekly updates would be great for now.
[00:11:05] Trevor Anderson: We can kind of talk about the kind of annual goals or you know, career career goals like later on, see if the company's looking.
[00:11:18] Trevor Anderson: I. I'll dig into that a little bit more and see if they're. They track that at all in some capacity.
[00:11:23] Trevor Anderson: I've done this in the past, so I don't know if Fluent is doing it, but looks like they just have that performance evaluation.
[00:11:30] Trevor Anderson: But
[00:11:33] Manav Paul: no, sounds good.
[00:11:34] Trevor Anderson: But yeah, it will also. Yeah, I think would be helpful. Is like Lattice. Yeah.
[00:11:40] Trevor Anderson: If you post those updates at the end of the year, it's able to summarize all those updates for you to Use for your annual.
[00:11:50] Trevor Anderson: Your annual summary.
[00:11:52] Manav Paul: Yeah.
[00:11:58] Trevor Anderson: Okay. Back to kind of what's currently going on.
[00:12:04] Manav Paul: Yeah, I mean in terms of this last week, mainly it's just been a lot of model audience requests coming in through the team, so been picking up those tickets and working through them.
[00:12:16] Trevor Anderson: Yeah. So you're still having to do that like manually right now?
[00:12:19] Manav Paul: Right now because of all the FTSO situation and then the.
[00:12:25] Manav Paul: Just no validation testing yet for the final production ready automation pipeline.
[00:12:31] Manav Paul: I am doing it all by hand and these are some of the biggest audience requests we've ever gotten, so.
[00:12:38] Trevor Anderson: Oh, geez.
[00:12:39] Manav Paul: But as you know, the OZEMPIC ones to cmi, those are kind of our, like that's our biggest client.
[00:12:46] Manav Paul: Those pay the most. So there's a huge request that just came in for that. So. Okay, working on that.
[00:12:52] Manav Paul: So at least they'll bring in. At least my. My work will bear good fruit.
[00:12:59] Trevor Anderson: Yeah, I want to get that manual thing kicked out the door and start using this pipeline that we have.
[00:13:05] Trevor Anderson: Bill, I just, I just chatted with Grant on his block on like his open.
[00:13:13] Trevor Anderson: I asked him if he has any blockers.
[00:13:16] Trevor Anderson: It looks like he has one right now that I just reached out to David about.
[00:13:22] Trevor Anderson: Said he's working on it, but I wanted to ask on your end, like is there any. There was.
[00:13:27] Trevor Anderson: So it was just one. Right.
[00:13:29] Manav Paul: Right. Yeah. So I answered his question. I gave him a slack. I.
[00:13:34] Manav Paul: I don't know if you saw it or not because he hasn't responded.
[00:13:36] Manav Paul: I don't know if he told you that he saw it, but whatever his open question was, I answered it, gave him some details on it.
[00:13:42] Trevor Anderson: Yeah, let me go back to that real quick.
[00:13:46] Manav Paul: We're still waiting on that because the sp, it needs ray access to the two.
[00:13:53] Manav Paul: The two different buckets so I can deliver the. The actual files to both vendors.
[00:14:00] Manav Paul: So I'm still waiting on that. And then that's for the current declared audience pipeline.
[00:14:06] Manav Paul: There's a boatload of requests for the automated segment Registry, which is like the new.
[00:14:13] Manav Paul: The new declared audience pipeline that I don't even know if that's been, you know, like worked on.
[00:14:19] Manav Paul: Yeah, I'm assuming not because that's not priority right now obviously, but that is there as well.
[00:14:24] Manav Paul: So I'm also fully blocked on that.
[00:14:26] Trevor Anderson: So what it. Okay, so for the one about the. Yeah, you need.
[00:14:30] Trevor Anderson: The service principal needs to be able to write.
[00:14:33] Manav Paul: Yeah. To the two buckets properly.
[00:14:37] Trevor Anderson: Yeah, I'll check on that one. What is this other thing that you're talking about?
[00:14:41] Manav Paul: I have like a I didn't make an FDSO for it.
[00:14:43] Manav Paul: Me and David had a meeting, like two weeks back maybe, and then we talked about some requests I had and then I sent them over through Slack.
[00:14:51] Manav Paul: I can send you the same requests, but It's. It's like 10 different requests, so.
[00:14:56] Trevor Anderson: Oh, send it to me.
[00:14:57] Manav Paul: Yeah, I'll send it right over.
[00:15:00] Manav Paul: But that, I'm assuming, hasn't even been started yet, just because, you know, we have all the other ones, which is fine.
[00:15:04] Manav Paul: I'm just making sure it's, you know, being voiced that that is blocked as well.
[00:15:11] Trevor Anderson: Yeah, it probably hasn't been worked on. Yeah, it's just gonna take.
[00:15:15] Manav Paul: But yeah, until luckily it's looking like it's a Q3 goal anyway to finish the segment registry.
[00:15:22] Manav Paul: So it's not like we're behind. But
[00:15:27] Trevor Anderson: yeah, it looks like QT was strictly like the pipelines.
[00:15:30] Manav Paul: Yeah.
[00:15:30] Trevor Anderson: Set up.
[00:15:31] Manav Paul: Right.
[00:15:32] Trevor Anderson: Q3 and segment registry. You're talking about like, AI.
[00:15:37] Manav Paul: Yeah, it's the. It's the new pipeline with the AI and the.
[00:15:40] Manav Paul: The full, like, automation where Virginia can like, put in a ticket and then it'll just go and build it out,
[00:15:45] Trevor Anderson: pick it up from there. Okay.
[00:15:46] Manav Paul: Yeah, so that is like a Q3 thing. But, yeah, like I said, until.
[00:15:54] Manav Paul: Until these requests are fulfilled, then I can't.
[00:16:00] Trevor Anderson: Oh, you're gonna have to copy and paste that. I can't see what you just said.
[00:16:03] Manav Paul: Okay.
[00:16:07] Trevor Anderson: From a private conversation. Okay, okay.
[00:16:13] Manav Paul: But, yeah, those are just the request sets for now.
[00:16:17] Manav Paul: And once those are done, then I can move forward with the automation and then actually testing it because pretty much all the base code is written.
[00:16:24] Manav Paul: So once this is all sorted, I can kind try and do a dry run and then iterate from there if it's.
[00:16:30] Manav Paul: Even if it's working or not working, whatever it is. But yeah, I'm busy on these requests anyway.
[00:16:37] Manav Paul: Luckily, these are my audience requests, so it's not a big deal.
[00:16:40] Manav Paul: But hey, we're not behind, like I said.
[00:16:42] Manav Paul: But this is definitely the next priority once Q2 stuff is handled because, yeah, can't move forward with that one.
[00:16:48] Manav Paul: And that's a pretty big project that has a lot of eyes on it.
[00:16:52] Trevor Anderson: What does this GitHub handle?
[00:17:00] Manav Paul: Oh, that's because I need to. I guess actually, that might not even be for them.
[00:17:05] Manav Paul: I might have thrown that in their backs in.
[00:17:06] Manav Paul: But there's like a step where Virginia needs to approve certain, like, audiences and it's gonna go through the repo, so she needs a GitHub account so she can, I guess, approve it.
[00:17:22] Manav Paul: But I might have accidentally. I don't think that's for them.
[00:17:30] Trevor Anderson: So she needs to. She needs.
[00:17:33] Manav Paul: She needs access.
[00:17:35] Trevor Anderson: Yeah. Separate access request. Right. To GitHub.
[00:17:42] Manav Paul: Yeah. Because when a new audience is being added to this new pipeline, it writes to a YAML file.
[00:17:48] Manav Paul: So she essentially has to have like, I guess, authority over the repo. So she can.
[00:17:55] Manav Paul: Basically, her way of accepting it would be like granting the change to the.
[00:17:58] Manav Paul: To the file and then eventually I'll link the JIRA in there and kind of put that into the process.
[00:18:05] Trevor Anderson: Is she going to be merging the code into like, main branch?
[00:18:10] Manav Paul: It's like very brief.
[00:18:11] Manav Paul: In the tickets I was given, I probably am just gonna have to build some kind of JIRA linkage to it because I don't think she wants to look at GitHub anyway.
[00:18:20] Trevor Anderson: So that's what I'm thinking too.
[00:18:23] Manav Paul: But that was just how it was outlined in the ticket. It was.
[00:18:26] Manav Paul: Her and JC will have like GitHub access and things like that.
[00:18:29] Manav Paul: But honestly, that's all kind of just based, you know, it's someone's cloud writing a.
[00:18:35] Manav Paul: Your ticket in the Miclo's looking at. Anyway, so I can. We can work from there.
[00:18:39] Trevor Anderson: Okay. Yeah, I think we. We should try to.
[00:18:42] Trevor Anderson: I think Joseph already had a little bit of a diagram going on this whole like.
[00:18:48] Manav Paul: Yeah.
[00:18:49] Trevor Anderson: Process. But we should flush that out a little bit more.
[00:18:51] Manav Paul: Yeah, this is. I'm. Right now I'm going purely off of the.
[00:18:55] Manav Paul: How the tickets had outlined and then from there I'll, you know, make my tweaks and whatnot.
[00:19:00] Manav Paul: I just don't. What do you mean?
[00:19:00] Trevor Anderson: The tickets? Like the, The Jira epic?
[00:19:03] Manav Paul: Yeah, like the epic. Like, however they had outlined it. I don't want to, you know, like the. The Jira.
[00:19:08] Manav Paul: Jira linkage is like kind of my idea thing.
[00:19:11] Manav Paul: I just feel like from, you know, working with Virginia, working with the team, I know that that'll actually be the better solution.
[00:19:16] Manav Paul: But, you know, I don't want to delay into things saying, oh, you know, I'm working on this JIRA linkage thing still.
[00:19:19] Manav Paul: It's not ready yet. And, you know, it's like we didn't even ask for that, so.
[00:19:22] Trevor Anderson: Right, right. Okay.
[00:19:25] Manav Paul: But that being said, like, we're definitely ahead, so if this DevOps stuff gets sorted sooner rather than later, then I might be able to get it all out in Q3.
[00:19:32] Manav Paul: I don't see why not especially, like, you know, I want to see this code being tested first so I can really get a grasp of how long Everything's going to take.
[00:19:39] Manav Paul: But you know, if there's not too many issues and not so many tweaks that need to be done on the actual pipeline side and the building audiences side, then we have plenty of time to throw in whatever kind of adjustments we want with Jira and then, you know, spend most of our time working on those AI agents.
[00:19:55] Trevor Anderson: Yeah. Okay. Let me look into these requests. Looks like some of these might be like to extend TUS3.
[00:20:03] Trevor Anderson: That's. That was already existing request. Right. That you just.
[00:20:08] Manav Paul: Yeah, this will have to be done for this new audience pipeline anyway, so. Or the old one as well.
[00:20:15] Manav Paul: Anyway, so I guess it applies to both. But just.
[00:20:19] Manav Paul: Yeah, I mean let them handle the main ones first right now because, you know, this is a later thing for sure.
[00:20:23] Manav Paul: But yeah, it would be nice if you can follow up with it once all these current ones are sorted.
[00:20:28] Trevor Anderson: Yeah. Okay. I'll keep this in mind. Thank you.
[00:20:33] Manav Paul: But yeah, other than that, it's just been those model audience tests. It's all going.
[00:20:38] Trevor Anderson: I'm sure that's.
[00:20:39] Manav Paul: Yeah, it's going on schedule.
[00:20:42] Manav Paul: I'm probably going to pick up some of Joseph's too, just because he's out and they're going to want it done anyway, so.
[00:20:50] Manav Paul: But other than that, no blockers besides the fdso.
[00:20:54] Trevor Anderson: Okay. Yeah. What does Joseph have right now? Masking policy.
[00:21:03] Manav Paul: It's in the. I know he has some in the AS board that.
[00:21:05] Trevor Anderson: Oh, yes.
[00:21:06] Manav Paul: I pick up. Yeah.
[00:21:07] Manav Paul: Because there's like a lot of requests that came in and then the contractors picked up some.
[00:21:12] Manav Paul: I picked up some. I think there's one that Joseph picked up that.
[00:21:16] Trevor Anderson: Yeah. How does that. Can you go to that and show me.
[00:21:19] Manav Paul: Yeah.
[00:21:19] Trevor Anderson: Board that works. How do you.
[00:21:23] Trevor Anderson: You just look at the board and say, well, this one looks like I have capacity. I can pick this up.
[00:21:29] Trevor Anderson: Or how does that.
[00:21:30] Manav Paul: So yeah, for the most part. For the most part we pick up our own. This usually.
[00:21:37] Manav Paul: It's recently been working on automation so the contractors have just been picking them up and handling them.
[00:21:42] Manav Paul: But JC had assigned some to me and Joseph recently just because I think Virginia was saying that we have a lot that are overdue and it was too many for the contractors handle alone.
[00:21:52] Manav Paul: So we kind of signed them.
[00:21:55] Trevor Anderson: Yeah.
[00:21:56] Manav Paul: Just because we. We need the extra hands. So this is the one I'm currently working on that cmi.
[00:22:02] Trevor Anderson: Okay. So how does this look? How do you know what to do here? Like this is audience type.
[00:22:07] Trevor Anderson: Modeled audience. That tells you what audience it is.
[00:22:11] Manav Paul: Yeah. So I mean also I know that this is model just because I've done so many for them now.
[00:22:16] Manav Paul: But I look here, I see these parameters, I go into the seed file query in databricks, I query this with this parameter and then I go and I find these are the answers she wants.
[00:22:29] Manav Paul: So I go in, I get those answers, I download the seed files, put them in the tap, they'll process, then I'll model, that'll process.
[00:22:38] Manav Paul: And then today I did the score range which is basically taking the lookalike and turning it into a, like into an audience with the fully expanded from the whatever, let's say 5,000 person seed, we expanded to a 12 million audience.
[00:22:52] Manav Paul: That's going to take a data process and then tomorrow I will do the custom audience which is like tweaking that audience with filters basically.
[00:22:58] Manav Paul: So I'll say if for this one for example it'll be these, all these five seeds have to be included and then I also have to exclude Washington, Nevada and Connecticut because it's a healthcare or health audience, then that'll process After I do all these five audiences, then distribute, then I have to do taxonomy for all the marketplaces and then send it to the client and the client will receive it.
[00:23:28] Manav Paul: Taxonomy is like that pricing stuff. So this is a. Yeah, this is the pricing information here.
[00:23:32] Manav Paul: So in Live Ramp and TU and stuff you go in and put it in there and then send it over to the client's dsp.
[00:23:40] Manav Paul: But yeah, that's what I'm right now.
[00:23:42] Manav Paul: I just finished the lookalike thing so waiting for tomorrow they'll do the custom audiences.
[00:23:47] Trevor Anderson: Got it.
[00:23:47] Manav Paul: And then Joseph has these two.
[00:23:49] Manav Paul: So I will probably look at these because I just finished today's work for the other ones so have some time.
[00:23:56] Manav Paul: Anyway, I think this is the one he said he, he has like a lot of audiences here but there's specifically, there's 14 I think modeled.
[00:24:05] Manav Paul: So I'll probably just pick those modeled ones up for him.
[00:24:09] Trevor Anderson: This is all playful.
[00:24:10] Manav Paul: Yeah, actually it might be this one now I think about it. Yeah, maybe this one has to take a look.
[00:24:25] Manav Paul: But yeah, other than that there's.
[00:24:29] Trevor Anderson: So I'm curious how this would work with the pipeline. Like I feel like we need actual
[00:24:34] Manav Paul: like very standardization or.
[00:24:36] Trevor Anderson: Yeah. Parameters there would be.
[00:24:38] Manav Paul: I've already mapped one out.
[00:24:39] Manav Paul: I mapped out like a standardization that actually Granth was also wanting one because we're going to probably add it to the model audience pipeline as well.
[00:24:46] Manav Paul: So we were taking a look.
[00:24:47] Manav Paul: It's brief but these current fields that are there right now, these will Stay.
[00:24:52] Manav Paul: But then we also have to add certain other fields.
[00:24:54] Manav Paul: And also, like, you see how this rate slash cost is just one giant text field.
[00:24:58] Manav Paul: Those will all be separated with, you know, CPM is one field, POM is one field.
[00:25:03] Manav Paul: They'll have to be a standardized ticket format, which we'll work on.
[00:25:06] Manav Paul: And then with that being set up, Virginia can just go in and fill everything out and then the ticket will be picked up, read, and thrown into the pipeline.
[00:25:17] Trevor Anderson: So have we explored, like, JIRA validation or like, you know, you can't create a ticket without these fields filled in.
[00:25:27] Manav Paul: I haven't taken a look yet, but that'll definitely be something. Yeah.
[00:25:31] Trevor Anderson: If we're doing it on the JIRA side, like, you can explicitly say, like, this is a required field.
[00:25:37] Manav Paul: Yeah. I think honestly, this whole board will have to have a full, like, kind
[00:25:41] Trevor Anderson: of design a little bit.
[00:25:42] Manav Paul: Yeah. Revamp. But once it's sorted, it'll.
[00:25:45] Manav Paul: It won't be anything big enough where it's going to be confusing.
[00:25:47] Manav Paul: Like, Virginia will be able to make a ticket just as she always does.
[00:25:51] Manav Paul: But once that standardization is set, you know, it'll.
[00:25:55] Manav Paul: It'll be great for the future because then she can just go and make a ticket.
[00:25:58] Manav Paul: Every ticket looks the same.
[00:25:59] Manav Paul: There's another thing too, because one, every ticket she makes always looks a little different.
[00:26:03] Manav Paul: And then every ticket every person makes. So, like, there's gonna be an account manager coming in.
[00:26:08] Manav Paul: You Are we. I've had two account managers in the past. Every person makes a ticket look different.
[00:26:12] Manav Paul: Right. It's just how it is. So it's.
[00:26:14] Manav Paul: It's a little difficult actually switching between people's tickets because you don't even know what to look for.
[00:26:19] Trevor Anderson: Yeah.
[00:26:20] Manav Paul: One person might like to write the CPM in the description.
[00:26:22] Manav Paul: The other person might like to put the client's information description.
[00:26:26] Manav Paul: Another person might like to put the client's information in the comments. You know what I mean?
[00:26:29] Manav Paul: So a standardized, like, format will be really helpful for just the future.
[00:26:34] Trevor Anderson: So how does she do that right now? She goes through that create button,
[00:26:38] Manav Paul: I guess. I don't think tickets. I don't even know.
[00:26:41] Trevor Anderson: Oh, you don't know because if you
[00:26:43] Manav Paul: click that, this is feedback. I don't think I can.
[00:26:46] Trevor Anderson: That top button.
[00:26:47] Manav Paul: Oh, here?
[00:26:48] Trevor Anderson: No, no, the one at the top, like by the search bar. It won't create it in. Just click that.
[00:26:54] Trevor Anderson: Yeah, yeah, yeah.
[00:26:55] Manav Paul: I, Yeah, I don't make tickets normally, but I guess she goes through here and then.
[00:27:02] Manav Paul: Yeah, there are some fields I guess on there. This form will be like yeah, completely different.
[00:27:08] Trevor Anderson: There's some more fields details, sir. Detail section. Because that's where the. All that.
[00:27:15] Manav Paul: This is where she puts the rate and stuff.
[00:27:20] Trevor Anderson: How'd you get there?
[00:27:22] Manav Paul: You go to more fields.
[00:27:25] Trevor Anderson: Oh, wait one second, I'm trying to figure that out. Wait, where, Sorry, more fields.
[00:27:32] Manav Paul: You see past assignee. There is more fields down here at the bottom. At the very bottom. If you scroll.
[00:27:46] Trevor Anderson: Showing for me. No, weird.
[00:27:49] Manav Paul: Maybe it's a permissions thing.
[00:27:56] Trevor Anderson: I just see sprint is the last thing for me and then more fields is not.
[00:28:02] Trevor Anderson: It might be a permissions thing, but.
[00:28:06] Manav Paul: Yeah. So this will have to be like a more standardized form. We'll adjust with proper fielding.
[00:28:12] Trevor Anderson: Okay.
[00:28:13] Manav Paul: And then. Yeah.
[00:28:14] Manav Paul: Then from there whether a person wants to pick it up or really it'll be the automation mostly.
[00:28:18] Trevor Anderson: But anyway, if you scroll. If you scroll up on this, you'll see.
[00:28:24] Trevor Anderson: So yours looks different than mine because mine shows summary description, audience type.
[00:28:30] Trevor Anderson: Is that what your says? Yeah, yeah.
[00:28:33] Trevor Anderson: And then mine actually shows distribution platforms with the check boxes.
[00:28:38] Manav Paul: Yeah, that's more fields. You're in more fields.
[00:28:43] Trevor Anderson: Oh, what? Mine just shows it outside and that's the only thing it shows. It just says. Oh no.
[00:28:49] Trevor Anderson: Oh, mine doesn't shows the drop down. It just shows it straight up. Okay.
[00:28:52] Manav Paul: Okay.
[00:28:52] Trevor Anderson: I don't know. I don't know why it's different. I see other fields.
[00:28:57] Manav Paul: Yeah.
[00:28:57] Trevor Anderson: Okay. You see due date, requested audience size, rate, cost. Okay. Destination.
[00:29:06] Trevor Anderson: So we'll probably have to make these like actual.
[00:29:09] Manav Paul: Yeah, actual fields with actual fields that are required. Yeah.
[00:29:12] Manav Paul: And then not just strings that are just. You can write whatever you want.
[00:29:15] Manav Paul: Some validation will be there for sure.
[00:29:17] Trevor Anderson: Could be. Yeah. Depending on the selection. It could be a drop down or check.
[00:29:22] Manav Paul: Yeah, I kind of outlined it. I'll actually send over whenever I get a chance.
[00:29:27] Manav Paul: I like had like a brief little every field we would need and what type the field would be. So.
[00:29:33] Manav Paul: But also I still have to take a look at it with Grant as well.
[00:29:35] Manav Paul: Just see if there's anything that needs to be adjusted for. If it's model audience, separate fields.
[00:29:40] Manav Paul: I'm sure you can do something like that where she selects something like model audience and the drop down here.
[00:29:45] Manav Paul: The fields that are then offered are slightly different.
[00:29:48] Trevor Anderson: Yeah, that's. Oh, that's perfect. That's probably what. Exactly what we want to do.
[00:29:55] Manav Paul: Yeah. Keep her away from GitHub for. You know, because there's no reason anyway. And then this will.
[00:30:01] Trevor Anderson: Now I'm curious how it goes back to GitHub like. Or how all those GitHub integrate with Jira rather.
[00:30:06] Manav Paul: Well, then what I would do here is I wouldn't have to have her grant the.
[00:30:12] Manav Paul: I guess the merge or whatever the.
