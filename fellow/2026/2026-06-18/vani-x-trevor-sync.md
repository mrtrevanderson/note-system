---
title: "Vani x Trevor Sync"
date: "2026-06-18"
start_time: "2026-06-18T13:30:00-04:00"
end_time: "2026-06-18T14:00:00-04:00"
meeting_id: "040000008200E00074C5B7101A82E00807EA0613D9C0D4CC7CDDDC010000000000000000100000003BD349D27723DD4E820A26EF210B5652"
note_id: 80307556
recording_id: 14673956
fellow_url: "https://fluentco.fellow.app/meetings/040000008200E00074C5B7101A82E00807EA0613D9C0D4CC7CDDDC010000000000000000100000003BD349D27723DD4E820A26EF210B5652/"
meeting_link: "https://fellow.link/S35Z99JBTBQA"
---

# Vani x Trevor Sync

## Participants

| Name | Email | Organizer | RSVP |
|------|-------|-----------|------|
| Trevor Anderson | tanderson@fluentco.com | yes | accepted |
| Vani Kaithi | vkaithi@fluentco.com | no | notResponded |

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
[ ] Vani Kaithi to post a comment on the ticket about the suggested table naming change to 'userwise_appsflyer_metrics', confirm with Kapil, and potentially raise in show and tell.

## AI Summary

Trevor announced that the team must begin submitting **weekly Lattice updates** starting next week to showcase **AI utilization** and demonstrate ROI on company platforms. Vani is working on **AppFlyer data ingestion for Playful**, ingesting user-wise aggregated metrics (with rolling 7-day overwrites) and in-app events data from Athena S3. Key technical challenges include handling duplicate rows using row hash deduplication and refining table naming conventions—Trevor suggested renaming from 'userwise_af_data' to **'userwise_appsflyer_metrics'** for better clarity. The data feeds into fluent performance metrics for tracking clicks, conversions, and fulfillment costs. Vani also completed integration of new survey fields (depth and gaming) from Lead Manager into the advertiser report dashboard, which displays cumulative ROAS metrics over 90 days post-install.

### Chapters

#### Lattice Weekly Updates Rollout

- [00:00:18] Dan and the organization are rolling out a new requirement for **weekly Lattice updates** from all team members, replacing Trevor's current practice of writing summaries from Jira updates.
- [00:01:19] Team members will answer five status questions every Friday covering AI wins, highlights, next week's focus, blockers, and other updates, which managers can use to compile their own summaries.
- [00:03:04] The primary purpose is to **showcase AI utilization** across workflows and development with tools like Claude, as the Data Engineering team has the highest utilization in the organization and leadership wants to justify **ROI on platform spending**.
- [00:05:17] Vani will begin submitting weekly updates starting next week (week of June 22, 2026), with Slack integration available for reminders though actual updates must be posted in Lattice.

#### AppFlyer Data Ingestion

- [00:05:54] Vani is ingesting two new tables from Athena S3 for the Playful performance metrics project: user-wise AppFlyer data (CSV files) and in-app events data (parquet files).
- [00:06:44] The user-wise AppFlyer data has a **rolling 7-day overwrite pattern** (e.g., June 11-17 today, June 12-18 tomorrow), handled via merge strategy on the install_time field with create timestamps from within the files.
- [00:08:52] The in-app events data contains duplicate rows where all **87 columns are identical** (including create timestamp), likely from users clicking multiple times, requiring deduplication to prevent inflated fulfillment costs downstream.
- [00:11:39] After analyzing combinations of the 87 columns, Vani plans to create a **row hash** for each row to enable deduplication, then drop the hash column after use since no unique key combination was found.
- [00:13:17] Trevor suggested renaming tables from 'userwise_af_data' to **'userwise_appsflyer_metrics'** for clarity (spelling out AppFlyer and using 'metrics' instead of the vague 'data'), with Vani to confirm with Kapil and potentially discuss in show and tell.
- [00:16:34] The ingested data feeds into the fluent performance metrics (ticket 1463), providing **clicks, conversions, and fulfillment cost** metrics for downstream reporting.

