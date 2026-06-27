---
title: DE Daily Standup
date: 2026-06-26
time: 06:35-07:00
participants: [Dan Duling, Trevor Anderson, Akhil Gonna, Kapil Sreedharan, Vani Kaithi, Venkatesh Mannam, Manav Paul, Joseph Melkin]
fellow_url: https://fluentco.fellow.app/meetings/040000008200E00074C5B7101A82E00807EA061AA07C9A5FA178DC01000000000000000010000000AB9154BF9C4AA34B9FC26D395D9AA196/
captured_at: 2026-06-27T12:02:38Z
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
[ ] Venkatesh Mannam to validate that all campaigns and advertisers from Pulse (including Minion and Playful Rewards) are present in the gold advertiser and gold campaign tables.
[ ] Venkatesh Mannam to validate advertiser list against the Pulse creative reporting dashboard (showing 160 advertisers) to ensure gold advertiser table has complete coverage.
[ ] Kapil Sreedharan to perform validation testing on the fluent performance metrics view to ensure data quality after the fact table join solution.
[ ] Akhil Gonna to create documentation for Tremendous (orders and campaigns) including JC and Sharita as points of contact.
[ ] Akhil Gonna to get the count of how many Tremendous campaigns were successfully matched to Lead Manager campaigns versus unmatched using the fuzzy logic approach.
[ ] Venkatesh Mannam to complete testing of UCM migration changes for Gold Ad group conversions (including partner type dimension) and send to Chalky for review.
[ ] Kapil Sreedharan to follow up with IT team by noon if no response received regarding the pending request.
[ ] Manav Paul to send the job link for the declared audience delivery pipeline test to Trevor for review.

---

The team held their daily standup covering multiple data engineering workstreams. **Key progress included** Joseph's work on the modeled audience pipeline (facing polling timeout issues that may require a different strategy if processing takes 4-6 hours), Kapil's resolution of the fluent performance fact table blocker through an outer join approach, and Akhil's collaboration with JC to reload missing Tremendous campaigns using fuzzy logic matching. **Validation work is ongoing** for campaign and advertiser tables across Minion and Playful Rewards systems, with Venkatesh tasked to verify all campaigns from the Pulse dashboard are present in gold tables. Vani completed validation of two tickets for the next release, and Manav began testing the declared audience delivery pipeline. The team will be operating with reduced capacity next week due to Canada Day (July 1) and US Independence Day (July 2-3) holidays.

### Modeled audience pipeline issues

- Joseph created PR **1627** to repoint the extract task to the new staging path discussed previously, which will unblock Manav's delivery task work once merged.
- The modeled audience pipeline failed due to **polling timeout after 4 hours**, even though Grant's smoke test only took 2 hours. The seed uploaded successfully to TAP UI but remains stuck in 'processing' status.
- Trevor suggested implementing a **different polling strategy** (separate job checking every 30 minutes) if processing consistently takes 4-6 hours, rather than maintaining a constant connection.
- The team will wait for the current run to complete before deciding next steps, and may contact Fluent if the processing issue persists across different seeds (similar issues occurred in manual approach).

### Fluent performance metrics development

