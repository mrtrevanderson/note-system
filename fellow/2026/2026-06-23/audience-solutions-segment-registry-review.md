---
title: Audience Solutions Segment Registry Review
date: 2026-06-23
time: 15:05-16:00
participants: [Trevor Anderson, Brian Silveri, Granth Sajjanshetty, Joseph Ramesh, Manav Paul, JC Camacho]
fellow_url: https://fluentco.fellow.app/meetings/040000008200E00074C5B7101A82E0080000000060A8291CF802DD0100000000000000001000000039F3C08209EC9F4AAF56524199722796/
captured_at: 2026-06-24T05:00:00-07:00
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

AI-Detected Action Items
[ ] Brian Silveri to follow up with Virginia, Dan, and Adrian to align on expectations for Phase 1 (platform) vs Phase 2 (AI agents) deliverables.
[ ] Manav Paul to review the segment registry epic, ensure all stories are relevant and properly prioritized, and close out irrelevant stories with explanatory comments.
[ ] Brian Silveri to set up a discovery call with Virginia (including the full team) to discuss reporting requirements and data needs for the audience solutions UI.
[ ] Push through the DevOps permission requests for the segment registry platform once Griggs completes current priority FTSOs.
[ ] Manav Paul to follow up with Dan about the Anthropic console account setup for AI agent testing.

---

The team reviewed the **Audience Solutions Segment Registry project**, a segment creation tool designed to help the team scale to **2000+ segments** and automate segment building for a new account manager starting **July 7, 2026**. Key decisions included implementing **PII masking in Q3**, restricting data access to **gold mailable universe tables only**, and sending only **email hashes (not full PII) to external partners** like TU and LiveRamp to protect proprietary first-party data. The team clarified project phases: **Phase 1 prioritizes the segment registry platform** (automated pipeline for declared audiences), while **Phase 2 splits the team** between AI agent development for segment suggestions and a reporting UI for performance insights. Several dependencies were identified, including DevOps permissions for testing and an Anthropic console account for agent development.

### Segment Registry project scope

- The Audience Solutions leadership requested a **segment creation tool (Segment Registry)** to build segments at scale and manage the process, targeting **2000+ segments** with automation capabilities.
- A new account manager starting **July 7** will use the tool to build segments, identify gaps, and refresh existing segments without requiring deep technical knowledge or databricks access.
- The segment creation process will use a **standardized JIRA ticket format** where the account manager submits requests, and the automated system picks them up and builds them out.
- The pipeline will **refresh all segments every Sunday night** using a YAML file, and check every morning for new segment requests to build them immediately without waiting for the weekly refresh.
- The team needs to solve how Virginia's team will discover available data and create segment requests when not initiated by customers, with databricks access planned for July-August and a lightweight UI targeted for September.

### Data governance and PII protection

- Currently **full PII is sent to both TU and LiveRamp** for audience builds, but the TU contract allows them to store and use Fluentco's proprietary first-party data, which represents significant value leakage.
- The team decided to **send only email hashes** (not full PII like names, addresses) to external partners to protect proprietary data, with final approval needed from Virginia on this strategic change.
- The source table must be the **gold mailable universe** (post-FTC cleansed) to exclude underage users, suppression files, and pre-FTC users from segment builds.
- **PII masking will be added to Q3 scope** for the audience solutions dataset, ensuring the new account manager and non-engineering roles cannot access raw email addresses or personal information.
- Trevor is creating an **audience solution analyst group** in databricks with restricted access only to schemas and tables required for building audiences, excluding bronze and silver layers entirely.

### AI agent development plans

- Manav has built a **basic first draft of the AI agent** for segment exploration and suggestion, but testing is blocked by DevOps permissions and the need for an Anthropic console account.
- The AI agent would **automatically suggest segments, suggest questions, and build them out** without human intervention once approved, representing full start-to-finish automation.
- The **exploration agent is not a big focus** until other components are completed, as the current priority is the automated building process rather than the discovery/suggestion phase.
- Dan Hall originally requested feeding **all question-answer data into Claude** to see if it could automatically generate segment suggestions, with logic to avoid requesting already-created segments.
- Manav will follow up with Dan about the **Anthropic console account** needed for agent testing, which Dan is coordinating with Tim.

### Project phases and priorities

- The team will **immediately start testing story 1593** (platform testing with gold mailable universe) as soon as the prod infrastructure deployment is complete.
- Brian confirmed the **segment registry platform is a prerequisite** for AI agent work and must be completed first, as agents cannot function without the underlying infrastructure.
- **Both the platform and AI agents are Q3 goals**, with the platform being the immediate priority and agents being tackled once the platform is operational.
- After platform completion, **the team will split**: half working on AI agents for segment suggestion, half on the audience solutions UI for reporting insights.
- The reporting UI will enable queries through **Claude-to-databricks connector or databricks directly**, providing insights on segment performance across platforms rather than building numerous individual dashboards.

### Dependencies and blockers

- Work was dependent on **finalizing the gold mailable universe definition**, which has now been completed and is available for use.
- Testing is blocked by **DevOps permissions issues** - Manav cannot test the pipeline or write to buckets until DevOps completes the required permission configurations.
- The reporting UI requires **revenue and usage data to be brought into databricks first**, showing how much each segment has made Fluentco and how many people have used it.
- Trevor needs to prioritize **DevOps permission requests** for this project once Griggs completes current FTSO priorities to unblock testing and development.

### Decisions

- Only **email hashes will be sent** to TU and LiveRamp (not full PII) to protect proprietary first-party data from being stored by partners.
- The **gold mailable universe table** will be the source for segments, excluding pre-FTC users, underage users, and suppressed addresses.
- **PII masking will be implemented in Q3** for the audience solutions dataset to prevent non-engineering users from accessing raw PII.
- Data access for audience solutions analysts will be **restricted to gold tables only** (no bronze or silver layer access).
- Once prod deployment is complete, the team will **immediately focus on story 1593** to test the platform using the gold mailable universe.
- The **segment registry platform is a prerequisite** for AI agent work and will be prioritized first in the development sequence.
- **Q3 work will be split into two phases**: Phase 1 focuses on the platform, Phase 2 divides the team between AI agents and reporting UI development.

