---
title: "Joseph x Trevor Sync"
meeting_id: "040000008200E00074C5B7101A82E00807EA0618FD97E52AB7D9DC010000000000000000100000008CA1E4E3AC1FD440821EA2A2B456A11A"
note_id: 80784114
recording_id: 15045267
date: 2026-06-24
start_time: "2026-06-24T10:30:00-04:00"
end_time: "2026-06-24T11:00:00-04:00"
url: "https://fluentco.fellow.app/meetings/040000008200E00074C5B7101A82E00807EA0618FD97E52AB7D9DC010000000000000000100000008CA1E4E3AC1FD440821EA2A2B456A11A/"
participants:
  - name: "Trevor Anderson"
    email: "tanderson@fluentco.com"
    rsvp: accepted
    organizer: true
  - email: "JRamesh@fluentco.com"
    rsvp: accepted
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
[ ] Joseph to draft and send an email summary of all segment registry concerns and questions to Trevor for review and forwarding to Virginia by end of day.
[ ] Message Manov to schedule a 30-minute walkthrough of the segment registry architecture later today.

## AI Summary

Joseph and Trevor discussed significant concerns about the new segment registry implementation being developed by Manov and Dan. Joseph identified multiple technical and process issues, including **unclear PII requirements** (the spec mentions no names, addresses, or phone numbers, contrary to what Virginia likely needs), **performance concerns with querying large tables** (100M users, 1.4B survey records) sequentially for potentially 3,000 segments, and **fundamental differences in output format** (segment-wise vs. the current user-wise approach). The team agreed that Virginia needs to be consulted immediately to clarify requirements before implementation proceeds, as the current design may not align with client SLAs or existing delivery formats. Trevor will coordinate a meeting with Manov to review the architecture, while Joseph will draft a summary of concerns for Virginia by end of day.

### Chapters

#### PII Requirements and Compliance
- The segment registry spec indicates **no names, addresses, or phone numbers** will be sent to clients, which conflicts with current practices where all PII is sent (matching what Virginia requires for similar products like Playful Rewards). *(0:00:53)*
- Survey tables contain only **email hashes**, not full PII. If the no-PII approach is followed, the users table (containing demographic data) wouldn't need to be joined, fundamentally changing the architecture. *(0:02:06)*
- Unclear how hashed identifiers alone provide value to data marketplace consumers, as **the whole point of these segments is to buy PII**. This needs clarification from Virginia. *(0:02:14)*
- The team must work with Virginia to confirm requirements and revisit the EPIC before implementation, as **SLA changes would be required** if the data format changes from current practices. The feasibility of the current design is questionable since neither Joseph nor Grant reviewed it. *(0:03:39)*

#### Technical Architecture and Performance
- Each segment is defined in **separate YAML files** (potentially 3,000 files for the target segment count), making it difficult to locate specific segments without using grep commands across all files since only IDs are in filenames. *(0:05:38)*
- The current design queries both the users table (**~100M records**) and survey table (**~1.4B records**) sequentially for each segment, which could result in **thousands of redundant full table scans** with poor performance. *(0:07:11)*
- The segment definitions reference **raw tables instead of gold tables** that Leo produces, which deviates from best practices and the current compliant pipeline that uses properly filtered silver/gold tables. *(0:08:35)*
- Segment size validation shows **1,000 minimum rows** in the spec, but Virginia's actual requirement is **1 million+ for declared audiences**, indicating missing eligibility checks in the design. *(0:09:25)*
- A **table-based approach for storing segment metadata** (with history/audit capabilities) would be more optimal than reading from 3,000 YAML files. The current DA pipeline already has such a table with ~60 columns tracking all segment details. *(0:19:57)*

#### Output Format Discrepancy
- The new segment registry produces **segment-wise output** (one file per segment listing users), while the current approach uses **user-wise output** (one row per user with pipe-separated segment list), creating a fundamental mismatch. *(0:09:44)*
- Current delivery format for LiveRamp and TransUnion includes one row per user with email, hashes, PII, and all segments that user belongs to, following established **client-specific column mappings** (10 columns for LiveRamp, 14 for TransUnion). *(0:10:21)*
- The segment-wise approach would produce **massive duplication** since users can fall into 100+ different segments, and it's unclear how this aligns with client SLA requirements for file structure and delivery format. *(0:10:27)*
- If only survey tables are used (without users table), the pipeline would **miss demographic segments** like state, gender, and age that exist only in the users table, excluding entire categories of segments from processing. *(0:17:17)*

