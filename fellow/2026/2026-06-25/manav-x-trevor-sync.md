---
title: "Manav x Trevor Sync"
meeting_id: "040000008200E00074C5B7101A82E00807EA0619A5440CF490DDDC01000000000000000010000000AB76D0CA659EAC419E1852FA011E20DB"
note_id: 80919070
recording_id: 15102629
date: 2026-06-25
start_time: "2026-06-25T11:30:00-04:00"
end_time: "2026-06-25T12:00:00-04:00"
url: "https://fluentco.fellow.app/meetings/040000008200E00074C5B7101A82E00807EA0619A5440CF490DDDC01000000000000000010000000AB76D0CA659EAC419E1852FA011E20DB/"
participants:
  - name: "Trevor Anderson"
    email: "tanderson@fluentco.com"
    rsvp: accepted
    organizer: true
  - email: "mpaul@fluentco.com"
    rsvp: notResponded
    organizer: false
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
[ ] Manav Paul to confirm with Virginia and TransUnion whether the hash-only data approach (no PII) is viable for proper audience performance on the Data Marketplace.
[ ] Manav Paul to check with Brian and Dan on file format preference (one file per segment vs one row per user), and if they have a strong preference, confirm feasibility with TransUnion, LiveRamp, and Lotame.
[ ] Manav Paul to confirm the minimum segment eligibility threshold with Brian and Dan (current: 1 million records, proposed: 5,000 records).
[ ] Manav Paul to follow up with Brian and Virginia on open questions from the segment registry review once they return from time off.
[ ] Manav Paul to create Confluence documentation on standardized JIRA ticket format for audience solutions, including required fields for destinations, credentials, rate, and cost (coordinate with Grant for modeled audience compatibility).

## AI Summary

Trevor and Manav conducted a technical review of the **segment registry project**, working through open questions and design decisions from Joseph's analysis. The discussion covered key architectural choices including using the **mailable universe table** for data selection, implementing a **hash-only approach** (no PII) to minimize data sharing with TransUnion, and storing all segment definitions in a **single YAML file** rather than separate files per segment. The team is moving toward eventually cutting out LiveRamp and TransUnion entirely to perform modeling in-house, as TransUnion currently takes a significant revenue cut from modeled audiences.

Several decisions require follow-up confirmation with Brian and Dan, including the hash-only data approach, file format preferences, and minimum segment eligibility thresholds. Manav will also create a **standardized JIRA ticket format** with required fields to improve workflow consistency. Additional discussion covered data governance, with Trevor working on creating a read-only access group for audience solutions analysts and identifying which schemas need migration to production.

### Chapters

#### Data Privacy Strategy
- The segment registry requires **hash-only values with no PII**, which contradicts current practices but aligns with Fluentco's strategy to minimize data sharing with TransUnion and prevent them from reconstructing Fluentco's identity graph. *(0:02:20)*
- TransUnion actively seeks to obtain Fluentco's data and gradually build their own version of the graph, prompting the company to limit data exposure to only what's necessary (hashed emails). *(0:02:44)*
- Fluentco's long-term goal is to **cut out LiveRamp and TransUnion entirely** and perform all modeling in-house using their own identity graph, which leadership believes is sufficiently large to support independent modeling. *(0:04:45)*
- TransUnion currently takes a **significant revenue cut** from all modeled audiences, providing additional financial incentive for Fluentco to bring modeling capabilities in-house, though the implementation timeline is uncertain. *(0:05:27)*

#### Segment Registry Technical Architecture
- The team reviewed Joseph's technical analysis document which identified gaps and open decisions requiring clarification, with several items stemming from unawareness of existing data infrastructure rather than genuine technical issues. *(0:06:04)*
- All segment definitions will be stored in a **single YAML file** that gets updated incrementally, rather than creating 2,000+ individual files, with jobs reading from this centralized configuration. *(0:15:30)*
- The system will use a **two-job architecture**: a weekly job (Sunday nights) to refresh all existing audiences, and a daily morning job to immediately build newly requested segments without forcing requesters to wait until the weekly refresh. *(0:16:32)*
- Trevor suggested adding a **database table to mirror the YAML file** for reporting purposes, allowing Virginia and the team to easily query segment counts, track new segments over time, and generate analytics without parsing code files. *(0:17:23)*

#### File Format & Data Structure
- The proposed approach uses **multiple files per segment** (1,200 segments = 1,200 files) versus the current approach of one file per vendor with all users and their segment memberships, requiring confirmation that this change is necessary before adding vendor friction. *(0:10:05)*
- The **minimum segment eligibility threshold** needs confirmation: current threshold is 1 million records (below which segments get routed to modeled audience pipeline), while the proposed approach suggests 5,000 records, representing a dramatic 200x reduction. *(0:12:47)*
- The new routing capability will allow segments below the threshold to be **automatically routed to the modeled audience pipeline** instead of the declared audience pipeline, creating a smooth workflow for handling smaller segments. *(0:13:21)*

