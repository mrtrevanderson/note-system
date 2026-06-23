---
title: Venkatesh x Trevor Sync
date: 2026-06-22
time: 07:35-08:05
participants: [Trevor Anderson, Venkatesh Mannam]
fellow_url: https://fluentco.fellow.app/meetings/040000008200E00074C5B7101A82E00807EA06169FB049FA87D9DC010000000000000000100000003E35C481FDC3A84B8B6593846F13978C/
captured_at: 2026-06-23T12:03:00-07:00
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
[ ] Venkatesh Mannam to confirm with Bharat whether source advertiser ID + source system name combination is unique across all source systems.
[ ] Venkatesh Mannam to create FPSO for NetSuite catalog and add to DevOps board to begin NetSuite data ingestion process.
[ ] Venkatesh Mannam to submit weekly updates in Lattice every Friday answering the five questions (AI wins, work completed, upcoming work, blockers, and other items), starting this Friday.
[ ] Follow up with Kapil regarding the naming convention preference (source vs platform) for advertiser table columns.

---

Trevor and Venkatesh discussed technical design decisions for the advertiser table schema, focusing on **primary key structure** and **naming conventions**. They decided to use **source advertiser ID + source system name** as the composite key instead of advertiser name, and agreed to stick with 'source' prefix rather than 'platform' terminology. A significant challenge was identified with NetSuite integration: current records lack source IDs (falling back to advertiser names), which will create duplicate rows when proper NetSuite data arrives. The team decided to implement a **slowly changing dimension Type 2** approach, marking old records as inactive rather than deleting them to prevent breaking downstream dependencies.

Trevor introduced a new process requirement for **weekly updates in Lattice** starting this Friday, which will help with visibility and automatically generate end-of-year summaries. The team also decided to **ingest all NetSuite data** into bronze tables comprehensively rather than selectively, with an FPSO needed to start the ingestion process. Trevor closed by expressing interest in discussing Venkatesh's career goals and aspirations to help guide work assignments and professional development at Fluentco.

### Advertiser Table Schema Design

- Discussion about **naming standards** for advertiser table columns, with debate between 'source' vs 'platform' prefix for advertiser ID and name fields.
- Team reviewed NetSuite schema documentation shared by Kapil, which uses 'platform' terminology and includes internal linkings across systems (Lead Manager, Tune(?), Minion(?)).
- Decided to keep **NetSuite ID as parent record** with Fluent Advertiser ID remaining as the primary key for the advertiser table.
- Agreed to use **source advertiser ID + source system name** as composite key instead of advertiser name, since advertiser names can change and would break primary key stability.
- Trevor will confirm with Kapil about naming convention preference, but team will proceed with 'source' prefix rather than 'platform' for now.

### Fluent Advertiser ID Logic

- Current logic generates Fluent Advertiser ID based on source advertiser ID and name, with **lookup functionality** to reuse existing IDs when advertiser names change.
- Identified potential **collision issue** between Tune and Lead Manager systems, as both use four-digit advertiser IDs (Minion uses GUID format, avoiding overlap).
- Major challenge: **NetSuite records currently lack source IDs** (stored as null), requiring fallback to advertiser name for ID generation until proper NetSuite data arrives.
- When NetSuite source IDs become available, system will create **duplicate rows** for same advertisers—one using name-based ID, one using proper source ID-based ID.
- Team decided to **mark old records as inactive** rather than delete them to avoid breaking downstream systems (like Fluent Campaign table) that may already be using the IDs.
- Will implement **slowly changing dimension Type 2** with status column and end date fields to maintain history of advertiser name changes across all source systems.

### NetSuite Data Ingestion Planning

- Team needs to **create FPSO for NetSuite catalog** and get it on DevOps board to begin data ingestion process, which has been taking considerable time.
- Kapil mentioned that in **Q3**, Ryan may request additional columns like account manager data from NetSuite to be included in Fluent Advertiser and Campaign tables.
- Decided to **ingest all NetSuite data** into bronze tables comprehensively rather than selectively, as more columns will likely be needed over time.
- Silver and gold tables will only be created for **immediate requirements**, but comprehensive bronze layer will enable easy expansion as new downstream needs emerge.

### Weekly Lattice Updates Process

- Trevor introduced requirement for **weekly updates in Lattice** to help compile team status reports more efficiently instead of manually tracking everyone's work.
- Updates include **five questions**: AI wins, work completed this week, plans for next week, blockers, and anything else to share.
- Team members should answer these questions **every Friday starting this Friday** in the Lattice updates section.
- Lattice will use weekly updates to **automatically generate end-of-year summaries**, making the annual review process easier by aggregating weekly progress.
- HR noted the data engineering team is **far ahead of the rest of Fluentco** on providing high-quality updates and maintaining strong internal communication.

### Career Development Discussion

- Trevor expressed interest in understanding **Venkatesh's career aspirations** and personal goals to help with career planning and development at Fluentco.
- Trevor offered to help ensure work assignments align with desired growth areas and can coordinate with Bharat on projects matching **subject matter expertise interests**.
- Discussion will focus on Fluent career path, including **domain preferences** and whether to specialize in certain parts of Databricks or specific product lines.
- Trevor emphasized willingness to share career knowledge from different organizations and help navigate growth both **within and outside Fluentco** if desired.
- Venkatesh agreed to set up time for a deeper career conversation, acknowledging Trevor's expertise would help guide goal achievement and professional development.

### Action Items

- Venkatesh Mannam to confirm with Bharat whether source advertiser ID + source system name combination is unique across all source systems.
- Venkatesh Mannam to create FPSO for NetSuite catalog and add to DevOps board to begin NetSuite data ingestion process.
- Venkatesh Mannam to submit weekly updates in Lattice every Friday answering the five questions (AI wins, work completed, upcoming work, blockers, and other items), starting this Friday.
- Trevor Anderson to follow up with Kapil regarding the naming convention preference (source vs platform) for advertiser table columns.

