---
title: JC sync
date: 2026-06-26
time: 12:30-12:45
participants: [Trevor Anderson, Kapil Sreedharan, Jean Carlo Camacho, Venkatesh Mannam]
fellow_url: https://fluentco.fellow.app/meetings/040000008200E00074C5B7101A82E00807EA061A7EE6A7BD4AF4DC0100000000000000001000000093D3F101B4A2C24488AD2BF97C0507CF/
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
[ ] Venkatesh Mannam to create a bug ticket for the date of birth pipeline issue in the FIG system.
[ ] JC Camacho to schedule a walkthrough meeting with Kevin, Aaliyah, Venkatesh, and the team for Monday or Tuesday (June 29-30) to review the advertiser dimension table design.

---

The team discussed a critical data pipeline issue affecting the FIG pipeline, where **date of birth fields have been null for ~22% of users since June 2024** due to a data type mismatch. The fill rate is currently 78% instead of the expected 97-98%, and backfilling will require reprocessing massive amounts of historical data. The conversation shifted to integrating **Playful Rewards data into the Fluent advertiser dimension table**, with clarification that Playful data flows through Tune rather than being a separate source system. JC suggested adding dual source columns to distinguish between traffic source and origination source.

The team agreed to schedule a walkthrough with the BI team (Kevin and Aaliyah) for early the following week to review the table design and ensure alignment. JC emphasized the importance of engaging downstream consumers earlier in development to avoid knowledge gaps and ensure consistent data definitions across reports.

### Date of Birth Pipeline Issue

- A critical bug was identified where the **date of birth field has been returning null values since June 2024** due to a data type mismatch—the source changed from long to string format, but the pipeline still expects long.
- The fill rate for date of birth is only **78% when it should be 97-98%**, as date of birth is mandatory in Lead Manager surveys where users must provide it.
- Backfilling historical data from June 2024 onwards will require clearing the checkpoint and reprocessing massive amounts of data, presenting a significant technical challenge.
- The bug was discovered through a new dashboard that revealed the high 'skip' rate (no date of birth), demonstrating how **increased visibility through dashboards is surfacing data quality issues**.
- Venkatesh will create a bug ticket to track the fix, and the team acknowledged that resolving this issue is critical given its nearly two-year duration.

### Playful Rewards Data Integration

- The team discussed integrating Playful Rewards data into the Fluent advertiser dimension table, with clarification that **all Playful transactional data flows through Tune** rather than being a separate platform.
- Campaign creation and advertiser information for Playful should be available in Tune, making a separate Playful data source unnecessary for most attributes like campaign, offer, advertiser, and affiliate data.
- The advertiser dimension table structure includes source system IDs from Tune, Lead Manager, and Minion, and is being extended to include Playful Rewards and NetSuite for cross-system joining capabilities.
- NetSuite has an existing customer table with linkages between different systems (showing corresponding IDs across platforms), though it doesn't currently include Playful IDs since those are managed through Tune.
- JC suggested adding **two source columns to the table**—one for traffic source (Tune) and one for origination source (Playful)—to help users understand the data path and where to fetch specific information.

### BI Team Collaboration

- JC recommended scheduling a **walkthrough with Kevin and Aaliyah early next week** so the data engineering team can explain their table design to the BI team who are building dashboards on top of this data.
- Aaliyah was identified as a key resource with knowledge from both Upside and Fluent, making her Kevin's go-to person and critical to the success of the table design and reporting efforts.
- The BI team has already built similar linkages between NetSuite, Lead Manager, and Tune, so they can help fill knowledge gaps about data linkages and table structure.
- Kevin's BI team has already created reports showing Playful and Tremendous data, having completed the homework on **linking data at the affiliate and advertiser level**.
- The team agreed that engaging downstream consumers earlier in development would save research time and ensure alignment on definitions, as metrics like revenue can have different valid definitions across reports.

## Transcript