#### Survey Fields Integration

- [00:18:50] Vani added new survey fields from Lead Manager to the gold email details table for the advertiser report, specifically the **depth and gaming fields** which weren't previously in the gold table.
- [00:20:01] The BI team is building a separate advertiser report dashboard using these survey fields for grouping, similar to existing Link dashboard functionality but with enhanced survey field display capabilities.
- [00:22:25] The dashboard displays **cumulative ROAS metrics over 90 days** post-install, showing revenue growth at day 1, day 3, etc., with typical plateau occurring around 20-30 days based on the example data reviewed.

#### Other Sprint Work

- [00:24:46] The business unit mapping table classification work was completed and **released in sprint 28**.
- [00:25:04] The Tune catalog migration ticket is closed but not done, as it requires first migrating the fact table to use the fluent advertiser ID instead of Tune-only identifiers (discussed in recent grooming).

### AI-Detected Action Items

- Vani Kaithi: Post a comment on the ticket about the suggested table naming change to 'userwise_appsflyer_metrics', confirm with Kapil, and potentially raise in show and tell.

### Decisions

- Dan and the organization will require **weekly Lattice updates from all team members** to track AI utilization and justify platform spending ROI.

## Transcript

[00:00:03] Trevor Anderson: And hopefully today's been going well for you.
[00:00:06] Vani Kaithi: Yeah. Yep. Awesome few things.
[00:00:13] Trevor Anderson: Well, I just have one update for my end and we could jump into some of the things that you're working on.
[00:00:18] Trevor Anderson: But I guess what, what Dan and organization is going to be rolling out. They kind of, they.
[00:00:26] Trevor Anderson: We would like to see weekly updates in the Lattice application.
[00:00:30] Trevor Anderson: I'm not sure if you have seen this tool or used it in any capacity.
[00:00:36] Vani Kaithi: I have seen that tool but never used it.
[00:00:40] Trevor Anderson: Yeah.
[00:00:40] Trevor Anderson: So I guess what happens right now is I, I do my best in tracking updates that happen on Jira and then I write a weekly update to Verdan.
[00:00:51] Trevor Anderson: But what he want and then Dan let us add up to the.
[00:01:00] Trevor Anderson: Kind of executive leadership and so on and so forth.
[00:01:05] Trevor Anderson: But what, what Fluent would like to see and I guess Dan as well, is that everyone on the team is posting weekly updates that he can use to write his summaries.
[00:01:19] Trevor Anderson: Oh, so this would be like once a week on Friday in the Lattice application there's a My update section and it shows, it asks like five questions and they're just like a status update questions like I have them right here.
[00:01:39] Trevor Anderson: What, what were your AI wins for the week? Oh yeah. I could share.
[00:01:45] Vani Kaithi: Share.
[00:01:51] Trevor Anderson: Yeah. Let me share.
[00:01:54] Vani Kaithi: Yeah.
[00:01:54] Trevor Anderson: So this is what it looks like here.
[00:01:57] Trevor Anderson: So you'll see this tab like in Lattice you just click this update section and then there'll be my updates area and it will have like every week it will have a spot to put answers to these questions.
[00:02:16] Trevor Anderson: AI wins for the week.
[00:02:19] Trevor Anderson: It doesn't need to be anything crazy, just you know, some quick notes on these different questions here.
[00:02:26] Trevor Anderson: Biggest impact or highlights for the week.
[00:02:30] Trevor Anderson: This probably be like, you know, some of the tickets that we're working on throughout the week.
[00:02:36] Vani Kaithi: Yeah.
[00:02:37] Trevor Anderson: Focus and attention for next week.
[00:02:39] Trevor Anderson: So like what's upcoming, any blockers or anything that you might need support or help with and then anything else kind of a catch all bucket that you would like to share and then I can use these notes that you provide to me to write my update as well and then Dan can do the same.
[00:03:02] Vani Kaithi: Okay. Okay, sure.
[00:03:03] Trevor Anderson: Yeah.
[00:03:04] Trevor Anderson: And, and, and I think the company, what they're looking for mostly is and they shared with me this morning that they're looking just to see how, you know, our team is doing a lot with AI automations and using it within our workflows and developing with cloud code.
[00:03:22] Trevor Anderson: Right.
[00:03:22] Trevor Anderson: Like so yeah, they mostly want to showcase and showcase how our department is utilizing it because I think we're kind of the furthest but we have the Most utilization probably within the organization and we're doing a lot with it.
[00:03:43] Trevor Anderson: But also it's probably a justification for the organization, you know, they're probably spending a lot.
[00:03:48] Trevor Anderson: So for these platforms.
[00:03:51] Trevor Anderson: So it's mostly like, are we, you know, seeing a benefit, a return on investment from how we're utilizing this technology?
[00:04:00] Trevor Anderson: So this would probably be.
[00:04:03] Trevor Anderson: Any updates that are shared for this one would probably be useful for the company to track that.
[00:04:10] Trevor Anderson: Right?
[00:04:11] Vani Kaithi: Yeah, Yep.
[00:04:13] Trevor Anderson: It's fairly straightforward. There's also a Slack, some slight Slack integration here as well.
[00:04:21] Trevor Anderson: So on Slack there's a Lattice connection that you can do on the apps. How do I share that?
[00:04:49] Trevor Anderson: Like down here at the bottom there's.
[00:04:53] Trevor Anderson: You can connect Lattice to it and this will just give you a reminder. Like this.
[00:04:57] Vani Kaithi: Yeah, yeah, I have connected to this. Yeah.
[00:05:01] Trevor Anderson: And then.
[00:05:01] Trevor Anderson: So you can't like actually post your update in Slack, unfortunately, I, I wish that was the case.
[00:05:07] Trevor Anderson: But if you click buy, it'll just take you right to the. The week weekly update section.
[00:05:15] Vani Kaithi: Okay. Yeah.
[00:05:17] Trevor Anderson: So I think we have an early week this week, but for next week it would be good if you can put your.
[00:05:24] Trevor Anderson: You can do it this week too.
[00:05:25] Trevor Anderson: But for at least starting next week, it would be great to see these updates in here.
[00:05:31] Vani Kaithi: Yeah, sure. Trevor, do that for next week.
[00:05:36] Trevor Anderson: Yeah, that'd be great. Thank you. Other than that, that's pretty much all I had to share.
[00:05:45] Trevor Anderson: But I'm curious what's going on in your world? Anything that you want to talk about?
[00:05:54] Vani Kaithi: Yeah, mostly working on this ingesting app Flat data.
[00:06:01] Trevor Anderson: Oh yes. This is new for playful.
[00:06:03] Vani Kaithi: Yeah, for playful performance metrics which sidebar is building for the aggregate performance metrics.
[00:06:14] Vani Kaithi: So for this we are right now ingesting the two new tables from the Athena S3 location.
[00:06:26] Vani Kaithi: It's user wise app Claire Data and the in app events data.
[00:06:32] Vani Kaithi: So for the user wise app Claire Data we are receiving that as like CSV files.
[00:06:44] Vani Kaithi: So every day they are overwriting the last seven days, like last six days files as well.
[00:06:51] Vani Kaithi: So today we have received from 11th to 17th, so tomorrow we will receive again from like 12th to 18th.
[00:07:02] Trevor Anderson: I got it. So it's rolling. Seven day, is it daily?
[00:07:06] Vani Kaithi: Yeah, yeah.
[00:07:08] Trevor Anderson: Okay, so you have to overwrite the last seven days of data.
[00:07:13] Vani Kaithi: Yeah, yeah.
[00:07:13] Trevor Anderson: For everyday processing.
[00:07:14] Vani Kaithi: Okay. I'm doing like incremental wise so it can do on certain columns.
[00:07:21] Vani Kaithi: So in this we have like it's already aggregated to install time on this field.
[00:07:30] Vani Kaithi: So I'm using the same fields and doing the merge on the.
[00:07:36] Vani Kaithi: If they have like exact match on that, then override it, update that insert so that way. So for this.
[00:07:48] Vani Kaithi: Yeah. For all these files the create timestamp will be like today itself.
[00:07:54] Vani Kaithi: So that will be easy for us to. To merge on that. We can use the same create timestamp. So inside.
[00:08:04] Trevor Anderson: Okay.
[00:08:05] Vani Kaithi: Yeah.
[00:08:07] Trevor Anderson: So it's going to be a timestamp like. Like as you.
[00:08:12] Trevor Anderson: Is it going to be the day that you process it or the file name here that create time stamp.
[00:08:19] Vani Kaithi: The create timestamp is inside the file.
[00:08:22] Trevor Anderson: Oh. Not everyone has it though. Not every file has that. Some other
[00:08:31] Vani Kaithi: for bronze we are just using the auto loader and ingesting everything using the checkpoint and checkpoint.
[00:08:40] Vani Kaithi: So for everything there will be like create timestamp from the event.
[00:08:48] Trevor Anderson: Got it.
[00:08:52] Vani Kaithi: So using this field I'm doing the merge and for the in app events there is some discussion I was going on with the Bharat.
[00:09:08] Vani Kaithi: So this. These are like the parquet files we are receiving. So it's like every day one parquet file.
[00:09:18] Vani Kaithi: You see one parquet file. So in that what we are seeing is there are. Yeah. So for the bronze we.
[00:09:29] Vani Kaithi: We just use the same auto loader and ingested it.
[00:09:35] Trevor Anderson: Okay.
[00:09:36] Vani Kaithi: But in the bronze what we are seeing is we are seeing like duplicates data. So this. Yeah.
[00:09:50] Vani Kaithi: For same app flyer ID and for same. And every column is like same value. There is no change.
[00:09:57] Vani Kaithi: And same create timestamp as well. I think the user clicked many times or something like that.
[00:10:05] Vani Kaithi: So we are seeing like we can deduplicate on any certain column or.
[00:10:11] Vani Kaithi: Or else we need all the columns to do the deduplication.
[00:10:16] Trevor Anderson: Yeah. If all the columns are the same drop or. Yeah, just deduplicate. Okay.
[00:10:22] Vani Kaithi: Yeah.
[00:10:24] Trevor Anderson: Interesting.
[00:10:26] Vani Kaithi: And yeah, because in the downstream we are going to use this table for the fulfillment cost.
[00:10:35] Vani Kaithi: There is. Yeah, there is fulfillment cost. So this will get inflated if we don't do the deduplication.
[00:10:47] Trevor Anderson: Right. It'll be higher.
[00:10:49] Vani Kaithi: Yeah, yeah.
[00:10:50] Trevor Anderson: So what do you guys. What's like the pattern for dropping duplicates?
[00:10:53] Trevor Anderson: Do you guys use a spark drop duplicates or how you.
[00:10:56] Vani Kaithi: Yeah, yeah. Drop duplicates if there are.
[00:11:02] Vani Kaithi: Sometimes we use like the unique key in that so that it will just drop.
[00:11:09] Vani Kaithi: But here there are so many columns. Yeah, yeah.
[00:11:13] Vani Kaithi: For that only doing some analysis to see if we can use any combination of columns to draw a key.
[00:11:22] Vani Kaithi: Yeah, but yeah, I couldn't. It's like there are 87 columns in this. Most. Most are like used.
[00:11:34] Vani Kaithi: I searched with many like. But still there are like duplicates.
[00:11:39] Vani Kaithi: So what I'm thinking is like create a row hash in the code itself.
[00:11:46] Vani Kaithi: When after reading the branch and then using that column, just drop, duplicate and drop that column after using that.
[00:11:55] Vani Kaithi: Thinking that approach.
[00:11:57] Trevor Anderson: Okay, interesting. So the hashes, like every single column.
[00:12:02] Vani Kaithi: Yeah, Row hash for every single row there will be.
[00:12:05] Trevor Anderson: Got it. What's AF here stand for? Analytics Feed, Apps Flyer. Oh, this?
[00:12:17] Vani Kaithi: I was listening to this first. App Flyer, App supplier.
[00:12:22] Trevor Anderson: Okay, so this. That's an actual application. And then what is User wise stand for? User.
[00:12:34] Vani Kaithi: Not sure.
[00:12:36] Trevor Anderson: Apps Fire. I'm just going to look it up. Let's see what comes up. Nothing's coming up.
[00:12:55] Trevor Anderson: Apps Flyers, like the application. Wonder what User wise is.
[00:13:07] Vani Kaithi: And that is.
[00:13:11] Trevor Anderson: Are we gonna. How are we gonna name this? So it's going in playful catalog, right?
[00:13:15] Vani Kaithi: Yeah, it's going to be playful. Yeah.
[00:13:17] Trevor Anderson: So we probably want Apps Flyer to be a name there.
[00:13:24] Trevor Anderson: Right now it's named User Wise, but I'm not even sure what that stands for.
[00:13:31] Vani Kaithi: Yeah, we thought of using the same names. Usable is a of data and a of user in appearance.
[00:13:42] Trevor Anderson: You might want to spell out Apps Flyer.
[00:13:46] Vani Kaithi: Okay. For this. Okay.
[00:13:47] Trevor Anderson: Yeah. Because if. If you're not an engineer or, you know.
[00:13:54] Vani Kaithi: Yeah, yeah.
[00:13:55] Trevor Anderson: You know, someone working on this, you have no idea what AF stands for, right?
[00:13:58] Trevor Anderson: Yeah, I think that's probably a good idea.
[00:14:02] Trevor Anderson: I don't know about User wise, but that's probably important to someone.
[00:14:09] Vani Kaithi: Yeah. User Wise operates the playful rewards app itself.
[00:14:25] Trevor Anderson: Must have something you can play for.
[00:14:28] Vani Kaithi: Yeah. This is like the offer wall, which shows the game offers and record step.
[00:15:00] Trevor Anderson: Okay. I probably just keep both.
[00:15:02] Trevor Anderson: Like, I would probably put userwise underscore Apps Flyer or the other way around.
[00:15:07] Trevor Anderson: Apps Flyer, underscore Userwise, but have both. Have both fully written out.
[00:15:14] Vani Kaithi: Yep, yep.
[00:15:19] Trevor Anderson: But other than that, this seems pretty. And then underscore data, like, is pretty vague as well.
[00:15:28] Trevor Anderson: Like what, you know what, what, what is this actually from? Like what endpoint? What.
[00:15:38] Trevor Anderson: What type of data is this? Like, underscore data is not like a true entity.
[00:15:42] Trevor Anderson: So I'm wondering if we have a better name for that.
[00:15:46] Vani Kaithi: This is like Apple Agree Data. Like it has like clicks, installs, revenue and all.
[00:15:56] Vani Kaithi: So all these columns.
[00:16:06] Trevor Anderson: So it's not purely like one entity. It's like a lot of them.
[00:16:11] Trevor Anderson: You have campaign sessions, we have metrics.
[00:16:27] Trevor Anderson: And what is this? I think. Sorry, where is this going?
[00:16:30] Trevor Anderson: Like what, downstream is this going into
[00:16:34] Vani Kaithi: the MV? Fluent performance for 1463. Yeah, fluent performance metrics.
[00:16:46] Trevor Anderson: And what are we taking? What metrics are we taking from?
[00:16:50] Vani Kaithi: From this? We are gonna take the, like, the Playful clicks, conversions and the fulfillment cost.
[00:16:56] Trevor Anderson: Clicks, convergence, fulfillment. Okay.
[00:17:02] Vani Kaithi: Yeah, like this.
[00:17:03] Trevor Anderson: So.
[00:17:08] Trevor Anderson: Okay, Maybe I could post a question like instead of data it could be like metrics at least a little bit.
[00:17:22] Trevor Anderson: It has a little bit better description than just data.
[00:17:28] Vani Kaithi: Yeah.
[00:17:29] Trevor Anderson: And is that the only. That's not the only table. Right? You just show me a few different ones.
[00:17:34] Vani Kaithi: Yeah.
[00:17:35] Trevor Anderson: Or is that. Or is that the main one that we're looking for?
[00:17:39] Vani Kaithi: This is the aggregate metrics one like installs clicks and all the other one is like events data.
[00:17:49] Trevor Anderson: This one in app Events. So this one has a decent name. Right. Like we. I know it's in app Events.
[00:17:55] Trevor Anderson: The other one might be good to call it metrics.
[00:17:58] Vani Kaithi: Yeah. Yeah.
[00:17:59] Trevor Anderson: All right. Maybe if you want to post in the comments like that.
[00:18:04] Trevor Anderson: I suggested the name should be userwise underscore apps flyer.
[00:18:08] Trevor Anderson: Underscore metrics instead of underscore data.
[00:18:11] Vani Kaithi: Yep, yep. I loved it.
[00:18:14] Trevor Anderson: And see if Kapil, who were you working on this with? Kapil and Bharat.
[00:18:20] Vani Kaithi: Yeah, Kapil.
[00:18:21] Trevor Anderson: Yeah. Yeah, maybe maybe ask Kapil and what
[00:18:25] Vani Kaithi: he thinks about that and yeah, if I'll speak and I can ask in show and tell everyone.
[00:18:31] Trevor Anderson: Yeah, yeah, I can bring it up again too. But yeah, other than this, this is. This looks pretty good.
[00:18:41] Trevor Anderson: And the ingestion method makes sense as well. That seems sound.
[00:18:50] Vani Kaithi: Yeah. Mostly working on this this week and other than this previous week I just worked on that.
[00:19:01] Vani Kaithi: Ingesting the Fig 2 new server fields into our fig goal table.
[00:19:07] Trevor Anderson: Mostly the. The what new field? Sorry.
[00:19:11] Vani Kaithi: Yeah.
[00:19:16] Trevor Anderson: Oh the new attributes for the.
[00:19:19] Vani Kaithi: Yeah. Advertiser report one. So they asked for all these fixed survey fields in the. Out of this.
[00:19:26] Vani Kaithi: We just don't have in our gold table the depth and gaming one.
[00:19:32] Vani Kaithi: We have ingested these into our goal email details table.
[00:19:39] Trevor Anderson: Do you remember where those come from or. Yeah, where did those fields come from?
[00:19:44] Vani Kaithi: Is that from the lead manager?
[00:19:46] Trevor Anderson: A lead manager. Oh, got it. And so that's being used in. Is that the link dashboard there?
[00:19:58] Trevor Anderson: Do you have access to that?
[00:20:01] Vani Kaithi: Is this a new one or this dashboard? Yeah, they are using this like this survey fields.
[00:20:08] Trevor Anderson: Oh okay.
[00:20:09] Vani Kaithi: Similar to this we have added to our report for the fact table Advertiser pack table is.
[00:20:16] Trevor Anderson: Yeah. Okay. Is this not the. There was an separate report that I think.
[00:20:22] Vani Kaithi: Yeah, yeah, yeah. BI is building separate dashboard based on that.
[00:20:29] Vani Kaithi: For that they need the similar way of display to display all the to group by survey fields.
[00:20:38] Trevor Anderson: This is what they're calling the advertiser report.
[00:20:40] Vani Kaithi: This is not that report. I don't have access to that.
[00:20:44] Trevor Anderson: Oh, or it's not created yet maybe.
[00:20:47] Vani Kaithi: Yeah, I think it's still developing.
[00:20:51] Trevor Anderson: Okay.
[00:21:03] Trevor Anderson: Okay. Do you know how to navigate that BI dashboard?
[00:21:07] Trevor Anderson: I'm not really sure what I'm looking at there. Oh, this row s1.
[00:21:17] Trevor Anderson: Like there's a lot of things to look at.
[00:21:20] Vani Kaithi: Yeah, we can do like we can do by advertiser or we can do by like partner also or campaign also.
[00:21:29] Vani Kaithi: We can select anyone, just play to that. And the install date wise,
[00:21:40] Trevor Anderson: What are mostly people looking for? Is it revenue here? Because I don't that bottom table, maybe.
[00:21:49] Trevor Anderson: What's that?
[00:21:51] Vani Kaithi: This is graph.
[00:21:57] Trevor Anderson: Maybe you're not the best person to ask about this.
[00:22:00] Trevor Anderson: Yeah, you're like, you're like I just provide the.
[00:22:04] Trevor Anderson: I just make it positive, provide the data that makes it possible to build this.
[00:22:07] Vani Kaithi: But yeah, we have the ROAS fields. I built this active actually previously.
[00:22:15] Vani Kaithi: So we have the client revenue, ROAS fields and revenue per user.
[00:22:22] Trevor Anderson: Why is there so many different ROAS fields?
[00:22:25] Vani Kaithi: So still. So what does the report do is like we have selected the install date. Right.
[00:22:33] Vani Kaithi: So in this range, how many people has installed the app or app and after the install, like in one day, how much is the this revenue and then like after the install, after three days, how much is revenue?
[00:22:51] Vani Kaithi: Like that. So it will not. These things will not depend on the this selection.
[00:23:00] Trevor Anderson: Okay, I see. So they're cumulative growth metrics in a way.
[00:23:06] Vani Kaithi: Yeah, yeah. Till 90 days for every thing.
[00:23:12] Trevor Anderson: Okay. Can you kind of see like exponential growth over how many days?
[00:23:24] Trevor Anderson: I would expect it to keep getting bigger. Is that the case? Yeah, it does. Okay.
[00:23:32] Trevor Anderson: So I guess just looking at this for that particular thing that you have highlighted, there's like a plateau around 20 days or so or 20, 20 to 30 days it drops off or there's no incremental growth there.
[00:23:47] Trevor Anderson: Okay, interesting. There's a lot to look at here. This is a pretty granular report, right?
[00:23:56] Trevor Anderson: Like is there.
[00:23:57] Vani Kaithi: Yeah.
[00:23:58] Trevor Anderson: Is there more aggregated view of this or is it because you're looking at just a partner and a particular segment?
[00:24:08] Trevor Anderson: Looks like.
[00:24:09] Vani Kaithi: Yeah, we can do day wise or year old. We can do like partner advertiser wise as well.
[00:24:24] Trevor Anderson: Yeah, there's not. Is there a view that shows like everything rolled up or. No.
[00:24:28] Vani Kaithi: No, I guess no.
[00:24:30] Trevor Anderson: Okay.
[00:24:37] Vani Kaithi: Okay.
[00:24:40] Trevor Anderson: What are these other items that you have complete here? They're.
[00:24:44] Trevor Anderson: So those were the two main things that you've been working on?
[00:24:46] Vani Kaithi: Yeah, yeah. These are from previous print. The business unit mapping table classification.
[00:24:54] Trevor Anderson: Yeah. That went out and released 28. Right. Okay.
[00:24:57] Vani Kaithi: Yeah. Yeah.
[00:25:04] Trevor Anderson: And that last one. Close. Not done. That one was.
[00:25:09] Vani Kaithi: Yeah. To migrate to fluent catalog. So we decided.
[00:25:13] Trevor Anderson: Oh, this was that one with tune.
[00:25:15] Vani Kaithi: Yeah.
[00:25:15] Trevor Anderson: Being tune only. And we need.
[00:25:18] Vani Kaithi: Yeah. First we need to migrate the fact table on to use the other like fluent advertiser id. Yeah.
[00:25:32] Trevor Anderson: Okay. Before we can move this. Got it. And we just went through that on grooming. Okay, awesome.
[00:25:51] Trevor Anderson: Anything else from around?
[00:25:53] Vani Kaithi: Nothing. That's it.
[00:25:59] Trevor Anderson: Okay, sweet. Well, we have the review the new uni catalog naming guides later today.
[00:26:07] Vani Kaithi: Yeah.
[00:26:07] Trevor Anderson: So excited to see you there. And I guess we have show and tell as well. So I'll see you later then.
[00:26:15] Vani Kaithi: Sure. Thank you, Trevor.
[00:26:17] Trevor Anderson: Of course. Yeah, you too. Bye. Bye.
[00:26:19] Vani Kaithi: Bye.