#### JIRA Workflow Standardization
- The current JIRA board structure is disorganized with **tickets created anywhere and everywhere** after Aaron left the team, with the weekly standup consisting mainly of status updates rather than structured ticket management. *(0:21:48)*
- Standardized ticket format is needed with **required fields** for rate, cost, destinations, and credentials (currently these values are thrown into single fields), especially important with a new account manager starting next week. *(0:21:56)*
- Manav will create a Confluence document detailing the proper JIRA ticket creation process, coordinating with Grant to ensure the format works universally for both declared and modeled audiences. *(0:22:44)*

#### Data Governance & Access Control
- Trevor is creating a **read-only persona group** for audience solutions analysts to access necessary data assets, with contractors Monisha and Shaml only needing query access to extract parameters and download results. *(0:26:00)*
- The contractors only need access to **two tables** (survey_event and users) in the audience_relations_dev.master_marketable schema for their seed building work, with the rest of their workflow happening in separate tools. *(0:26:47)*
- The dev workspace contains **many unused schemas from Kurt's old setup** that likely aren't needed, with Grant currently auditing which schemas should be migrated to production and which can be deprecated. *(0:29:07)*
- Virginia is currently over-privileged as part of the product owners group with access to bronze and dev environments, while the new governance model will provide more targeted, role-appropriate access levels. *(0:28:27)*

### Action Items
- Manav Paul: Manav Paul to confirm with Virginia and TransUnion whether the hash-only data approach (no PII) is viable for proper audience performance on the Data Marketplace.
- Manav Paul: Manav Paul to check with Brian and Dan on file format preference (one file per segment vs one row per user), and if they have a strong preference, confirm feasibility with TransUnion, LiveRamp, and Lotame.
- Manav Paul: Manav Paul to confirm the minimum segment eligibility threshold with Brian and Dan (current: 1 million records, proposed: 5,000 records).
- Manav Paul: Manav Paul to follow up with Brian and Virginia on open questions from the segment registry review once they return from time off.
- Manav Paul: Manav Paul to create Confluence documentation on standardized JIRA ticket format for audience solutions, including required fields for destinations, credentials, rate, and cost (coordinate with Grant for modeled audience compatibility).

### Decisions
- The segment registry will use the **mailable universe table** as the data source, as it's the only viable option for the required functionality.
- The pipeline will use **post-FTC data** and leverage the mailable universe table's built-in checks for underage users and suppression lists.
- The new approach will **join to the user table** to properly capture demographic and geographic data, as 60% of the catalog requires this information.
- All segment definitions will be stored in a **single YAML file** rather than creating separate files per segment, with the file being updated incrementally as new segments are added.
- A **two-job architecture** will be implemented: a weekly refresh job (Sunday nights) for existing audiences and a daily morning job to build newly requested segments without making requesters wait until Sunday.

## Transcript

