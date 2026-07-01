---
title: Cybage - DE team standup
date: 2026-06-30
time: "06:05-06:30"
participants: [Trevor Anderson, Brian Silveri, Akhil Gonna, Bhavesh Gidwani, Joseph Ramesh, Kapil Sreedharan, Vani Kaithi, Venkatesh Mannam, Granth Sajjanshetty, Manav Paul, Ajay Kumar, Bharat Pamnani, Pratik Ahire, Samreen Belawat, Samreen Shaikh, Shubham Mandlik, Amit Kumar]
fellow_url: https://fluentco.fellow.app/meetings/040000008200E00074C5B7101A82E00807EA061ED04731FE45E9DC01000000000000000010000000B4E52B2A9B75554886302B59A05B811C/
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

The DE team held their standup covering multiple workstreams. Key discussions included **migration of MV Fluent Advertiser Performance to the Fluent catalog** (ticket 1453), with the team deciding to keep all related subtasks together and ensure proper data sourcing. A significant issue emerged with the **Tab API performance**, where uploading 25K rows took 6-12 hours instead of the expected 4 hours, raising concerns about scalability to the target of 800K-1M rows. The team also addressed **UCM conversion count definitions**, confirming that UCM's approach (counting all non-click conversions) is correct versus gold's optimized-only approach. Other progress included hot fixes to production systems (reducing runtime from 2 hours to 1 hour), partner type dimension work across adflow metrics, and rewards table development.

### Tab API Performance Issues

- Bug fix for ticket 1634 was merged and deployed, resolving the initial processing issue that prevented the pipeline from running properly. [10:05]
- The **Tab API upload took 6-12 hours for 25K rows**, significantly longer than the expected 4 hours, raising concerns about scalability since the goal is to upload 800K-1M rows. [10:12]
- Grant emailed Tab representatives to clarify SLAs and expected processing times, as the current performance doesn't align with what was initially promised (faster API processing). [10:37]
- Virginia and Terry are aligned on increasing volume to maximum capacity (not just 25K), but current performance indicates **12 hours for 25K could mean days for 1M rows**. [13:06]
- The team agreed to escalate beyond email to a direct meeting with Tab reps to discuss volume, throughput expectancy, SLAs, and potentially reconfigure file upload strategy for better performance. [13:20]

### MV Fluent Advertiser Performance Migration

- Deduplication logic for ticket 1489 now includes checking where 'reason is served' as an additional criterion for handling duplicate records. [01:24]
- Work on ticket 1463 involves creating a view based on app supplier data and merging it with MV Fluent Performance tables. [01:38]
- Subtasks 1597 and 1596 for ticket 1453 needed details added regarding catalog changes and source table pointers for the MV Fluent Advertiser Performance migration. [02:26]
- The team **decided to keep all subtasks under ticket 1453** rather than splitting them, including moving the table to Fluent catalog, changing sources to point to Fluent data (campaign, vertical), and rebuilding fact tables to accept multiple sources. [04:49]
- While MV Fluent Advertiser Performance exists in the dev catalog (Fluent_dev_metrics), it's still in central_data_prod_metrics in production and needs to be migrated as part of this ticket. [05:38]

### Conversion Count Logic and UCM

- For ticket 1531, the team is sticking with **existing UCM counts for conversions** as recommended by Kapil, rather than gold's definition. [16:31]
- Gold counts only optimize conversion events, while **UCM counts all non-click conversions**, which could cause value inflation if gold's approach was used. [16:41]
- Brian confirmed that **UCM's definition (all non-click conversions) is the correct approach**, not gold's optimized-only conversion counting. [17:13]
- The optimized conversion filter was added previously to handle dynamic CPC targeting, which uses a Pulse setting to determine which conversion event dynamic CPC targets (campaign_conversion_type_name = 'Optimize conversion'). [17:36]
- Brian will set up a conversation with Venkatesh and Chalky to align on the distinction between Pulse configuration settings versus the correct count of conversions for the ad group conversions table. [19:04]

### Production Hot Fixes and Performance

- Venkatesh deployed multiple hot fixes to production, with almost all tasks completed except the device task currently syncing to RDS. [15:54]
- With updated Databricks configurations, **pipeline runtime was reduced from 2 hours to 1 hour**, showing promising performance improvements. [16:14]
- The team is waiting for the NetSuite team to complete security testing before they can proceed with testing NetSuite IDs functionality. [19:25]
- Bug fix for the MySQL sync issue has been completed and moved to closed status. [19:57]

