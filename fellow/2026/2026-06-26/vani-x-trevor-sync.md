---
title: Vani x Trevor Sync
date: 2026-06-26
time: 10:30-11:00
participants: [Trevor Anderson, Vani Kaithi]
fellow_url: https://fluentco.fellow.app/meetings/040000008200E00074C5B7101A82E00807EA061A0AADEDB16D05DD01000000000000000010000000F1C52C238D80A040A673F52E964A8D9E/
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

---

Trevor and Vani discussed progress on Q2 work and preparation for Q3. The **Power BI team suggested changing the position implementation to a dimension approach** rather than separate columns, which will require adding a position column and causing the fact table to fan out with more rows. They reviewed sprint 14 planning, with **two items ready for next week's release**, though several other tickets are blocked on dependencies including the fulfillment cost table. Significant time was spent discussing **PII masking implementation via GitHub workflows** using column-level tags and data-type-specific masking functions, though work is blocked waiting for DevOps to create an access group. The team is preparing for Q3, having accomplished quite a bit in Q2, and looking forward to implementing the **centralized Fluentco-wide dimensional table design** for easier reporting without complex joins.

### Position Dimension Implementation

- The Power BI team suggested making **position a dimension** (rather than separate columns) and will show the approach to Ryan for validation and feedback.
- Implementation will add **one position column that groups by values 1-4**, causing the fact table to fan out with more rows for each position.
- The approach uses one impression position column with grouping, allowing Power BI users to **slice and dice by position dimension** in reports.
- Trevor noted this dimension approach might have been better originally, but things moved fast during initial development and he missed providing that input at the time.

### Sprint Planning and Backlog