[00:00:01] Trevor Anderson: So you know, on this nuq,
[00:00:23] Manav Paul: Set it over it.
[00:01:02] Trevor Anderson: So this. Is only. So this is. Are we doing the whole segment registry in the. In that repo?
[00:01:17] Manav Paul: This repo I have. They asked me to make another segment
[00:01:21] Trevor Anderson: registry a new repo.
[00:01:22] Manav Paul: Yeah, I made one for it.
[00:01:40] Trevor Anderson: It's just called Segment Registry.
[00:01:42] Manav Paul: Yeah.
[00:01:45] Trevor Anderson: Yes. So Joseph left some, he did some digging on the segment registry. I think he left some questions.
[00:01:52] Trevor Anderson: I'm not sure if he showed you the Confluence stock on it.
[00:01:55] Manav Paul: I saw that he had his comments. I can have him taking a look at it.
[00:02:01] Trevor Anderson: He did a. He wrote up a summary doc which helps, but. Okay.
[00:02:04] Trevor Anderson: There's these gaps and open decisions that are covered in the conversation.
[00:02:09] Trevor Anderson: Well, this is probably easier to look at these and answer these, but this is probably something that.
[00:02:15] Trevor Anderson: Well, let's just go through some of the stuff real quick because I want to hear your thoughts on some of these things.
[00:02:20] Trevor Anderson: Sure. So in the segment registry it does call for hash only values, no pii, which is.
[00:02:29] Trevor Anderson: Which just have pointed out, which is kind of a contradiction from what we do currently. Right.
[00:02:34] Manav Paul: Yeah.
[00:02:36] Manav Paul: I think the main approach because I was talking to Brian is they want to avoid sending any data to like TransUnion that they don't have to send.
[00:02:44] Manav Paul: I guess the reason being is because TU is going to always want our data and want to basically slowly construct our graph on their end.
[00:02:51] Manav Paul: They're actually like I guess actively trying to get rid of TU altogether.
[00:02:54] Manav Paul: So they're just trying to send them the least amount of data as necessary.
[00:02:58] Manav Paul: So I guess that's why they wanted to go this route as opposed to sending all that PI we were sending before.
[00:03:04] Manav Paul: Like the, the email hashes should be enough essentially and they want to go with that route.
[00:03:12] Trevor Anderson: Is that fine on the vendor side or the vendors looking to have.
[00:03:16] Manav Paul: It shouldn't matter on there. And they're only going to get hashed emails anyway.
[00:03:21] Trevor Anderson: Oh, eventually.
[00:03:23] Manav Paul: Yeah. Like the, the actual
[00:03:27] Trevor Anderson: model like on the Data marketplace.
[00:03:29] Manav Paul: Yeah, I think it's just hasht emails on there and I can double check though I have no visibility as to what it looks like on that side.
[00:03:39] Manav Paul: But I can, I can contact one of the TU guys and double check.
[00:03:43] Manav Paul: But yeah, we had brought, we had brought this up to Brian. That's what his reasoning was.
[00:03:47] Trevor Anderson: So why don't, why do right now we deliver pii?
[00:03:52] Manav Paul: I guess this is just how we've been doing it because of like how Kurt was doing it.
[00:03:56] Manav Paul: I guess they realized this approach is not great because then we're sending them so much of our Data and they just want to go ahead and they're using.
[00:04:03] Manav Paul: Yeah. Because even we had that issue. Remember when I was that balancing script that I wrote for the.
[00:04:10] Manav Paul: The query, the extraction query and.
[00:04:13] Trevor Anderson: Yep.
[00:04:14] Manav Paul: You know, Virginia asked to you like, can we send in more data than the recommended 25k?
[00:04:19] Manav Paul: And they were like, obviously they were like, absolutely.
[00:04:21] Manav Paul: Because they rather have as much data from us as they can.
[00:04:25] Manav Paul: And I guess, yeah, Fluent's not wanting to do that for those reasons.
[00:04:30] Manav Paul: They want to keep their dad to themselves. Sounds like.
[00:04:32] Manav Paul: And I don't think they're really happy with tu it looks like either. So.
[00:04:38] Trevor Anderson: Yeah, that's what it seems. But don't we need them for modeling our data?
[00:04:45] Manav Paul: I was talking to Brian. I think his idea is eventually we're going to try and do it all in house.
[00:04:50] Manav Paul: Cut out Live Ramp, cut out transunion, do the modeling on our side. And how are we gonna.
[00:04:54] Trevor Anderson: How are we gonna extrapolate? Are we gonna use.
[00:04:57] Manav Paul: I guess they're saying our Fluent identity graph is big enough where we don't need to use data, so we're going
[00:05:02] Trevor Anderson: to model using our.
[00:05:04] Manav Paul: Our own data.
[00:05:05] Trevor Anderson: Yeah, Experian data.
[00:05:07] Manav Paul: I really have no idea on that too much.
[00:05:10] Manav Paul: It's just an idea Brian had brought up during that collaboration week.
[00:05:13] Manav Paul: But it sounds like that's what they're hoping to achieve where we can cut out.
[00:05:17] Manav Paul: Because Live Ramp also, we have issues with Live Ramp and all kinds of other sectors that the things they do with us.
[00:05:24] Manav Paul: So I know they want to cut them out for sure. And I guess TU as well.
[00:05:27] Manav Paul: Because also TU takes such a gigantic cut out of all model audiences because they're using their data, all the revenue from that.
[00:05:33] Manav Paul: So I guess if we can do it in house, they're going to want to do that.
[00:05:36] Manav Paul: I don't know when that's hoping to be implemented, but that's just an idea that Brian was bringing up that I guess they're looking into.
[00:05:44] Trevor Anderson: Okay, Let's have more to investigate. Figure out. Not sure about those. Yeah.
[00:05:57] Trevor Anderson: These conversations, but hear about them through. Through you guys
[00:06:03] Manav Paul: for this one. Yeah.
[00:06:04] Manav Paul: I think this is just because the document that he was reviewing is basically I took his shirt off of what I read from all the epics.
[00:06:13] Manav Paul: I think a lot of it is kind of just, you know, just from being unaware.
[00:06:18] Manav Paul: Like I don't think this table, what Joseph is saying.
[00:06:20] Manav Paul: I think this is the right approach to use the correct table.
[00:06:22] Manav Paul: I think the table that was given in one of the. The epics that was defined there.
[00:06:26] Manav Paul: I think that is the wrong table and they just didn't know that.
[00:06:28] Manav Paul: So I think for this one we can go with a table that he was mentioning because we obviously can't.
[00:06:34] Manav Paul: We need to use the available universe. So.
[00:06:36] Trevor Anderson: Yeah. Yeah. I think a lot of these. Wait, so.
[00:06:40] Manav Paul: Because I didn't. Yeah, I didn't give my own like scrutiny on anything yet.
[00:06:43] Manav Paul: That doc was purely just what I put together from the tickets and then what they were their ask and then.
[00:06:50] Manav Paul: Yeah, this is what we really need to do is go through it now and actually from, you know, the.
[00:06:55] Manav Paul: The data engineering from audience solutions side be like, all right, this is.
[00:06:58] Trevor Anderson: So let's make those decisions. Let's do that.
[00:07:00] Manav Paul: Yeah. Yeah. So I agree on this one. It should be Mal Universe. I mean there's no really way around it.
[00:07:08] Manav Paul: It has to be. And these. Like I said, I think there's just nuances from not being aware on the.
[00:07:23] Manav Paul: The higher upside.
[00:07:24] Trevor Anderson: Yeah.
[00:07:25] Manav Paul: Doesn't actually affect the actual pipeline. Yeah.
[00:07:38] Trevor Anderson: So this one we need to use post ftc. Yeah.
[00:07:43] Manav Paul: And I guess that mailable universe table, they have also checks if they're underage.
[00:07:49] Trevor Anderson: Yeah, underage. Any other suppressed.
[00:07:52] Manav Paul: Yeah, so they have. They have a table right now that they use. So we'll just use that table.
[00:07:57] Trevor Anderson: Okay. This needs to be confirmed with Virginia probably. Yeah.
[00:08:06] Manav Paul: I'm also gonna double check with I guess tu see what they actually receive when somebody buys our data from the.
[00:08:15] Manav Paul: Or buys our audience from the marketplace.
[00:08:18] Trevor Anderson: Okay. Is this something you can confirm for us?
[00:08:21] Manav Paul: Yeah, I can ask Virginia and I'll double check because I know Brian said he doesn't want to send in more than.
[00:08:26] Manav Paul: But it's a. I. I'll work with Virginia and double check and see what she's thinking about it.
[00:08:31] Manav Paul: But I, I know Brian's answer is he only wants to send the hashes.
[00:08:36] Trevor Anderson: I know that's the. The path, but is that possible?
[00:08:39] Manav Paul: Yeah, that's why I'll check with. I'll check with to you and see as well if that makes sense for.
[00:08:44] Manav Paul: For getting the proper results from our audiences because the audiences aren't going to perform properly and there's no point in worrying about if we send them our data or not.
[00:08:59] Trevor Anderson: Demographic and geographic segments.
[00:09:07] Trevor Anderson: The new approach reading 60% of our catalog is demographic and geographic or some hybrid of the two.
[00:09:15] Trevor Anderson: The new approach reading only survey table cannot express these.
[00:09:20] Manav Paul: Yeah, I agree with this as well. The approach to join the user table I don't see an issue with.
[00:09:25] Manav Paul: It makes sense to make sure we have the proper demographic data and geographic data.
[00:09:31] Manav Paul: So I agree with that.
[00:09:34] Trevor Anderson: User table. New approach must join to the user table.
[00:10:00] Trevor Anderson: Okay,
[00:10:05] Manav Paul: Yeah, for this one I will.
[00:10:08] Manav Paul: First of all I'm going to double check with probably Brian and Dan and see if there's a like specific reason why they want it one file per segment as opposed to the other, the one row per user.
[00:10:18] Manav Paul: If there's even like a reason or that was just the approach they wrote.
[00:10:20] Manav Paul: If they don't care, then I'll just go with the current.
[00:10:23] Manav Paul: If they do care then I'll go ahead and contact tu, Libramp and lot of man, see if you know, if this is possible and then because this will affect our vendors and the way they receive our data.
[00:10:33] Manav Paul: So unless there's a real hard set reason why they want to go with the new route, then I will stick to how we do it now.
[00:10:42] Manav Paul: So I'll work with. Yeah, I'll confirm with them.
[00:10:53] Trevor Anderson: I was confused by this one because right now it's user based. Right.
[00:11:00] Trevor Anderson: And we deliver a set of users in those users, we show what segments they're a part of. Right?
[00:11:07] Manav Paul: Yeah.
[00:11:08] Trevor Anderson: But
[00:11:11] Manav Paul: I guess the proposed pipeline, multiple files per segment. I guess that a one file with
[00:11:17] Trevor Anderson: all the segments, multiple files. What do you mean by that?
[00:11:22] Manav Paul: Like the new approach instead of.
[00:11:24] Manav Paul: I think currently we do one file to tu, one file, a lot of main one file to live ramp and that file will have every user and it'll specify what segment they're for.
[00:11:33] Manav Paul: This one separates the file per segment. So if there's 1200 segments, there's 1200 files.
[00:11:43] Manav Paul: So unless there's a real reason why they want to do it that way, I don't see why we should change it.
[00:11:50] Manav Paul: You know what I mean?
[00:11:51] Trevor Anderson: Okay. I think, I think. Well this was up to us on how we wanted to organize, you know, our delivery.
[00:12:01] Trevor Anderson: I don't, I don't think it's. This was a requirement on the vendor side.
[00:12:07] Manav Paul: No, I mean on the like I wanna, I wanna confirm a Brian and, and. Yeah.
[00:12:10] Trevor Anderson: Oh, okay. Yeah, yeah.
[00:12:12] Manav Paul: The vendors, they're not even knowing we're gonna change it.
[00:12:14] Manav Paul: I don't, I, Like I said, I don't think we should change it at all if we don't have to because then this just adds more friction on their end and we don't want to do that on the vendor's end if we don't.
[00:12:47] Trevor Anderson: Okay. What is this one? Minimum segment eligibility.
[00:12:55] Manav Paul: Yeah, for this one I'm 1 million
[00:12:59] Trevor Anderson: records for declared audience.
[00:13:02] Manav Paul: Yeah. Right now that's our current threshold.
[00:13:06] Manav Paul: So I'm gonna probably like I said confirm Brian and Dan the same way and see if there's a reason why they want to change it.
[00:13:14] Manav Paul: If not, then I'll stick with the 1 million because that's what we do now.
[00:13:16] Manav Paul: Plus, we're gonna be able to do a, you know, have a job that will route it.
[00:13:21] Manav Paul: So if it's less than a million, we can just route it straight to the model audience pipeline instead of the DA pipeline.
[00:13:26] Manav Paul: So.
[00:13:27] Trevor Anderson: Okay, so we just need a confirmed floor.
[00:13:30] Manav Paul: Yeah. If this is the floor they want to go with for whatever reason, I'll
[00:13:34] Trevor Anderson: confirm that's a high floor.
[00:13:37] Manav Paul: Yeah, big difference.
[00:13:38] Trevor Anderson: It's like 5,000 right now.
[00:13:40] Manav Paul: No, no, right now it's 1 million. The new approach is 5,000. Oh, yeah.
[00:13:48] Manav Paul: So right now, if it's less than a million, we make it modeled.
[00:13:51] Trevor Anderson: Oh, I see. Maybe this is just outdated.
[00:13:56] Manav Paul: Yeah. Actually, I think this might even be like. I don't know if the number matches what?
[00:14:02] Manav Paul: I'll have to ask them. I'll have to see. Because right now, because, you know, we model the data.
[00:14:06] Manav Paul: If we don't have enough, if we have a million records and usually, or plus we're like, okay, we can work with that and just send that straight to the vendors, but otherwise, if it's less, we have to model it and turn into a bigger segment.
[00:14:16] Trevor Anderson: Got it.
[00:14:18] Manav Paul: But that's what's good about this.
[00:14:19] Manav Paul: The ability to just like be able to route it to the correct pipeline. Everything will be very smooth.
[00:14:28] Trevor Anderson: Okay, this one, I think this is
[00:14:30] Manav Paul: the same issue as before.
[00:14:32] Trevor Anderson: No. So this one was regarding our repo structure.
[00:14:36] Manav Paul: Oh, okay.
[00:14:36] Trevor Anderson: Okay. On. Was it here? No, it was under segments. So I think this one was regarding.
[00:14:47] Trevor Anderson: Well, let me just read. It stores one definition file per segment. So the number of file.
[00:14:52] Trevor Anderson: So it grows literally in the subfolder here.
[00:14:56] Manav Paul: So they saying that every segment gets its own file, right?
[00:15:00] Trevor Anderson: Yeah.
[00:15:01] Manav Paul: I think this is. Might have just been a test because this is the current. The current audiences.
[00:15:08] Manav Paul: I think that's just a test because the new one should just write to the same YAML file each segment.
[00:15:14] Manav Paul: It shouldn't be a new file each time. This is just the current segments we have.
[00:15:18] Manav Paul: Remember the whole canary test situation? That's off of this.
[00:15:21] Trevor Anderson: Yeah. So how we're going to be handling that?
[00:15:24] Manav Paul: I think that's why there's three here, because I just added those three to test. Yeah.
[00:15:28] Trevor Anderson: But if we have 2,000. Yeah.
[00:15:30] Manav Paul: It should be one file.
[00:15:34] Trevor Anderson: What do you mean by that?
[00:15:35] Manav Paul: It should just be one file that will have all these on it.
[00:15:39] Trevor Anderson: Like okay.
[00:15:40] Manav Paul: Like one giant file.
[00:15:42] Trevor Anderson: It'll be like an array of.
[00:15:44] Manav Paul: Yeah, we'll just keep writing to that file every time. So that's how I'm going to handle.
[00:15:47] Manav Paul: Like, for example, if, let's say Virginia is the one that makes a JIRA ticket, and our job will pick up the JIRA ticket, reads through it, and then it'll Write to the 1yaml file with all the information required, and then our job will then go pick it up from the YAML file.
[00:16:02] Manav Paul: It'll notice that there's a new one. It'll build it.
[00:16:05] Trevor Anderson: I see. Okay. So. So it's not going to have to parse through.
[00:16:08] Manav Paul: No misunderstanding. I can double check. Maybe I wrote it wrong in the.
[00:16:14] Trevor Anderson: I think so, because that's how I picked it up here.
[00:16:16] Manav Paul: Yeah, but that, that won't be the case.
[00:16:22] Trevor Anderson: Also, what happens if a segment is updated? If.
[00:16:26] Trevor Anderson: If, like, this one is updated, how would that logic be handled?
[00:16:30] Manav Paul: Because, like, if there's a change in you.
[00:16:32] Trevor Anderson: Yeah.
[00:16:32] Manav Paul: Okay. So there. I'm gonna have two jobs, most likely. One will be the weekly job.
[00:16:39] Manav Paul: This will refresh all the audiences that currently exist.
[00:16:41] Manav Paul: So it's going to go through this YAML file each time. So there's a change. It'll update it anyway.
[00:16:46] Trevor Anderson: Okay.
[00:16:47] Manav Paul: Every Sunday night, if there. I also have one job that checks every morning if there's any new.
[00:16:54] Manav Paul: Yeah, so then the logic with that is, you know, you don't want to build out, you know, thousands of segments every single morning and refresh them for no reason.
[00:17:02] Manav Paul: Just build the ones that are new.
[00:17:03] Manav Paul: Because if, let's say Virginia makes a request, I'm not going to make her wait until Sunday night for it to be built.
[00:17:08] Manav Paul: So that'll handle the new segments, which will be on this file, and then the ones that are already created, that refresh will happen every Sunday night.
[00:17:15] Trevor Anderson: Let's see. Okay.
[00:17:16] Manav Paul: Yeah, so it shouldn't matter. There's a change to this because it's going to check it every.
[00:17:21] Manav Paul: It's going to check it on Sunday anyway.
[00:17:23] Trevor Anderson: Okay.
[00:17:23] Trevor Anderson: My, my suggestion enhancement for this, not, not for this, that process that you just described, but in conjunction with it, would be to have a table that also has the same information in it.
[00:17:36] Manav Paul: Oh, okay, I understand.
[00:17:38] Trevor Anderson: Like a table. Like each of these is a column. Right. And we're able to track the.
[00:17:45] Trevor Anderson: All the active segments or new segments.
[00:17:47] Manav Paul: Yeah, I, I agree. That'll be good. Especially for reporting purposes, too.
[00:17:51] Trevor Anderson: Yeah, exactly.
[00:17:52] Manav Paul: Virginia just wants to go see what segments.
[00:17:54] Trevor Anderson: How many, like, how many segments do we have? Right.
[00:17:57] Trevor Anderson: Or how many new ones in the past month or how many, you know, that could be used for reporting.
[00:18:03] Manav Paul: I agree. Yeah. Because this, this, you know, this is just for the code to handle for.
[00:18:06] Trevor Anderson: Yeah, you don't, you, you don't want as a person to go through that.
[00:18:10] Manav Paul: Yeah, yeah, exactly.
[00:18:12] Trevor Anderson: Okay, let me just comment that then.
[00:18:54] Manav Paul: Just give me one second. I'll be right back.
[00:18:56] Trevor Anderson: Yeah, It.
[00:19:37] Manav Paul: All right. Back.
[00:19:53] Manav Paul: It.
[00:20:18] Trevor Anderson: Okay. I think that addresses this.
[00:20:20] Trevor Anderson: So a few follow ups that need to happen but looks like we're able to answer some of those questions so it's good.
[00:20:35] Trevor Anderson: Yeah, just let me know how those follow ups go. Brian's out though, so I'll let you
[00:20:43] Manav Paul: know as soon as.
[00:20:43] Trevor Anderson: Oh, he's back on Monday.
[00:20:45] Manav Paul: Yeah, I'll be out Monday though, so when I get back. So I went onto the follow up.
[00:20:50] Trevor Anderson: You're out all next week? No.
[00:20:52] Manav Paul: Yeah. Yeah.
[00:20:54] Trevor Anderson: Okay.
[00:20:55] Manav Paul: Well I can, I can let him know now and then have. I can let Joseph know to pick it up or something.
[00:21:01] Trevor Anderson: Let him know now if there's anything that Virginia can answer to let. Or ask her now as well.
[00:21:07] Manav Paul: She's out though, so she won't give any information now.
[00:21:10] Trevor Anderson: Oh, she's out this week.
[00:21:12] Manav Paul: Yeah, she kind of is responding but it's. I'll ask her anyway. I'm just gonna ask just in case.
[00:21:20] Trevor Anderson: Sure.
[00:21:20] Manav Paul: Yeah.
[00:21:21] Trevor Anderson: Yeah. And then something else I was curious about popped up this week on the, on their JIRA board.
[00:21:32] Manav Paul: Yeah.
[00:21:33] Trevor Anderson: Well one we have no way to mod.
[00:21:35] Trevor Anderson: Like I was trying to figure out who the admin is for this because I think in this.
[00:21:39] Trevor Anderson: In the segment registry we want there. There is fields that we want to make sure that we have.
[00:21:48] Manav Paul: Yeah, they're gonna have to be standardized because then also it'll be easier for when for example they're gonna have a new account manager starting I think next week.
[00:21:56] Manav Paul: So yeah, having a standardized way of making tickets is going to be helpful for.
[00:22:02] Trevor Anderson: Do we have the format that we, that we want?
[00:22:07] Manav Paul: I have like a basic one.
[00:22:08] Manav Paul: I'm going to probably work with Grant too because I want it to be universal for modeled as well.
[00:22:13] Manav Paul: And then I can send it over what we're thinking.
[00:22:15] Manav Paul: But it's going to be similar to the ones we have now except like for example the rate and the cost.
[00:22:19] Manav Paul: They have to be a field for each of the values can't just be thrown into one.
[00:22:24] Trevor Anderson: Yes.
[00:22:25] Manav Paul: And then there's going to have to be a few extra values. For example like for the destinations.
[00:22:30] Manav Paul: I'm going to have a field for each destination, each Credential and these need
[00:22:34] Trevor Anderson: to be required fields. Yeah. Okay.
[00:22:37] Trevor Anderson: Can you, can we have a confluence doc of what, what we want the tickets to look like?
[00:22:44] Manav Paul: Yeah, I, I will. Once I get that format sorted, I'll put it in there. I'll.
[00:22:48] Manav Paul: I'll make a whole doc on how to properly create a ticket.
[00:22:52] Trevor Anderson: Okay. Do you need a actual item for that? A tier item?
[00:22:57] Manav Paul: Sure. Yeah, that's fine.
[00:22:58] Trevor Anderson: Okay.
[00:23:01] Manav Paul: Keep more organized.
[00:23:02] Trevor Anderson: Thank you. So I could just track it. Should I add that for you or someone?
[00:23:07] Manav Paul: Yeah, add it for me.
[00:23:12] Trevor Anderson: Well, is this something that you would work on this week or
[00:23:19] Manav Paul: I can work on it just depends on. Yeah, I should be good.
[00:23:22] Manav Paul: It shouldn't be too hard and I think Grant could be able to work on it with me if anything.
[00:23:25] Manav Paul: I can probably tackle it tomorrow or today. Shouldn't take too long. Just throw it in for now.
[00:23:30] Manav Paul: I'll keep you updated, but I think I can do it.
[00:23:37] Trevor Anderson: What is this? Like a template?
[00:23:39] Manav Paul: Yeah, I'd say audience solutions JIRA ticket format.
[00:23:59] Trevor Anderson: I'll film some details here.
[00:24:51] Trevor Anderson: Okay. Yeah, that was one thing.
[00:24:58] Trevor Anderson: Two like this structure right now I'm a little confused.
[00:25:02] Trevor Anderson: She has like folders for combos for priority.
[00:25:07] Manav Paul: It was. Aaron was handling it before.
[00:25:09] Manav Paul: Then Aaron hopped off our team and now there's just tickets everywhere.
[00:25:15] Manav Paul: You don't even really like, even during our once once a week standups.
[00:25:21] Manav Paul: It's really just what's everyone working on. And then we just kind of move it ourselves.
[00:25:25] Manav Paul: Things like that. Tickets are made like literally anywhere and everywhere.
[00:25:29] Manav Paul: We just, we just get assigned to them or one of us picks it up. So this whole.
[00:25:35] Manav Paul: I think this whole board need a refresh.
[00:25:37] Manav Paul: It is for the best and I'll include a whole thing in the doc.
[00:25:41] Trevor Anderson: Okay. Is Monica the only one on like Virginia's team and shaml?
[00:25:46] Manav Paul: These are two, two contractors that they brought in when we started the model audience pipeline work so they could help us complete these tasks because there was too many for us to do the pipeline and also continue working on.
[00:25:58] Trevor Anderson: So I'm going to be.
[00:26:00] Trevor Anderson: So I'm working on a governance related item right now for, for this team and I need a Persona group for audience solutions work in general.
[00:26:11] Trevor Anderson: Like a read access only group.
[00:26:14] Manav Paul: Okay.
[00:26:15] Trevor Anderson: And this group will have access to all the data assets that are like needed for basically audience solutions, analytics and I guess model.
[00:26:27] Trevor Anderson: Well it'll be for like, it'll be read only.
[00:26:30] Trevor Anderson: So like I guess right now whatever the current process is to query data to create audiences. Yeah.
[00:26:41] Trevor Anderson: And so I'm thinking right now it's just going to be like audience solutions analysts or something like that.
[00:26:46] Manav Paul: Sure, yeah.
[00:26:47] Manav Paul: I mean like for example Monisha and Shaml they're only going to need read so they could extract the find the parameter and extract the seed and then the rest.
[00:26:55] Manav Paul: The rest of their work is done in taps. So they don't need anything else for that.
[00:26:59] Trevor Anderson: They will just have visibility to certain.
[00:27:02] Manav Paul: They just need to be able to query the table and be able to download the the results of the query.
[00:27:08] Manav Paul: That's literally all they need to do.
[00:27:09] Trevor Anderson: Okay, but they don't. They only need access to like certain tables too. Right.
[00:27:13] Manav Paul: It's literally. I think I can send you the exact table.
[00:27:16] Trevor Anderson: That's okay. Okay, so I'm gonna do that.
[00:27:18] Manav Paul: And then I think actually those shamble does sometimes I think work work with the databricks dashboards.
[00:27:26] Manav Paul: I don't know if she's still doing that. I don't know if that's something that is affected by the.
[00:27:29] Manav Paul: The change.
[00:27:30] Manav Paul: But just something to keep in mind because I know she was working on making a reporting dashboard before.
[00:27:35] Manav Paul: I have no idea what the status is on that. But if that's something that's affected by the
[00:27:42] Trevor Anderson: dashboard.
[00:27:43] Manav Paul: Dashboard, right.
[00:27:44] Trevor Anderson: Yeah, it's more like what. What table? She's.
[00:27:49] Manav Paul: Yeah. So I think looking to add to the dashboard so you know. Yeah, I'll send you the exact table.
[00:27:58] Trevor Anderson: I'm looking for like schema level access actually like not table level. So I don't know if.
[00:28:06] Trevor Anderson: Yeah, there's certain schemas sure. But yeah, I'm going to create that group. I don't know.
[00:28:15] Trevor Anderson: I think Virginia. I'll add her in there too
[00:28:18] Manav Paul: and okay, they're just gonna probably need.
[00:28:23] Trevor Anderson: Virginia has like over. She is like over privileged right now.
[00:28:27] Trevor Anderson: She's part of the product owners and so she has access to like a bunch of bronze and dev and here I'm gonna send.
[00:28:34] Manav Paul: This is the schema that they need access to.
[00:28:38] Manav Paul: This covers both tables that we use the survey event and the other one, the marketable.
[00:28:48] Manav Paul: With the marketable user. So that's. I sent it through a Slack but that's the only schema they need.
[00:28:53] Manav Paul: Anyway.
[00:29:01] Trevor Anderson: Is all this available in the prod workspace?
[00:29:04] Manav Paul: I have no idea. I just got access to the prod workspace a couple days ago.
[00:29:08] Manav Paul: I haven't even really looked at it yet. I know it's in dev for sure.
[00:29:13] Trevor Anderson: You have playful in here so that's good. Master marketable dev.
[00:29:20] Manav Paul: All right, so there's product. Sorry. And then it should be in okay, it's not there.
[00:29:26] Manav Paul: Maybe it's not in not there yet. So it's just prod yet? Yeah, it's in depth for sure.
[00:29:35] Trevor Anderson: Just check.
[00:29:37] Manav Paul: Yeah, that's, that's their most important table. So that just needs to be there.
[00:29:41] Manav Paul: Audience relations dev and then master marketable.
[00:29:44] Trevor Anderson: Set the.
[00:29:44] Manav Paul: Yeah, it's down there.
[00:29:46] Trevor Anderson: And do all these schemas need to go to prod?
[00:29:49] Manav Paul: I don't think I, I, is that something that.
[00:29:52] Manav Paul: But I don't even think we use anything else besides master Marketable mostly.
[00:29:57] Manav Paul: And then like I guess the, that playful debt is important.
[00:30:00] Manav Paul: And then I guess maybe that experience data. I really don't know what else.
[00:30:03] Manav Paul: I know personally, I've never touched anything besides master Marketable.
[00:30:07] Manav Paul: I think there's a bunch of junk in here that was from the old one that Kurt was working on that we just don't need at all.
[00:30:13] Trevor Anderson: I think is that part of the audit Grant is working on is what goes to prong. Okay.
[00:30:24] Manav Paul: So the two tables are Survey event and users that we use. So it's a couple up
[00:30:33] Trevor Anderson: in a different schema?
[00:30:34] Manav Paul: No, it's in a schema. It's in the center. It's like a few down. It's right there. Right above that.
[00:30:38] Manav Paul: Yeah, Users and the one above that survey, those are the two we use.
[00:30:42] Trevor Anderson: What is this one then? Is this like a build off of users?
[00:30:47] Manav Paul: I don't even know. I have no idea. I don't think we use this for anything.
[00:30:53] Manav Paul: Maybe Kurt was using it at one point for something.
[00:30:56] Manav Paul: There's so many things in here I don't think anyone is currently using
[00:31:01] Trevor Anderson: what is marketable metrics then if you're saying we use gold marketable users.
[00:31:07] Manav Paul: We use users for the actual seed building.
[00:31:10] Manav Paul: Yeah, I don't maybe, maybe the metrics just for that dash, but maybe it's for the dashboard.
[00:31:16] Manav Paul: I, I don't know.
[00:31:18] Trevor Anderson: Okay, so we should only be using these gold ones.
[00:31:23] Manav Paul: I can confirm that users in Survey event are definitely used a lot of these. Other I don't think are.
[00:31:28] Manav Paul: I think it's just junk, honestly.
[00:31:30] Manav Paul: I can double check with Joseph and Grant that they're using any of these, but I think a little like I can confirm a lot of these.
[00:31:37] Manav Paul: We don't need nearly as many of these schemas. They're all just from the old, you know, the old dev.
[00:31:42] Manav Paul: And that was just a lot thrown in there.
[00:31:48] Trevor Anderson: What is this one? Is this a build off of lead manager?
[00:31:52] Manav Paul: Yeah, this is the data we use the, the actual user data.
[00:31:56] Trevor Anderson: This is lead. Lead manager and anything else. Or is it.
[00:32:00] Manav Paul: I'm not sure if there's anything else.
[00:32:02] Trevor Anderson: Okay. We should try to migrate to the new naming standard, because if this is only. If this is only.
[00:32:11] Trevor Anderson: Lead Manager should just belong on Lead Manager catalog as a gold table.
[00:32:19] Trevor Anderson: If it's combining data, then we can have it in. In a. In this one under a gold schema.
[00:32:30] Trevor Anderson: Because this is just. This is all confusing, all this stuff in here. I'll.
[00:32:38] Trevor Anderson: I'll ask Grant a little bit more about that. Yeah, let's see what he says. Okay. I'm going to jump.
[00:32:45] Trevor Anderson: I have another meeting, but thank you so much for going through all this stuff with me.
[00:32:49] Manav Paul: I appreciate it. We'll talk soon.
[00:32:51] Trevor Anderson: All right.
[00:32:52] Manav Paul: Follow up on everything and get back to you.
[00:32:54] Trevor Anderson: Yeah. Thank you. Yep.
[00:32:56] Manav Paul: Talk soon.
[00:32:56] Trevor Anderson: Okay, bye.
[00:32:57] Manav Paul: Bye.