#### Process and Team Collaboration
- Joseph discovered the confluence doc and repo **only yesterday**, having been unaware while Manov was working in isolation on the design without team review or collaboration. *(0:24:19)*
- **Need for inclusive planning meetings** when new initiatives are started, so concerns can be identified before implementation begins rather than after significant work is done. *(0:26:43)*
- The design was **never peer reviewed** by the team, creating problems. Dan posted it, Manov started working on it independently, and gaps in the collaborative review process led to these fundamental issues. *(0:27:10)*
- The organization will require **weekly team updates posted to Lattice** going forward, with Trevor collecting five simple questions (AI wins, work completed, planned work, blockers) from the team every Friday. *(0:27:44)*

### Action Items
- Joseph: Draft and send an email summary of all segment registry concerns and questions to Trevor for review and forwarding to Virginia by end of day.
- Trevor Anderson: Message Manov to schedule a 30-minute walkthrough of the segment registry architecture later today.

### Decisions
*(none recorded)*

## Transcript

[00:00:00] Joseph: Okay. Okay.
[00:00:01] Trevor Anderson: So the.
[00:00:02] Joseph: Okay, perfect.
[00:00:04] Trevor Anderson: Yes.
[00:00:05] Joseph: Yeah, I was going through this segment registry. I'm not sure if I can share the screen.
[00:00:13] Trevor Anderson: Please, please do. I'm gonna need some.
[00:00:18] Trevor Anderson: Yeah, I've been waiting till the modeled audience pipeline is set up. Okay.
[00:00:25] Trevor Anderson: But I really do, I really do want to date have some deep dive sessions into this.
[00:00:33] Trevor Anderson: So I'll probably be saying some time once that pipeline is created because I.
[00:00:39] Trevor Anderson: I do want to explore this with you guys too. But show me what.
[00:00:42] Trevor Anderson: Yeah, show me what you get, what you're looking into here. Yeah.
[00:00:46] Joseph: So here what Mano and Dan during has built this. So this is.
[00:00:53] Joseph: So the main thing is my first question is they have mentioned no names, address and phone numbers, no PI for any of the clients.
[00:01:01] Joseph: So I'm still wondering if Virginia approved this or did the SLA happen in such a way or like, how are we even like proceeding with this?
[00:01:10] Joseph: So this changes everything. If we do not send any of the PIs, we would not require the users table.
[00:01:20] Joseph: We can just rely on the survey tables. And that's how it has been.
[00:01:28] Trevor Anderson: What's what? Users table.
[00:01:30] Joseph: So user table will have the PI data with it.
[00:01:32] Trevor Anderson: The pulse.
[00:01:36] Joseph: No, this lead manager.
[00:01:41] Trevor Anderson: Oh, I. I see. So you don't have to join the users.
[00:01:47] Joseph: Yeah, yeah.
[00:01:48] Trevor Anderson: Information. You could just use the survey result answers. Okay.
[00:01:51] Joseph: Yeah.
[00:01:52] Trevor Anderson: Why does that. Does just the survey results have a hashed version? Is that why.
[00:01:56] Joseph: Yes, that has the email hash.
[00:01:58] Trevor Anderson: I got it.
[00:01:58] Joseph: Got it. But the only caveat is if we going through that route, if we just rely on the survey events.
[00:02:06] Joseph: So we have. One second.
[00:02:11] Trevor Anderson: I'm confused how that works in general though.
[00:02:14] Trevor Anderson: Like, if you just pass hashed identifiers to these marketplaces, what's the benefit to the consumer?
[00:02:23] Joseph: That's what. Yeah. And I'm pretty sure.
[00:02:26] Trevor Anderson: Don't they want. Isn't the whole point that they buy or they're buying pii?
[00:02:31] Joseph: Yeah, and that's a point I asked Brian yesterday.
[00:02:35] Joseph: But still I'm not sure because I'm pretty sure Virginia would require all of these because she even wants PI for.
[00:02:44] Joseph: What is that? Playful rewards.
[00:02:46] Joseph: So it's obvious that you would require all the PI for these segments, which is the lead manager segments.
[00:02:54] Joseph: So I think we'll have to work with her first and then we'll have to revisit this and make the changes accordingly.
[00:03:01] Joseph: So the only fear I have is I'm not sure even if this EPIC is properly managed.
[00:03:07] Joseph: Or I said, like, did anyone check the feasibility?
[00:03:12] Joseph: Or like I'm not sure of it because none of me or Grant had a look.
[00:03:18] Joseph: So it was just a design by Mano and Dan Dueling or Dan Hall.
[00:03:22] Joseph: I'm not sure who all were involved in this, but I wanted you to be.
[00:03:26] Trevor Anderson: Yeah, yeah.
[00:03:27] Joseph: So I wanted you to be a part of this so you can maybe guide us through.
[00:03:32] Joseph: Like I can at least connect us in some way or the other.
[00:03:36] Trevor Anderson: So, okay, yeah.
[00:03:39] Joseph: One is that PI. Think we would require it? I'm pretty sure Virginia would ask for it.
[00:03:44] Joseph: And that's the first question.
[00:03:45] Joseph: So if that's the thing, we'll have to join both tables, which is the survey table and the user table.
[00:03:52] Joseph: So that changes the picture here.
[00:03:57] Trevor Anderson: You're saying you're thinking that this didn't come from Virginia.
[00:04:04] Trevor Anderson: Like Virginia is not requesting for pr.
[00:04:06] Joseph: So Virginia is looking as the function as functionality. For example, she wants.
[00:04:12] Joseph: Okay, you guys get me an application or like whatever so that I can just point to that.
[00:04:19] Joseph: So it should build up all the segments which she asked and that should distribute it. That's it.
[00:04:24] Joseph: But I am not sure if she agreed to these specifics, like, like the column.
[00:04:33] Joseph: Column mappings or like, like those SLA kind of related stuffs.
[00:04:37] Joseph: I am not sure that Virginia would have approved that because currently we are sending everything.
[00:04:46] Joseph: Not everything in the sense like the PI which is mentioned here, which I have commented here.
[00:04:50] Joseph: We send everything and all of a sudden if we change it, okay, we can change it.
[00:04:55] Joseph: But for that the client is. Should accept it.
[00:04:57] Joseph: We'll have to make this SLA changes and whatsoever we will have to work on it before we even start implementing this.
[00:05:07] Trevor Anderson: Okay.
[00:05:08] Joseph: As a product view.
[00:05:13] Trevor Anderson: Okay, this is important. Yeah, I'm trying to figure out because I, I want to bring it up and.
[00:05:21] Trevor Anderson: Or have it at least as a posted question for Virginia.
[00:05:25] Joseph: Yes, review. And that was next week when she's available. Probably.
[00:05:32] Trevor Anderson: Okay, yeah.
[00:05:34] Joseph: So that is the one thing I am looking for. And even the.
[00:05:38] Joseph: So only difference I see here with the current pipeline is so each segment is defined as a separate YAML files, which means so this is the repo like Mano has built or like created.
[00:05:56] Trevor Anderson: And do I have access to this? Can you send it to me?
[00:05:59] Joseph: I can say because even I figured it out yesterday. Yesterday I didn't know that this exists.
[00:06:04] Trevor Anderson: So same it's a new repo.
[00:06:10] Joseph: Yeah, just sent in the Zoom chat.
[00:06:14] Trevor Anderson: Okay. Yeah.
[00:06:16] Joseph: And if you see here, what is this? Where is that? Yeah.
[00:06:22] Joseph: So we are going to store each of the segment like this.
[00:06:25] Joseph: So if we have thousand segments, we'll have thousand files here.
[00:06:30] Joseph: Okay, I'm not sure if that's a good practice.
[00:06:36] Joseph: And it's not easy to figure out where a specific segment is.
[00:06:42] Joseph: For example, we would just have the ID here, not the actual name of it.
[00:06:48] Joseph: So it's not a direct search command. It should be a GRIP command among all the files.
[00:06:55] Joseph: Because each segment will have its respective own yamls.
[00:06:59] Trevor Anderson: So you have to read the YAML.
[00:07:03] Joseph: Yeah. To know what it is, that's one thing. But just to process each stuff is like.
[00:07:11] Joseph: So each will be processed sequentially. So if there are like thousand segments.
[00:07:16] Joseph: I'm just making this up, but they are aiming for 3,000. For example, even if we have the current.
[00:07:22] Joseph: What is this? The size the thousand segments.
[00:07:26] Joseph: So for thousand segments for each of the segments it will go and query both the tables.
[00:07:33] Joseph: So this is about 100 million, I guess user table and the survey table is about 1.4 billion.
[00:07:41] Joseph: So it's a huge. And I don't think so it's performance.
[00:07:45] Joseph: It's like best practice to do that because for thousand times it's gonna do the same thing again and again.
[00:07:51] Joseph: Since it's living.
[00:07:53] Trevor Anderson: What do you. Wait, hold on, what are you saying?
[00:07:55] Trevor Anderson: Like for every segment, to validate if it exists, it has to check these tables?
[00:08:01] Joseph: Yes.
[00:08:02] Trevor Anderson: What do you mean? To create it or to check if it already. Even.
[00:08:12] Joseph: I'm not sure how that is being designed here. So I just saw this, how our segment is defined.
[00:08:17] Joseph: So here I see the actual query here.
[00:08:20] Trevor Anderson: Select this thing from. Data Lake, table, survey, all. Is that actually the table?
[00:08:31] Joseph: No, this is the raw table which Leo produces. So we have the gold tables.
[00:08:35] Joseph: But he's pointing out to the raw tables for some reason. So that was one of the comment here.
[00:08:40] Trevor Anderson: Okay, yeah.
[00:08:42] Joseph: That we can modify it.
[00:08:43] Joseph: But my thing is, my concern is how are we even going to orchestrate this if this each of the segment is going to read both the tables for thousand times.
[00:08:56] Joseph: It's going to take a lot of time. And that is one thing. And the other thing I noticed is.
[00:09:06] Joseph: Yeah, so these segments are. So the output. I just want to show you the output. How it comes up is.
[00:09:20] Trevor Anderson: That.
[00:09:25] Joseph: But here he has mentioned it as thousand rows minimum segment size.
[00:09:28] Joseph: But what Virginia wants is like 1 million, not thousand or 5,000.
[00:09:33] Joseph: So I'm not sure if they have the check here like the feasibility check if it's.
[00:09:38] Joseph: If a segment is eligible for a declared audience or not. So that should be checked.
[00:09:44] Joseph: And yeah, I see the output is as for each segment for segment one, it will have a list of users who fall into it.
[00:09:56] Joseph: But our current approaches one row per user.
[00:10:04] Joseph: For example, if you take Live Ramp, we have its fluent ID and email and hashes and PIs and we declare what all segments does this user fall under for a specific user.
[00:10:21] Joseph: But what this new segment registry does is it's segment wise, not a user wise.
[00:10:27] Joseph: So which means this would produce n number of files.
[00:10:32] Joseph: For example, if we have thousand segments, it would produce thousand files and not the.
[00:10:45] Joseph: What is this SLA that each client wants?
[00:10:48] Joseph: For example, LiveRamp wants in 10 files with this column mapping and the TransUnion wants this 14 columns with this column mapping.
[00:10:58] Joseph: And I don't know how that is being handled here because that is like segment wise and not user wise, user specific.
[00:11:09] Joseph: And that's a duplication also because user can fall under like 100 different segments.
[00:11:14] Joseph: So it's a kind of duplication there. And I don't know if that's the right way to do.
[00:11:21] Trevor Anderson: Are you saying in that other. In the legacy way it's user specifically, it's an array of.
[00:11:28] Joseph: Yes, you're right. You're right.
[00:11:29] Trevor Anderson: The, the. The for every user row there's an array of which segments it belongs to.
[00:11:35] Joseph: So yeah, you are right. So for TransUnion we. Oh, it's a pipe separated.
[00:11:40] Trevor Anderson: Oh, so it's not another variable. Yeah, similar to that. Okay, yeah.
[00:11:45] Joseph: And the new approach is completely different. It's like per segment and not per user.
[00:11:51] Joseph: So I'm not sure if this is what the end product like the end user would require or.
[00:11:57] Trevor Anderson: Wait, I'm a little confused by the delivery there then on the legacy way. Because if you're saying.
[00:12:03] Trevor Anderson: I thought it was by segment and that segment included unique users.
[00:12:11] Trevor Anderson: Like we deliver a segment with a list of users that belong to that.
[00:12:17] Joseph: No, we deliver the users to which all.
[00:12:23] Joseph: So each user will have a separate column which says what all segments does this user fall under?
[00:12:31] Joseph: So it will be a one row for one user.
[00:12:35] Trevor Anderson: Yeah, but when we upload it to their marketplace, to the data marketplace, isn't it like so defined group.
[00:12:42] Joseph: I. I get your point.
[00:12:44] Joseph: So we push this to Automate EU and Live Ramp and they do the further stuffs like the.
[00:12:53] Joseph: What do they call the further cleansing or something? Because what we do is we submit.
[00:13:01] Joseph: What is that called? A taxonomy and only then we push this. And, and that's a good question.
[00:13:08] Joseph: And I'm not sure how this SLA has been formed because it wasn't us who decided on how we should send an segment or something, but this is how we follow it for currently.
[00:13:22] Trevor Anderson: Okay. And so,
[00:13:27] Joseph: but there's.
[00:13:28] Trevor Anderson: In order to build that SLA though, don't you have to define like the segment parameters and all that?
[00:13:35] Trevor Anderson: So you're, you're kind of, you know, you're limiting the scope. Right? It's already within a.
[00:13:41] Trevor Anderson: It's already like within a segment anyways.
[00:13:44] Trevor Anderson: How are you getting more than one segment is what I'm curious about here.
[00:13:52] Joseph: Sorry, I'm not getting you.
[00:13:53] Trevor Anderson: You mean like when you build an audience, right? In my, My assumption is that when you build.
[00:14:01] Trevor Anderson: When you go ahead and build an audience and, and get ready to distribute that onto these marketplaces, you.
[00:14:11] Trevor Anderson: You, you scope that to only one segment. So I'm confused how. When we're uploading data that.
[00:14:20] Trevor Anderson: So a user will have more than one segment?
[00:14:22] Trevor Anderson: Because in our where clause, like in our filters, we're filtering for a specific segment. So I'm.
[00:14:28] Trevor Anderson: I'm curious how.
[00:14:29] Joseph: So are you talking about the modeled audience?
[00:14:32] Trevor Anderson: Yeah. Or both. I guess in general though. And for both because.
[00:14:39] Joseph: But for model audience, you are right. That's how we do it. We have a segment specific.
[00:14:46] Joseph: So for each segment we'll have a set of users and we uploaded to tap and then the process follows and then we distribute.
[00:14:52] Joseph: But this is the current approach we have. I'm not sure how they decided it.
[00:14:57] Joseph: So that's what we are sending it to our clients.
[00:15:01] Trevor Anderson: Are you saying although we might be building a specific audience, we are still saying that this user is showing in other segments that already exist
[00:15:16] Joseph: or do
[00:15:17] Trevor Anderson: we just
[00:15:19] Joseph: model audience?
[00:15:21] Trevor Anderson: No, in modeled audience. So this is actually a column. Is this a column in model audience or.
[00:15:26] Trevor Anderson: No, no, no. Okay. Yeah, I'm got a. Okay, we probably have to talk about that further.
[00:15:33] Trevor Anderson: That's interesting. I understand what you're saying though in terms of the schema or the.
[00:15:40] Trevor Anderson: Yeah, the column structure is different. Yeah, exactly. But maybe that's just a.
[00:15:47] Joseph: It's Virginia's call.
[00:15:50] Trevor Anderson: Well, I think that might be an overlook. Like when, when this was being built out.
[00:15:55] Trevor Anderson: Like we might overlook that fact might be.
[00:16:00] Joseph: Yeah, if that answers. Maybe we can decide on which whichever way we want to follow.
[00:16:07] Trevor Anderson: Because I, because I guess if you, if you're using, you know, AI to help build out that document, you know, the, the.
[00:16:19] Trevor Anderson: I guess the assumption was that every distribution would be segment.
[00:16:25] Trevor Anderson: Like built by segment, you know, so I mean, I understand if, if, if, if who.
[00:16:31] Trevor Anderson: You know, if Dan had that assumption as well, then.
[00:16:34] Joseph: Yeah, I got it.
[00:16:35] Trevor Anderson: AI probably has that assumption. And yes, it just, you know, keeps going down and Down. Right.
[00:16:41] Joseph: So and actually that's a good question we should ask Virginia.
[00:16:44] Joseph: And on how did we decide this approach to have user wise for declared
[00:16:49] Trevor Anderson: audience
[00:16:52] Joseph: and not a segment wise. Yeah, okay, so this is one.
[00:17:02] Trevor Anderson: Yep, good questions.
[00:17:03] Joseph: And what else? And you know what feed rely only on the survey tables.
[00:17:17] Joseph: We would miss the other segments which are like other categories like state, gender, age and like whatsoever.
[00:17:25] Joseph: Because this lives in the user table and not in the survey table. Yes, these columns.
[00:17:31] Joseph: So we'll miss all those segments in this process.
[00:17:35] Trevor Anderson: And don't, don't we use some of those for like filtering out for compliance?
[00:17:45] Joseph: Yeah, that's already cleansed. That's a separate pipeline. The male level universe. Yeah, so whatever.
[00:17:50] Joseph: Yeah, we are already querying against the gold tables. So it's already compliant.
[00:17:56] Trevor Anderson: Filtered from the silver.
[00:17:57] Joseph: Yes, yes, you are right.
[00:17:59] Trevor Anderson: Okay.
[00:17:59] Joseph: Yeah, this is one. If we would miss all these. And what's the other one?
[00:18:07] Joseph: Yeah, this is the output which I was talking to you.
[00:18:09] Joseph: Like we'll have to decide on how we are like even planning to deliver it. Yeah.
[00:18:16] Joseph: And these were the main concerns. But this completely changes the picture.
[00:18:20] Joseph: If Virginia is looking for a different way, we'll have to revisit and make the changes in epics and stories and whatsoever.
[00:18:30] Trevor Anderson: Okay, I guess one question is.
[00:18:37] Trevor Anderson: You bring up some valid points and I think we should revisit and maybe refine this document to answer those questions, right?
[00:18:46] Joseph: Yes. But my one question is as it stands, like would this I guess be.
[00:18:55] Trevor Anderson: Well, would it be possible to implement those, the changes that are suggested in this document.
[00:19:04] Trevor Anderson: It might not, I think especially with the YAML, the segment YAML.
[00:19:10] Trevor Anderson: Yeah, I don't know about that structure.
[00:19:12] Joseph: Yeah, that's crazy.
[00:19:13] Joseph: It's going to be 3,000 files and, and we can't, for example, if, if that is a common variable which changes.
[00:19:22] Joseph: We can't even go and query each and everything and make the changes. If that has a similar.
[00:19:28] Joseph: For example, for millennials, if the age is like 20 to 25 and if there is a bug there, but if you need to fix it, it's going to be a hectic work.
[00:19:39] Joseph: If it's in YAML, if these all live in the same file, that's a different case.
[00:19:46] Trevor Anderson: I feel, I feel like maybe this should be stored something. Okay, I'm thinking about this right now.
[00:19:57] Trevor Anderson: But should we have a table that stores the history of these? Yeah, like can't store it.
[00:20:07] Trevor Anderson: Should we have a table that has like all the same things that are in the CML here, like segment id, name, description That's a good question.
[00:20:17] Joseph: We have that for our current DA pipeline which I built.
[00:20:20] Joseph: But I'm not sure if that is being addressed here in the new approach.
[00:20:24] Trevor Anderson: Because that should be used as an audit.
[00:20:27] Trevor Anderson: Like we should be able to use that to check records instead of, instead of going through a YAML file, it's much more optimal to read from a table.
[00:20:36] Trevor Anderson: Right?
[00:20:37] Joseph: Yeah, right.
[00:20:39] Trevor Anderson: However, if we want something like human readable.
[00:20:44] Trevor Anderson: I don't know if this GitHub would be designed for AI to iterate through or for. For us to. To look at.
[00:20:53] Trevor Anderson: But if it's for.
[00:20:54] Trevor Anderson: For engineers to look at, then we should do groupings and folders and things like that. Right.
[00:21:00] Trevor Anderson: Like we should organize it better instead of having 3,000 in one folder. Right? Yeah, right.
[00:21:06] Trevor Anderson: Or change the naming structure or change, you know, do something that makes it easier right. To.
[00:21:11] Trevor Anderson: To look at. So,
[00:21:18] Joseph: so this is the current table we have for the current pipeline.
[00:21:21] Joseph: So this will have everything like what vertical subject area, this and that does
[00:21:25] Trevor Anderson: have the query in here.
[00:21:29] Joseph: No, we'll have to add that. You're right. We'll have to add those specifications.
[00:21:34] Trevor Anderson: Okay, we should definitely have that. We should definitely have this though for model audience.
[00:21:40] Trevor Anderson: Like I think that needs to be
[00:21:43] Joseph: a. Yeah, that's already.
[00:21:44] Trevor Anderson: That needs to be a ticket or something. What?
[00:21:47] Joseph: No, we have it. It's just we need to implement it in this testing and validation path.
[00:21:52] Trevor Anderson: Oh yeah, so you should definitely have that.
[00:21:59] Joseph: So this will have everything. This has about 60 columns.
[00:22:03] Joseph: So run ID the Jira ticket and the segment ID segment name. Is it a health segment?
[00:22:09] Joseph: The survey parameter was used.
[00:22:11] Joseph: The survey answer which was used business logic and audience description of that specific so and so and QID the lead manager thing.
[00:22:20] Joseph: So this tied to the lead manager table and question name and parameter value.
[00:22:28] Joseph: When did the start and all the the other config details here.
[00:22:32] Joseph: The segment name which is being uploaded and there is also a part for query. What is that?
[00:22:41] Trevor Anderson: Why are all these? No,
[00:22:44] Joseph: because this will be like in a subsequent runs. So we are just validating the first part.
[00:22:50] Joseph: So these are the other phases. Okay, so this validation was done for only the phase one.
[00:22:57] Joseph: So other validations required that.
[00:22:59] Trevor Anderson: What is it called?
[00:23:00] Joseph: The new workspace started. Okay, I think this is missing here. Query used. One second. I'm pretty sure.
[00:23:17] Joseph: Yeah, we are missing it here. Maybe you'll have to add that.
[00:23:20] Trevor Anderson: Maybe just search query real quick secret.
[00:23:23] Joseph: No, it's not here.
[00:23:24] Trevor Anderson: Okay, it's not there. You just looked at everything.
[00:23:27] Joseph: Yeah, okay, that's a good point. We can have that.
[00:23:32] Joseph: And yeah, these are the queries I have, Trevor, I'm concerned about because we are almost there for the Q3 and we'll have to visit it as soon as possible with Virginia first to get the product.
[00:23:49] Trevor Anderson: Can you write me a. Well, I want to just post it for Virginia or even review it again later today.
[00:23:57] Trevor Anderson: Are you able to write like a summary? Like our. An email or something?
[00:24:01] Joseph: Right, I can give you that, no problem.
[00:24:03] Trevor Anderson: Email summary. Yep. And then maybe. I don't know if you're.
[00:24:10] Trevor Anderson: What, like, are you planning on going through this more in detail or are you.
[00:24:15] Joseph: I'm planning because only yesterday I got to know that we have the confluence doc for it and.
[00:24:21] Joseph: And what is it? Repo.
[00:24:23] Joseph: Because we weren't aware of that because Mana was working in Silo and then I was spending most of the time and reviewing this and I'll spend some time.
[00:24:32] Joseph: By end of today, I can send you the draft with all the doubts I have and you can like send it over to Virginia.
[00:24:40] Trevor Anderson: Okay, thank you. Yeah, I think that's at least a good start.
[00:24:44] Trevor Anderson: Let's just like get the communication out that there's some initial things that we need to address before progressing.
[00:24:51] Trevor Anderson: Yes, right.
[00:24:52] Joseph: Yeah.
[00:24:52] Trevor Anderson: Right.
[00:24:53] Joseph: Yeah. Because even I'm blocked with this validation part for.
[00:24:56] Joseph: Since we are waiting on the DevOps for the model audience pipeline, I. I'm.
[00:25:00] Joseph: I'm almost working on this segment registry only.
[00:25:02] Joseph: I'll try to figure out if I'm able to figure out more details on it.
[00:25:08] Trevor Anderson: Okay. And then in general, segment registry is supposed to be a pipeline.
[00:25:13] Trevor Anderson: Like just a pipeline with no real agentic capabilities. Right.
[00:25:18] Trevor Anderson: Like all it does is supposed to process, you know, what requests from Jira or.
[00:25:24] Joseph: Yeah, because I'm wondering why do we even require this new pipeline for declared audience if we already have one?
[00:25:32] Joseph: If.
[00:25:32] Joseph: If it already satisfies each and every clients, why do we even require a new declared audience pipeline?
[00:25:39] Joseph: We can just use an agent to route it to either model or declared audience. That's it.
[00:25:45] Trevor Anderson: I don't know what. What? I'm confused as well.
[00:25:52] Joseph: Yeah, maybe.
[00:25:54] Trevor Anderson: What is this like
[00:25:59] Joseph: picture of this new approach? Because I'm still trying to understand what it is and.
[00:26:05] Joseph: Yeah, I think it would be better if Mano can walk us through.
[00:26:09] Trevor Anderson: Okay, let me. Let me message Manov and see if he has time later today.
[00:26:15] Joseph: Yeah, maybe a half an hour call would be enough.
[00:26:18] Trevor Anderson: Yeah. Okay.
[00:26:19] Joseph: Yeah.
[00:26:21] Trevor Anderson: All right. I have a in store call I need.
[00:26:23] Joseph: No problem.
[00:26:25] Trevor Anderson: But I'm gonna try.
[00:26:27] Trevor Anderson: Yeah, I want to see if we can have some time today to just talk about it, go through it because.
[00:26:31] Trevor Anderson: Yeah, lots of questions, but yeah. Summary on your concern.
[00:26:35] Joseph: I'll do that. I'll do that.
[00:26:37] Trevor Anderson: Thank you for digging into it though. This is much needed.
[00:26:41] Joseph: But one request from my side.
[00:26:43] Joseph: I think if the team is working on any new initiative, at least we would require a common meeting for all, including us, so that we wouldn't get into such situations like this even before start implementing it before planning it at least.
[00:27:01] Joseph: Because if this would have been addressed earlier, if we would have been a part of this call or something, we would have called out these.
[00:27:10] Trevor Anderson: Yeah, I think there's a little bit of a gap there right now because Dan, you know, Dan had posted this.
[00:27:16] Trevor Anderson: I'm sure Manob took it on upon himself to just start working on it. Right.
[00:27:20] Joseph: Yeah. Never.
[00:27:22] Trevor Anderson: It was never peer reviewed, you know.
[00:27:23] Joseph: Yes.
[00:27:24] Trevor Anderson: By the team. And I think that can create some problems. Definitely. So. I agree. I agree. Yeah.
[00:27:33] Joseph: Yeah.
[00:27:34] Trevor Anderson: Thank you. Thanks for. Oh, one thing I wanted to say was. The.
[00:27:44] Trevor Anderson: The organization is going to start wanting us to. Our team to post weekly updates to Lattice.
[00:27:52] Trevor Anderson: Oh, I don't know if you saw that. Yeah, I don't.
[00:27:55] Joseph: I see the emails, but I wasn't ever. If we are like, okay, it's a new.
[00:27:59] Trevor Anderson: They're asking me to get updates from the team.
[00:28:03] Joseph: So what kind of updates are like, it's a kind of technical or what
[00:28:08] Trevor Anderson: we did this week there. It shouldn't be too much of a lift.
[00:28:12] Trevor Anderson: They're just like five questions on the Lattice application and it's like, what are your AI wins for the week?
[00:28:19] Trevor Anderson: What did you do this week? What are you going to do next week? Any blockers?
[00:28:23] Joseph: I understand. Okay.
[00:28:25] Trevor Anderson: Pretty simple.
[00:28:26] Trevor Anderson: But like it's like every Friday I'm gonna try to get answers from everyone on our team and then I have to.
[00:28:34] Trevor Anderson: I have to write a summary based off. Off of that. So. Yeah.
[00:28:38] Joseph: Yeah.
[00:28:38] Trevor Anderson: So that makes sense.
[00:28:40] Joseph: Yeah, I know that. Thank you.
[00:28:41] Trevor Anderson: I can let you know more information about it, but I just wanted to give you a heads up.
[00:28:45] Joseph: Yeah, thank you. Thanks a lot. Sure.
[00:28:47] Trevor Anderson: Okay, thank you. I'll talk to you later.
[00:28:50] Joseph: Bye.
[00:28:50] Trevor Anderson: Bye.