### Other Development Work

- Akhil created a tab_underscore table for rewards last week and needs to review ticket 1454 (event mapping) with Brian, as there's already a fact table in Fluent catalog with event information. [08:45]
- JC helped resolve blockers with the tremendous campaign and rewards table fulfillment work. [09:31]
- Vani is working on ticket 1530 to expose **partner type as a dimension** across adflow metric views, having added it to goal_sub, partner, and partner tables, and is validating changes end-to-end before raising a PR. [15:23]
- Virginia requested five additional audiences to be rebuilt, which Mano had missed by not adding an age factor when pushing the audiences. [14:47]
- Trevor is working on a new Q3 epic for deprecation and consolidation, similar to the optimization epic from previous quarters. [20:50]

### Action Items

- [ ] Add details to subtasks 1597 and 1596 for ticket 1453, specifying catalog changes and source table pointer requirements for MV Fluent Advertiser Performance migration. [02:44] (Trevor Anderson)
- [ ] Brian Silveri to schedule time to review ticket 1454 (event mapping) with Akhil to determine if it's still needed given the existing fact table with event information in Fluent catalog. [09:25]
- [ ] Brian Silveri to meet with Grant and Joseph after standup to review the board and determine priority tasks while Kapil is out. [14:49]
- [ ] Brian Silveri to review the conversion logic history and context for why optimized conversions filter was added to handle dynamic CPC. [17:53]
- [ ] Brian Silveri to set up a conversation with Venkatesh and Chalky to align on conversion count definitions and distinguish between Pulse configuration settings versus correct conversion counting for the ad group conversions table. [19:04]
- [ ] Review cost optimization (898) and system stability (993) epics, then move remaining stories to appropriate Q3 tech OKRs or consolidation epic, and triage any items that don't fit. [21:04] (Trevor Anderson)

### Decisions

- All subtasks for ticket 1453 (MV Fluent Advertiser Performance migration) will remain under the same ticket rather than being split into separate tickets. [04:49]
- The MV Fluent Advertiser Performance table will be **moved to the Fluent catalog** with sources pointing to Fluent data (campaign, vertical) and fact tables updated to allow multiple sources. [06:26]
- The team will **backfill data for unmatched records** in ticket 1458 as requested. [07:42]
- **UCM's definition for conversion counts** (all non-click conversions) is the correct approach to use, not gold's optimized-only conversion counting method. [17:13]
- The team will **escalate Tab API performance issues** from email to a direct meeting with Tab representatives to discuss volume, throughput, SLAs, and potentially reconfigure the file upload strategy. [13:20]

## Transcript