- Kapil resolved Shubham's blocker by implementing an **outer join between Tune fact table and Minion fact table** to get complete metrics (Tune has clicks/conversions, Minion has impressions).
- The fluent performance metrics view depends on **gold advertiser and gold campaign tables** requiring validation that all campaigns/advertisers are present, including those from Minion, Playful Rewards, and Syndication (Walmart).
- The Pulse creative reporting dashboard shows **160 advertisers** and multiple campaigns that must be validated against gold tables to ensure complete coverage for the metrics view.
- Venkatesh confirmed all Minion advertisers from the finance report are present in gold tables, validated by querying the Pulse configuration silver advertiser table directly (since active status flag isn't in gold advertiser schema).

### Tremendous campaign integration

- JC reloaded the **missing Tremendous campaigns** by working with Sharita to manually ingest them with campaign names and a new status column to identify active/inactive campaigns (missing ones were retired/no longer used).
- The BI team uses **fuzzy logic matching** based on keywords from offer description and recipient name to join Tremendous campaigns to Lead Manager, which Akhil is replicating (no exact full string matches available).
- The team learned that **Tremendous is only used for Lead Manager campaigns** for fulfillment purposes, which clarifies the scope of integration work.
- Sharita was identified as the point of contact for Tremendous API management, and the team agreed to create documentation including her and JC as contacts for future issues.

### Position views and metrics

- Vani validated and attached **two tickets to next release**: apply data ingestion and position views in UIT, both confirmed working correctly.
- Current position views only provide **P1, P2, P3, P4 view counts** as metrics but don't allow slicing other metrics (like CTR) by position dimension, limiting analysis capabilities.
- Kevin's BI dashboard can show how many views occurred at each position, but **cannot show CTR or other metrics broken down by position** (e.g., P1 CTR vs P2 CTR separately).
- The team anticipates a future request to add **position as a dimension** for slicing all metrics, though no formal ask has been made yet. Trevor decided to wait until explicitly requested before redesigning the data model.

### UCM migration work

- Venkatesh began migrating **Gold Ad group conversions to use UCM**, which requires adding partner type dimension to the existing UCM structure.
- Current Gold Ad group conversion uses **only optimized conversions** for conversion counts, while UCM sums all conversions, potentially inflating numbers. However, DS team confirmed they only use EPC metric, so this difference won't impact them.
- Switching to UCM will allow **changing job frequency from 15 minutes to 30-60 minutes** and reduce runtime to 2-3 minutes (vs current longer runtimes from analytics feed), eliminating current warning issues.
- The team is waiting on response from IT team regarding a request, with Kapil to follow up by noon if no answer received, and Trevor to escalate next week if needed.

### Declared audience pipeline

- Manav is working with Joseph to test and validate the **declared audience delivery pipeline**, including finalizing JIRA ticket format and answering open questions from segment registry documentation.
- For dev environment testing, the team clarified they should **test the full copy workflow** from staging to vendor destination folders (even though same bucket) at small scale rather than skipping steps.
- Trevor confirmed the dev folder is only for testing small subsets, not maintaining one-to-one copies of production data, and storage costs are negligible for dev testing at this scale.

## Transcript

[00:00:03] Venkatesh Mannam: Will be joining in two minutes. Just an fa.
[00:00:06] Trevor Anderson: Okay.
[00:00:21] Kapil Sreedharan: Morning, sir.
[00:00:24] Akhil Gonna: Morning.
[00:00:25] Trevor Anderson: Capital. Happy Friday, everyone.
[00:00:29] Kapil Sreedharan: Happy Friday.
[00:00:32] Trevor Anderson: Oh, looks like so Grant is out Kills. Running late.
[00:00:51] Trevor Anderson: For him.
[00:00:57] Venkatesh Mannam: You're against that?
[00:01:04] Joseph: I can go travel.
[00:01:08] Trevor Anderson: All right, Joseph, take it away.
[00:01:13] Joseph: Am I audible? Because I don't hear. I think Trevor voice is breaking for me. Is it the same?
[00:01:20] Venkatesh Mannam: Yeah, you are good.
[00:01:21] Akhil Gonna: Okay. Okay.
[00:01:24] Trevor Anderson: Yep.
[00:01:25] Joseph: And morning team.
[00:01:27] Joseph: I just created a 1627 to repoint the extract task to the new staging path that we discussed yesterday.
[00:01:34] Akhil Gonna: Trevor.
[00:01:35] Joseph: So just place the PR and we'll merge it and so that he can rebase and Mano can work on the delivery task.
[00:01:45] Joseph: Yeah, just waiting on that. Validations for the model audience pipeline.
[00:01:50] Joseph: I have triggered the other job as I mentioned this morning. Yep, just waiting on that.
[00:01:58] Joseph: And also Monisha has raised another PR for a set of declared audience. About 44 audience.
[00:02:03] Joseph: And we'll have to review that. And is there a timeline from breaks
[00:02:09] Akhil Gonna: for the product space? And there's piece.
[00:02:12] Trevor Anderson: It looks like it's being implemented currently. I'll check this morning.
[00:02:17] Trevor Anderson: But I just checked the status of the ticket and says it's an implementation phase right now.
[00:02:23] Trevor Anderson: So hopefully it gets done today.
[00:02:25] Akhil Gonna: Okay, perfect. Yep.
[00:02:28] Joseph: And apart from that, no blockers.
[00:02:32] Trevor Anderson: What's the plan for that modeled audience pipeline?
[00:02:35] Trevor Anderson: I see that it failed yesterday because the polling. Yes. Timed out.
[00:02:41] Trevor Anderson: How long are we expecting the polling to take?
[00:02:44] Joseph: So based on Grant's smoke test, he. It took about like two hours for him.
[00:02:51] Joseph: So we set the polling like the timeout time to be like about four hours. But.
[00:02:58] Joseph: But still, I don't know why the. I can see that in the ui, in the tap ui, but that.
[00:03:02] Joseph: That has not been processed. So I'll wait for this. The other. What is the task? The other seed?
[00:03:08] Joseph: Because for some seeds it might happen and this has happened in the manual approach as well.
[00:03:13] Joseph: So if we get the same error here, we can just email to you on to get the further insights from them on.
[00:03:19] Joseph: Why is that blocked? And it still says processing.
[00:03:24] Trevor Anderson: So it says you initiated the upload. Right. But yes.
[00:03:28] Trevor Anderson: And these process the seed is that what's going on?
[00:03:31] Joseph: Upload is done. That's complete. So that's why we are able to see that in the tap ui.
[00:03:36] Trevor Anderson: And you're pulling the check if it's finished processed.
[00:03:39] Akhil Gonna: Yes, you're right. Yeah.
[00:03:44] Trevor Anderson: Okay. Well, yeah, try to get it to run successfully with that strategy.
[00:03:50] Trevor Anderson: But if it really does take like four to six hours, we might want to think about a different strategy.
[00:03:58] Venkatesh Mannam: Okay.
[00:03:59] Trevor Anderson: Maybe you have a different job that Gets ran that checks every like 30 minutes. Right.
[00:04:05] Trevor Anderson: Instead of having a constant thing up.
[00:04:10] Akhil Gonna: Okay. Okay.
[00:04:12] Joseph: We'll wait for this run and then
[00:04:14] Akhil Gonna: we can maybe open a call.
[00:04:16] Trevor Anderson: Yeah. Decide on the next steps there. But yeah. Awesome.
[00:04:19] Akhil Gonna: Thank you.
[00:04:19] Trevor Anderson: Keep me updated on that. Yeah, sure. Any other updates?
[00:04:25] Joseph: No, I'm good.
[00:04:26] Trevor Anderson: Okay, thanks Joseph.
[00:04:38] Kapil Sreedharan: Yeah, I can go.
[00:04:39] Trevor Anderson: Go for it.
[00:04:41] Kapil Sreedharan: Yeah. I reviewed the. The. The fact table for fluent performance at. There was a blocker for.
[00:04:54] Kapil Sreedharan: Who was that? Shubham, I think. Yes.
[00:04:57] Trevor Anderson: Yeah.
[00:05:01] Venkatesh Mannam: Yeah.
[00:05:01] Kapil Sreedharan: So I think we are good here. He basically. Okay, so this whole thing is, you know, we need the. We.
[00:05:13] Kapil Sreedharan: We need to join the fact table from the.
[00:05:16] Kapil Sreedharan: The tune and also we'll need to join with the Minion fact table in order to get the impressions.
[00:05:27] Kapil Sreedharan: So tune has all the metrics like. Like click conversions, all those things.
[00:05:33] Kapil Sreedharan: But what is missing is the impressions. And I think they were. Yeah. Stuck or stuck on that. Yeah.
[00:05:41] Kapil Sreedharan: So the solution will be to do a join outer join on. On both and he should be able to proceed.
[00:05:50] Kapil Sreedharan: I tested it out. I think overall it looks good. I created a draft PR so that he can look into that.
[00:06:05] Kapil Sreedharan: Now one. One thing that we need to validate is if our gold camp.
[00:06:10] Kapil Sreedharan: Because this whole fluent performance is dependent on this advertiser and gold advertiser and gold campaign table.
[00:06:24] Kapil Sreedharan: Yeah. So we need to make sure that all the campaigns and all the advertisers are in that table. Yeah.
[00:06:32] Kapil Sreedharan: So I think Venki, you'll need to validate that. And I also asked in. There's the PR for. For the camp.
[00:06:50] Kapil Sreedharan: Sorry, there's a JIRA for that cam gold campaign. Let me find the number.
[00:07:05] Venkatesh Mannam: One second.
[00:07:19] Trevor Anderson: Is this one 1511?
[00:07:20] Kapil Sreedharan: Yeah, yeah, that one. So if you scroll down in the comments.
[00:07:34] Akhil Gonna: Yes.
[00:07:41] Kapil Sreedharan: Yeah. So you know, in Pulse there's this dashboard that has a list of all the campaigns in Pulse.
[00:07:48] Kapil Sreedharan: And we need to. Yeah, first we need to make sure that whatever the campaigns.
[00:08:01] Kapil Sreedharan: Are in this dashboard, is that. Is it linked to a rom?
[00:08:06] Trevor Anderson: I think it just. This is just homepage.
[00:08:11] Kapil Sreedharan: Okay. There's the Excel also that I. Oh, you can search Creative. Yeah, creative reporting.
[00:08:22] Trevor Anderson: This guy.
[00:08:23] Kapil Sreedharan: Yeah,
[00:08:29] Trevor Anderson: Yeah.
[00:08:30] Kapil Sreedharan: So you click. You can click on all the camp. You can click on generate report. Yeah.
[00:08:41] Kapil Sreedharan: So this would give all the campaigns that's on Pulse. It also gives a status.
[00:08:48] Kapil Sreedharan: Now our goal campaign for Minion should have all of these campaigns. So we'll need to validate that.
[00:08:55] Kapil Sreedharan: And maybe, you know, it has 160 advertisers here, right?
[00:09:00] Kapil Sreedharan: It says search 160 advertisers in the search box.
[00:09:04] Kapil Sreedharan: So we should also have, you know, all these advertisers in that gold advertiser table.
[00:09:12] Kapil Sreedharan: So that is for Minion.
[00:09:14] Trevor Anderson: Okay.
[00:09:15] Kapil Sreedharan: And then both the gold advertiser and gold campaign should also have campaigns and advertisers for all the playful rewards.
[00:09:24] Trevor Anderson: Yes.
[00:09:25] Kapil Sreedharan: Yeah. And I think syndication is also managed in Pulse, right?
[00:09:31] Kapil Sreedharan: I'm not sure, but syndication, it's only Walmart, so.
[00:09:35] Kapil Sreedharan: And Walmart campaigns, I'm not 100% sure where it is, but yeah, we need to do, you know, those kind of validations.
[00:09:41] Trevor Anderson: Okay.
[00:09:42] Kapil Sreedharan: I don't know if. Venki, did you do that validation on your gold advertiser?
[00:09:47] Venkatesh Mannam: I checked against all our Minion advertisers couples. So they are present.
[00:09:52] Kapil Sreedharan: Okay, cool. And how did you check which you compared against which table?
[00:09:59] Venkatesh Mannam: Basically what I did is since we don't have the active status flag in our gold table on the gold advertiser in Minion schema.
[00:10:07] Venkatesh Mannam: So I was checking, I mean like I was querying directly from the Pulse configuration silver table advertiser.
[00:10:15] Kapil Sreedharan: Okay.
[00:10:17] Venkatesh Mannam: And that report, I mean like those advertisers matched in the finance report as well.
[00:10:23] Venkatesh Mannam: But I will check against this report as well, the one that you attached here.
[00:10:29] Kapil Sreedharan: Okay.
[00:10:29] Akhil Gonna: Do.
[00:10:30] Kapil Sreedharan: Yeah, do some validations.
[00:10:31] Trevor Anderson: Yeah, it says 365. I don't know if that's.
[00:10:37] Trevor Anderson: I have a time period here though, so I don't know if it's a. More than. More than this.
[00:10:48] Kapil Sreedharan: Yeah. So yeah, we need to validate these two. And.
[00:10:53] Kapil Sreedharan: And then the other, the fluent performs, I think they should be good with that.
[00:10:59] Trevor Anderson: Okay, So for the fluent performance metrics view. Is that.
[00:11:12] Trevor Anderson: So that's going to be using these advertiser and campaign.
[00:11:15] Kapil Sreedharan: Yes.
[00:11:16] Trevor Anderson: Yeah. So it needs to have all. Basically the whole list of them.
[00:11:22] Akhil Gonna: But yeah, yeah.
[00:11:23] Trevor Anderson: Do you need a. Do you need a new ticket for that, Venky?
[00:11:28] Akhil Gonna: Which one?
[00:11:29] Joseph: Kaban?
[00:11:29] Venkatesh Mannam: I'm sorry?
[00:11:31] Trevor Anderson: Do you need a new ticket to do the validation on the advertiser?
[00:11:34] Venkatesh Mannam: I. No, no. Anyway, like I have one for net suit, right.
[00:11:37] Venkatesh Mannam: So when I'm making changes, I can do a quick validation.
[00:11:40] Venkatesh Mannam: And by the way, Kapil, you mentioned that we need to ingest playful rewards also.
[00:11:45] Venkatesh Mannam: Is it for advertisers as well?
[00:11:49] Kapil Sreedharan: Yeah, we need advertisers. I don't know how to get that though. But we need that. Right?
[00:11:55] Kapil Sreedharan: For the fluent performance metrics, we need to know the playful advertisers too, right?
[00:12:01] Venkatesh Mannam: Yeah, yeah.
[00:12:01] Akhil Gonna: Right.
[00:12:05] Trevor Anderson: In the campaign table, we're going to have a column that shows the advertiser
[00:12:12] Venkatesh Mannam: yeah, they're going to point to my gold table. I mean, the fluent gold table.
[00:12:18] Venkatesh Mannam: And they're going to bring the fluent advertiser id.
[00:12:22] Trevor Anderson: Okay. That's what I. I was making sure of. But yeah, that sounds good.
[00:12:29] Venkatesh Mannam: Yeah.
[00:12:30] Kapil Sreedharan: If you don't know how to get the playful advertiser, maybe a question to Jesse. I don't know.
[00:12:39] Kapil Sreedharan: He might know.
[00:12:41] Akhil Gonna: Okay. Yeah.
[00:12:48] Kapil Sreedharan: Yeah. So, Trevor, I'll. I'll do some validations on the. On the performance and fluent performance.
[00:12:57] Trevor Anderson: Okay, thanks for. Yeah, digging into that
[00:13:02] Kapil Sreedharan: work with Chuba.
[00:13:05] Akhil Gonna: See.
[00:13:06] Trevor Anderson: Yeah, It was this one, right? No, not this one. It was this guy here, right? MV Fluent performance.
[00:13:23] Trevor Anderson: No, it wasn't. Was it this one? Which ticket was it? Do you know, Shubin?
[00:13:38] Venkatesh Mannam: I think they are not in this stand up.
[00:13:40] Vani Kaithi: Well,
[00:13:42] Trevor Anderson: no, I was asking Kapil. Which. Which item do you know? I think it was. Oh, was it this one?
[00:13:49] Kapil Sreedharan: I was on mute. It's.
[00:13:53] Trevor Anderson: It was this one here. Well, this one just says playful 1459.
[00:14:00] Kapil Sreedharan: Actually, I don't know. Yeah, 145 9.
[00:14:06] Trevor Anderson: 1 4. It's not even any of these. Oh, it's under control. I see. Okay, great. Anything else, Phil?
[00:14:28] Kapil Sreedharan: No, no, that's. Yeah, we'll. We'll, you know, go through some validated.
[00:14:34] Akhil Gonna: Okay,
[00:14:36] Trevor Anderson: thank you. And you're out. You're on Monday.
[00:14:40] Kapil Sreedharan: Yeah, Monday, Tuesday, I'm on.
[00:14:43] Trevor Anderson: Oh, I guess Wednesday too.
[00:14:45] Kapil Sreedharan: Oh yeah, Wednesday
[00:14:47] Trevor Anderson: everyone's out. Wednesday on the Canada side and then us. Us Thursday, Friday. Okay.
[00:14:52] Trevor Anderson: Next week's gonna be interesting. All right. Thanks.
[00:14:57] Kapil Sreedharan: Thanks
[00:15:00] Trevor Anderson: anyway. Who's next?
[00:15:08] Akhil Gonna: Can you hear me? I can go next. Drama to Sonic. Yeah.
[00:15:13] Akhil Gonna: So yesterday, the first one yesterday, JC have reloaded the campaign. What are the missing campaigns?
[00:15:22] Akhil Gonna: He have asked Sharita and he have manually ingested the missing campaigns with the campaign names with an act.
[00:15:32] Akhil Gonna: With a new column active with a new column status.
[00:15:37] Akhil Gonna: And there we were able to figure out whether the campaign is active or inactive.
[00:15:42] Akhil Gonna: And what are the missing campaigns? Earlier they were like either retired or they are no longer used.
[00:15:51] Akhil Gonna: But.
[00:15:51] Akhil Gonna: Yeah, but the join they are making from the PI side is using the fuzzy logic where they're trying to get some keywords from the offer description and the recipient name.
[00:16:03] Akhil Gonna: And yeah, he shared me some sample query where I can try to replicate. So that's what I'm doing now.
[00:16:11] Akhil Gonna: But apart from that, yeah, I'm. I'm working on this reward cold table.
[00:16:17] Akhil Gonna: Yeah, the campaign table looks good. Now I can. I'll be able to close that campaign table ticket.
[00:16:27] Akhil Gonna: Yep, no problem.
[00:16:28] Trevor Anderson: That's good. Thanks for Working with JC yesterday on that.
[00:16:31] Akhil Gonna: Yeah, no problem. Thanks.
[00:16:39] Trevor Anderson: We do we need to do anything with those not. Not active campaigns that we're missing?
[00:16:48] Akhil Gonna: Yeah, JC have already loaded into the table.
[00:16:51] Trevor Anderson: Oh, he had. Okay. How did he get. Or how did he know that or how was it missing originally?
[00:17:00] Akhil Gonna: Yeah, they were missing because they're no longer used. So that was the reason they were missing.
[00:17:08] Akhil Gonna: But what he had done is he reached out to Sharith and asked the mapping like for that specific campaign id, what's the campaign name and he got the list and he manually ingested into the campaign table.
[00:17:23] Akhil Gonna: Yeah.
[00:17:24] Trevor Anderson: And Sharita, does she manage the. Is this Tremendous API ingestion?
[00:17:32] Akhil Gonna: Yes, yes, I think so.
[00:17:34] Akhil Gonna: I know she is the point of contact for all the Tremendous, but I believe either from the team or she is responsible.
[00:17:44] Trevor Anderson: Okay, that's probably good information to know. Do we have any docs on Tremendous?
[00:17:48] Trevor Anderson: I mean might be useful to put her as a point of contact, but
[00:17:52] Akhil Gonna: yeah, maybe I'll create one for orders and campaign. Yeah.
[00:17:57] Trevor Anderson: And yeah, if we have any issues, we know JC and Sharita.
[00:18:03] Akhil Gonna: Yep.
[00:18:05] Trevor Anderson: Okay.
[00:18:05] Kapil Sreedharan: Yeah. What I learned was the Tremendous is only used for Lead Manager campaigns
[00:18:15] Trevor Anderson: fulfillment on Lead Manager.
[00:18:16] Kapil Sreedharan: Yeah. Yeah. That's what JC said yesterday, right?
[00:18:20] Akhil Gonna: Yeah.
[00:18:22] Kapil Sreedharan: Akhil, were you able to link the tremendous campaign IDs to lead manager campaign with that fuzzy logic?
[00:18:31] Akhil Gonna: Not the campaign IDs based on the names I was able to find.
[00:18:35] Akhil Gonna: Like let's say there is name called Ad Parlor Monopoly. Go.
[00:18:40] Akhil Gonna: I couldn't find the exact name in Lead Manager but if I search on Monopoly I was able to match.
[00:18:47] Akhil Gonna: But there is no exact match for the full string.
[00:18:52] Kapil Sreedharan: But the query that JC send, are we able to implement that?
[00:18:58] Akhil Gonna: Yeah, even that's like the fuzzy log. He's finding some keywords out of the full string.
[00:19:06] Akhil Gonna: But yeah, by using the similar logic I was able to find something.
[00:19:12] Kapil Sreedharan: Okay.
[00:19:12] Venkatesh Mannam: I think.
[00:19:13] Kapil Sreedharan: Yeah, I think that should be okay. That's what the BI uses, right?
[00:19:16] Akhil Gonna: So yeah, yeah, that's what he mentioned as well.
[00:19:21] Trevor Anderson: Does it get all of them or. Or does it match all of them?
[00:19:25] Akhil Gonna: Yeah, there are so many as of now. I mean. But yeah, I'll. I'll get the exact count.
[00:19:30] Akhil Gonna: How many I could match. Yeah, yeah, most of them. I. I was able to see them.
[00:19:36] Trevor Anderson: Okay, can you see?
[00:19:37] Trevor Anderson: Yeah, just get the count of how many were able to match versus not match would be useful.
[00:19:43] Akhil Gonna: Yeah, sure.
[00:19:45] Trevor Anderson: I know Vanke Barat had put together a function, a fuzzy match function.
[00:19:52] Trevor Anderson: I don't know if it can be leveraged here. Maybe.
[00:19:58] Akhil Gonna: Yeah, sure.
[00:20:02] Trevor Anderson: Okay, thanks Akil.
[00:20:05] Akhil Gonna: Thank you.
[00:20:14] Trevor Anderson: All right. Yeah, go for it.
[00:20:18] Vani Kaithi: I can. Yeah. Yesterday I have validated this two tickets.
[00:20:26] Vani Kaithi: The ingestion of the apply data and adding the position views in uit both looks good.
[00:20:32] Vani Kaithi: I have attached them to the next release.
[00:20:36] Vani Kaithi: Yeah, Kapil Is there anything more to this position views that we need to work on?
[00:20:41] Vani Kaithi: I was just going through the message
[00:20:45] Kapil Sreedharan: for now.
[00:20:46] Akhil Gonna: No, but
[00:20:48] Kapil Sreedharan: the. Yeah. The thing with this P1 view is. Yeah. Right now they only get metrics for.
[00:20:59] Kapil Sreedharan: They're not able to slice pipe. You know the position. Right.
[00:21:03] Vani Kaithi: Yeah, yeah.
[00:21:04] Kapil Sreedharan: Because we don't have a dimensions for that.
[00:21:08] Vani Kaithi: Yeah, yeah. It's just like metrics.
[00:21:11] Kapil Sreedharan: Yeah. So
[00:21:15] Akhil Gonna: if.
[00:21:16] Kapil Sreedharan: If Kevin asks we might have to do it.
[00:21:21] Vani Kaithi: Okay.
[00:21:24] Kapil Sreedharan: So Trevor, right now what is happening is on. Do you have the access to the
[00:21:33] Trevor Anderson: Kevin's dashboard, the BI dashboard? I don't think so.
[00:21:39] Kapil Sreedharan: Here, I put it in the chat. Zoom chat. We can see what this. How he made use of one exchange.
[00:22:02] Kapil Sreedharan: Yeah, yeah. It's the grid metrics. These ones on the. Yeah, that one in here. Yeah.
[00:22:14] Kapil Sreedharan: So he added these P1, P2 I think you have to control or command to select multiple.
[00:22:27] Kapil Sreedharan: Also you see click on CTR just below that.
[00:22:37] Trevor Anderson: Ctr.
[00:22:37] Kapil Sreedharan: Yeah, yeah, yeah. So the.
[00:22:41] Kapil Sreedharan: The problem now is so yeah we can get how many views for whatever this advertiser campaign combination.
[00:22:51] Kapil Sreedharan: How many views were there? So P P1, that was. That was the highest.
[00:22:55] Kapil Sreedharan: And then P2, P3, P4 now the CTR shows for all the positions.
[00:23:04] Kapil Sreedharan: Now if they want to see what was the performance of a particular campaign or a creative for P1 versus rest, they won't have that.
[00:23:15] Kapil Sreedharan: I think the kind of. I'm not sure but this is my.
[00:23:20] Kapil Sreedharan: What I think they're they want to get is they want to see how the campaign performed when the users are at position P1 versus rest of the positions or how the campaign performed at each of the positions.
[00:23:40] Trevor Anderson: Is this the only met are all metrics. Can you slice by position?
[00:23:46] Kapil Sreedharan: No, no. Right now.
[00:23:47] Akhil Gonna: No.
[00:23:50] Kapil Sreedharan: Yes. The CTR is, you know, for all the positions. It's that we don't have slice and dice by positions.
[00:23:59] Kapil Sreedharan: So right now it only shows. Okay for P1, how many views were there? P2, P3 how many views were there?
[00:24:05] Kapil Sreedharan: It doesn't show the CTR for P1 CTR for P2, P3, P4 they don't have way to slice it down.
[00:24:14] Trevor Anderson: I see. Is this a. This is a calculated metric though because what does this stand for?
[00:24:21] Trevor Anderson: Click through rate.
[00:24:22] Kapil Sreedharan: Yeah, yeah. These are our standard.
[00:24:26] Trevor Anderson: But this is calculated in the metrics view.
[00:24:29] Kapil Sreedharan: Yeah, yeah.
[00:24:30] Akhil Gonna: Okay.
[00:24:32] Trevor Anderson: And it's. You're saying it's a. It's. It's for all positions, right?
[00:24:36] Manav Paul: Like.
[00:24:37] Trevor Anderson: Yeah, yeah.
[00:24:37] Kapil Sreedharan: So all the metrics are calculated for. Yeah, all the positions. It's.
[00:24:44] Akhil Gonna: We.
[00:24:44] Kapil Sreedharan: We don't have slicing by positions. Nobody asked for it or nobody looks at it.
[00:24:52] Kapil Sreedharan: So what I'm thinking is this might come up where they want to slice by P1 the positions like similarly here, right.
[00:25:05] Kapil Sreedharan: Like similar to P1 views. They might also want P1 CTR, P2 CTR, things like that.
[00:25:13] Trevor Anderson: Yeah, they want to be a. They might want it as a dimension is what you're saying.
[00:25:17] Kapil Sreedharan: Yeah, yeah.
[00:25:19] Trevor Anderson: Okay.
[00:25:21] Kapil Sreedharan: We don't know. So it may come up.
[00:25:27] Trevor Anderson: Okay, well, until it does, I mean we can think about it, but I don't see a need to.
[00:25:34] Kapil Sreedharan: They.
[00:25:35] Trevor Anderson: If they want it like this, then keep it like that.
[00:25:38] Trevor Anderson: But yeah, if they want to move to the other way, we can redesign the data.
[00:25:43] Akhil Gonna: Yeah,
[00:25:46] Trevor Anderson: but I would expect them to want it as a dimension too. So this is interesting view.
[00:25:53] Kapil Sreedharan: Yeah, exactly right. This. It would make more sense.
[00:25:59] Trevor Anderson: Okay.
[00:26:09] Kapil Sreedharan: I think for now this ticket is. Is good.
[00:26:12] Trevor Anderson: So it's already in here. I thought it needs to be released before it goes.
[00:26:19] Kapil Sreedharan: He's. He's pointing to the sandbox.
[00:26:21] Trevor Anderson: Oh, to sandbox. Okay.
[00:26:24] Kapil Sreedharan: And we are running in sandbox. We have a job that is refreshing it.
[00:26:28] Trevor Anderson: Oh, okay. They're excited huh? To get start testing it. That's funny. Okay, great.
[00:26:42] Vani Kaithi: Yeah, I can pick up any new task.
[00:26:47] Trevor Anderson: Okay, let me. I don't have anything in mind currently, so I'll have to take a look. Unless you had.
[00:26:57] Trevor Anderson: Unless you were aware of anything funny that you could pick up.
[00:27:01] Vani Kaithi: But I'll see in the backlog and see if I can.
[00:27:05] Trevor Anderson: Okay. Yeah, take a look and let us know. Okay, thank you.
[00:27:14] Vani Kaithi: Thanks.
[00:27:20] Venkatesh Mannam: So I can go next.
[00:27:22] Akhil Gonna: Yeah.
[00:27:24] Venkatesh Mannam: I started working on that migration of Gold Ad group conversions to use ucm.
[00:27:31] Venkatesh Mannam: So there is an underlying requirement to add the partner type dimension to the. To the existing ucm.
[00:27:40] Venkatesh Mannam: So added those change and testing it currently and Kapil like currently gold.
[00:27:47] Venkatesh Mannam: I mean like we know that DSTM is not using conversion count and all but just an FYI that current Gold Ad group conversion is doing optimized.
[00:28:01] Venkatesh Mannam: Give me one sec. I misspelled this always. So there is this conversion type as.
[00:28:16] Venkatesh Mannam: Yeah, optimized conversion.
[00:28:18] Venkatesh Mannam: So for conversion counts, in existing Gold Ad group conversion we use only optimized conversions Kapil But in the current UCM we count all the.
[00:28:28] Venkatesh Mannam: Or we sum all the conversions.
[00:28:30] Akhil Gonna: Right.
[00:28:30] Kapil Sreedharan: Yeah.
[00:28:30] Venkatesh Mannam: So slightly the number can inflate in when we try to use the ucm. So just an affair.
[00:28:37] Venkatesh Mannam: But DS confirmed that they are only using the epc, right? Yeah.
[00:28:43] Kapil Sreedharan: Yeah, that's what Chalky said. They are not using that.
[00:28:45] Venkatesh Mannam: Yeah. Yeah.
[00:28:46] Akhil Gonna: Okay.
[00:28:51] Venkatesh Mannam: Yeah. Should be ready by today. I'll send it to Chalky review.
[00:28:56] Venkatesh Mannam: I'll make this change and I'll send it to Chalky for review.
[00:29:05] Venkatesh Mannam: And we haven't heard anything back from the IT team, right? Trevor and Kapil No. Okay.
[00:29:13] Akhil Gonna: Okay.
[00:29:14] Trevor Anderson: And then they asked like what this is for. Right, I saw that.
[00:29:18] Venkatesh Mannam: Yeah.
[00:29:26] Trevor Anderson: Maybe Kapil if they don't answer by end of day. If you could just follow up on that.
[00:29:31] Trevor Anderson: Yeah, and then I'll. If they don't get back to us by today, I'll. I'll follow up next week.
[00:29:40] Venkatesh Mannam: Okay.
[00:29:40] Kapil Sreedharan: Yeah, while you're out, I'll just email maybe by noon and then.
[00:29:47] Trevor Anderson: Okay. Yeah, that sounds good. Anything else, Venki? What is this?
[00:30:01] Trevor Anderson: Is this one from the warnings that happen?
[00:30:04] Venkatesh Mannam: Yeah. Anyway, we. If we switch to ucm, I think we can close this ticket.
[00:30:17] Trevor Anderson: Can you explain that? Sorry.
[00:30:18] Venkatesh Mannam: Oh, yeah. I mean like these three tickets that I talked about are interlinked.
[00:30:23] Venkatesh Mannam: So Brand told like we can switch to frequency of 30 minutes. And in the other chart, Chalky confirm.
[00:30:31] Venkatesh Mannam: I mean like when Kapil check with the DS team, they confirm that we can migrate to use ucm.
[00:30:37] Venkatesh Mannam: So this ticket can be closed if we are making changes to migrate from.
[00:30:43] Venkatesh Mannam: I mean like to source from UCM rather than the session events table.
[00:30:48] Trevor Anderson: Are you suggesting to stop this then?
[00:30:52] Venkatesh Mannam: Not stop, but we change the source from analytics feed to the ucms.
[00:31:01] Venkatesh Mannam: So that way that job gonna complete in less than like two, three minutes.
[00:31:05] Venkatesh Mannam: And we don't need to implement this to update the frequency to 30 minutes.
[00:31:12] Trevor Anderson: I see.
[00:31:13] Venkatesh Mannam: Yeah.
[00:31:15] Trevor Anderson: Okay. Do we still need to run it every.
[00:31:21] Venkatesh Mannam: I think once we switch to ucm, the underlying use, the underlying UCM are updated every one hour.
[00:31:28] Venkatesh Mannam: So maybe we have to switch it to everyone. Yeah, everyone. Yeah.
[00:31:34] Trevor Anderson: Okay, that makes more sense. That makes sense. Thank you.
[00:31:43] Trevor Anderson: Yeah, that would be good to get those warnings out of the way. Okay. And this one's related to. Yeah.
[00:31:54] Akhil Gonna: Yeah.
[00:31:54] Trevor Anderson: Okay. All right. Sounds good. Thank you, Venki. I can go, Trevor. Okay, great. Thank you.
[00:32:06] Manav Paul: I'm just gonna be working today on getting the pipeline out with Joseph.
[00:32:10] Manav Paul: Just testing through it and validating.
[00:32:12] Manav Paul: And then also we'll work on getting that JIRA ticket format going as well as answering those open questions from the segment registry document.
[00:32:22] Manav Paul: But no blockers.
[00:32:26] Trevor Anderson: Awesome. You ran Initial tests on this one. Right.
[00:32:30] Manav Paul: We'll be running the test today.
[00:32:31] Trevor Anderson: Okay. You're going to test it out today? Just send me the job link so I could take a look.
[00:32:36] Akhil Gonna: Sure.
[00:32:38] Trevor Anderson: Awesome. Thanks, Manav Yep.
[00:32:45] Joseph: And for a quick question, so as we discussed yesterday, for the dev environment, do we need to copy to the, for example, to the vendor destination, which is, which is in the same bucket?
[00:33:00] Trevor Anderson: Up to you guys.
[00:33:02] Joseph: Because it's a kind of data duplication.
[00:33:03] Joseph: Then I thought, okay, let me, let us just keep the copy there and just copy it when it's brought.
[00:33:11] Trevor Anderson: Wait, you're asking about the declared audience? Well, the dev folder is only for testing.
[00:33:22] Trevor Anderson: Like, you're not going to be running, you're not going to be keeping this, you know, run every test in dev.
[00:33:27] Trevor Anderson: Right. Or run every declared audience in dev. It's just the test pipeline is working like.
[00:33:34] Trevor Anderson: So you might have a file there to, you know, you might have a file or two in the dev folder, but you shouldn't have like one to one copy of what's in production.
[00:33:47] Trevor Anderson: Right. Should just be a small subset.
[00:33:49] Manav Paul: Well, no, we're just wondering if we should copy from the staging folder to then the vendor folders since they're going to be the same bucket.
[00:33:57] Manav Paul: Is it even worth it in the testing since it's all in the same bucket anyway?
[00:34:01] Manav Paul: If we should just skip that part of it specifically for dev testing.
[00:34:04] Trevor Anderson: No, you guys should, you should test
[00:34:07] Manav Paul: out the whole thing, right?
[00:34:08] Trevor Anderson: The whole thing? Yeah.
[00:34:09] Manav Paul: Okay, cool.
[00:34:10] Trevor Anderson: Yeah, it's not going to matter for data duplication. It's going to be at a small scale.
[00:34:14] Trevor Anderson: So it's not like we're, we're gonna, we should be concerned about, you know, storage price.
[00:34:23] Manav Paul: Okay, sounds good.
[00:34:24] Akhil Gonna: Makes sense. Yeah.
[00:34:27] Trevor Anderson: But yeah, you should test out the full functionality as if it was in, you know, production.
[00:34:32] Akhil Gonna: Okay, Makes sense. Thanks.
[00:34:35] Trevor Anderson: Okay, thank you. Barratta and Grant is out. I think that was everyone. Yeah.
[00:35:02] Trevor Anderson: Okay, well, thank you all very much. I don't hear from you. Have a great Friday and enjoy the weekend.
[00:35:16] Manav Paul: Thanks, guys.
[00:35:17] Trevor Anderson: Thanks, team.