### Decisions

- **NetSuite ID** will remain as the parent record identifier, with Fluent Advertiser ID continuing as the primary key.
- The primary key will use **source advertiser ID + source system name** instead of advertiser name, since advertiser names can change over time.
- Old NetSuite records (without source IDs) will be **marked as inactive** rather than deleted when new records with proper source IDs arrive, to avoid breaking downstream dependencies.
- The team will implement a **slowly changing dimension Type 2** approach with status column and end date to track advertiser name history.
- **All NetSuite data** will be ingested into bronze tables comprehensively, with silver and gold tables created only for immediate requirements.
- The team will begin submitting **weekly updates in Lattice** every Friday to improve visibility and support automated end-of-year summaries.

## Transcript

[00:00:07] Venkatesh Mannam - A: Hey, Devon. Morning.
[00:00:09] Trevor Anderson: Hey, sorry about that. I just.
[00:00:11] Trevor Anderson: I thought we started that at the 30 minutes there, but forgot I set it to 5 after.
[00:00:20] Trevor Anderson: I need to change that. I don't like that.
[00:00:23] Venkatesh Mannam - B: Oh,
[00:00:25] Trevor Anderson: I. I always like think the meetings start at the hour mark or the 30 minute mark, not five after.
[00:00:36] Venkatesh Mannam - B: Okay. But.
[00:00:38] Trevor Anderson: Sorry, I. I didn't mean to interrupt.
[00:00:41] Venkatesh Mannam - A: We are almost done there.
[00:00:42] Venkatesh Mannam - B: Yeah.
[00:00:43] Trevor Anderson: Okay.
[00:00:44] Venkatesh Mannam - B: Yeah.
[00:00:45] Trevor Anderson: Sweet. Wow.
[00:00:47] Venkatesh Mannam - A: And did we come up with any naming standards ever? Like if you are following the other chart.
[00:00:53] Venkatesh Mannam - A: So I'm waiting for the column names, that couple. And you are thinking about.
[00:01:02] Venkatesh Mannam - B: Right.
[00:01:05] Trevor Anderson: What was this for the advertiser table?
[00:01:07] Venkatesh Mannam - A: Yeah, advertiser table.
[00:01:08] Venkatesh Mannam - A: Because right now we store them as a source advertiser id, Source advertiser name. When I.
[00:01:15] Venkatesh Mannam - A: When we last discussed, we were planning to use something similar with the Net Suit system, right?
[00:01:19] Venkatesh Mannam - A: Like something platform ID or platform advertiser id, something like that.
[00:01:24] Venkatesh Mannam - B: Extinct,
[00:01:32] Trevor Anderson: You know. Yeah. My two cents is that source makes sense. Platform does the same job. I mean.
[00:01:41] Venkatesh Mannam - B: Yeah, yeah.
[00:01:43] Trevor Anderson: For those two columns, I like the source naming convention, you know, that works.
[00:01:50] Trevor Anderson: So source system, Source advertiser id, source advertiser name. That. That makes sense to me.
[00:02:01] Trevor Anderson: I heard that Capill maybe suggested in our last call that it could be platform.
[00:02:07] Trevor Anderson: It was it him that suggested?
[00:02:10] Venkatesh Mannam - B: Yeah, yeah, yeah.
[00:02:11] Venkatesh Mannam - A: I mean like when we are going through the Net Suit system, right.
[00:02:14] Venkatesh Mannam - A: And there is some dog that he shared.
[00:02:17] Venkatesh Mannam - B: Right. Let me open that.
[00:02:26] Trevor Anderson: Oh yes, the schema they shared.
[00:02:29] Venkatesh Mannam - A: Then it's a table schema.
[00:02:31] Venkatesh Mannam - B: So in that we have something like platform.
[00:02:35] Venkatesh Mannam - A: Where is this?
[00:02:58] Venkatesh Mannam - B: Yeah, I got that. If you can see my screen.
[00:03:10] Venkatesh Mannam - A: So when we are going through the other call on Thursday, we saw this table driven the customer table.
[00:03:20] Venkatesh Mannam - A: So in this like they mentioned as a.
[00:03:23] Venkatesh Mannam - A: This idea is Internet internal Net suit customer id and they have the company name and I believe it should be the advertiser name.
[00:03:34] Venkatesh Mannam - A: And here in the same table we found all the internal linkings across the systems.
[00:03:42] Venkatesh Mannam - A: Like this is Netsuite lead manager customer id client id. And this is Tune advertiser id.
[00:03:49] Venkatesh Mannam - A: And here we have Minion and the respect to active status flag and also
[00:04:00] Venkatesh Mannam - B: inactive active flag.
[00:04:07] Venkatesh Mannam - A: I think somewhere I saw the vertical.
[00:04:09] Venkatesh Mannam - B: Yeah, here also we have vertical.
[00:04:17] Trevor Anderson: Does it reference platform?
[00:04:23] Trevor Anderson: See that?
[00:04:26] Trevor Anderson: Does it have advertiser ID there?
[00:04:29] Venkatesh Mannam - B: Yeah, yeah, yeah.
[00:04:30] Venkatesh Mannam - A: Advertiser id.
[00:04:33] Trevor Anderson: So I think it's a customer table. There must be a advertiser. Oh, there is not.
[00:04:45] Trevor Anderson: It has a tune advertiser ID as a custom field.
[00:04:52] Venkatesh Mannam - A: I think these.
[00:04:52] Venkatesh Mannam - A: This client ID is also lead manager advertiser ID Trevor And this Tune Advertiser ID is Tune Advertiser ID and I think this ID field is there at the primary queue.
[00:05:05] Venkatesh Mannam - A: Maybe they are generating some ID in the netsuites.
[00:05:07] Trevor Anderson: Yeah, I think it's that one because this is per the description of this schema.
[00:05:13] Trevor Anderson: It's the advertiser records. So that's probably it we'll have to see once we ingest it. Right?
[00:05:20] Venkatesh Mannam - B: Yeah, yeah, that's right.
[00:05:25] Trevor Anderson: I'm fine with keeping it, but keeping it as source. But let me just put a.
[00:05:33] Trevor Anderson: Let me just put the open question
[00:05:34] Trevor Anderson: for Phil right now.
[00:06:12] Venkatesh Mannam - B: Group.
[00:06:32] Venkatesh Mannam - B: It.
[00:07:06] Trevor Anderson: Okay, yeah, yeah, I feel like that. And then. So we're keeping Netsuite ID as the parent record.
[00:07:17] Trevor Anderson: Still the name?
[00:07:20] Venkatesh Mannam - B: Yeah, I think we stick to the.
[00:07:22] Venkatesh Mannam - A: Net Suite id.
[00:07:23] Venkatesh Mannam - B: Right.
[00:07:24] Trevor Anderson: Okay. And Fluid Advertiser ID is still primary key.
[00:07:32] Venkatesh Mannam - B: Yeah.
[00:07:33] Trevor Anderson: Okay.
[00:07:35] Trevor Anderson: And so what's left on this? Is it pretty much done or. It says it's in review right now.
[00:07:41] Venkatesh Mannam - A: Yeah, I mean like I'm waiting for the column names if there are any changes but told like we can hold on until we finalize the column names if we are done.
[00:07:53] Venkatesh Mannam - A: I think that's it. The thing that we are discussing that me and Bharat were discussing, Trevor is.
[00:08:00] Venkatesh Mannam - A: Let me share my screen. So right now we are generating.
[00:08:08] Venkatesh Mannam - B: Give me one sec.
[00:08:09] Venkatesh Mannam - A: Yeah, so right now we are generating the fluent advertiser ID based on the source advertiser ID and the source advertiser name.
[00:08:16] Venkatesh Mannam - A: Right.
[00:08:18] Venkatesh Mannam - A: So there is some valid copilot comment that gave and when I checked with Brian he told like that's.
[00:08:24] Venkatesh Mannam - A: That can be a valid.
[00:08:25] Venkatesh Mannam - A: The thing is Disney can be changed to Disney ABC or Disney 1234 in future without changing the ID advertiser ID
[00:08:38] Trevor Anderson: and the name can change.
[00:08:40] Venkatesh Mannam - A: Yeah, the name can change in future as well.
[00:08:44] Venkatesh Mannam - B: So.
[00:08:46] Venkatesh Mannam - A: So internally I added some logics.
[00:08:49] Venkatesh Mannam - A: What that does is even though if there is a change in advertiser name now it looks at the existing table and if there is, I mean like obviously they have to stick to the same advertiser id right.
[00:09:01] Venkatesh Mannam - A: In the. In the respective source system.
[00:09:04] Venkatesh Mannam - A: So it checks on the source advertiser ID by using some lookup and if that's ID is same even though that when there is a change in source advertiser name it will not create new one but it will stick to the existing fluent advertiser ID without regenerating.
[00:09:20] Venkatesh Mannam - A: That way we don't end up creating multiple rows for the same advertiser ID in the same system.
[00:09:26] Venkatesh Mannam - B: Right?
[00:09:27] Trevor Anderson: Yes.
[00:09:28] Venkatesh Mannam - A: Yeah, but I thought that there is, that can be a possibility and I was Explaining him that there is some internal logic that looks up that look up at the existing table and reuse the same fluent advertiser id.
[00:09:42] Trevor Anderson: How does that work?
[00:09:45] Venkatesh Mannam - A: Internally I added that logic like whenever a source system, I mean like there is tune advertiser table or the minion advertiser table.
[00:09:53] Venkatesh Mannam - B: Right.
[00:09:54] Venkatesh Mannam - A: So it reads the whole table. And if it comes with the. When it tries to create a new fluent id.
[00:10:00] Venkatesh Mannam - A: Right, Fluent advertiser id, it looks up at the existing source advertiser id.
[00:10:05] Venkatesh Mannam - A: If it already present in the existing source.
[00:10:08] Venkatesh Mannam - A: I mean like the final fluent advertiser table, it uses the same fluent id. Fluent advertiser id.
[00:10:15] Venkatesh Mannam - A: Sorry, Rather than generating a new one.
[00:10:25] Trevor Anderson: Will that break if there's Multiple source advertiser IDs. Across. Across systems?
[00:10:40] Trevor Anderson: Like because you need the advertiser name to make it unique. Right?
[00:10:45] Venkatesh Mannam - B: Yeah, yeah. Oh yeah, that's also a good question.
[00:10:49] Venkatesh Mannam - A: I think the minion one is a GUID format, Trevor.
[00:10:54] Venkatesh Mannam - A: And the only overlap can be happening with tune and lead manager.
[00:11:01] Venkatesh Mannam - A: But let me see, I think lead manager is something with four digit.
[00:11:09] Venkatesh Mannam - B: Sorry, four. Yeah, four digitizer.
[00:11:37] Venkatesh Mannam - A: Yeah, Q needs four digit Trevor and lead manager.
[00:11:57] Venkatesh Mannam - A: Client id.
[00:12:00] Venkatesh Mannam - B: Oh, here also we have four digit here, three digit, five digit.
[00:12:13] Venkatesh Mannam - A: There can be overlap. If they match with one of the id,
[00:12:30] Trevor Anderson: Wouldn't it be unique on the source and the source id.
[00:12:40] Trevor Anderson: Oh like, like if you had a column that said source advertiser ID is 1, 2, 3 and source is tune, that would make it unique, wouldn't it?
[00:12:58] Venkatesh Mannam - A: But even though the same problem can come, right if.
[00:13:01] Trevor Anderson: No, no it wouldn't.
[00:13:03] Venkatesh Mannam - A: Oh, the source name. Right, okay.
[00:13:04] Trevor Anderson: Yeah, yeah, yeah, yeah. Because the source name shouldn't change.
[00:13:10] Trevor Anderson: It should always be coming from that system of origin.
[00:13:15] Venkatesh Mannam - A: Yeah, the only issue that I came up with, Bharat as well regarding that one is Bharat.
[00:13:20] Venkatesh Mannam - A: Right now the NET suite system ID is not present.
[00:13:24] Venkatesh Mannam - B: Right.
[00:13:25] Venkatesh Mannam - A: So it can go with the primary key collision.
[00:13:28] Venkatesh Mannam - A: So for that what we planned is if the ID is null, fall back to the source advertising name.
[00:13:40] Trevor Anderson: Yeah, yeah, but yeah, if we have primary key on source advertiser ID and source name, then that shouldn't change.
[00:13:54] Trevor Anderson: Like that shouldn't be modified.
[00:13:56] Trevor Anderson: Yes, the advertiser name can change, but then the primary key should be stay consistent.
[00:14:02] Venkatesh Mannam - B: Yeah, yeah,
[00:14:09] Trevor Anderson: I think we should just do that.
[00:14:15] Venkatesh Mannam - A: ID and the source system.
[00:14:16] Venkatesh Mannam - B: Right?
[00:14:17] Trevor Anderson: Yeah, like we should have a column
[00:14:19] Trevor Anderson: that says where it's coming from. Unless we already did. I think we already have that.
[00:14:24] Venkatesh Mannam - B: No, yeah, yeah, yeah, we already have that. Yeah,
[00:14:33] Trevor Anderson: You're gonna have to check if
[00:14:34] Trevor Anderson: that's unique, but I think it is.
[00:14:39] Venkatesh Mannam - B: Yeah, yeah, I can check that out. Yeah.
[00:14:42] Trevor Anderson: Can you confirm with bar on that?
[00:14:44] Trevor Anderson: But that should get us away from advertiser creating the primary key with advertiser name which is a changing field.
[00:14:53] Venkatesh Mannam - B: Yeah, yeah.
[00:14:56] Venkatesh Mannam - A: The only caveat that we discussed, Trevor, is only for the time being this caveat is so the Net Suit system, we don't have the net system IDs.
[00:15:07] Venkatesh Mannam - B: Right.
[00:15:08] Venkatesh Mannam - A: So for that we may have to add some internal logic and also the merge becomes quite complex because for the same advertiser in the NET suite system, after once we ingest the Net Suit source system data, the one that we are discussing, we're going to end up creating two rows.
[00:15:30] Venkatesh Mannam - A: The earlier row we should have some internal complex logic to mark them as a inactive, which has like source advertiser ID as null.
[00:15:39] Venkatesh Mannam - A: For the Net suit rows and the other rows we have to mark them as active.
[00:15:47] Venkatesh Mannam - B: Right.
[00:15:50] Trevor Anderson: Like if it's not coming in through NetSuite anymore.
[00:15:54] Venkatesh Mannam - A: I mean like right now, for the. Right now at the moment we don't have this. Net suite IDs. Right.
[00:16:00] Venkatesh Mannam - A: And we mark them as null Right now.
[00:16:02] Venkatesh Mannam - B: Yes.
[00:16:02] Venkatesh Mannam - A: So in next, in a week or so if we ingest, if we start ingesting the NetSuite data.
[00:16:09] Venkatesh Mannam - A: So we're going to end up creating a new source, I mean a new fluent advertiser ID for the Net Suit system advertisers and downstream users, if they are using anything, they may have to change it.
[00:16:23] Venkatesh Mannam - B: Right.
[00:16:24] Venkatesh Mannam - A: Because for the same nets, for the same advertiser, like Disney in netsuite, we will have.
[00:16:31] Venkatesh Mannam - A: We will end up creating two rows, one we created with fallback logic of source advertiser name in the source system.
[00:16:39] Venkatesh Mannam - A: And in a week or so if we start ingesting the Net suit data, we will ingest, I mean we will create a new row again which starts, I mean like which creates.
[00:16:53] Venkatesh Mannam - A: Using the source ID and the source system once we start ingesting the Net Suits system. Right data.
[00:17:02] Trevor Anderson: Yeah, so you should see that come in again.
[00:17:05] Trevor Anderson: Right.
[00:17:05] Trevor Anderson: The same, same source id, same source name. Yeah, but the advertiser name is going to be different.
[00:17:13] Venkatesh Mannam - B: Right.
[00:17:17] Trevor Anderson: Like you said, Disney changes their name.
[00:17:21] Venkatesh Mannam - A: Yeah, yeah, no, no, I'm talking about the other scenario Trevor right now.
[00:17:31] Trevor Anderson: Sorry, maybe I'm not following.
[00:17:33] Venkatesh Mannam - B: Let me add that.
[00:17:46] Venkatesh Mannam - A: Right now we don't have this data.
[00:17:48] Venkatesh Mannam - B: Right.
[00:17:48] Venkatesh Mannam - A: NetSuite system data.
[00:17:51] Venkatesh Mannam - B: Yep.
[00:17:52] Venkatesh Mannam - A: And if we create the fluent advertiser ID
[00:17:57] Venkatesh Mannam - B: based on
[00:17:59] Venkatesh Mannam - A: source ID
[00:18:02] Venkatesh Mannam - B: source ID
[00:18:03] Venkatesh Mannam - A: plus system name for Disney. I mean like since we don't have the Net suit system source IDs.
[00:18:12] Venkatesh Mannam - A: We're gonna fall back to the advertiser name. I mean like NET suit advertiser ID.
[00:18:19] Venkatesh Mannam - A: Sorry, NET suit advertiser name plus NetSuite system name.
[00:18:23] Venkatesh Mannam - A: And for Disney, we end up creating advertise rate. I mean like fluent advertiser ID as 1, 2, 3.
[00:18:30] Venkatesh Mannam - A: Now when the system comes with the net, I mean like when the Net Suit system data comes for the same advertiser id, we need to create a new row which will end up like 3, 4, 5 because we generate based on the source ID plus source name.
[00:18:52] Venkatesh Mannam - A: Right. I mean the Net Suit this is ended up creating using advertiser name +netsuite
[00:19:02] Trevor Anderson: is 1, 2, 3. A netsuite record.
[00:19:06] Venkatesh Mannam - B: Yep. Yeah.
[00:19:07] Venkatesh Mannam - A: Both are net suite records.
[00:19:08] Venkatesh Mannam - B: Yeah.
[00:19:13] Trevor Anderson: Source ID NetSuite. What is NetSuite?
[00:19:16] Venkatesh Mannam - A: In this case, NetSuite is a system name, right? Source System name.
[00:19:20] Trevor Anderson: Oh,
[00:19:23] Venkatesh Mannam - B: this problem.
[00:19:23] Trevor Anderson: Source id. Oh, we don't have source id.
[00:19:25] Venkatesh Mannam - B: Yeah, yeah, yeah, that's right. Yeah.
[00:19:29] Venkatesh Mannam - A: With the road, that's really gonna need
[00:19:30] Trevor Anderson: to be refactored or deleted or something.
[00:19:35] Venkatesh Mannam - B: Yeah, yeah.
[00:19:39] Trevor Anderson: Advertiser name. Oh, I see, I see.
[00:19:43] Trevor Anderson: Okay, yeah, I see what you're dealing with now.
[00:19:48] Trevor Anderson: I. I'd say we have to just
[00:19:50] Trevor Anderson: get rid of them instead of keeping them and saying like inactive because technically it's an old design.
[00:19:59] Trevor Anderson: Right?
[00:19:59] Trevor Anderson: We don't. Yeah, we don't want that. Top 1, 2, 3. We don't want that because we want 3, 4, 5.
[00:20:07] Venkatesh Mannam - A: Yeah, that's right.
[00:20:08] Venkatesh Mannam - B: Yeah.
[00:20:08] Trevor Anderson: So it's just going to need to be overwritten. We have to delete those rows.
[00:20:16] Venkatesh Mannam - A: The problem is downstream, side by stream, which are building with the Fluent campaign.
[00:20:21] Venkatesh Mannam - A: They're going to start using my Fluent advertiser id.
[00:20:24] Venkatesh Mannam - B: Right.
[00:20:26] Venkatesh Mannam - A: If they are done with their work. And we may have to clean up that system as well.
[00:20:33] Venkatesh Mannam - A: I mean like that table as well.
[00:20:35] Trevor Anderson: We will. Unless you, unless you somehow keep that primary key.
[00:20:47] Trevor Anderson: But if you're generating it, it's going to be complicated, right?
[00:20:51] Venkatesh Mannam - B: Yeah, yeah, that's right.
[00:20:55] Venkatesh Mannam - A: So in this case, what Bharat was saying is we can end up marking this as inactive and this one, this row would be active.
[00:21:05] Trevor Anderson: I know, but isn't that the same problem as just deleting it?
[00:21:10] Venkatesh Mannam - A: Yeah, I mean like they're gonna have
[00:21:11] Trevor Anderson: to switch it anyways, right?
[00:21:13] Venkatesh Mannam - B: Yeah.
[00:21:13] Venkatesh Mannam - A: Why we ended up marking them as inactive if down.
[00:21:16] Venkatesh Mannam - A: If some reason, if for some reason downstream is starting, started ingesting this data and using that data and we don't want to break their process.
[00:21:26] Venkatesh Mannam - B: Right. So.
[00:21:33] Trevor Anderson: Yeah,
[00:21:36] Trevor Anderson: it's just keeping those Rows in there is going to make it messy. But
[00:21:41] Trevor Anderson: they both follow different conventions. Right. So.
[00:21:51] Trevor Anderson: I mean we could mark it in as an active for temporary, but we definitely probably want to get rid of the.
[00:21:58] Trevor Anderson: Those records eventually.
[00:22:01] Venkatesh Mannam - B: Yeah, yeah.
[00:22:03] Trevor Anderson: So I would say when the time
[00:22:04] Trevor Anderson: comes to creating 3, 4, 5, we need a ticket, a cleanup ticket to address like actually removing those records and migrating anything that's using them.
[00:22:20] Venkatesh Mannam - B: Yeah, yeah, okay.
[00:22:24] Trevor Anderson: I mean that's fine.
[00:22:25] Trevor Anderson: Like we could, we could do that.
[00:22:26] Trevor Anderson: That's fine.
[00:22:27] Trevor Anderson: But it's just. Yeah.
[00:22:31] Trevor Anderson: Too conflicting logic.
[00:22:34] Trevor Anderson: Right?
[00:22:35] Venkatesh Mannam - B: Yeah, yeah, that's right. Yeah.
[00:22:38] Venkatesh Mannam - A: The other reason we thought of marking them is an active travel in the, in either of the system like lead manager Tune.
[00:22:46] Venkatesh Mannam - B: Right.
[00:22:47] Venkatesh Mannam - A: So for the same Disney if earlier, for some reason the source ID is 1, 2, 3 and later they change to 345.
[00:22:56] Venkatesh Mannam - B: Right.
[00:22:58] Venkatesh Mannam - A: In lead manager and tune they both, I mean like those two rows are still present in the, in their respective source systems and they mark their status as inactive or deleted.
[00:23:18] Venkatesh Mannam - A: I mean for the same advertiser in this, in a, in a tune system of the lead Manager, I see two rows with two different IDs right now in the production tables.
[00:23:34] Trevor Anderson: And same tune.
[00:23:36] Trevor Anderson: Like tune has two rows for the same advertiser.
[00:23:41] Venkatesh Mannam - A: Yeah, that's right, but with two different IDs. Advertiser IDs.
[00:23:45] Trevor Anderson: Okay.
[00:23:46] Venkatesh Mannam - A: The reason can be earlier they mark that row as like some so and so advertiser ID abc.
[00:23:52] Venkatesh Mannam - A: And later they change to yes. Yeah, xyz, something like that.
[00:24:01] Venkatesh Mannam - A: And we store those two rows in our final advertiser ID table at fluent Advertiser ID table.
[00:24:08] Venkatesh Mannam - A: So for that reason we thought like this is not a, this can be a similar scenario for the net suit rows as well.
[00:24:18] Venkatesh Mannam - A: So that's why we thought like we mark this as inactive and we mark this as active.
[00:24:23] Trevor Anderson: Yeah.
[00:24:23] Trevor Anderson: No, for that second scenario that you just described where the advertiser name changes will want to mark the old advertiser names as inactive.
[00:24:37] Trevor Anderson: So I guess we're doing a slowly changing dimension. Type 2. We're keeping history here.
[00:24:45] Venkatesh Mannam - B: Yeah, yeah.
[00:24:46] Trevor Anderson: So we'll need, yeah, we'll need like some, some identifier columns. Right. Like we need status.
[00:24:55] Venkatesh Mannam - B: Yeah, that's right.
[00:24:56] Trevor Anderson: I think also we need a date. We need two dates. We need like a create.
[00:25:05] Venkatesh Mannam - A: Yeah, create date. Update date. Yeah, we have those columns.
[00:25:08] Trevor Anderson: Yeah, Well we need a end date.
[00:25:19] Venkatesh Mannam - B: Oh, okay.
[00:25:20] Trevor Anderson: Like when it was marked as inactive.
[00:25:26] Venkatesh Mannam - B: Yeah, yeah.
[00:25:31] Trevor Anderson: And when.
[00:25:32] Venkatesh Mannam - B: Yeah, yeah.
[00:25:38] Trevor Anderson: Okay. No, should still have the same parent, right?
[00:25:45] Venkatesh Mannam - A: Yeah, that's right.
[00:25:46] Venkatesh Mannam - B: Yeah.
[00:25:46] Trevor Anderson: Okay. Okay.
[00:25:52] Trevor Anderson: Interesting. For NetSuite. We'll need a new catalog.
[00:26:00] Venkatesh Mannam - A: Yep, absolutely.
[00:26:01] Trevor Anderson: Do we have an open FPSO for creating the catalog right now?
[00:26:06] Venkatesh Mannam - A: I don't think we have, but we can create one.
[00:26:09] Venkatesh Mannam - B: Right.
[00:26:10] Trevor Anderson: We should probably get on top of that, considering how long it's been taking.
[00:26:18] Trevor Anderson: First step is create the ftso,
[00:26:23] Trevor Anderson: and
[00:26:23] Trevor Anderson: then next, I guess, is getting the API credentials. After that. But get it on the DevOps board.
[00:26:30] Trevor Anderson: Make them aware of it. Okay. NetSuite data infernetes. We did.
[00:26:40] Trevor Anderson: I know we only need certain things from this, but. Yeah, probably start with that.
[00:26:47] Trevor Anderson: I mean, I guess it would probably be good to get everything, though, to be honest.
[00:26:56] Venkatesh Mannam - B: And.
[00:26:57] Venkatesh Mannam - A: And also, Trevor, like, if you look at the.
[00:27:00] Venkatesh Mannam - A: I mean, like, when I was talking to Kapil on Thursday, so he was saying, like, in Q3, Ryan can come up with more requirements in our Fluent Advertiser table or the Fluent Campaign table, like their respect to account managers or so and so which is.
[00:27:17] Venkatesh Mannam - A: Which is present in our. In our NetSuite tables.
[00:27:21] Venkatesh Mannam - A: So what Kapila and me are thinking is, why don't we ingest all the data?
[00:27:27] Venkatesh Mannam - A: I mean, like, all the information as a browse.
[00:27:31] Venkatesh Mannam - A: And slowly, if someone downstream requires more columns like Account Manager or Account Manager ID in our tables, we can end up including those columns.
[00:27:42] Venkatesh Mannam - A: Right. In our final Fluent tables.
[00:27:44] Trevor Anderson: Yeah, we. We should just get everything in the bronze. Yeah, I agree.
[00:27:49] Trevor Anderson: Because most likely we'll, you know, pull more and more from this source.
[00:27:55] Venkatesh Mannam - B: Right.
[00:27:58] Trevor Anderson: We should just create a bunch of different tables in bronze.
[00:28:02] Venkatesh Mannam - B: Yeah, yeah.
[00:28:02] Trevor Anderson: Come from that suite. Yeah.
[00:28:04] Trevor Anderson: And then only, like, we only need to do silver and gold for the requirements for right now, but.
[00:28:11] Venkatesh Mannam - B: Yeah.
[00:28:12] Trevor Anderson: Yeah, at least we'll have everything in bronze.
[00:28:16] Venkatesh Mannam - B: Yeah, that's good. Yeah.
[00:28:17] Trevor Anderson: Okay, good. One thing before we leave.
[00:28:26] Trevor Anderson: I don't know if Fluent is rolling this out quite yet, but for our tech teams, what we want to start doing is see weekly updates in the Lattice application.
[00:28:35] Venkatesh Mannam - B: Okay. Okay.
[00:28:40] Trevor Anderson: And this will help for me. I. I provide a weekly team update every. Every week.
[00:28:48] Venkatesh Mannam - B: Okay.
[00:28:49] Trevor Anderson: But right now I have to, like, manually go track everyone, what everyone's doing.
[00:28:57] Trevor Anderson: But I can probably pull from these updates as well that everyone is posting to.
[00:29:03] Trevor Anderson: But it'd be good to see, like, if you could go, well, have you been inside Lattice at all?
[00:29:10] Venkatesh Mannam - B: Yeah, yeah, I was in there. Yeah.
[00:29:13] Trevor Anderson: Yeah. So there's like a update section, shows my updates.
[00:29:19] Venkatesh Mannam - B: Give me one sec. Yeah.
[00:29:21] Venkatesh Mannam - A: Updates.
[00:29:22] Venkatesh Mannam - B: Yeah.
[00:29:23] Trevor Anderson: Yeah. And there's like, five questions in there.
[00:29:25] Venkatesh Mannam - B: Oh, yeah, yeah.
[00:29:28] Trevor Anderson: AI wins. You know what? What did you do this week? What? What are you going to do next week.
[00:29:36] Trevor Anderson: Any blockers and anything else you would like to share? Yeah.
[00:29:41] Trevor Anderson: So like every week on Friday, if you could answer these questions, that would be really helpful.
[00:29:49] Venkatesh Mannam - B: Yep.
[00:29:50] Trevor Anderson: Yeah, yeah, starting this Friday, that would be great.
[00:29:54] Trevor Anderson: And yeah, I think, I'm not sure if you've done like end of year write ups or anything like that, but
[00:30:02] Venkatesh Mannam - A: yeah, we, we did last year and earlier we used to have like quarterly goals as well, traffic, but for some reason they removed those because that makes like a job harder for all the managers.
[00:30:16] Venkatesh Mannam - A: I mean like you guys came very recently. Like earlier Dan used to take care
[00:30:22] Trevor Anderson: of each and every person, like 20 to 30 people. I know it's probably insane.
[00:30:28] Venkatesh Mannam - B: Yeah.
[00:30:30] Venkatesh Mannam - A: So for some reason they removed those quarterly goals and they started only yearly goals.
[00:30:38] Trevor Anderson: Yeah. Have you set yearly goals for 20,
[00:30:42] Trevor Anderson: 26
[00:30:45] Trevor Anderson: or is it like more of like an end of year?
[00:30:47] Venkatesh Mannam - A: Yeah, end of the year I think they will give some CSV format, something like that and they ask us like, what have you done?
[00:30:57] Venkatesh Mannam - A: What are you planning to do next year? Something like those.
[00:31:01] Trevor Anderson: Okay, I see.
[00:31:03] Trevor Anderson: Well, for those, for the end of year summary, like Lattice will use all these weekly updates to provide a summary.
[00:31:09] Trevor Anderson: So it'll be good to, you know, post every week and then you, you know, you can have that information pretty much ready to go at the end of the year.
[00:31:16] Venkatesh Mannam - B: Yeah.
[00:31:16] Venkatesh Mannam - A: Yeah, that makes easier.
[00:31:18] Venkatesh Mannam - B: Yeah.
[00:31:21] Trevor Anderson: But outside of Lattice, what would be helpful is if we could.
[00:31:25] Trevor Anderson: And I'll, I'll think about a good way to go about this, but I would love to hear more about your career aspirations and you know, what you, you know, what your personal goals are with Fluent and you know, where you like to be at in, you know, next year and in a couple years.
[00:31:47] Trevor Anderson: So I can help you, you know, in your career planning as much as I can.
[00:31:54] Venkatesh Mannam - B: Right, yeah. Yeah, that makes sense. Yeah.
[00:31:56] Trevor Anderson: Just want to make sure you like what you're working on.
[00:32:00] Trevor Anderson: Like the content fits with the content that you're, you know, you're a part of is what you want to be doing.
[00:32:09] Venkatesh Mannam - B: Right.
[00:32:12] Trevor Anderson: It, you know, there's a limited space and where I could maybe, you know, your subject matter expertise, you want to be focusing on a certain part of the platform.
[00:32:24] Trevor Anderson: Right.
[00:32:24] Trevor Anderson: Like I could help you with, you know, planning with Brian on giving you the items that are more related to where you want to, you know, be right and things like that.
[00:32:36] Trevor Anderson: So yeah, I obviously can't wait for Magic Wand and just make everything happen, but I can at least like try to put you in the right direction.
[00:32:48] Trevor Anderson: Right.
[00:32:49] Venkatesh Mannam - B: Yeah.
[00:32:50] Trevor Anderson: And offer any Advice that I can, but that's something I've done in the
[00:32:56] Trevor Anderson: past just to,
[00:33:00] Trevor Anderson: you know, make sure
[00:33:02] Trevor Anderson: you're getting the fulfillment that you're looking for. Right.
[00:33:05] Trevor Anderson: As.
[00:33:06] Trevor Anderson: As much as I can.
[00:33:07] Trevor Anderson: So.
[00:33:08] Trevor Anderson: But would love to have that open discussion with you.
[00:33:11] Trevor Anderson: Just make sure that you're happy and doing, you know, doing what you want out of your career.
[00:33:20] Venkatesh Mannam - A: Yeah, definitely, we'll do Trevor, like, because you, you got more expertise so you would be like correct person to guide us, like to achieve our goal.
[00:33:28] Venkatesh Mannam - A: Something like that.
[00:33:29] Venkatesh Mannam - B: Yeah, I agree.
[00:33:30] Trevor Anderson: Yeah, yeah, definitely. And you know, I'm going, I'm learning
[00:33:35] Trevor Anderson: with you and
[00:33:38] Trevor Anderson: you know, I'm, you know, going through my own career journey as well and learning from different organizations and seeing, you know, how to grow and how to, you know, best fit.
[00:33:50] Trevor Anderson: But I'm, I'm always willing to pass that knowledge down to whoever I'm working with as well.
[00:33:56] Trevor Anderson: So that's why I'm, you know, inviting the open conversation because I think sharing these ideas can definitely help you and with Fluent, you know, your career with Fluent, but outside of Fluent as well, if you, you know, are looking for something else, I could definitely help navigate that for you.
[00:34:16] Trevor Anderson: You know, so just wanted to kind of offer that and, and have that, you know, open discussion with you, but we could start, start small on that and grow throughout the year.
[00:34:31] Venkatesh Mannam - B: Yeah, yeah.
[00:34:33] Trevor Anderson: But yeah, definitely want to hear like, you know, kind of, you know, focus on, let's start with Fluent, you know, initially, let's talk about kind of the things that you're working on here as a data engineer and see like your career path with fluency.
[00:34:51] Trevor Anderson: Like, do you, you know, are you, do you feel stronger in a particular domain or part of Databricks or part of a certain product line here at Fluid, you know, do you want to keep working on a certain domain or work more on a different domain or something like that?
[00:35:11] Trevor Anderson: Right, Yeah, I kind of talk about those kind of things.
[00:35:15] Trevor Anderson: And we can try to grab draft out some personal goals so we can try to achieve those.
[00:35:25] Venkatesh Mannam - B: Yeah, definitely.
[00:35:26] Venkatesh Mannam - A: We'll set up some time and we can discuss slowly.
[00:35:31] Trevor Anderson: Yeah, that's great. But yeah, for right now, just the weekly, just a weekly update, that'd be great.
[00:35:38] Trevor Anderson: But yeah, that's all pretty much update on mine.
[00:35:40] Trevor Anderson: I think they're going to roll this out like across all affluent.
[00:35:44] Trevor Anderson: But yeah, speaking with HR on this, they said we're the, our developer, our tech team or even data engineering specifically is very far ahead on providing high quality updates to the organization.
[00:36:05] Trevor Anderson: So we're far beyond the rest of the company when it comes to this. Yeah, we're on top of our stuff.
[00:36:12] Trevor Anderson: You know, we're doing good job.
[00:36:14] Venkatesh Mannam - A: Oh, yeah, that's nice to hear.
[00:36:15] Venkatesh Mannam - B: Yeah.
[00:36:16] Trevor Anderson: Yeah. So I think our communication in our team internally is very strong, like.
[00:36:24] Venkatesh Mannam - A: Yeah, yeah, I agree.
[00:36:25] Trevor Anderson: We're on top of everything and we know what's going on and we track things very well and there's a lot of, like, discourse and active communication.
[00:36:36] Trevor Anderson: Right. So they were doing a great job on that part.
[00:36:39] Venkatesh Mannam - A: Yeah, yeah, I agree. Because earlier in my companies, I never see.
[00:36:44] Venkatesh Mannam - A: I never saw this type of communication among the team and there is, like, lack of updates to the manager as well in my earlier projects.
[00:36:54] Venkatesh Mannam - A: I mean, new healthier companies.
[00:36:56] Venkatesh Mannam - B: Yeah.
[00:36:57] Venkatesh Mannam - A: Good team, actually.
[00:36:58] Venkatesh Mannam - B: Yeah,
[00:37:01] Trevor Anderson: yeah, there's a lot of communication.
[00:37:05] Trevor Anderson: It's almost hard for me to just
[00:37:07] Trevor Anderson: stay on top of just the. The cons that are going on, but it's better to have it than not to have it.
[00:37:12] Trevor Anderson: Like, if I didn't have this. This transparency, I would be in a much worse position.
[00:37:20] Venkatesh Mannam - B: Yeah, yeah, yeah.
[00:37:23] Trevor Anderson: Okay. Sweet. Well, thanks for the updates on your.
[00:37:26] Trevor Anderson: Your items, your open items. I will see what. What Kapil has to say regarding the.
[00:37:33] Trevor Anderson: The naming convention, but I would just keep it as source ID for now and sort, you know, using the source instead of platform.
[00:37:41] Trevor Anderson: But I'll just double check with him just to make sure I'm meeting with him like, right after this, so.
[00:37:46] Venkatesh Mannam - B: Yeah. Yep.
[00:37:47] Trevor Anderson: But, yeah, other than that, the. These BIM tables look good so far.
[00:37:53] Trevor Anderson: Hoping that tricky situation works out with the working and all that,
[00:38:01] Venkatesh Mannam - B: but.
[00:38:01] Trevor Anderson: Yeah, and then let's get that FDSO out for the next.
[00:38:05] Venkatesh Mannam - B: Yeah, sure thing.
[00:38:07] Trevor Anderson: Awesome.
[00:38:10] Venkatesh Mannam - B: Yeah, thanks, Jared. Thanks for your time.
[00:38:11] Trevor Anderson: All right, thank you. Have a good one.
[00:38:13] Venkatesh Mannam - B: Bye.
[00:38:13] Trevor Anderson: Bye. Bye.