[00:00:00] Samreen Shaikh: If you don't have. Reason as served in the duplicates. If you don't have that.
[00:00:16] Samreen Shaikh: Right now, I was taking the. Out of the two records. I was taking the latest record.
[00:00:30] Brian Silveri: Let's. Let's pass 149.
[00:00:34] Vani Kaithi: Okay.
[00:00:34] Brian Silveri: Yeah. Let me ask soon me here.
[00:00:38] Samreen Shaikh: Okay.
[00:00:40] Brian Silveri: Yeah, let's. All right. Hey, everybody. Good morning. Good evening.
[00:00:52] Vani Kaithi: Good morning.
[00:01:05] Brian Silveri: Let's get started. Who would like to go first?
[00:01:11] Samreen Shaikh: Yeah, we can start.
[00:01:13] Brian Silveri: Sure.
[00:01:15] Samreen Shaikh: So there was this, the discussion we just had for 1489, where reason is Akhil to served.
[00:01:24] Samreen Shaikh: So that was one check for the deduplication and for 1463. Yes, I am working on it.
[00:01:38] Samreen Shaikh: And I created the view based on the app supplier data.
[00:01:42] Samreen Shaikh: Now I have to merge it with the MV Fluent performance. So I'm working on that part.
[00:01:49] Granth S: Great.
[00:01:50] Samreen Shaikh: Yeah. For Shubham's ticket below it. Yeah. So we have received the comment for 1461 and for 1453.
[00:02:03] Samreen Shaikh: Brian, if you can open it.
[00:02:05] Brian Silveri: Sure. 1453, right?
[00:02:09] Samreen Shaikh: Yeah.
[00:02:11] Brian Silveri: Come on, Come on. All right, Take seven. Go ahead.
[00:02:26] Samreen Shaikh: So if you notice there are subtasks here and we don't have data for them for 1-597-1596. Details.
[00:02:39] Samreen Shaikh: We don't have details in there.
[00:02:40] Brian Silveri: Oh, details. Got it. Oh, sure, no problem.
[00:02:44] Brian Silveri: Trevor, can you do me a favor and just take the action to add the details?
[00:02:47] Brian Silveri: Because we're just talking about changing the catalogs and where you want the pointers to please
[00:02:53] Trevor Anderson: the source table. Do we want the source table to be influent as well?
[00:02:58] Trevor Anderson: Is that what the second subtask is asking for?
[00:03:02] Brian Silveri: Yes, that's what we discussed.
[00:03:04] Brian Silveri: Based off of that grooming is that we want to move this to the Fluent catalog and then change the source to point to the Fluent data for the stuff like the vertical and the campaign.
[00:03:16] Trevor Anderson: Oh, like our campaign table. Okay, got it. And then rebuild fact tables. What is that one referring to?
[00:03:25] Trevor Anderson: Do you know the last subtask?
[00:03:28] Brian Silveri: Sure. The fact table that powers this point it to the. The sources so.
[00:03:35] Brian Silveri: Or, sorry, the fact table that is being powered by this.
[00:03:38] Brian Silveri: We want to allow for it to take from multiple sources, not just the one.
[00:03:42] Trevor Anderson: I see. Okay. Yes, I can add details.
[00:03:46] Brian Silveri: Thank you. I appreciate it. We'll get you that answer today.
[00:03:50] Samreen Shaikh: Okay, thanks.
[00:03:51] Granth S: Great.
[00:03:52] Samreen Shaikh: Those will remain like subtask for this check itself. Or those will be because.
[00:03:58] Samreen Shaikh: Because we have made changes for this ticket. 14:53.
[00:04:03] Samreen Shaikh: Adding the columns to the MB Fluent Advertiser performance.
[00:04:14] Brian Silveri: In terms of the. This. 1461.
[00:04:17] Samreen Shaikh: No, no. 14591453. Yeah.
[00:04:23] Brian Silveri: Oh, do you want to add DQ checks to this one? Is that the question?
[00:04:27] Samreen Shaikh: No, no, the question. The thing was, we have added these columns to the mb.
[00:04:33] Trevor Anderson: Great.
[00:04:34] Samreen Shaikh: Four columns, so. And we have raised a PR for this.
[00:04:37] Samreen Shaikh: So should the subtask will remain under this ticket or will they be like.
[00:04:45] Samreen Shaikh: Should we move them to a separate ticket?
[00:04:49] Brian Silveri: Oh, no, we should. We should definitely keep it in this ticket. Trevor, unless I'm wrong, we.
[00:04:53] Brian Silveri: We definitely want to move this MV Fluent Advertiser Performance to the Fluent catalog.
[00:05:00] Brian Silveri: As part of this. I would assume it wouldn't be.
[00:05:04] Trevor Anderson: Yeah, yeah, yeah. Is it not already in the Fluent catalog? I thought I saw it yesterday,
[00:05:15] Brian Silveri: But
[00:05:15] Trevor Anderson: it might have been something outside. So
[00:05:18] Samreen Shaikh: we have to make that change.
[00:05:20] Vani Kaithi: So.
[00:05:20] Samreen Shaikh: Yeah, we'll do that. That's fine. Okay, thank you.
[00:05:25] Brian Silveri: Thank you.
[00:05:26] Trevor Anderson: Actually, yeah, I see it here in Fluent. Fluent DEV Metrics. MV Fluent Advertiser Performance.
[00:05:34] Trevor Anderson: Is that the table?
[00:05:35] Granth S: Yeah.
[00:05:36] Brian Silveri: MV Fluent Advertising Performance.
[00:05:38] Trevor Anderson: Yeah, looks like it's. It's there in the dev catalog.
[00:05:44] Vani Kaithi: I have added that river previously when I.
[00:05:47] Vani Kaithi: When we thought of moving that to Fluent Catalog, but we didn't move at that time.
[00:05:54] Brian Silveri: Based on in prod. Yeah, today, right now in production, it's in Central data Prod Metrics.
[00:06:03] Trevor Anderson: Oh, I see. So, yeah, but it in the dev environment sandbox.
[00:06:09] Trevor Anderson: It's in the dev catalog, but, yeah, it's not in production yet. Got it.
[00:06:13] Brian Silveri: Got it. So, Bonnie, safe to say that this has already been moved.
[00:06:18] Samreen Shaikh: No, no.
[00:06:21] Brian Silveri: Oh, got it. All right, good. So we need to do this.
[00:06:25] Samreen Shaikh: Yeah, yeah, yeah.
[00:06:26] Brian Silveri: Okay, great. But let's do that as part of this ticket.
[00:06:29] Brian Silveri: Make sure we're pointing to the right data from Fluent, and then make sure the fact table that's powering this or that is powering this table is allowing for multiple sources.
[00:06:39] Brian Silveri: Great. Okay, thank you very much. Any other updates?
[00:06:51] Samreen Shaikh: Yeah, it's for 1, 4. Bharat. Bharat.
[00:06:59] Brian Silveri: Got it.
[00:07:00] Samreen Shaikh: Yeah. So, yeah, I think for 1, 4, 5, 8. Yeah. We had a question there.
[00:07:21] Brian Silveri: Is there another question after this one?
[00:07:26] Granth S: Yes.
[00:07:27] bharatpam: This one.
[00:07:28] bharatpam: This one.
[00:07:28] Samreen Shaikh: Only the back. Yeah. So, yeah.
[00:07:35] Brian Silveri: Yep. Could you please write your important one that we. Oh, yes, yes.
[00:07:42] Brian Silveri: The answer is yes to that question.
[00:07:44] Brian Silveri: I'll just put the reply yes, please generate backfill the data unmatched, please, and thank you.
[00:08:09] Brian Silveri: Sorry, I did not see that question. I'm still. I'm still catching up on time off, so my apologies.
[00:08:15] Brian Silveri: And Copple is off, so that's why.
[00:08:18] Samreen Shaikh: Yeah, got it. So, yeah, I think that's it from us.
[00:08:24] Brian Silveri: Great. Thank you. Who's next?
[00:08:33] Akhil Gonna: How you feeling yeah, I'm still recording as my voice is still bad. But yeah, thanks for asking.
[00:08:44] Brian Silveri: I hope you feel better.
[00:08:45] Akhil Gonna: Yeah, thank you. So yeah last week I didn't create one tab underscore table for rewards.
[00:08:52] Akhil Gonna: Maybe later I'll review it to you brand but we had question regarding the other 1454 the event mapping.
[00:09:01] Akhil Gonna: Yeah so there is already a fact table which is in fluent catalog which has already the event information.
[00:09:08] Akhil Gonna: Maybe post stand up or maybe later today when you have chance. I need.
[00:09:13] Akhil Gonna: We need to review this ticket one more time to see if we still really need it and why we need this for the.
[00:09:25] Brian Silveri: Okay, I'll schedule some time for this to talk through it.
[00:09:28] Akhil Gonna: Yeah, great. Thank you so much.
[00:09:30] Brian Silveri: No problem.
[00:09:31] Akhil Gonna: Yeah, from the snow blockers last week JC was with JC and JC really helped us having the tremendous campaign and the rewards table fulfilled.
[00:09:43] Brian Silveri: Great. Excellent. Thanks Kim. Yeah, no problem.
[00:09:47] Akhil Gonna: Thanks.
[00:09:53] Brian Silveri: Who's next?
[00:09:55] Granth S: I can go next.
[00:09:56] Brian Silveri: Brian Grant.
[00:09:59] Granth S: Okay, so yeah, okay. Actually 1634 should be in done.
[00:10:05] Granth S: So we merged that in so that's fixed the issue the bug that I spoke about yesterday.
[00:10:12] Granth S: So we did start off the pipeline and the seed did process but it did take longer than the four hours that we initially thought that it would take.
[00:10:25] Granth S: It took about anywhere between I want to say about 6 to 12 for about 25000 rows.
[00:10:34] Granth S: So yeah, took longer than expected.
[00:10:37] Granth S: So I've emailed the tab representatives asking what the SLAs are like, what's like this, what's the expected processing time.
[00:10:50] Granth S: But yeah, so just waiting to hear back from them. So.
[00:10:55] Granth S: But yeah, at least the good news is we were able to fix the bug quickly.
[00:10:59] Granth S: But yeah, apart from that all good, no blockers.
[00:11:03] Granth S: I'll take a look at the segment registry lock that Joseph created that we have a call about today.
[00:11:10] Granth S: So I'll take a look at it.
[00:11:16] Brian Silveri: Great.
[00:11:16] Trevor Anderson: So it did upload to the API. It's just our timeout was too short of a window.
[00:11:22] Trevor Anderson: So I guess the PR to fix that was just to extend it.
[00:11:27] Granth S: Yes.
[00:11:28] Trevor Anderson: Okay. And then you just. You just check to see what the expected time is, right?
[00:11:33] Akhil Gonna: Yes.
[00:11:34] Granth S: Okay, we just need to increase the polling timeout.
[00:11:39] Trevor Anderson: But how long did it take?
[00:11:42] Granth S: Honestly the last time I checked was yesterday. 9:30. It was still processing, then I went to sleep.
[00:11:48] Granth S: So I'm assuming anywhere between 6 to 12. And then I checked it again this morning at 6:30.
[00:11:54] Granth S: So yeah, like I said anywhere between 6 to 12.
[00:11:59] Trevor Anderson: I want to say 6 to 12 hours. Okay.
[00:12:02] Granth S: Which isn't great really because honestly we Were told it should be about 4 hours and it should be faster through the API.
[00:12:12] Granth S: Doesn't look like it.
[00:12:13] Granth S: That's hence the email to you asking what's the expected time for something like this?
[00:12:21] Granth S: Because right now it's just 25. Virginia wants to send more again, how long do we have to wait?
[00:12:28] Granth S: The idea main goal was to make
[00:12:30] Brian Silveri: it faster, but yeah, it's not.
[00:12:34] Granth S: It's not exactly. So somewhere somebody's not showed us the entire picture.
[00:12:41] Trevor Anderson: You uploaded 25 files or no, one
[00:12:45] Granth S: file with 25k rows.
[00:12:47] Brian Silveri: Okay, yeah, yeah. And.
[00:12:49] Brian Silveri: And Grant and Joseph, just to give you some context, I just met with Virginia and Terry and they are, they are aligned with us increasing that amount to maximum, not just the 25k.
[00:13:06] Brian Silveri: So if, if 25k is taken 12 hours, imagine when we load a million or 800k.
[00:13:14] Granth S: Exactly. So.
[00:13:15] Brian Silveri: So yeah, we got a problem. I think an email might not be the strongest option.
[00:13:20] Brian Silveri: I think it might be like, hey, we need to meet to discuss, you know, volume and throughput expectancy and SLAs and maybe we need to perhaps reconfigure how we're sending this data to conform with a better file upload maximum.
[00:13:40] Brian Silveri: So yeah, let. My response is like, hey, here, run through it. Like, hey, this is.
[00:13:44] Brian Silveri: We tested that with 25. We want to go up to a million. How. How do we make this work?
[00:13:49] Granth S: Yeah, no, no, and I agree with you. So at least this email can start the dialogue.
[00:13:55] Granth S: And then once that's done, once the dialogue started, I'll pull the entire team and we can definitely have a call.
[00:14:02] Granth S: Yeah, like with email going back and forth, we won't be able to solve this.
[00:14:08] Brian Silveri: I would agree.
[00:14:09] Akhil Gonna: Yeah.
[00:14:10] Brian Silveri: But yeah, no, I agree with you, bro. Thanks, Brand. I appreciate it. Awesome. Thank you.
[00:14:17] Joseph: In addition to that, what Grant said, because initially we tested with 10,000 records, so that was able to run in like two hours, I guess, if I'm not wrong.
[00:14:26] Granth S: Yeah.
[00:14:27] Joseph: But now after Virginia wanted to increase to 25,000 and it's taking longer, and if she wants to send the entire base and wonder, yeah, we'll meet with the TU reps.
[00:14:44] Joseph: Yeah, right.
[00:14:44] Brian Silveri: Got it.
[00:14:45] Joseph: It's the same. I'm gonna continue with it.
[00:14:47] Joseph: And I think Virginia has asked for another five audiences to be rebuilt.
[00:14:51] Joseph: I think Mano has missed to add that age factor. Shall just push this audience now.
[00:14:58] Brian Silveri: Yeah, I'll, I'll sit with you guys, uh, right after this, uh, if that's okay.
[00:15:04] Brian Silveri: Because I want to go through the board, especially since he's off and figure out what we've got to tackle.
[00:15:08] Brian Silveri: So I'll just ping you two after we're done this, if that's okay.
[00:15:11] Akhil Gonna: Perfect. Great.
[00:15:15] Brian Silveri: Thank you, Joseph. Thank you, Grant. Who's next?
[00:15:18] Vani Kaithi: I can go next.
[00:15:20] Brian Silveri: Thanks, Vonnik.
[00:15:20] Akhil Gonna: Go ahead.
[00:15:23] Vani Kaithi: Yeah.
[00:15:23] Vani Kaithi: Working on the D15 30 exposing the partner type as dimension across our adflow metric views for this we made changes in the goal sub, partner and partner tables.
[00:15:36] Vani Kaithi: We have added partner tab type as a dimension in that. Yeah, validating the changes end to end.
[00:15:43] Vani Kaithi: Brand will raise PR for this.
[00:15:46] Granth S: Yeah.
[00:15:47] Brian Silveri: Great. Great work, Lonnie. Thank you.
[00:15:51] Samreen Shaikh: Thanks, Tim.
[00:15:53] Brian Silveri: Awesome.
[00:15:54] Venkatesh Mannam: Thank you. Yeah, mostly yesterday with the few hot fixes to the production brand.
[00:16:04] Venkatesh Mannam: So almost all the tasks are done except device task is syncing to RDS right now.
[00:16:12] Venkatesh Mannam: So the number seems promising.
[00:16:14] Venkatesh Mannam: With the latest updated databrick configurations, the runtime has been reduced like from two hours to one hour, something like that.
[00:16:23] Akhil Gonna: So it's.
[00:16:24] Venkatesh Mannam: The numbers are promising when compared to the earlier runs.
[00:16:31] Venkatesh Mannam: Regarding 1531 brand, like Kapil mentioned that we're gonna stick to the existing UCM counts of the conversions and all.
[00:16:41] Venkatesh Mannam: So just F F I like if you look at the background problem, the fourth problem on the fourth point.
[00:16:49] Venkatesh Mannam: So gold counts only optimize conversion at the moment, as I discussed with you earlier. So.
[00:16:54] Venkatesh Mannam: But UCM counts all the non click conversions, so maybe if it goes in that route the value is going to inflate.
[00:17:04] Akhil Gonna: Right?
[00:17:06] Brian Silveri: Yeah. So if UCM right now is using this definition of all non click conversions.
[00:17:13] Brian Silveri: Yeah, but gold is only optimized. I agree. The UCM is the correct definition here.
[00:17:21] Akhil Gonna: Okay. Okay.
[00:17:22] Venkatesh Mannam: Any.
[00:17:22] Brian Silveri: This is correct.
[00:17:24] Venkatesh Mannam: Added this optimized conversions. Brian, like if you remember
[00:17:30] Akhil Gonna: because.
[00:17:31] Brian Silveri: No, not. Not off the top of my head, Venky, but I have to go back and look. I'm sorry.
[00:17:35] Venkatesh Mannam: Yeah, it's fine.
[00:17:36] Venkatesh Mannam: I'm like at the comment Akil mentioned that it's something to handle the dynamic cpc,
[00:17:44] Trevor Anderson: so.
[00:17:44] Brian Silveri: Oh, that makes sense. Yeah.
[00:17:46] Venkatesh Mannam: Change in logic for conversion count to handle dynamic cpc.
[00:17:53] Brian Silveri: All right, let me look. I'll. I'll take a look. Venki.
[00:17:56] Venkatesh Mannam: Yeah, it's fine. Anyway, like the DSC is only using EPC in this table.
[00:18:02] Venkatesh Mannam: I'm not sure if they want these type of columns like conversion count, click count and all the stuff.
[00:18:12] Venkatesh Mannam: So I think Chalky on this like what columns they need in the final gold conversion. Gold. This table.
[00:18:19] Venkatesh Mannam: Ad group conversions table. I haven't heard anything back from him.
[00:18:24] Brian Silveri: Yep. All right. Which conversion event? The dynamics. Yes. So I just looked at my messages with Chalky.
[00:18:34] Brian Silveri: There's a setting in Pulse that determines which conversion event that dynamic CPC targets, which is why he wanted Campaign conversion Type name equals Optimize conversion, and we put that in.
[00:18:50] Brian Silveri: But I think that this is an issue between configurations in Pulse versus what is the right count of conversions.
[00:19:03] Brian Silveri: Son of a.
[00:19:04] Brian Silveri: Okay, I think a conversation with you, me and Chalky might be required here, Venki, just so that we're aligned.
[00:19:10] Brian Silveri: Okay, let me set that up. Yeah, great. Okay. Fun. All right, great. What else, Becky? Anything else?
[00:19:21] Venkatesh Mannam: Nothing much like, I haven't had anything back from the.
[00:19:25] Venkatesh Mannam: I mean, like, they told they're going to test it once they clear the security.
[00:19:29] Brian Silveri: Test something that's right for the NetSuite IDs.
[00:19:33] Venkatesh Mannam: Yeah, yeah, I'm waiting on.
[00:19:35] Akhil Gonna: That's it.
[00:19:36] Brian Silveri: Yeah. All right. I haven't heard back either. I'm on the ticket, too.
[00:19:39] Brian Silveri: But once we hear back, Venki, we'll move as fast as we can.
[00:19:44] Venkatesh Mannam: Yeah, sure thing.
[00:19:46] Brian Silveri: Great.
[00:19:49] Trevor Anderson: Hey, Venki, did you put the bug fix in for the MySQL sync change that you did?
[00:19:57] Venkatesh Mannam: Yeah, yeah, it's in closed status now.
[00:20:00] Trevor Anderson: Oh, okay, great. Just want to make sure it was there.
[00:20:03] Venkatesh Mannam: Yeah, yeah.
[00:20:05] Brian Silveri: And Venki, this. This one.
[00:20:08] Venkatesh Mannam: Yeah, it's in.
[00:20:12] Brian Silveri: And this is a bug or this is something. Okay.
[00:20:17] Venkatesh Mannam: Something.
[00:20:18] Venkatesh Mannam: I mean, like, we are deprecating extra job and we're kind of indulging this into existing pipeline.
[00:20:25] Brian Silveri: Got it. Okay. I just wanted to figure out which parent this was for. So it's for the cost optimization.
[00:20:30] Brian Silveri: Great. Super.
[00:20:36] Trevor Anderson: Brian, I have a new Epic for Q3. It's similar to the optimization one. It's just
[00:20:48] Akhil Gonna: great.
[00:20:50] Trevor Anderson: I think it's deprecation. Consolidation. Consolidation, yeah, I see it. I feel like. Yeah, that one.
[00:21:00] Trevor Anderson: Thank you. Was working on rolling that item there.
[00:21:04] Brian Silveri: Yeah, I would feel free to take.
[00:21:07] Brian Silveri: I think might behoove us to just take the remaining stories that are in the cost optimization for this quarter and then move them into your thing here or into a separate thing if they don't fit into one of your tech okrs.
[00:21:22] Brian Silveri: So it might be a combination of your different ones tech okrs.
[00:21:27] Brian Silveri: So go through this cost optimization and the system stability.
[00:21:31] Brian Silveri: So 898 and 993 and then move those things around to what you think are the more suitable Q3 deliverables, if any.
[00:21:42] Brian Silveri: If not, leave them here and then we can triage the remaining ones.
[00:21:45] Trevor Anderson: Okay, thank you.
[00:21:47] Brian Silveri: Great. All right. Did we get everybody? Okay, great. Thank you all.
[00:21:57] Brian Silveri: Grant and Joseph, can you guys just hang back real quick for me, please? Everyone else can depart.
[00:22:04] Brian Silveri: Thank you, everybody. I appreciate your time. Please enjoy the rest of your day.
[00:22:08] Venkatesh Mannam: Thanks, team.
[00:22:09] Samreen Shaikh: Thanks.