## Transcript

[00:00:00] Trevor Anderson: Hey, Joseph grant.
[00:00:04] Brian Silveri: Hey, Trevor.
[00:00:05] Trevor Anderson: Hey, Brian.
[00:00:06] Brian Silveri: Hey, Joseph.
[00:00:20] Brian Silveri: Just waiting on my mouth.
[00:00:23] Granth S: Brian, if you don't mind me asking,
[00:00:25] Brian Silveri: which state do you live in? New Jersey. Oh, so I mean, you're, you might
[00:00:31] Granth S: be seeing the World cup excitement.
[00:00:33] Brian Silveri: Oh, yes, absolutely. I'm right across the bridge from Philadelphia. So it's, it's, it's craziness.
[00:00:40] Brian Silveri: It's great. It's crazy, but I planned accordingly, so.
[00:00:44] Trevor Anderson: Yeah.
[00:00:46] Brian Silveri: Nice. It's wise to not get into that craziness. My brother did go to a game.
[00:00:57] Brian Silveri: Well, the first one in Philly, that was last Sunday. It was crazy. So he's like, do not recommend.
[00:01:05] Brian Silveri: Great, but do not recommend. Yeah, it's a long time to get in and a long time to get out, so.
[00:01:13] Granth S: Yeah, no, I mean, I don't doubt it. And especially I think the stadiums are like.
[00:01:18] Granth S: We have at least 60, 70,000
[00:01:20] Granth S: people.
[00:01:20] Brian Silveri: People in it.
[00:01:22] Granth S: So I
[00:01:22] Manav Paul: mean,
[00:01:23] Granth S: it's, it's almost like a day strip, honestly.
[00:01:27] Brian Silveri: Yeah, you. You've got to be committed to not getting there.
[00:01:32] Brian Silveri: You got to be committed to getting there early. Waiting, leaving late.
[00:01:46] JC Camacho: All right,
[00:01:49] Brian Silveri: let's see.
[00:01:55] Trevor Anderson: It's a. Waiting on someone I could. Or my. Is it manob?
[00:01:59] Brian Silveri: Yeah. Could you ping it for me, please? Yeah, let me do that.
[00:02:04] Trevor Anderson: For some reason, this meeting wasn't showing on my calendar.
[00:02:09] Brian Silveri: Really?
[00:02:10] Trevor Anderson: Yeah, I had a pop up from Zoom that came up, but it wasn't.
[00:02:17] Brian Silveri: The reminder didn't pop up?
[00:02:19] Trevor Anderson: No, the reminder popped up, but it's not lo. It's not listed on my calendar, which is weird.
[00:02:25] Trevor Anderson: So that's how I knew about this.
[00:02:28] Brian Silveri: Okay. Okay.
[00:02:33] Trevor Anderson: Is there a feature in Zoom where you can call someone in.
[00:02:36] Brian Silveri: Yes, yes, there is.
[00:02:38] Trevor Anderson: You just go to participants. Here, let me try to do that.
[00:02:41] Brian Silveri: Yeah, yeah, you got it. You can just click not join in that.
[00:02:45] Brian Silveri: The chevron and then you can do ask to join or you can hit invite and I can just search for him in the list.
[00:02:56] Trevor Anderson: Yeah, it looks like he's available. Maybe same thing, didn't show up for him.
[00:03:01] Brian Silveri: Yeah, I'll also copy the invite link.
[00:03:07] Brian Silveri: That's the easiest thing to do is just click on that participants and just hit copy invite link.
[00:03:17] Brian Silveri: You want me to do it or.
[00:03:18] Trevor Anderson: You got it. I'm learning. It's on participants and then.
[00:03:25] Brian Silveri: Yeah, so if you click where it says participants and it says five people right now, to the right of that there's a little top chevron and if you click on the set chevron, you'll see invite, copy, invite link.
[00:03:42] Brian Silveri: But you should I see it if you don't see it? I don't think I have that setting disabled.
[00:03:50] Brian Silveri: Do either of you guys see it?
[00:03:51] Trevor Anderson: Does anyone else see it? Because I'm not seeing it.
[00:03:53] Joseph: I see it.
[00:03:54] Brian Silveri: All right, great.
[00:03:56] Trevor Anderson: It's just me. Problem.
[00:03:58] Brian Silveri: Just you, Trevor. Just you. Maybe gotta upgrade. Upgrade your version of Zoom.
[00:04:07] Trevor Anderson: Oh, I can't share my Zoom when it's not. Yeah, it's not showing on my calendar here for some reason.
[00:04:13] Brian Silveri: That's pretty crazy. Yeah, well, you were on the original list of invitees. I didn't change that.
[00:04:20] Brian Silveri: I wonder if it's because I moved the meeting a half hour later or earlier.
[00:04:26] Trevor Anderson: Can someone invite you, help me out? I did.
[00:04:30] Brian Silveri: I. I took care of it.
[00:04:31] Trevor Anderson: All right, I appreciate that.
[00:04:33] Manav Paul: Hey.
[00:04:34] Brian Silveri: All right, welcome.
[00:04:37] Brian Silveri: All right, so let me baseline it for everybody as to where we are, how we got here, what it is, and we can go from there.
[00:04:52] Brian Silveri: So there was a request from the audience Solutions leadership group to drive on the ability for them to have the a segment called Segment Registry, but in reality it's a segment creation tool to handle building these segments at scale and, you know, manage the segment process.
[00:05:29] Brian Silveri: And man of you just check me if I'm wrong, if I'm speaking out of turn or anything incorrectly here.
[00:05:34] Manav Paul: All good so far.
[00:05:36] Brian Silveri: The we got this request and basically Dan did his own like AI generated thing to kind of make a related PRD ish thing.
[00:05:54] Brian Silveri: I've since linked it to our ideas board. So we got it.
[00:05:58] Brian Silveri: I also included I think the link in the meeting invite here.
[00:06:05] Brian Silveri: Yeah, nope, the da then the guide My mind. Sorry, hold on. And then. Got so many tabs open. Sorry guys.
[00:06:29] Brian Silveri: In Manav this is one that you basically created.
[00:06:32] Brian Silveri: So I included this here as to what the intent is, what it does, this whole write up here as to what it is, the automated pipeline for how this works, how we query things and how we deliver it, where we're going to get it from, what it contains, which is every survey response and the information that we want passed.
[00:06:59] Brian Silveri: This is basically the foundation for them to build segments in an automated way because they want to get to 2000 plus segments basically now, very fast.
[00:07:14] Brian Silveri: And they want this new person who they're hiring, who is starting July 7, to be able to use this tool to build new segments and identify gaps in our segments, refresh things and build them at speed.
[00:07:36] Brian Silveri: You guys all know this, so it's nothing hard, but Manav basically has done the heavy lifting for defining how we're going to do this.
[00:07:45] Brian Silveri: Taking the JIRA request, automating it and bringing that stuff in using our existing.
[00:07:52] Brian Silveri: Well existing soon to be production grade pipeline and stack and then doing a dashboard for it so that they can see insights into this.
[00:08:04] Manav Paul: A lot of this is straight off of the tickets that Dan had created as well.
[00:08:08] Manav Paul: A lot of the stuff was defined in there so I just put a centralized place for us to see it.
[00:08:12] Manav Paul: And that being said, a lot of it I feel like can be adjusted as we work on it.
[00:08:18] Manav Paul: We can reiterate but this is just basically the base stuff from what was given in the tickets and what I thought was necessary just for mvp.
[00:08:28] Brian Silveri: Yeah. So I Dan created six different epics I consolidated.
[00:08:37] Brian Silveri: I got rid of the eight because two of them you already done.
[00:08:40] Brian Silveri: So I took the six remaining and and consolidated them into one epic to drive on and the stories that were in each I moved over.
[00:08:52] Brian Silveri: So this makes it a little bit more easier for us to pick up.
[00:08:56] Brian Silveri: But I'll pause there and you can ask questions about what this is trying to do and where we're trying to go.
[00:09:03] Joseph: Ryan, I did leave some comments on the conference doc.
[00:09:08] Manav Paul: Yeah, yeah, we can take a look at that.
[00:09:19] Brian Silveri: Do you want to go through that? I don't have to. I don't have to show. You guys can.
[00:09:24] Manav Paul: Yeah, I can pull it up as well. One sec. If you have it open it'll be easier.
[00:09:34] Brian Silveri: I do. Yeah.
[00:09:35] Manav Paul: Okay.
[00:09:36] Brian Silveri: Yeah, no problem. There you go. Oh, even an AI summary.
[00:09:49] Joseph: Yeah, this is regarding the PII because for T1 live ramp I think we send PI currently and based on the doc I don't see it here and we only query the survey table which is the table which is produced by LEO directly and that would also have the all the users like pre FTC like suppression and like whatever and the minor miners as well.
[00:10:19] Joseph: So that is one of the comment and PI is missing here and if you scroll down yeah this would have all the users this will not have any or like mailable universe because this queries directly the source table, not the gold.
[00:10:45] Joseph: So I'm not sure like if Virginia gave a confirmation that we sent only email hashes to all the clients.
[00:10:58] Brian Silveri: If she didn't we should by default Less is more in my opinion especially to you and Manav We talked about this at the last on site is like we don't want to keep sending our proprietary data for them to use because they have carte blanche to use it.
[00:11:17] Brian Silveri: So if we pass it to you and we're decomming to you, we're giving them our graph basically by loading all of these people with their pii, just sending hash is enough to get the job done.
[00:11:29] Brian Silveri: That's how I see it for the first question.
[00:11:32] Brian Silveri: For the second question, we should definitely not be including the pre FTC users, underage users and anything.
[00:11:41] Brian Silveri: So whichever table is the.
[00:11:44] Brian Silveri: I don't say gold, but whatever you guys decided on is our final version of the table.
[00:11:50] Brian Silveri: That's what needs to be present
[00:11:55] Joseph: but
[00:11:56] JC Camacho: should be labeled our gold mailable universe.
[00:12:00] Brian Silveri: Great.
[00:12:01] JC Camacho: Yeah.
[00:12:01] JC Camacho: I think anything that that is, that uses the word mailable should be post FTC and cleanse what you just said, Brian.
[00:12:08] JC Camacho: You know, nobody under 18 any suppression files that need to be hit on it because again, when, when somebody.
[00:12:17] JC Camacho: Right. I think this goes back to what the definition of mailable is.
[00:12:19] JC Camacho: And it's all of these things that you're mentioning. Right.
[00:12:21] JC Camacho: So that way, you know, we, we know this is the mailboom Now Virginia wants us to go and, and you know, do an analysis based on something else.
[00:12:31] JC Camacho: Then that's a different story.
[00:12:32] Brian Silveri: Absolutely.
[00:12:32] JC Camacho: Mailable is this.
[00:12:34] Brian Silveri: Yeah. And is that hard? Oh, go ahead, Joseph.
[00:12:38] Joseph: No, currently we send all the PI for the. For both DU and live ramp for the audience.
[00:12:45] Brian Silveri: Yeah, I understand.
[00:12:47] Joseph: Okay.
[00:12:49] Brian Silveri: I. It's because we didn't have a reason not to send it to them because we were engaged in a contract.
[00:13:00] Brian Silveri: But our contract with TU now is a terrible contract that we're trying to get out of and trying to go to a different party.
[00:13:10] Brian Silveri: And TU has in the contract the language is they have the ability to take whatever we load and use that data because they're having to build it for an audience.
[00:13:23] Brian Silveri: So they're basically storing this data which is our first party data which has value and dollars for free.
[00:13:33] Brian Silveri: We're paying them to take our data. Just terrible.
[00:13:38] Joseph: And does the same goes for the live ramp.
[00:13:42] Brian Silveri: Yes. Especially now that Liveramps got bought.
[00:13:45] Brian Silveri: But trying to think of the right phrasing here like it's Live ramp technically is doing the same thing that TU is doing.
[00:13:56] Brian Silveri: So we could make a strategic decision where we don't load that and we only load the required attributes to get to a seated audience.
[00:14:07] Brian Silveri: Or you could load and continue to load that stuff and it's just kind of a business as usual until we get off of TU and live frame.
[00:14:15] Brian Silveri: That's really up to Virginia. I have my point of view. She owns this product. It's her decision.
[00:14:22] Brian Silveri: I just want to call that out as a thing that I see especially when we're taking this from.
[00:14:29] Brian Silveri: To a brand new person, which is what this thing is going to allow us to do, which is basically put it in front of a, you know, a brand new, new hire and say go make segments, which is great.
[00:14:42] Brian Silveri: But with it comes knowledge that we need to ensure we're protecting the user from not having access to more data than they should.
[00:14:51] Brian Silveri: And so that gold mailable table is critical path in here.
[00:14:56] Brian Silveri: It should also be limiting PII for people because Trevor, that's our Q3 goal.
[00:15:04] Brian Silveri: Our PII masking should be in full effect in the audience solutions data set.
[00:15:12] Brian Silveri: So we should add that to the scope here if it isn't already in this, to ensure that PII is masked so that even if a user has access aside from a data engineer, you know, role, they won't see pii.
[00:15:31] Brian Silveri: And that account manager that we just hired fits that exactly.
[00:15:35] Brian Silveri: They're not a data engineer, they're not making data products. They don't need access to the pii.
[00:15:40] Brian Silveri: We can mask it and there's no need to send it unless absolutely necessary. That's good.
[00:15:50] Brian Silveri: I'm open to feedback, but the rest of you tell me if I'm not aligned. That makes sense.
[00:15:57] Trevor Anderson: Yeah.
[00:15:58] Trevor Anderson: And I guess just going on with the governance idea, we'll need a specific Persona group for whoever's going to be working on building these audiences.
[00:16:11] Trevor Anderson: And that's actually something I'm looking into right now, is having an audience solution analyst group in databricks that would only have access to the schemas that they.
[00:16:26] Trevor Anderson: Schemas and or tables that are required to build these audiences. Right?
[00:16:35] Brian Silveri: Yeah.
[00:16:37] Trevor Anderson: And so I'm going to need help from, from, from the team here on what are the least required tables that are necessary to build these audiences?
[00:16:47] Trevor Anderson: Because I don't, I don't want to just provision access to the entire catalog.
[00:16:52] Trevor Anderson: Like they wouldn't need bronze and silver. Oh yeah, absolutely. Right.
[00:16:57] Brian Silveri: So and that would apply as Virginia's scope of fluent data expands, that becomes even more critical path, especially for her analytics.
[00:17:11] Brian Silveri: As JC mentioned.
[00:17:12] Brian Silveri: Like we do not want to over privilege even if we get like hey, you can go through playful rewards data as an example.
[00:17:21] Brian Silveri: Okay, well you can go through playful rewards data.
[00:17:24] Brian Silveri: We just need to make sure that we are not over provisioning you to get access to non golds.
[00:17:32] Brian Silveri: That should be a hard no.
[00:17:34] Brian Silveri: And PII should also be enforced because you don't need to see these people's raw emails and addresses in order to make segment decisions.
[00:17:50] Brian Silveri: Please.
[00:17:50] Brian Silveri: Yeah, and then you're going to have to go through compliance and compliance will tell you whether or not they agree with you or not.
[00:17:56] Brian Silveri: And that's, that's how that PII role is set up and that's how we're waiting just for the IT team to finish that job.
[00:18:04] Brian Silveri: Then we'll be done.
[00:18:06] Manav Paul: Is this for to come up with segments Brian, or to create the segments?
[00:18:11] Brian Silveri: Well, it in, in Virginia's mind the answer is it's both.
[00:18:17] Brian Silveri: They want to be able to do both and that's fair because they should be.
[00:18:25] Manav Paul: For the. Yeah, for the create segments part of it.
[00:18:27] Manav Paul: I am hoping that we will be able to implement some kind of.
[00:18:31] Manav Paul: I mean it'll be simple but on the JIRA side, just like how they make tickets now for us we can continue with that aspect just with a more uniform format for each ticket where they can, this account manager can go in, make a ticket and then our system will then pick it up and build it out from there.
[00:18:47] Manav Paul: But if it's, you know, if they need to see the data to come up with segments that can't be solved in that sense.
[00:18:52] Manav Paul: But I was just thinking in terms of just the actual creation, they won't even have to go into databricks, just have to put in the, the necessary information into the ticket and then we can have it pick it up.
[00:19:02] Brian Silveri: Yeah, I, if you took it from a brand new person coming to fluent scenario, which is exactly what's happening now.
[00:19:09] Brian Silveri: Yeah, we're going to get a brand new body.
[00:19:11] Brian Silveri: They have no idea what data is available, they have no idea what segments are out there, they have no idea what's inside those segments, they have no idea how those segments are performing.
[00:19:22] Brian Silveri: And they wouldn't know.
[00:19:24] Brian Silveri: Besides getting the request from client ABC to say, oh, make me a segment of 30 year old GLP1 users, you know, that is great but they wouldn't know what is even available to choose even to have that conversation.
[00:19:42] Brian Silveri: And that's the gap that Virginia's saying, oh, just give me access to the data, I'll find it.
[00:19:49] Brian Silveri: Yes, that is a thing you could do.
[00:19:53] Brian Silveri: Or we could make it a whole lot easier for them to see the questions that are available out there and what those questions are providing.
[00:20:04] Brian Silveri: Right.
[00:20:04] Manav Paul: And that's where the talks have also like a potential front end come in. Right.
[00:20:08] Manav Paul: Where we can have like almost a portal for them to go in and I guess query all these, all this data but in more of a simple business format.
[00:20:18] Brian Silveri: That is correct. And that is the second OKR from Audience Solutions this quarter coming up.
[00:20:25] Brian Silveri: That's her request is to have a reporting UI basically that let's get as much insights into the data as possible, which we have.
[00:20:35] Brian Silveri: It's not like we don't have the data. We just got to surface to them in a, in a better way.
[00:20:39] Brian Silveri: And that's her ask or one of the asks.
[00:20:42] Manav Paul: Yeah, I mean it's like you said, if they could just go in and say, you know, we want 30 year old GLP1 users and then they can go in there, search the data and then give them the exact parameter they need and then they can go put that into a JIRA ticket and our system can pick it up and build it out.
[00:20:56] Manav Paul: That'll be very efficient. You know, everyone's permissions will be simplified.
[00:21:02] Brian Silveri: Yeah.
[00:21:02] Brian Silveri: The hope there is that they can, by the end of this quarter coming up, they will have a platform that lets them see what's out there, let them create new things in an automated way, create new segments in an automated way without, without just putting in a request after they found data or hey, refresh segment number 7259 because we, we added a new question and we get more people inside that segment because the segment is stale.
[00:21:40] Brian Silveri: As an example, that, that's, that's her goal by the end of the quarter among many other goals.
[00:21:48] Brian Silveri: But that's, that's the biggest one to start.
[00:21:52] Brian Silveri: This, this is the foundation for all of those things or at least this is the second part of the foundation.
[00:22:03] Brian Silveri: The first one is deploying the segment platform or our platform enterprise wide currently.
[00:22:10] JC Camacho: What can, well, is anything built yet?
[00:22:16] Manav Paul: I have built out like very basic kind of a first draft of the pipeline but nothing is tested yet because of DevOps permissions and things like that.
[00:22:26] Manav Paul: But also did some research on the agent side of things and kind of built out something on that end that also, like I said with the DevOps stuff is waiting on that before we can test it.
[00:22:38] Manav Paul: But I think they, I don't even think the agent stuff is in there for this quarter, right Brian?
[00:22:44] Brian Silveri: No, no. If you could. Hooray. Yeah, I think that that's the right pathway. Hooray.
[00:22:51] Brian Silveri: But I didn't commit to an agent to do this stuff.
[00:22:55] Manav Paul: Yeah, I just, you know, I think it's best to get whatever this system is, where this account manager going and create things first and then explore because I haven't, I haven't gotten tested yet and you know, it could be extremely gimmicky and you know, we're going to need a lot of fine tuning.
[00:23:10] Manav Paul: But I think in the long run it'll be great because then it can go in, suggest segments, suggest questions, create its own, you know, not even create a ticket, but it can just go straight through and build them out themselves.
[00:23:21] Manav Paul: And then you know, it's all. Everything's automated, start to finish.
[00:23:25] Brian Silveri: Just needed a rule.
[00:23:26] JC Camacho: So correct me if I'm wrong. Right.
[00:23:29] JC Camacho: I thought that the goal here was for an automated process that would create 2000.
[00:23:38] JC Camacho: I would now use those implementation work segments at any given time and I'm going to make this up now weekly.
[00:23:44] JC Camacho: Right. And then within the process go in and say this makes sense. This doesn't make sense.
[00:23:51] JC Camacho: These are the true 2000.
[00:23:54] JC Camacho: Again I'm using 2000 here segments that are available this week based on the fact that within these segments there is a certain amount of coverage.
[00:24:08] JC Camacho: Right.
[00:24:09] JC Camacho: Let's say I want to make this up again, 10,000 or 30,000 records or more, some type of rules that are built in that deems it a good segment to be there.
[00:24:19] JC Camacho: Right.
[00:24:20] JC Camacho: And then this would be maybe at a monthly basis and it would just continue to reiterate and, and keep adding if there are any.
[00:24:28] JC Camacho: And always checking to make sure that you're.
[00:24:30] JC Camacho: It's not suggesting something that it was that was already there. Right.
[00:24:35] JC Camacho: And then so this person starts on the seventh would go in there.
[00:24:39] Brian Silveri: Yeah.
[00:24:39] JC Camacho: Analyze. Make sense. Okay, this makes sense. This doesn't make sense. From that. Am I. Is that not.
[00:24:48] Brian Silveri: That last part is the next phase.
[00:24:53] Brian Silveri: I need to figure out what the hell does a UI look like for a person to be able to do that. So
[00:25:00] JC Camacho: if this UI is non existent because I feel like we're expecting the candidate, the new person to come in and be able to do this.
[00:25:11] JC Camacho: But no, but we.
[00:25:13] JC Camacho: That person doesn't have programming skills so she's not going to be able to go and query this stuff.
[00:25:18] JC Camacho: Right.
[00:25:19] Brian Silveri: Well, at the very least we A can do two things for those users B A is guarantee them access to the data that they should have access to to allow them to then use tools like Claude and say hey, go through the mailable gold table and tell me all of the segments that are out there and which one of these people and what's the TAM inside these segments as an example.
[00:25:50] Brian Silveri: And then I hope be able to build out a JIRA request that says here is my request for a new segment.
[00:25:58] Brian Silveri: Build this segment based off of these parameters or questions or question id, whatever the hell the JIRA intake form is going to be and then kicks it off and then that JIRA touches nobody except for the gates that you guys have put into place of like please confirm that your audience size is correct and you've Returned the right users, blah, blah, blah, and then approve and then gate to grant.
[00:26:22] Brian Silveri: I can't remember the helicopter two was like, oh, yes, this is tntu.
[00:26:25] Brian Silveri: Are you good to use this segment? Yes or no? And then they, they basically hit yes or not.
[00:26:34] Joseph: You're mixing up two things. So this is more of a declared audience pipeline, the segment registries.
[00:26:41] Joseph: The other gate which you mentioned is for the model audience pipeline.
[00:26:46] Manav Paul: Yeah, the only gates that would be there is I feel like for the sizing and stuff.
[00:26:51] Manav Paul: And then depending on that, we would pass the request on to the automation pipeline for modeled or automation pipeline that is this one for declared, depending on like the sizing of the seeds and whatnot.
[00:27:02] Manav Paul: But yeah, that would basically determine any request can be made and then it would figure out whether it should be declared or modeled and then go from there.
[00:27:12] Brian Silveri: Great.
[00:27:14] Joseph: If I'm not wrong. Sorry, sorry, Brian.
[00:27:17] Joseph: So this pipeline would replace the current DA pipeline, right?
[00:27:21] Trevor Anderson: Yeah.
[00:27:22] Brian Silveri: Okay.
[00:27:27] Manav Paul: This just goes off that YAML file every Sunday night.
[00:27:31] Manav Paul: It will refresh all the segments and then my idea is for any segment that is newly requested, it will pick it up, it'll check every morning and just build those ones out so it won't have to refresh, you know, all the thousands of segments every day.
[00:27:44] Manav Paul: Those will only be refreshed at the end of the week.
[00:27:46] Manav Paul: But if there's like a new one that's being requested in the middle of the week, it'll build that one out right away.
[00:27:56] Brian Silveri: The, the issue that Virginia is going to come back on, and it's a fair question, is how does their team create the new segment request that isn't initiated by a customer, or let's assume it is initiated by a customer.
[00:28:13] Brian Silveri: How do they go about doing that? By finding out what data is available to them.
[00:28:18] Brian Silveri: And JC I think that's on you and I to figure out how to give them access to that data so that they can query that information.
[00:28:26] Brian Silveri: In this phase, let's say, you know, in July and August and by September, our hope is to have some sort of lightweight UI that lets them do this work.
[00:28:40] Manav Paul: So I just have a question.
[00:28:41] Manav Paul: How does Virginia kind of make these ticket request now for declared and modeled?
[00:28:49] JC Camacho: I think she's in there querying the data and looking, looking at like counts of stuff.
[00:28:53] JC Camacho: And that's why she, you know, she has access to all those, to all those tables.
[00:28:58] Manav Paul: Okay.
[00:29:00] Brian Silveri: Yeah.
[00:29:00] JC Camacho: And again, I feel like, I feel like I'm missing something here, but I, I thought that, that that was the purpose here where we, they would cut out that part.
[00:29:08] JC Camacho: Because this tool or this new process would automatically start saying, hey, you guys should try these segments.
[00:29:15] JC Camacho: You guys should try these segments.
[00:29:16] Manav Paul: That is the agent part of it.
[00:29:18] Manav Paul: It doesn't look like that's going to be a big focus right now until we get everything else sorted.
[00:29:24] Manav Paul: But you're not wrong about that.
[00:29:27] Manav Paul: That is the whole the exploration agent, where it'll suggest segments and then build them out.
[00:29:31] Manav Paul: But yeah, right now this process we're talking about is just the actual building.
[00:29:37] Manav Paul: However they are getting the information and data now to actually go and make that request.
[00:29:43] Manav Paul: That doesn't look like it's going to be able to be changed.
[00:29:45] Manav Paul: It's just after the request is made, this pipeline will automatically pick it up, automatically build it out, and then every week on Sunday, automatically refresh all the segments for all the clients and marketplaces.
[00:29:58] JC Camacho: This works once I tell it, hey, pick up people who are 30 to 35 who have one car and have kids, for instance.
[00:30:08] JC Camacho: I have to make sure I tell it that in order for.
[00:30:10] Brian Silveri: That's right, you're prescriptive in your request for Phase one. That's how I see it.
[00:30:17] Brian Silveri: JC For Phase one, you've got to tell it what to do, right?
[00:30:20] JC Camacho: So, because again, I mean, I had worked a little bit with, with, with Dan hall before.
[00:30:25] JC Camacho: You know, he had given this to pass this along, but his original request to me was, hey, can you give me every question answer available?
[00:30:35] JC Camacho: Because I want to feed it into Claude and I want to see what it does.
[00:30:41] JC Camacho: If I feed it all the data that we have, can you shoot me out segments?
[00:30:46] Brian Silveri: Yes,
[00:30:48] JC Camacho: in an automated.
[00:30:51] JC Camacho: My idea was always just like, okay, we're building something that's going to go and grab every question, everything that we have at whatever given cadence and say, hey, you should try this.
[00:31:02] JC Camacho: And so that's why I was thinking like, okay, so how does it know not to request something that's already been created and things like that, Right?
[00:31:09] Brian Silveri: So I, I have that. Just everyone's clear.
[00:31:12] Brian Silveri: I got that intake from, from Dan originally of like, hey, you want this thing to basically do that, make these agents that sit on top of this thing and automate it.
[00:31:29] Brian Silveri: So I do have this epic. If we get to it, amazing.
[00:31:33] Brian Silveri: And that would totally be where she wants to get to for auto creation or auto suggestion of segments.
[00:31:41] JC Camacho: I think she thinks that this is what we're delivering now.
[00:31:47] Brian Silveri: Well, you can't, you can't do it without this.
[00:31:52] JC Camacho: Because if you think about it, right, like in my opinion, the person who comes in on, on the 7th.
[00:31:58] JC Camacho: And we know you and I know who it is.
[00:32:03] JC Camacho: That person won't be able to do what Virginia does today, I think.
[00:32:07] Brian Silveri: Well, you're. You're correct on that one.
[00:32:09] JC Camacho: So that's why. So we're expecting her to do this stuff. Right.
[00:32:13] JC Camacho: Because that's why I think like, again, again, it's a quick talk with Virginia just to understand what her.
[00:32:17] Brian Silveri: What her.
[00:32:19] JC Camacho: But if she's thinking, oh, she's going to be able just to go somewhere and see. Oh, okay.
[00:32:25] JC Camacho: These segments were created last week and this is the counts and things like that.
[00:32:29] JC Camacho: Maybe I can join these two together and create between segment A and C. Create new segment X. Right.
[00:32:38] JC Camacho: That's a different ask than.
[00:32:40] JC Camacho: Oh no, you're still going to have to go into that data, run some counts, create some tables.
[00:32:47] Brian Silveri: That's true. You're absolutely correct. Two distinct things.
[00:32:51] JC Camacho: Yeah.
[00:32:51] Brian Silveri: As a. From a deliverable perspective, I mind you
[00:32:54] JC Camacho: mind you, I'm not trying to, I'm not trying to say that everything that we're working on here is awesome.
[00:32:58] JC Camacho: And I think that definitely we're working towards what I think the ultimate goal is.
[00:33:02] JC Camacho: I just, I think that there's definitely some might be a miscommunication with, with.
[00:33:06] JC Camacho: With Virginia as to what she's thinking when, when she's asking for the.
[00:33:12] JC Camacho: What the hell is called for an update on. On the 2000 segments or whatever got
[00:33:17] Brian Silveri: her expectation is the AI agent tool for this. So lumping it in.
[00:33:23] Brian Silveri: So basically we've got to do the infrastructure for part of this basically which was those epics and then.
[00:33:31] Brian Silveri: And also this. I'll follow up JC but I think you're 100 right.
[00:33:38] Brian Silveri: With her and Dan and Adrian say hey, this. Want to make sure that we're.
[00:33:43] Brian Silveri: Are we aligned in what you get for this phase versus this phase? Because you can't do this. AI agents.
[00:33:52] Brian Silveri: Well, Manav you tell me can you run this EPIC in parallel of the other epic.
[00:33:59] Brian Silveri: But it's a hard dependency. Right.
[00:34:01] Manav Paul: You can work on it, but it's not gonna.
[00:34:04] JC Camacho: I think they were all still dependent on the finalization of what our mailable universe was.
[00:34:10] Joseph: Right.
[00:34:11] JC Camacho: Because there was a lot of changes.
[00:34:13] Brian Silveri: Oh sure.
[00:34:14] JC Camacho: So I think that that's what, that's what my message was lately to her was just, okay, we have to finish.
[00:34:19] JC Camacho: We can't pick from something that's not done yet. Right.
[00:34:24] JC Camacho: So obviously, you know, like the selection part is one thing, but the definition and the completion of our mailable universe cold table had to happen.
[00:34:36] JC Camacho: Right. And actually I think you have it here. Dependencies Gold table confirmed. I think.
[00:34:43] Brian Silveri: Well, we're, we're now set right. For our definition of gold maleable universe.
[00:34:50] JC Camacho: No, we're, we're. We're set. But the thing is, I think. Is it, Is it. Is it done?
[00:34:55] Joseph: Yes, it's already.
[00:34:57] Brian Silveri: Yeah.
[00:35:00] JC Camacho: So then. Yeah, I guess the next step is okay.
[00:35:03] JC Camacho: Now that we know where we're picking from just to start thinking about this.
[00:35:09] Brian Silveri: Yep. So I will follow up with them.
[00:35:13] Brian Silveri: So, Manav I guess the next question I've got for you and the team is this.
[00:35:18] Brian Silveri: As soon as you guys are done and deploy out to prod our infrastructure changes, what stories can we pick up in the.
[00:35:29] Brian Silveri: And how do you want to divide and conquer between the segment registry platform, that's I'm calling it, which is basically the under the covers and the AI agent work?
[00:35:41] Brian Silveri: How we go about doing it? Clearly we got to do both.
[00:35:44] Manav Paul: Yeah, I will take a look and break it up because I kind of had, when I was working on it, I kind of had Claude take a look at everything and just slowly pick up everything it can.
[00:35:53] Manav Paul: So I have to see what it's really actually worked on and what it hasn't. And then.
[00:35:58] Manav Paul: Yeah, the main thing is waiting on this DevOps stuff so then we can actually test. But a lot of.
[00:36:02] Manav Paul: A lot of the code is already written.
[00:36:04] Brian Silveri: I will say of the platform side.
[00:36:07] Manav Paul: Both sides.
[00:36:10] Brian Silveri: Oh great. Well then let's.
[00:36:12] Brian Silveri: Let's just start going through as soon as we're done and we deployed our thing to prod, let's immediately close out 1593 to get the platform tested, using our gold mailable universe as our breakdown to make sure that this works.
[00:36:30] Brian Silveri: So this is what the team focus on and then just your job.
[00:36:34] Brian Silveri: Manav is basically going through the epic, making sure all the stories are still relevant and in the right state.
[00:36:39] Brian Silveri: And if we need to do them, great. If not, just close them out to not close, not done.
[00:36:45] Trevor Anderson: Okay.
[00:36:45] Brian Silveri: And put a comment why.
[00:36:47] Brian Silveri: And then we can figure out how do we run in parallel to make the AI agents do this stuff?
[00:36:56] Manav Paul: Yeah, I mean I didn't realize until now how big of an issue.
[00:37:01] Manav Paul: I guess this before creating the request is that side of things.
[00:37:05] Manav Paul: I only realized that there was an issue for the actual creation of the segments.
[00:37:10] Manav Paul: So I guess these AI agents, well, they will be able to solve that issue.
[00:37:16] Manav Paul: The main thing is I thought that the entire pipeline with the AI agents suggesting segments and then once they're approved, also having it auto build out was the plan.
[00:37:25] Manav Paul: But now that I'm seeing how big of a stressor I guess it is just to create these requests. Yeah.
[00:37:35] Manav Paul: I don't know. Which one do you think? Is there like a priority on which one?
[00:37:39] Manav Paul: I understand working on both, but is there like one you would want to prioritize or.
[00:37:44] Manav Paul: I just feel like the pipeline is more like.
[00:37:49] Manav Paul: I don't want to say doable, but I think we can get that sorted and prod ready sooner.
[00:37:56] Brian Silveri: This, to me, this pipeline, I. E. The platform is a must do in order to get to the AI agent.
[00:38:05] Brian Silveri: You've got to have this done and ready because at the very least with the segment registry platform, VA could go in, create a, create a request in Jira and that segment would be created.
[00:38:16] Manav Paul: Yeah.
[00:38:16] Manav Paul: I mean at the same time we can have the agent create a request and then we'd have to go manually, go and build it.
[00:38:22] Manav Paul: So it's really just, I guess which one is more of a struggle for the team.
[00:38:28] Brian Silveri: Oh, well, I think this also takes the work out of all of you guys.
[00:38:34] Manav Paul: Yeah, I think this is a better solution or not. I understand working on both.
[00:38:38] Manav Paul: I just think this is more attainable and more of a bigger fix in general for the whole team.
[00:38:46] Manav Paul: The platform, I mean,
[00:38:52] Brian Silveri: I, I think we have to do both, but I think you do this one first.
[00:38:56] Joseph: Sure.
[00:38:58] Brian Silveri: That's how I see it.
[00:39:01] Manav Paul: But we are, we are going to go with the fact that These are both Q3 goals though, because it's not like before.
[00:39:08] Manav Paul: Okay. That's my main question. Yeah. Okay.
[00:39:10] Brian Silveri: Yes, yes.
[00:39:12] Manav Paul: Because we can work on both. I'm just trying to figure out which one is the one we're trying to.
[00:39:15] Manav Paul: If we have to prioritize. Okay. That makes sense.
[00:39:17] Brian Silveri: Everyone, as soon as we're done, our platform work pops on doing this. Okay.
[00:39:22] Brian Silveri: And we close this out correctly.
[00:39:25] Manav Paul: Okay.
[00:39:26] Brian Silveri: And then we tackle what I would consider phase two.
[00:39:31] Brian Silveri: And then I split the rest of you guys out to doing the audience solutions ui.
[00:39:36] Brian Silveri: So half of you will do the agents, half of you will do the ui basically reporting dashboard is what that UI is going to be.
[00:39:44] Brian Silveri: Give them the reporting insights that they need as to how all these segments are performing across the universe or is platforms.
[00:39:57] Brian Silveri: Oh, this is how it's doing on load of May. This one how the segment is doing on tu.
[00:40:00] Brian Silveri: This one's how it's doing a live ramp. Pick a thing. She can ask it questions.
[00:40:06] Brian Silveri: The new person can ask it questions either in claude, using our new CLAUDE to databricks GENIE connector or directly in databricks.
[00:40:15] Brian Silveri: I prefer the CLAUDE one and that makes life a whole lot easier for us rather than us having to build or bi team have to build dashboard after dashboard for what they want.
[00:40:28] Brian Silveri: Sure. That is the second phase of 20 of Q3.
[00:40:32] Brian Silveri: First phase, do the platform, second phase, agent and UI as best we can, split them.
[00:40:41] Manav Paul: Got it.
[00:40:43] Joseph: Performance, meaning, Ryan, is it the revenue you're talking about of each segments?
[00:40:50] Brian Silveri: Yeah, it's all going to boil down to revenue, Joseph.
[00:40:53] Brian Silveri: Just as an FYI, Terry's going to want to know how much did this segment make fluent, how many people have used it, how much has it made us?
[00:41:08] Brian Silveri: And then.
[00:41:09] Joseph: Okay, go ahead. No, for that I think we'll need those data inside databricks first, right?
[00:41:15] Brian Silveri: Oh, absolutely. Yeah.
[00:41:17] Joseph: So we'll have to plan accordingly to bring that in first and then we can have the ui.
[00:41:21] Brian Silveri: You got it. You got to get the data. But that, that's what they're asking for. That's. That's re.
[00:41:28] Brian Silveri: And that's a fair point. We should. We have to have reporting for them and right now we don't.
[00:41:33] Brian Silveri: So I'll rename this from UI to reporting because that's really what it is. It's.
[00:41:37] Brian Silveri: This phase is, is reporting. I, I haven't even the dashboard, but that's what they're looking for.
[00:41:47] Joseph: I think we'll need to set up a call with Virginia to get.
[00:41:50] Brian Silveri: Oh, for sure. I have to do discovery with her as well. I'll include all of you guys.
[00:41:56] Brian Silveri: So we just have a joint session.
[00:41:58] Brian Silveri: But right now I'm not worried about this because it's not top of mind.
[00:42:03] Brian Silveri: This one is because we should already be knee deep in it. We're just behind. Yeah.
[00:42:11] Manav Paul: And Trevor, after Griggs is done with the FTSOs that are currently priority, if we can work on pushing those request sites in over the other day for this project, that'll be big in making sure we can actually get everything done on time.
[00:42:27] Manav Paul: And then I'll also follow up with Dan about for the agents we needed anthropic console account.
[00:42:33] Manav Paul: So I know he's working on that, but I'll just check and see how that's going so we can make sure everything is moving smoothly.
[00:42:40] Brian Silveri: Great.
[00:42:41] Trevor Anderson: What is that console count?
[00:42:44] Manav Paul: That's just for the agents.
[00:42:45] Manav Paul: But the request I sent the other day, Those are for DevOps for this, the pipeline, the platform, just so we can actually test everything.
[00:42:53] Manav Paul: Because like I said, with the permissions and everything, it wouldn't be able to test things or write to the buckets until all those things are sorted on their end.
[00:43:01] Brian Silveri: Oh, from the AI agent permissions, the
[00:43:05] Manav Paul: ones I'm talking about right now for DevOps, that's for the platform to test the actual pipeline.
[00:43:11] Brian Silveri: Okay.
[00:43:12] Manav Paul: The agents, those I haven't tested yet because I need an anthropic console account to even run them with the API key.
[00:43:19] Manav Paul: So I know Dan is working with Tim on that, but I'm not sure how that's going.
[00:43:24] Manav Paul: I'm going to follow up today and see.
[00:43:27] Brian Silveri: Got it. Thank you for clarifying. Yeah. J.C. Trevor, any questions or does anybody have any questions?
[00:43:38] JC Camacho: Nothing on me right now.
[00:43:42] Trevor Anderson: Okay. Yeah, I'll have some questions as we roll this out, but nothing initially here.
[00:43:50] Brian Silveri: Yeah, it's a. It's a big deal. So let's make sure our pipes are good first.
[00:43:58] Brian Silveri: That's why I didn't commit to the AI agent yet. Yeah. Because I want to make sure our pipelines are.
[00:44:06] Brian Silveri: An infrastructure is ready. We do not want to make the same mistake that was made before. No.
[00:44:11] Brian Silveri: No band aids and bubble gum here. Right. All right, great. I'll give you guys 10 minutes back.
[00:44:18] Brian Silveri: Thank you all.
[00:44:19] JC Camacho: Thank you.
[00:44:20] Brian Silveri: Thanks, you guys.