[00:00:00] Venkatesh Mannam: The JSON format.
[00:00:01] Trevor Anderson: Right?
[00:00:02] Kapil Sreedharan: Yeah.
[00:00:03] Venkatesh Mannam: So at some point they change the data type and that process hasn't been updated.
[00:00:08] Venkatesh Mannam: And from then we are getting nulls to the date of birth field.
[00:00:13] Kapil Sreedharan: Okay, Trevor. So the.
[00:00:15] Venkatesh Mannam: I don't know if you sync with
[00:00:16] Kapil Sreedharan: Trevor, but on fig. So we have this FIG pipeline that reads from fig.
[00:00:24] Kapil Sreedharan: There is an issue there where we are not populating the date of birth. I.
[00:00:32] Kapil Sreedharan: Yeah, I actually talked to JC sometime back to check how.
[00:00:36] Kapil Sreedharan: What his match rate is on the date of birth. He says it is 97%. So. Yeah.
[00:00:42] Kapil Sreedharan: So looking into our pipeline, there's an issue and that's what Venki described.
[00:00:51] Kapil Sreedharan: So we need to fix that.
[00:00:54] Trevor Anderson: Okay. Is there a ticket for it?
[00:00:57] Kapil Sreedharan: Yeah, we had to create a winky. Create a buck. Buck ticket.
[00:01:03] Venkatesh Mannam: Yeah, yeah, sure thing.
[00:01:08] Trevor Anderson: Oh, so we can't join right now. Is that what the problem is?
[00:01:12] Kapil Sreedharan: We can join. We're joining it. But you know the. There's no date of birth or It's.
[00:01:22] Kapil Sreedharan: Yes, it's 78 fill rate. That is users fig users with the date of birth is 78% where.
[00:01:34] Kapil Sreedharan: Whereas it should actually be, you know, close to 197 or 98%. So Venki, if you add this fix. Yeah.
[00:01:51] Kapil Sreedharan: Are we gonna get the historical. You said from this issue started from 2020 or from.
[00:01:57] Venkatesh Mannam: From when 2024. June.
[00:02:01] Kapil Sreedharan: Okay. Are we able to backfill?
[00:02:07] Venkatesh Mannam: We should be. I mean like we have to clear the checkpoint and run the whole thing. Right.
[00:02:15] Venkatesh Mannam: That's a massive data.
[00:02:24] Kapil Sreedharan: How much data is there?
[00:02:31] Venkatesh Mannam: We have.
[00:02:53] Trevor Anderson: So how is it getting. How is it filling date of birth right now? Is it.
[00:02:57] Trevor Anderson: Are you saying in Lead Manager users don't have date of birth and we have to join to Experian or something to.
[00:03:08] Trevor Anderson: To get that information?
[00:03:12] JC Camacho: No, in.
[00:03:12] Trevor Anderson: Are we getting databirds?
[00:03:14] Kapil Sreedharan: Yeah, in lead Manager. That is the.
[00:03:18] Trevor Anderson: Yeah.
[00:03:18] Kapil Sreedharan: In Lead Manager when a user takes a survey, they have to provide the date of birth.
[00:03:24] Kapil Sreedharan: Apparently that is mandatory.
[00:03:28] Trevor Anderson: Yeah. So that's like a field that someone enters. Okay.
[00:03:33] Kapil Sreedharan: Yeah. And we are getting that date of birth in the source. It's a file or JSON file or something.
[00:03:41] Trevor Anderson: Oh, you're saying.
[00:03:42] Trevor Anderson: Okay, for every record in Lead Manager we should be getting a date of birth, but we're only getting 70 something percent of them.
[00:03:49] Kapil Sreedharan: Yeah. For every user. So every user identified, that's email, there should be a date of birth.
[00:03:56] Trevor Anderson: I see. Okay. And so.
[00:03:59] Trevor Anderson: And what happened was they changed the data type so we stopped getting that field populated or something like that.
[00:04:11] Trevor Anderson: Yeah.
[00:04:11] Kapil Sreedharan: Venki, what was the issue There was some code change right
[00:04:17] Trevor Anderson: along the spring.
[00:04:21] Venkatesh Mannam: So earlier we expected that in a string format. Trevor. Sorry, the long, long format.
[00:04:30] Venkatesh Mannam: But at some point we start.
[00:04:33] Venkatesh Mannam: They started sending us in the string format and our process is not supporting that.
[00:04:39] Venkatesh Mannam: And from then like we are storing that as nulls.
[00:04:44] Trevor Anderson: Oh, I see. Okay. So what is it? Like only new records that have corruption or nulls.
[00:04:53] Venkatesh Mannam: I mean like starting 2024, June, July, all the profiles got null as a date.
[00:05:02] Trevor Anderson: So this has been happening for a while.
[00:05:04] Venkatesh Mannam: Yeah, it's been like almost two years now.
[00:05:09] Trevor Anderson: That's crazy. Okay,
[00:05:14] Kapil Sreedharan: yeah, so this came up in. That's the thing now, you know, we. With this dashboard,
[00:05:27] Trevor Anderson: everyone's going to see everything, right?
[00:05:28] Kapil Sreedharan: Yeah, yeah, yeah. Things are, you know, popping up. This one skipped.
[00:05:37] Trevor Anderson: Yeah. What is that?
[00:05:40] Kapil Sreedharan: This one? Actually I think Kevin needs to rename it or something.
[00:05:43] Kapil Sreedharan: But skip means there is no date of birth for this.
[00:05:50] Trevor Anderson: There's no.
[00:05:51] Kapil Sreedharan: Yeah, it's null.
[00:05:52] JC Camacho: And
[00:05:56] Kapil Sreedharan: you know, this should be lower because all the users should have a date of birth as per, you know, fig, because it's mandatory on our FIG or Lead Manager.
[00:06:13] Kapil Sreedharan: So we should have much lower number here. Now.
[00:06:16] Kapil Sreedharan: Other thing is, Kevin has not implemented, you know, the count of users, so we should also be getting like the count of users.
[00:06:27] Kapil Sreedharan: So if we get that, we'll have a better picture. But at least, you know, looking at the.
[00:06:31] Kapil Sreedharan: Whatever the revenue or clicks, this should be technically lower.
[00:06:38] Kapil Sreedharan: The reason why we have this high number is because. Yeah, our pipeline issue.
[00:06:44] Trevor Anderson: Yeah, and then what would be. So no FIG profile. What does that.
[00:06:53] Kapil Sreedharan: This one is, you know, out of all the users and gold customer v1 table or out of all the users who came into our network, these users are not there in fig.
[00:07:15] Trevor Anderson: So users in Lead Manager, there was no join to a FIG profile.
[00:07:22] Trevor Anderson: I thought FIG was built off Lead Manager.
[00:07:24] Kapil Sreedharan: Yeah, yeah, it's. It's the same actually. So Lead Manager is.
[00:07:29] JC Camacho: No, hold on, hold on.
[00:07:31] JC Camacho: This is saying that there's no fake profile for the record, that we got from Tune, right?
[00:07:35] Kapil Sreedharan: Yeah, yeah.
[00:07:37] JC Camacho: Not Lead Manager.
[00:07:38] Trevor Anderson: Thank you.
[00:07:39] Kapil Sreedharan: Yeah, not Lead Manager. But Tune is basically our network, right?
[00:07:42] Kapil Sreedharan: Anyone who comes into Pulse, whatever they, you know, they do clicks, conversions that will be registered in Tune.
[00:07:50] Kapil Sreedharan: Yeah, yeah.
[00:07:52] Trevor Anderson: But there was no identity for this, for those clicks and conversions, basically, there was no.
[00:07:59] Kapil Sreedharan: Yeah, that user.
[00:08:01] JC Camacho: Well, there could be a Kapil of different things too. Right.
[00:08:03] JC Camacho: And again, I don't know exactly how this is being built, but ideally what we do is we capture every transaction, at least for.
[00:08:11] JC Camacho: Let me speak about ROAS on the Athena Side, we capture all transactions from Tune and that's our starting point.
[00:08:18] JC Camacho: Now there could be a transaction that comes over and we can't link any. An email to.
[00:08:23] JC Camacho: For whatever reason, they'll still make it through the funnel. Right. Because we want to count.
[00:08:27] JC Camacho: We want to account for all the revenue.
[00:08:30] JC Camacho: So this, this, this particular case here could also be something like that. Right.
[00:08:34] Kapil Sreedharan: But yeah, it's the same thing.
[00:08:35] JC Camacho: I don't know. So we can clarify what, what it means when we say no fake profile. Right?
[00:08:42] Kapil Sreedharan: Yeah, it's the same thing. This is, you know, based out of tune.
[00:08:46] Kapil Sreedharan: And then we join with the menial events to get some stuff and also join with the FIG fig table to get the.
[00:08:56] Kapil Sreedharan: You these FIG attributes. There are a bunch of other FIG attributes. Let's see what with gender it is.
[00:09:06] Kapil Sreedharan: Okay. So jc, it's an issue on. In our pipeline.
[00:09:10] JC Camacho: Okay.
[00:09:12] Kapil Sreedharan: Where it has been there's a. This issue was there since last two years.
[00:09:21] Kapil Sreedharan: I guess initially they were sending date of birth as a long or some integer type and then it was switched to string.
[00:09:28] Kapil Sreedharan: But in our pipeline we are expecting a different data type. Long and yeah, it doesn't take string.
[00:09:36] Kapil Sreedharan: And for those it is giving null.
[00:09:39] JC Camacho: Okay. Okay.
[00:09:41] Trevor Anderson: So.
[00:09:52] Trevor Anderson: Hey, can you, can you share this report again? I'm just gonna bookmark it so. Because. Thank you.
[00:10:03] Trevor Anderson: I actually don't know how.
[00:10:09] Kapil Sreedharan: Yeah, so yeah, we'll. We'll get that fixed. I think we should push the fix soon. That's not good. Okay.
[00:10:30] Kapil Sreedharan: Anyways, I think we know what the issue is. Any. Anything else? I think Venki, regarding tremendous.
[00:10:42] Kapil Sreedharan: Sorry. Playful Rewards. We are able to get the.
[00:10:45] Kapil Sreedharan: You are able to join with the TUNE data and get the account manager's name right?
[00:10:54] Kapil Sreedharan: Is there anything else pending there?
[00:10:57] Venkatesh Mannam: No, I mean like so jc like are we going to reach out to their respective account managers?
[00:11:03] Venkatesh Mannam: Because I see like almost 10 different account managers for the advertisers per advertiser.
[00:11:11] Venkatesh Mannam: I mean like. No, no, for all the advertisers.
[00:11:15] JC Camacho: Okay.
[00:11:16] JC Camacho: I'm still trying to understand what it is that you're trying to like to capture that you can't capture today based on putting all these tables together.
[00:11:28] JC Camacho: Because the thing is this, right?
[00:11:30] JC Camacho: So everything that is flowing through Playful is flowing through tune. Right?
[00:11:34] JC Camacho: So if you're already getting the advertiser information from tune, then what do you need from Playful?
[00:11:43] Kapil Sreedharan: Are you missing campaign data? Campaign or what?
[00:11:52] JC Camacho: But as long as we link it to Tune, you can get campaign offer, advertiser, affiliate.
[00:11:56] JC Camacho: You can get all that information.
[00:11:58] Trevor Anderson: Yeah.
[00:12:00] Venkatesh Mannam: Okay. Okay.
[00:12:01] JC Camacho: Yeah, it's not like it's so like because the data is.
[00:12:06] JC Camacho: Or better yet, the transactional data or whatever it is that we're capturing from playful.
[00:12:11] JC Camacho: Just because it's coming from that doesn't mean that it's solely that, you know, that there's attributes there that are not as far as the advertiser and all that.
[00:12:20] JC Camacho: All that information as far as what deems to be part of the campaign creation should be in the TUNE platform.
[00:12:30] Venkatesh Mannam: Okay. Okay.
[00:12:31] JC Camacho: Now you. If you want you.
[00:12:33] JC Camacho: What you could do is when you create this table, you can create flags for what the sources are, right?
[00:12:38] JC Camacho: So you could have a playful. A playful flag that says yes or no. Okay.
[00:12:43] JC Camacho: So that if anyone looks at that campaign, they know that it's flowing through. Playful, right?
[00:12:49] JC Camacho: I mean, but I don't know how it is redesigning it. But
[00:12:54] Kapil Sreedharan: Venki, you're creating in. In that campaign table, you're creating a new entry for playful, right?
[00:13:01] Kapil Sreedharan: Is that.
[00:13:02] Venkatesh Mannam: Yeah, yeah. A Kapil like. Similar to how the other systems like for Mini and Tune lead manager.
[00:13:08] Venkatesh Mannam: So trying to create a row for the playful advertiser as well, saying like source system is playful and what is the ID that is mapped in the playful system?
[00:13:19] Venkatesh Mannam: I mean playful reward system. And what's the name that they mentioned in that system?
[00:13:26] Kapil Sreedharan: Oh, okay, so that means is there jc, is there like a playful platform where they go in and configure campaigns?
[00:13:38] Trevor Anderson: They do that on tune, right. Is what you're saying.
[00:13:41] JC Camacho: If. If there is. But it's nothing that's managed on. From us. It's. It's. I think it's all managed on the.
[00:13:47] JC Camacho: On that playful side.
[00:13:48] Kapil Sreedharan: Okay, and do we get that feed
[00:13:57] JC Camacho: as of now? No. The only stuff that we're getting is that with the stuff that is out there, right?
[00:14:01] JC Camacho: Those reference tables that associate the associate like the campaign.
[00:14:08] Venkatesh Mannam: What is it?
[00:14:08] JC Camacho: I sent it earlier those screenshots that I got, but they haven't shared anything else.
[00:14:16] JC Camacho: And I don't know if you guys checked out that whole documentation that these guys had been working on the Dan's had been working on prior.
[00:14:26] JC Camacho: I didn't see anything there that associated any more information than we already got.
[00:14:33] JC Camacho: But I can review that doc again.
[00:14:36] Kapil Sreedharan: Okay. Venki, you want to share your screen with that campaign table?
[00:14:40] Kapil Sreedharan: Just want to explain show JC what we are doing.
[00:14:45] Venkatesh Mannam: Yeah, yeah, sure thing. Give me one.
[00:15:04] Kapil Sreedharan: But you're not working on the campaign table, right? You're working on the advertiser.
[00:15:08] Venkatesh Mannam: Yeah, advertiser, right.
[00:15:18] Kapil Sreedharan: Okay, but Jesse, basically what it is is we are creating this Fluent advertiser and Fluent campaign table.
[00:15:27] Kapil Sreedharan: And this is supposed to have all the advertisers that is managed in all of Fluent's platforms.
[00:15:36] Kapil Sreedharan: By platform, I mean Pulse lead manager. Yeah. And also tune. So. Right.
[00:15:44] JC Camacho: But the playful data flows through tune. Right. So even though it's being quote unquote, source. I see.
[00:15:50] JC Camacho: Again, I'm not entirely sure if we, if we're supposed to say playful data is Fluent data or not.
[00:15:58] JC Camacho: But the thing is where the traffic is flowing through, is flowing through Tune.
[00:16:02] JC Camacho: So every campaign, I think the, the way it's set up, I think it's supposed to be set up from the.
[00:16:08] JC Camacho: On the Tomb site. Right. So if you're looking for advertiser information, it should be linked there.
[00:16:13] Kapil Sreedharan: Okay.
[00:16:14] Kapil Sreedharan: So you know, if we are not setting up, if any of our Adobe theme is not going to playful and setting up advertisers or campaigns, then it won't come into this table.
[00:16:26] JC Camacho: I think.
[00:16:27] Kapil Sreedharan: Yeah.
[00:16:28] JC Camacho: Yeah.
[00:16:28] JC Camacho: I think what we should do early next week is have Venki and everyone who's here right now, but definitely Venki, have a walkthrough of what you guys are building with Kevin and Aaliyah and obviously myself as well explain to them what you're doing because they're the ones that are tying all this stuff together in their dashboards.
[00:16:49] JC Camacho: Right.
[00:16:50] JC Camacho: So if there are any gaps as to what you are trying to build, Aaliyah will be like, so Aaliyah was a resource that was on the upside and then came over.
[00:17:00] JC Camacho: Right. So she has a lot of knowledge from both worlds. Right. That's why she's.
[00:17:05] JC Camacho: She is like, Kevin's like, go to when.
[00:17:07] JC Camacho: When these like these reports have been been created and I think like a conversation with her and actually to have an open, an open like door with her, it will be very critical to.
[00:17:21] JC Camacho: To the success of the table that you're trying to create.
[00:17:26] Venkatesh Mannam: Yeah, J.C. like what we are trying to build is if you look at this example 717 ads, right.
[00:17:31] Venkatesh Mannam: So it is present in all the systems like the TUNE lead manager and the minion and its respect to source system ID.
[00:17:38] Venkatesh Mannam: Suppose in the tune its advertiser ID is 2037.
[00:17:42] JC Camacho: Are you sharing something? Sorry.
[00:17:44] Venkatesh Mannam: Oh, I'm not sharing. Oh, sorry. My.
[00:17:48] Kapil Sreedharan: Yeah, yeah.
[00:17:51] Venkatesh Mannam: So if you look at this example at 717 ads. So we're trying to build this final table.
[00:17:57] Venkatesh Mannam: So in the TUNE system, what Is its advertiser ID like for the 717 ads? It's 2037.
[00:18:04] Venkatesh Mannam: Similarly, in the lead manager, it's respective source advertiser ID.
[00:18:08] Venkatesh Mannam: In the minion it's advertiser ID.
[00:18:10] Venkatesh Mannam: So similarly we are expecting if there is any system configured for the playful as well, we're gonna ingest a new row like 717ads and it's fluent vertical that is consolidated using the net suite system.
[00:18:25] Venkatesh Mannam: And the source system has a playful rewards and its respective ID in its system like the playful reward system.
[00:18:34] Venkatesh Mannam: And in future like we are trying to ingest the NET Suite system data as well.
[00:18:38] Venkatesh Mannam: That way we can populate this row as it's a NET Suite system id.
[00:18:44] Venkatesh Mannam: That way anyone who is trying to join across the system, they would be able to join it across, I mean like in the financial finance system and across multiple sources.
[00:18:55] Venkatesh Mannam: Right. So that's what we are trying to build.
[00:18:59] JC Camacho: And I 1000% understand that.
[00:19:03] JC Camacho: And again I really think that we should talk, we should have a conversation with them because I think they have built something similar to this already where they do the link between.
[00:19:18] JC Camacho: It's most. It's mainly between like netsuite and lead manager and tune. Right.
[00:19:22] JC Camacho: Obviously you're and I say similar Lucy because you guys, you guys are obviously doing taking it a step further.
[00:19:31] JC Camacho: But I think that the links and then any gaps in your knowledge of the linking, of the linkage I think would be answered by you know, by having that open conversation with them.
[00:19:44] Venkatesh Mannam: Okay. Okay.
[00:19:44] JC Camacho: Well again I really think that we should have like a conversation sooner than later.
[00:19:49] JC Camacho: Just set up, let's set up a call with them and we can, you know, we can discuss and get you the answers that you're seeking.
[00:19:59] Venkatesh Mannam: Yeah, yeah. So we saw similar report in NetSuite system. Jesse, if I.
[00:20:05] Venkatesh Mannam: So there is some table in the NET suite we are trying to ingest but we have blocked on the access.
[00:20:11] Venkatesh Mannam: So when Kapil try to analyze analysis this data. So there is some table.
[00:20:21] Venkatesh Mannam: Which has already linkage between the systems.
[00:20:30] Kapil Sreedharan: Yeah. Next week they have a customer table.
[00:20:33] Venkatesh Mannam: Yeah.
[00:20:34] Kapil Sreedharan: So if Disney is a customer, they'll an entry for Disney and here below there, if you scroll down there should be a corresponding id Read Manager id Tune id, Minion id.
[00:20:49] Kapil Sreedharan: But then this also means that you know there's no playful ID I think.
[00:20:55] JC Camacho: Right.
[00:20:56] Kapil Sreedharan: It could be just managed through tune.
[00:20:58] JC Camacho: Through tune. Correct. And that's the thing. Right.
[00:21:06] JC Camacho: Like so Kevin, you know the BI team has a report that is showing numbers for tremendous and they're showing numbers for playful and user wise and all that stuff.
[00:21:18] JC Camacho: So they've already done their homework as to how to link all this stuff at an affiliate advertiser level.
[00:21:26] JC Camacho: Right. So again, you know, we, we should have a conversation with them.
[00:21:33] JC Camacho: So I think, again, I think a lot of the questions that Venki's having might be answered.
[00:21:41] Kapil Sreedharan: Yeah. And the consumers that the. That are going to use this, you know, at least for the phase one is.
[00:21:53] Kapil Sreedharan: Looks like it is Jose. I think they're building it in Domo.
[00:22:01] Kapil Sreedharan: I'm not sure if they're going to use Power bi, but this is a prototype that they will.
[00:22:08] Kapil Sreedharan: So they want adflow, playful syndication, all these bus and see it in one place.
[00:22:15] JC Camacho: Got it.
[00:22:16] Kapil Sreedharan: Yeah. So, yeah, that's the thing.
[00:22:19] Kapil Sreedharan: So that's why we need a dimension table that contains advertiser across these views.
[00:22:26] JC Camacho: No, I get it.
[00:22:28] Trevor Anderson: Is there a way to split out playful specific campaigns in the tune? Yeah, Dimension right now?
[00:22:36] Kapil Sreedharan: Yeah, yeah, there's that affiliate.
[00:22:39] JC Camacho: Yeah. It's based off affiliate id
[00:22:42] Trevor Anderson: and that's
[00:22:43] Kapil Sreedharan: how Venki got the, you know, the account manager name and all those for playful.
[00:22:51] Trevor Anderson: Well, so you know how when the advertiser dim table, we have the source system is tuned right now maybe we can extrapolate that further.
[00:23:07] Trevor Anderson: By saying like, oh, you know, yeah, it comes from tune, but it's actually originates from playful.
[00:23:13] Trevor Anderson: Like.
[00:23:13] JC Camacho: So that's why I think like you maybe we create like two source columns.
[00:23:18] JC Camacho: One's like traffic source ones, maybe origination source.
[00:23:21] JC Camacho: Or we could find a better name for it where if you're looking at this record, one of them would say playful and the other one would say tuned.
[00:23:30] JC Camacho: Because at the end of the day you want somebody to be able to find the path where they need to go fetch data from based on looking at this table.
[00:23:38] JC Camacho: So if somebody says, hey, I need playful data, they would go to your table and see, well, okay, the traffic flows through tune.
[00:23:44] JC Camacho: So I know if I'm looking for revenue data, I have to go to Tune, but you know, they're looking for something else that's specific to playful.
[00:23:51] JC Camacho: Like, okay, I got to go to the playful data to get this stuff. Right.
[00:23:55] JC Camacho: So I think like, as long as the table can answer I think those type of questions, I think it would be definitely useful to the end user.
[00:24:17] JC Camacho: Silence.
[00:24:20] Kapil Sreedharan: Okay, sounds good.
[00:24:21] JC Camacho: Yeah, everyone's.
[00:24:29] Kapil Sreedharan: Okay. So, yeah, so I think we should be okay. Just Venky, check with Brian and ask him to organize.
[00:24:39] Venkatesh Mannam: Yeah, sure thing.
[00:24:40] Kapil Sreedharan: Completely. Yeah.
[00:24:45] JC Camacho: I mean, if you want, I. I mean, yeah, we can include Brian, but I mean, I was saying like, I.
[00:24:51] JC Camacho: If you want, you want me to set up the call. You, you, you're gonna have Brian do it.
[00:24:56] Kapil Sreedharan: No, you can set it up.
[00:24:58] JC Camacho: All right, well, I'll throw that. I'll throw on our calendars either, you know, Monday or Tuesday.
[00:25:03] Kapil Sreedharan: Okay. Yeah. I'm assuming Kevin should. I probably should be also aware of this.
[00:25:09] Trevor Anderson: Oh, no, no.
[00:25:10] JC Camacho: Yeah. I'm gonna include Kevin and Aliyah. Yeah, yeah. Because again, she. You know, it's funny.
[00:25:18] JC Camacho: After our call Kapil she did answer, but I was, like, too late.
[00:25:21] Kapil Sreedharan: Yeah, yeah, yeah. I think, you know, that's also the thing. Right. I.
[00:25:28] Kapil Sreedharan: I guess before we start all of this, we should also talk to, you know, the downstream consumers.
[00:25:36] JC Camacho: I agree.
[00:25:39] Kapil Sreedharan: We only end up talking to them once we hit, you know, these roadblocks or questions.
[00:25:45] JC Camacho: Well, that's what I'm saying, because, I mean, there are a lot of reports out there, so you could also save time on.
[00:25:49] JC Camacho: On the research part as well. Right. Like, yeah. Because again, these guys are.
[00:25:54] JC Camacho: They've been doing it for years now as far as fix, you know, fixing everybody's hiccups.
[00:26:00] Trevor Anderson: Yeah.
[00:26:00] JC Camacho: So, you know, it's something as simple as the revenue definition.
[00:26:06] JC Camacho: You ask five different people, they're going to give you five different answers. Right.
[00:26:09] JC Camacho: And depending on what report you're looking at, that number is not wrong.
[00:26:13] JC Camacho: It's just based on that report and the definition that they.
[00:26:15] JC Camacho: Whoever decided to ask for that report, that number is correct. All right, cool.
[00:26:25] JC Camacho: So I'll set up the call for next week, and then we can. We can go from there.
[00:26:29] Kapil Sreedharan: Okay, cool. Yeah. Thanks, J.C. i know.
[00:26:31] Venkatesh Mannam: Yep. Thanks.
[00:26:33] JC Camacho: Very good.