- **Two items are ready for next week's release** (if the release can go out next week as planned).
- Trevor suggested all developers would benefit from **access to Power BI dashboards** to see how their data is being used; Vani gained access that morning.
- Multiple sprint 14 tickets were reviewed: **fulfillment cost table blocked** on Fluentco advertiser/campaign and Tremendous rewards tables (Akli's work), GENIE space for Roas Data Explorer (groomed with story points), partner revenue validation (blocked on Minion contract terms ingestion), and auction candidate metrics (unclear if supporting fact table exists).
- Most backlog items still need grooming before work can begin; no clear next tasks available beyond the two completed items.

### PII Masking Implementation

- Plan to **migrate PII masking from Databricks jobs to GitHub workflows** for easier manual execution, similar to the Wyndham implementation already in place.
- Implementation creates **two workflows**: one for tagging PII columns at account level, another for creating mask functions and applying policies to the central data catalog.
- Uses column-level tags to identify PII fields; Wyndham implementation uses **row-level masking to hide entire tables** from non-data-engineering users per SOC2 requirements.
- **Masking functions vary by data type**: stars for strings, date format for dates, SHA hash for emails, null for structs, and special values for integers.
- Work is **blocked waiting for DevOps** to create a separate access group (rather than having data engineering manually maintain access lists for PII data).
- Future access requests will go through **Okta**, where DevOps will add approved users to the group to enable viewing of PII columns.

### Fulfillment Cost Table Planning

- Fulfillment cost fact table is **blocked on dependencies**: Fluentco advertiser, campaign, and Tremendous rewards tables that Akli is currently working on.
- Vani is reviewing the fulfillment cost design to prepare for implementation once dependencies are ready, focusing on understanding how advertiser, campaign, and rewards tables will work together.
- Discussion about whether **Playful and Tremendous fulfillment should be combined or kept separate**; unclear if Playful is a subset of Lead Manager or operates independently.
- Playful's user-wise tables contain **fulfillment cost data for events**, which was unexpected and raises questions about overlap with Tremendous data.
- Need to determine if Tremendous handles fulfillment for multiple clients (like **Capital One payouts**) and whether there's data overlap with Playful campaigns to inform table design.

### Q3 Preparation and Team Progress

- **Q2 went well** with the team accomplishing quite a bit; this week and next expected to be less hectic before Q3 starts.
- Only **a few straggling Q2 items remain**; team is ready for Q3 kickoff with most work completed or near completion.
- Looking forward to the CBUR implementation with **many fact tables and metrics** being built as part of the new data architecture.
- The **centralized Fluentco-wide dimensional table design** is easier from a business standpoint, eliminating complex joins in favor of pre-joined tables for Power BI reporting.

## Transcript

[00:01:21] Trevor Anderson: Okay. Hello.
[00:01:24] Vani Kaithi: Yep. How are you?
[00:01:28] Trevor Anderson: Good. Hope your week went pretty well.
[00:01:32] Vani Kaithi: Yeah, yeah, it was a kind of relaxed week for me.
[00:01:36] Trevor Anderson: That's good.
[00:01:37] Vani Kaithi: Yeah, yeah.
[00:01:38] Trevor Anderson: It looks like this week and probably next week too are going to be a little bit less on the hectic side.
[00:01:47] Vani Kaithi: Yeah.
[00:01:49] Trevor Anderson: Which is good. We're about to start the quarter so. But it looks like Q2 went pretty well.
[00:01:56] Trevor Anderson: So we accomplished quite a bit as a team, which is awesome to see. Yeah. And it looks like you have.
[00:02:10] Trevor Anderson: Yeah. You have two items here that are in the next release that's going on next week.
[00:02:14] Trevor Anderson: If we can get it out next week.
[00:02:17] Vani Kaithi: Yeah, yeah. There has been some discussions going on that position use with Power BI Team, so. Yeah.
[00:02:28] Trevor Anderson: Oh, that's good. They're taking a look at it, right?
[00:02:31] Vani Kaithi: Yeah, yeah.
[00:02:32] Trevor Anderson: Because we were taking a look at it this morning and the stand up. But what are they just.
[00:02:38] Trevor Anderson: They're asking questions about it.
[00:02:41] Vani Kaithi: Yeah, yeah.
[00:02:42] Vani Kaithi: They're suggesting to do that position as a dimension so that they can validate that as well and show that to Ryan as well and get his point of view on that.
[00:02:55] Vani Kaithi: Like which one?
[00:02:56] Trevor Anderson: Yeah, but that changes the way we build it, right?
[00:02:59] Vani Kaithi: Yeah, yeah. Just. We need to add just one more column to that as a position.
[00:03:05] Vani Kaithi: So it will like divide whole fact table into like. It will be like fan out on that.
[00:03:12] Vani Kaithi: Positions 1, 2, 3, 4.
[00:03:15] Trevor Anderson: Yeah. Won't that make more rows?
[00:03:17] Vani Kaithi: Yeah, yeah, it's gonna do.
[00:03:18] Trevor Anderson: Yeah, yeah.
[00:03:22] Trevor Anderson: So are you still going to keep the extra columns though or like position one column v one view.
[00:03:30] Vani Kaithi: I think there will be one more extra column beside this like, like impression position which says 1, 2, 3, 4 and we'll group by that as well.
[00:03:43] Trevor Anderson: Just one. And then we just use the what views column or something like that.
[00:03:47] Vani Kaithi: Yeah, yeah.
[00:03:48] Trevor Anderson: To. And then. Okay. And then we bring in the dimension position and then the metric probably I
[00:03:54] Vani Kaithi: can use that and they can slice and dice by that.
[00:03:59] Trevor Anderson: Yeah. I don't know why we didn't do that originally but you know, I guess they asked for it.
[00:04:06] Trevor Anderson: Maybe if I saw that, if I saw that happening I probably would have said something originally.
[00:04:14] Trevor Anderson: But I guess things move fast and sometimes I, I miss quite a bit. Like also I don't quite. I'm not.
[00:04:23] Trevor Anderson: I don't quite fully understand everything at this point. So you know, some.
[00:04:28] Trevor Anderson: Sometimes I'm unaware of something that maybe I would have an opinion on but
[00:04:34] Vani Kaithi: I also don't have access to that dashboard today itself. I just saw the dashboard so. Yeah.
[00:04:42] Trevor Anderson: Were you able to click on it this morning?
[00:04:44] Vani Kaithi: Yeah, yeah.
[00:04:45] Trevor Anderson: Okay. Yeah, it's good. I feel like we should have a good.
[00:04:51] Trevor Anderson: I feel like all the developers would benefit from where they're.
[00:04:57] Trevor Anderson: Where we're sending our data to and being able to check it out. Like how it's being used, you know.
[00:05:03] Vani Kaithi: Yeah. Yeah.
[00:05:07] Trevor Anderson: Okay.
[00:05:07] Trevor Anderson: So you have these two done items you mentioned in the standup that you can pick anything else up.
[00:05:14] Trevor Anderson: Were you able to take a look at all? I've been pretty busy this morning, so
[00:05:17] Vani Kaithi: I haven't look at the back. You can look at the sprint 14 tasks.
[00:05:22] Vani Kaithi: But I saw one ticket with to build like a fulfillment cost table.
[00:05:30] Vani Kaithi: But I guess that is dependent on this fluent advertiser and campaign one and the tremendous rewards table which Akli is working on.
[00:05:39] Trevor Anderson: So.
[00:05:40] Vani Kaithi: Yeah, I didn't find any other ticket
[00:05:43] Trevor Anderson: other than that GENIE space for. Let me. I'm just looking at 14 right now.
[00:05:52] Trevor Anderson: Launch genie space for Roas Data Explorer. That's part of a Q2 Roas.
[00:06:03] Vani Kaithi: Yeah, I guess do a story grooming on this. I can remember.
[00:06:09] Trevor Anderson: Yeah, we. Looks like this one was groomed though. It has story points and all that.
[00:06:13] Vani Kaithi: Okay. Okay. Jeez.
[00:06:24] Trevor Anderson: Have you created a genie space at all?
[00:06:27] Vani Kaithi: Oh, no, no.
[00:06:32] Trevor Anderson: What do we have right now? Do we even have. We have a few. We have two. Capill created both of them.
[00:06:42] Trevor Anderson: Ad flow metrics and gold customer.
[00:06:54] Trevor Anderson: The other one is validate fact partner revenue monthly output against finance report.
[00:07:00] Vani Kaithi: Yeah, that is my. I have created that ticket.
[00:07:04] Vani Kaithi: But for this they have to ingest all the contract terms before so that we can validate the partner revenue.
[00:07:16] Vani Kaithi: Yeah, that is dependent on that.
[00:07:20] Trevor Anderson: We didn't just.
[00:07:21] Vani Kaithi: What from the minion side the finance team or someone has to like ingest all the contract terms for all the.
[00:07:33] Vani Kaithi: For all the advertisers and for all the partners.
[00:07:38] Vani Kaithi: So based on that we have some logic to calculate the partner revenue which we built I guess around last year.
[00:07:47] Vani Kaithi: But still now we don't have all the contract terms in place which are coming from Minion, so.
[00:07:58] Trevor Anderson: Okay. Okay. There's another auction candidate.
[00:08:03] Trevor Anderson: This is update auction candidate metrics and add GENIE space. Do we have the auction?
[00:08:13] Trevor Anderson: Wasn't someone working on the fact table for this auction explainer?
[00:08:19] Vani Kaithi: I think mana worked previously. Not sure.
[00:08:25] Trevor Anderson: Thought it was like Venky's thing, but.
[00:08:30] Vani Kaithi: But I guess this is a thing to plan.
[00:08:36] Trevor Anderson: Yeah.
[00:08:47] Trevor Anderson: Archer, explain. This is supposed to be off of a table Fact auction candidate. Does that exist?
[00:08:57] Trevor Anderson: No idea. Okay. And then you said. Yeah, Fact fulfillment costs is blocked.
[00:09:08] Vani Kaithi: Yeah, yeah.
[00:09:10] Trevor Anderson: Upgrade central data code base. Okay, that's not. That's for the API.net. And we only have sprint 14.
[00:09:29] Trevor Anderson: Okay. There's something in the rest of the backlog. Well, it has been groomed in the backlog.
[00:09:41] Trevor Anderson: Migrate pia masking databricks jobs to GitHub workflows.
[00:09:57] Trevor Anderson: What is that
[00:10:01] Vani Kaithi: for the PI masking one?
[00:10:02] Trevor Anderson: Yeah, that's the sign that you.
[00:10:05] Vani Kaithi: Yeah, yeah, that was. Since it's like a one time thing. Previously we used to have a databricks job.
[00:10:14] Vani Kaithi: So what Bharat suggested was to have like a gate workflow so that we can run manually whenever we need it instead of maintaining a job in databricks.
[00:10:25] Vani Kaithi: That was that.
[00:10:29] Trevor Anderson: Is it because you have to. What do you mean? Can't you just run the job manually too?
[00:10:36] Vani Kaithi: Yeah, but since it's like one time thing, we thought of doing the git workflow like we just do it once and then we won't use that job.
[00:10:54] Vani Kaithi: And also that ticket is also to update for all the new tables as well to update with the PI tax.
[00:11:05] Trevor Anderson: How would a GitHub workflow work here? Like it's not.
[00:11:09] Trevor Anderson: Is it checking the data or doing anything like that?
[00:11:12] Vani Kaithi: No, no.
[00:11:12] Vani Kaithi: It just applies the tags to the PII columns and also apply the mask policies using git workflow.
[00:11:23] Vani Kaithi: Similar to what we did for Wyndham.
[00:11:27] Trevor Anderson: I don't remember Wyndham. Maybe that was before my time.
[00:11:30] Vani Kaithi: Yeah, for Wyndham also we have restricted the tables to only data engineering.
[00:11:37] Vani Kaithi: The whole tables using the git workflow we have.
[00:11:43] Vani Kaithi: Let me show you second,
[00:12:04] Vani Kaithi: For random data, we have created two new git workflows.
[00:12:10] Vani Kaithi: One is for creating tags and another is for like setting the data policies.
[00:12:18] Vani Kaithi: So for data policies it will first create a mask function and then using that function it will create a policy and apply to our central data catalogs.
[00:12:40] Vani Kaithi: So this mask policy will use the tags. Where is this?
[00:12:49] Vani Kaithi: Yeah, these tags are account level, which we define only like once.
[00:12:58] Vani Kaithi: It can be used in any environment.
[00:13:00] Trevor Anderson: So you set the tag at the table level is what you're doing.
[00:13:03] Vani Kaithi: Yeah, this is a column level, like PII columns.
[00:13:06] Trevor Anderson: Got it, got it. And how do you identify the PI?
[00:13:10] Vani Kaithi: We just did it manually for the PII masking one. This Wyndham is like whole table.
[00:13:16] Vani Kaithi: This is row level masking all rows we have.
[00:13:21] Vani Kaithi: So the user who is not a data engineering team will see any data.
[00:13:30] Trevor Anderson: They don't even see like a hashed version of it. What do you mean?
[00:13:34] Vani Kaithi: They won't able to see the sample data even like they won't see anything.
[00:13:39] Vani Kaithi: The whole table will be matched at a Table level.
[00:13:43] Trevor Anderson: The whole table.
[00:13:44] Vani Kaithi: Yeah, yeah.
[00:13:45] Trevor Anderson: Not. Not just a column.
[00:13:47] Vani Kaithi: Yeah, for. Yeah, for the Wyndham one, it should be given access only to the data engineering team.
[00:13:55] Vani Kaithi: That was the request from the SoC2. I guess.
[00:14:00] Trevor Anderson: I see. Okay. But for like other cases.
[00:14:03] Vani Kaithi: Yeah, for this PI masking task which is in backlog, that is for across all the table.
[00:14:10] Vani Kaithi: All the tables in the central data.
[00:14:13] Trevor Anderson: Okay.
[00:14:14] Vani Kaithi: Yeah, that also we have like work in progress, But I guess DevOps is creating a separate group so they.
[00:14:24] Vani Kaithi: They can maintain the group which has to. We have to give access.
[00:14:32] Vani Kaithi: So we have identified various tables with PII columns and in that columns it like zip.
[00:14:45] Vani Kaithi: It will be mass tagged with PII address location. So like this.
[00:14:52] Trevor Anderson: I see. And what is this? Does this just redact the information or does it.
[00:15:03] Vani Kaithi: Yeah, based on the masking function it redacts.
[00:15:09] Trevor Anderson: Does it just show redacted or does it show like a hash or.
[00:15:13] Vani Kaithi: Yeah, we have like mask stars for the string data it will be like stars for the string.
[00:15:18] Trevor Anderson: Okay.
[00:15:19] Vani Kaithi: For date format it will be like date format. Like 999 all.
[00:15:23] Trevor Anderson: Okay.
[00:15:25] Vani Kaithi: For email what we are doing is we are generating a SHA version of that email instead of masking.
[00:15:31] Trevor Anderson: Okay.
[00:15:32] Vani Kaithi: And for struct, it will be whole null because instruct, we can't define each and every field in that struct.
[00:15:44] Vani Kaithi: So for integer it's like minus one or something. Nine. Nine. I don't remember exactly.
[00:15:52] Trevor Anderson: I see.
[00:15:53] Vani Kaithi: Yeah. Based on the data type, we have the masking functions.
[00:15:59] Trevor Anderson: Okay. Is there anything blocking this ticket? Is this something? This is a Q3 tech initiative, so
[00:16:06] Vani Kaithi: there were some discussions with the DevOps team, DevOps team on this. So.
[00:16:12] Vani Kaithi: So right now, in future, if anyone needs access, data engineering team has to make some changes.
[00:16:19] Vani Kaithi: Right.
[00:16:20] Vani Kaithi: So instead of maintain doing like that, we thought of creating a separate group which will have the users only who we need to give the access.
[00:16:38] Vani Kaithi: Like. Like our service principles and all.
[00:16:40] Trevor Anderson: So we need a. We need a. Like a PII group or something.
[00:16:45] Vani Kaithi: Yeah, yeah.
[00:16:46] Trevor Anderson: Okay, that's something. That could be a sub task or that could be just an additional story. I don't.
[00:16:52] Vani Kaithi: Yeah, yeah, there is some ticket with DevOps, I guess on that.
[00:16:57] Trevor Anderson: There is, yeah.
[00:16:59] Vani Kaithi: So yeah, once it's done, we can work on this and we can apply the PI masking to the new tables as well.
[00:17:12] Trevor Anderson: Do you know what that ticket is?
[00:17:15] Vani Kaithi: No, it's way long back.
[00:17:20] Trevor Anderson: Okay. I don't. I don't think we. I don't think we. I don't think that's a blocker for this.
[00:17:30] Vani Kaithi: It's with Brand. Brand was checking with them, but yeah.
[00:17:42] Trevor Anderson: So what. What is this? What is that? Does it set the policy based on the group assignment as well? Okay.
[00:17:52] Trevor Anderson: It's associated to the group assignment.
[00:17:55] Vani Kaithi: Yeah.
[00:17:56] Trevor Anderson: So. So it allows data engineering group
[00:18:01] Vani Kaithi: access for pii. All account users. For all account users. It will be masked. Not just data engineering.
[00:18:09] Trevor Anderson: Okay. Is there a way to. Is. Would there be a way to see it or would it always be masked?
[00:18:19] Vani Kaithi: Yeah, for that only if anyone request access, they need to raise a request in okta and then DevOps will add those users to that group so that they can view all the PI columns.
[00:18:39] Vani Kaithi: Yeah, so that was the idea. So that Okta request and all. I guess DevOps is working on.
[00:18:52] Trevor Anderson: I don't know if they're working on it. Okay. Okay. We'd rather wait until confirms. Okay, that's fine.
[00:19:13] Trevor Anderson: Yeah. Let me see this request field.
[00:19:18] Trevor Anderson: Yeah, I don't think they added anything like that unless I remember.
[00:19:42] Trevor Anderson: Let me check the fdsos real quick. Okay. Does this describe the.
[00:19:57] Trevor Anderson: What you just talked about, this doc that you're sharing?
[00:20:05] Vani Kaithi: This one, The.
[00:20:10] Trevor Anderson: Yeah. Does this go over kind of what you were just describing?
[00:20:14] Vani Kaithi: Yeah. Yeah.
[00:20:14] Trevor Anderson: Okay.
[00:20:15] Vani Kaithi: How we define the tags and policies, the implementation. There was some document as well for DevOps.
[00:20:32] Vani Kaithi: What needs to be done on this?
[00:20:36] Trevor Anderson: I don't see fds. I'm looking it up. Okay.
[00:21:00] Vani Kaithi: It.
[00:21:31] Trevor Anderson: Yeah. I don't know what else has been groomed here. I'm looking. Oh, update permissions. Was that.
[00:21:43] Trevor Anderson: It's from Sumi.
[00:21:51] Vani Kaithi: It.
[00:22:26] Trevor Anderson: Do you have any suggestions? We're going through all these right now.
[00:22:33] Vani Kaithi: Yeah, I didn't see these backlog.
[00:22:47] Vani Kaithi: I think all this needs to be groomed.
[00:23:16] Trevor Anderson: Yeah. Let's see. I'm. I don't think I have any stories ready for tech initiatives either.
[00:23:27] Vani Kaithi: I was just going through this building fact fulfillment cost, the design.
[00:23:33] Trevor Anderson: Yeah.
[00:23:33] Vani Kaithi: Since right now we have idea how the Advertiser Fluent campaign and the rewards table looks like.
[00:23:41] Trevor Anderson: So.
[00:23:42] Vani Kaithi: Yeah, just looking into that.
[00:23:44] Trevor Anderson: Okay. Just getting prepared for it.
[00:23:47] Vani Kaithi: Yeah. Yeah.
[00:23:48] Trevor Anderson: Until we have the. The available source ready.
[00:23:53] Vani Kaithi: Yep.
[00:23:54] Trevor Anderson: Okay. Sure. We'll get some more direction next week on these things as well.
[00:24:14] Trevor Anderson: Wonder who's going to work on these genie spaces though.
[00:24:17] Trevor Anderson: Looks like Brian's the same like both of them though. Okay. And what this fulfillment.
[00:24:30] Trevor Anderson: Cause so this is all playful or tremendous.
[00:24:35] Vani Kaithi: Yeah. Right now we have from playful also we have this fulfillment cost.
[00:24:47] Vani Kaithi: Do we combine and make one fulfillment double or.
[00:24:51] Vani Kaithi: I guess this is just related to tremendous since it's a rewards.
[00:24:58] Trevor Anderson: Well, tremendous fulfillment. Right. Is Based off of Lead manager.
[00:25:03] Vani Kaithi: Yes.
[00:25:08] Trevor Anderson: And I think Playful is this playful subset of the lead manager.
[00:25:16] Trevor Anderson: Like it's like there are some playful campaigns and managing lead manager, but lead manager also has other stuff as well.
[00:25:26] Trevor Anderson: Outside of playful. I didn't know Playful had a. Had a fulfillment.
[00:25:39] Vani Kaithi: Yeah. The user wise tables which I have ingested.
[00:25:43] Trevor Anderson: Oh, the new ones here.
[00:25:45] Vani Kaithi: Yeah. In that I see the fulfillment cost for the events.
[00:25:50] Trevor Anderson: Oh, interesting. I wonder if those match to what's in Tremendous.
[00:26:00] Vani Kaithi: Is Tremendous and Playful separate, right?
[00:26:04] Trevor Anderson: They are, but isn't Tremendous? I thought they were related.
[00:26:12] Trevor Anderson: Okay, I want Fulfillment was all related to the gaming and like gaming industry.
[00:26:19] Trevor Anderson: Like, like when someone cashes their rewards. Right. That's like the fulfillment cost.
[00:26:28] Vani Kaithi: Okay. Okay. Yeah. In this place it's all gaming related.
[00:26:37] Trevor Anderson: For other stuff. No, I think Tremendous covers other stuff too.
[00:26:43] Trevor Anderson: Because I remember Brian saying something about like Capital One too as well.
[00:26:51] Trevor Anderson: Like Tremendous is used to do the fulfillment of Capital One payouts.
[00:26:56] Vani Kaithi: Okay. Okay. Yeah. I guess Akhil has already just had that rewards thing.
[00:27:08] Trevor Anderson: But I do wonder if if Tremendous has like campaign information, if those come up.
[00:27:17] Trevor Anderson: Like if we're able to see the same values.
[00:27:21] Trevor Anderson: Like, like fulfillment values and like I don't know if there's any overlapping campaigns and either in both Tremendous and this app Squire data.
[00:27:33] Vani Kaithi: Okay. Okay. Yeah, I, I'm curious about that.
[00:27:38] Trevor Anderson: I need to look into more about like what these, what the fulfillments are actually for. Not.
[00:27:44] Trevor Anderson: Yeah, like what Tremendous is fulfilling. Like what, what data source is it fulfilling? Okay.
[00:27:58] Trevor Anderson: All right. Well enjoy the, the quiet while it's here.
[00:28:04] Trevor Anderson: Probably pick up soon as we as we kick off Q3, but looks like there's only a few like strangling or straggling items for Q2, but it looks like we're ready for Q3.
[00:28:20] Vani Kaithi: Yeah, I was looking forward to all this. How this cbur things gets. Yeah.
[00:28:32] Vani Kaithi: There are so many like fact tables and the metrics which we are building.
[00:28:37] Trevor Anderson: I know, but it's a good design. I.
[00:28:40] Trevor Anderson: I'm glad that we're doing this whole like having a centralized, fluent, wide dimensional tables.
[00:28:49] Trevor Anderson: I think it's so much easier from a business standpoint because.
[00:28:53] Trevor Anderson: Yeah, for reporting you don't have to do all the joining and everything.
[00:28:56] Trevor Anderson: You just have like these tables that have everything in it.
[00:28:59] Vani Kaithi: Yeah, yeah, yeah.
[00:29:01] Vani Kaithi: Instead of previously we used to have like the minion reports and the power BI reports and we are different.
[00:29:08] Vani Kaithi: Yeah.
[00:29:10] Trevor Anderson: Yeah. I'm glad that we're going in this direction, but still a lot of work to be done for that.
[00:29:14] Vani Kaithi: But yeah.
[00:29:16] Trevor Anderson: Okay. All right. Well, have a great rest of your Friday.
[00:29:21] Trevor Anderson: Yeah, I guess you're just going to look into that fulfillment costs day model and that sounds good to me.
[00:29:31] Trevor Anderson: But thanks for, I guess, getting through those two items pretty quickly.
[00:29:37] Vani Kaithi: Yeah, no, it's true.
[00:29:39] Trevor Anderson: I'm glad that both of those look good, though.
[00:29:41] Vani Kaithi: Yeah.
[00:29:43] Trevor Anderson: Awesome. Well, have a great weekend.
[00:29:45] Vani Kaithi: Happy Friday.
[00:29:47] Trevor Anderson: Thank you.
[00:29:48] Vani Kaithi: Bye.
[00:29:49] Trevor Anderson: Bye. Bye.
