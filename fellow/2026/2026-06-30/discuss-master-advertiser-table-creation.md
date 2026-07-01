---
title: Discuss Master Advertiser Table Creation
date: 2026-06-30
time: "10:05-11:05"
participants: [Trevor Anderson, JC Camacho, Kevin Witter, Alija Collins, Kapil Sreedharan, Venkatesh Mannam, Brian Silveri]
fellow_url: https://fluentco.fellow.app/meetings/040000008200E00074C5B7101A82E00800000000301952DD8405DD01000000000000000010000000EAADCC466CEC334AA043EC37844957AC/
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

The team discussed the architecture and implementation of a **unified master advertiser table in Databricks** to consolidate data across seven disparate systems (Tune, Lead Manager, Minion, Playful, Adflow, NetSuite, and Ad Parlor). The initiative addresses the current challenge of **inconsistent advertiser IDs and naming conventions** across platforms, with the team deciding that **NetSuite IDs will serve as the authoritative Fluent IDs** starting Q3 2026. The master tables will also adopt **Conlin's vertical and sub-vertical definitions** as the classification standard.

The discussion covered technical mapping approaches for connecting systems, **Playful Rewards integration challenges** (which uses proprietary IDs rather than flowing through Lead Manager), and dual executive reporting requirements from Ryan (micro-level advertiser performance) and Conlin (macro-level consolidated client reporting). The team also addressed the parallel **CAKE to Everflow migration** for publisher tracking and broader vision for unified master entity tables across all business objects, aligning with Don's 'Fluent 1' initiative for consolidated data architecture.

### Master Advertiser Table Architecture

- The team is building a **unified advertiser table** in Databricks to consolidate three source systems (Tune, Lead Manager, Minion), each storing advertiser IDs in different formats (e.g., 717 Ads stored as 2037 in Tune, 1921 in Lead Manager, GUID in Minion). [02:24]
- Current challenge is the **lack of standardized naming conventions** across systems - each platform stores advertiser, campaign, and reason code information differently without unified standards. [03:56]
- Lead Manager has already begun using **NetSuite IDs for campaigns**, providing a head start for the broader initiative to adopt NetSuite IDs as the authoritative Fluent ID across all platforms. [05:00]
- The master tables will integrate **six systems initially**: Playful, Adflow, Lead Manager, NetSuite, Tune, and Apps Flyer, with Ad Parlor for Travant to be added in Q3. The scope expanded from the original plan to accommodate additional data sources. [11:21]
- Once complete in early Q3, the team will conduct a company-wide demo to showcase the unified Fluent data universe and explain access methods for all stakeholders. [25:45]

### Unified Master Entity Tables

- The team identified the need for **unified naming conventions** across five different categorizations currently in use: sub F2, publisher, partner, affiliate ID, creating significant confusion. [43:48]
- For new advertisers going forward, the goal is to create **one universal Fluent ID** regardless of how different business units name them in various platforms (LM, Adflow(?), Playful, Rewards). [45:21]
- **NetSuite is the source of truth** for billing and will serve as the master ID system—if NetSuite doesn't have an ID, no one gets paid, making it the logical choice for the unified identifier. [45:52]
- All entity types (advertisers, campaigns, affiliates, partners) will have **one master ID with linked source IDs** by platform, even when different business units use different names (e.g., 'Disney Plus' vs 'Disney Plus Hulu ESPN'). [46:24]
- Manual entry issues create duplicates like 'Ancestry' and 'Ancestry.com' both being active with revenue, exemplifying why business process improvements and technological controls are needed alongside the data solution. [48:40]
- This master table initiative is part of **Don's 'Fluent 1' vision** for consolidated and clear data, with Brian driving toward unified entity definitions across the organization. [50:08]

### Executive Reporting Requirements

- Two distinct reporting needs drive the initiative: **Ryan requires micro-level** advertiser performance sliceable by any dimension (down to creative level for specific demographics), while **Conlin needs macro-level** Fluent performance by advertiser for client conversations. [26:43]
- Conlin's use case requires **consolidated reporting** to show Disney their $1.2M monthly revenue in a single view, whereas today he must compile data from multiple product-line reports. [27:33]
- Conlin's reporting depth stops at **advertiser and campaign level** (e.g., Capital One mobile vs. app, cashback vs. non-cashback campaigns), contrasting with Ryan's granular creative-level requirements. [27:57]
- The existing ROAS dashboard mappings (e.g., offer tag routing rules for Rewards DTS vs. O&O) were reused as foundational logic to ensure **metric consistency** across the new advertiser report and existing dashboards. [29:11]

### Cross-system Mapping Methods

- For **Tune to Lead Manager mapping**, the business passes Lead Manager campaign IDs in Tune's Absub 5 field (for Rewards and O&O only), which is extracted via split on the third underscore position to obtain the campaign ID. [06:33]
- Each Lead Manager campaign ID corresponds to **exactly one advertiser** (no many-to-many relationships), enabling straightforward mapping from campaign to advertiser across systems. [07:45]
- Campaign ID extraction from Absub 3 requires **live user-generated activity data** rather than static reference tables, though a Tune offer ID to Lead Manager campaign ID table exists for Stack-configured offers. [10:04]
- **Offer Description field in Tune Offers table** serves as the joining key to map Playful Rewards product data back to Tune advertiser names, though the field requires manual updates by AM/operations teams and isn't fully populated. [19:12]

### Playful Rewards Integration

- Playful Rewards uses **proprietary advertiser IDs** rather than flowing through Lead Manager or Tune, though they embed Apps Flyer links into Tune for some tracking. [14:07]
- Two distinct reporting approaches exist: one pulling from Tune (for Playful Rewards offers in Tune platform) and another pulling strictly from **Apps Flyer using campaigns and media sources**, with no direct advertiser ID concept. [15:27]
- The Playful Rewards **products reference table** (containing values like Candy Crush Saga, Monopoly Go) provides the closest proxy to advertiser names, though it doesn't map exactly to Lead Manager definitions like Scopely. [17:33]
- A **SharePoint-managed mapping** maintained by the Rewards team connects Tune source strings to source names for the ROAS dashboard, though its active maintenance status is uncertain. [22:13]

### Affiliate and Publisher Data

- Affiliate data flows through Tune params and lead manager links embedded in CAKE, with the ROAS dashboard able to break out by affiliates easily on the pub dev side. [30:45]
- Campaign ref table from lead manager is brought over daily and maps campaigns to offer names and verticals, useful for advertiser mapping but **not for affiliate/publisher mapping**. [32:03]
- Publisher IDs are extracted from **absub 2 or absub 1 fields in Tune** by parsing the second component after the underscore, then mapped back to reference tables (Tune F reference or CAKE reference). [32:27]
- In Tune, what's called 'affiliate ID' was renamed to 'partner ID' and represents a **product ID** (e.g., 3299 for Rewards), while sub F2 contains the actual affiliate ID used by pub dev teams. [39:34]
- The team uses multiple extraction methods depending on the field—absub 2 or absub 1 in Tune—to get publisher IDs that then map to CAKE (soon Everflow) for account manager and media buyer information. [42:02]

### CAKE to Everflow Migration

- The platform is **transitioning from CAKE to Everflow** for publisher/affiliate tracking, with reference tables currently being updated to reflect the new system. [33:08]
- Any logic shared now would become obsolete since the team uses CAKE reference tables currently but will merge with Tune reference tables differently once Everflow is implemented. [33:38]
- Publishers aren't populating information properly in the UI, causing the same data quality issues seen with advertisers—only a few partners are live and reports are missing data. [35:10]
- The transition needs to be **'one and done'** with all publishers migrated at once, not rolling in groups over time, to avoid data inconsistencies and reporting gaps. [35:43]
- Brian requested to be kept informed on Everflow progress alongside Trevor to avoid duplicating work, with the migration expected to begin in July once advertiser and vertical work is complete. [43:25]

### Action Items

- [ ] Update the EDM entity model to show known relations between entity names (partner, advertiser, campaign, affiliate, etc.) across different platforms and sources. [44:01] (Trevor Anderson)

### Decisions

- **NetSuite IDs will serve as the unified Fluent IDs** for advertisers, partners, and campaigns across all platforms starting in Q3 2026. [04:40]
- **Conlin's vertical and sub-vertical definitions** will be the authoritative classification system in master tables, superseding system-specific categorizations from Adflow and Lead Manager. [12:21]
- Master advertiser and campaign tables in Databricks will consolidate data from **all platforms** (Tune, Lead Manager, Minion, Playful, Adflow, NetSuite, and Ad Parlor for Travant). [11:59]
- The team will **map comparable fields between CAKE and Everflow** to preserve historical publisher data and append to existing tables rather than lose history during the platform migration. [34:29]

## Transcript

[00:00:00] JC Camacho: Awesome. Aliyah, Kevin, you guys are here. It's good. Do we all know each other? We do, right?
[00:00:12] Brian Silveri: I don't know if Leah, if we've
[00:00:14] Trevor Anderson: met,
[00:00:15] Trevor Anderson: this might be the first time.
[00:00:20] Brian Silveri: I'll introduce myself. I am the.
[00:00:23] Trevor Anderson: I'm a new data engineering manager.
[00:00:27] Trevor Anderson: I started in April
[00:00:28] Trevor Anderson: but overseeing our
[00:00:31] Brian Silveri: data engineering group here at Fluent.
[00:00:35] Alija Collins: Awesome. I'm a senior business analyst under Kevin. I've been at Fluent for the last six years.
[00:00:45] Alija Collins: Nice meeting you guys.
[00:00:48] Trevor Anderson: Congrats on the six years.
[00:00:52] JC Camacho: So I wanted to get everyone together here.
[00:00:55] JC Camacho: The reason why I want to I want Venky and team to show what they are building on the databricks side as far as, for lack of better wording, a reference table for advertiser and affiliates.
[00:01:11] JC Camacho: They've had questions, Aliyah and Kev regarding about how do we link certain advertisers to certain platforms that we're getting data from.
[00:01:23] JC Camacho: I had mentioned YouTube as a good point of reference, but again, I also want them to show you guys what they're creating and their intent and that way, you know, you guys could just say, oh, you know, okay, we've done that before.
[00:01:36] JC Camacho: We've linked that before and again, pretty much provide them a little bit of guidance and support to their end goal.
[00:01:47] KW: That works.
[00:01:49] Venkatesh Mannam: Cool.
[00:01:50] JC Camacho: So Venki, I think you could start off if you want, showing them the what you had showed me last time.
[00:01:55] JC Camacho: As far as the table or go, I'll leave the platform to you.
[00:02:00] Brian Silveri: Sure, sure.
[00:02:00] Venkatesh Mannam: Jesse, thanks for referring that brand like chime in if you have any other information apart from the one that I'm sharing with them.
[00:02:08] JC Camacho: Sure.
[00:02:10] Venkatesh Mannam: Let me know if you guys can see my screen.
[00:02:21] Brian Silveri: Yep, sure.
[00:02:24] Venkatesh Mannam: Thanks. Hey guys, this is Venkatesh from Data engineering team.
[00:02:29] Venkatesh Mannam: So what we're trying to build is like we are trying to build one unified table for advertisers.
[00:02:36] Venkatesh Mannam: So basically like as of now in the databricks we have these three sources ingested.
[00:02:43] Venkatesh Mannam: One is Tune, lead manager and Minion where we store their respective source IDs.
[00:02:48] Venkatesh Mannam: I mean like source advertiser ID in their respective source system.
[00:02:52] Venkatesh Mannam: So in this case like in tune for advertiser name 717 ads they store the internally they store the ID as 2037.
[00:03:01] Venkatesh Mannam: Similarly for lead manager they store as 1921 and in minion they store as in the GUID format.
[00:03:08] Venkatesh Mannam: So what we're trying to build is so that like downstream users can consume based on a unified fluent advertiser id.
[00:03:17] Venkatesh Mannam: Suppose like in the NET suite system it can be called as 717ads.com or whatever it can be. Right.
[00:03:25] Venkatesh Mannam: So as of now, since we don't have any linkage with, I mean, interrelationship between across the systems, because each source system stores the advertiser name based on their usage or based on their A.
[00:03:43] Venkatesh Mannam: Like they stored in the different formats, I would say.
[00:03:46] Brian Silveri: Sure, yeah, sorry, yeah, yeah. It's. It's not just the different formats.
[00:03:52] Brian Silveri: It's that they don't follow a unified. Yeah.
[00:03:55] Brian Silveri: Naming convention or standard for how they apply both from an advertiser or campaign or. Or anything.
[00:04:03] Brian Silveri: Anything. Reason code.
[00:04:04] Brian Silveri: So our job here is really to build what I would consider a authoritative advertiser mapping that rolls up this linked information across these systems so that we can present it one time and it can be the authoritative source for Fluent using this data.
[00:04:29] KW: Are you guys going to build.
[00:04:31] KW: Although we have all these various IDs from the various different sources, are you guys going to create a unified ID that identifies all these resources?
[00:04:39] KW: Eventually.
[00:04:40] Brian Silveri: Okay, yeah. So that is the plan for Q3, Kevin.
[00:04:44] Brian Silveri: For us to use the NetSuite ID as the fluent ID, so to speak, especially for advertisers and partners and campaigns, because they all have to go back to netsuite anyway to be paid out.
[00:05:00] Brian Silveri: So we're going to use the NetSuite ID and Jason has already started that, thankfully, unbeknownst to us, with Lead Manager.
[00:05:08] Brian Silveri: So they've already got a head start for using NetSuite IDs for campaigns.
[00:05:13] Brian Silveri: We're just going to apply this across platforms and then land that in databricks, so that Databricks has NetSuite ID or Fluid ID for advertiser, for campaign, for partner, and we can tie back.
[00:05:28] Brian Silveri: Even if those platforms still have their weird naming conventions, we still have a unified ID that we can aggregate and show one entity for.
[00:05:39] KW: If Venkatesh can stop sharing, I'll show.
[00:05:42] KW: I went through this whole exercise already, vendor cash, and it was a headache to really try to go through that, especially from the lead manager side to tune.
[00:05:52] KW: So just to give you some context of what happened, Tune, when it first came out, it was supposed to be the replacement for the Cake and Lead Manager tool, so to speak.
[00:06:03] KW: And in so doing, that never transpired, they realized we need to have both systems.
[00:06:08] KW: In doing so, Tune couldn't incorporate its own advertiser id, so it created its own advertiser ID for the same thing.
[00:06:17] KW: Given the headache of what Venticache and what you guys went through.
[00:06:21] KW: So let me share my screen real quick. Share my screen. Let me share the right Screen.
[00:06:29] KW: Okay, let me make sure you guys are seeing an Excel spreadsheet.
[00:06:32] Brian Silveri: Yeah.
[00:06:33] KW: Okay, so I went through the exercise of going through all the most of the fields inside of Tune, how to break them out, how they map to the actual lead manager field.
[00:06:43] KW: The case that you guys are referring to right now, what we do now is the advertiser ID that's in tune does not correlate to the advertiser ID inside of lead management.
[00:06:54] KW: To eliminate that, the business has started passing the campaign ID inside of tune under what's called the Absub 5, which you'll see here highlighted.
[00:07:06] KW: And I can share this with you guys if needed.
[00:07:11] KW: And let me just make this a little bit bigger for visual purposes.
[00:07:15] KW: So from the AB5 we extract what it for rewards and owned and operated from tune only, we extract the campaign ID by you know, doing the split part on the underscore, the third part on the other score.
[00:07:30] KW: And that is how we obtain the lead manager campaign id.
[00:07:34] KW: Each campaign ID from lead manager corresponds specifically to one advertiser.
[00:07:41] KW: So let me share my other thing.
[00:07:45] KW: So what we do is we extract the campaign id, we map it back to lead manager reference table for campaigns.
[00:07:55] KW: I'll make this a little bit bigger. We map it back to lead manager reference table for all campaigns.
[00:08:04] KW: And since each campaign has is referred to a specific advertiser, no camp, no campaign goes to multiple advertisers.
[00:08:12] KW: It only goes to one advertiser. And we tie that campaign ID to the advertiser id.
[00:08:18] KW: And that's how we have the uniform way of getting the camp, the uniform, the campaigns, any advertisers together between Tune and lead manager.
[00:08:31] KW: I want to see if that helps your situation.
[00:08:36] KW: This way you don't have to worry about the naming convention.
[00:08:38] KW: The naming convention will always be the same from tune to lead manager.
[00:08:43] KW: So that eliminates one aspect of trying to map the can map advertisers to between lead manager and tune.
[00:08:53] JC Camacho: Yeah.
[00:08:53] Brian Silveri: So Kevin, the that mapping that you're doing here, I guess to be clear, is that, is that systematically done or are you doing this like an ad hoc spreadsheet basis?
[00:09:07] Brian Silveri: Because right now those that isn't a one to one naming convention match in those two systems, right?
[00:09:13] Brian Silveri: Like the name is different, the name is somewhat different.
[00:09:17] KW: So I don't utilize the names that's in tune. I map the campaign ID from tune to lead manager.
[00:09:24] KW: So I have a uniform naming convention.
[00:09:26] Brian Silveri: Got it.
[00:09:26] Brian Silveri: So your linkage is tune id, basically tune campaign ID and say oh yeah, great, this is equals to this lead manager id and this use Lead Manager campaign name.
[00:09:38] KW: Correct.
[00:09:39] Brian Silveri: Got it, got it.
[00:09:41] Alija Collins: Just add something here, please.
[00:09:44] Alija Collins: We can only like, we can only parse out Campaign ID from the F sub 3, but it has to come from like data or activity has to be generated by a user.
[00:09:55] Alija Collins: So it's not from like a static table, it's from. It's from live data. So you wouldn't be like.
[00:10:04] Alija Collins: I mean, there is a table that we use where you can grab Tune offer name or I'm sorry, Tune offer ID and tie it to a lead manager campaign id.
[00:10:13] Alija Collins: But that's only if they're on the stack.
[00:10:15] Alija Collins: If they're configured in Lead Manager on the stack stack offer wall or something that they have in.
[00:10:22] Alija Collins: In their lead manager interface.
[00:10:25] Brian Silveri: Yeah.
[00:10:25] Brian Silveri: So luckily JC has been helpful in getting us that raw data both from Lead Manager and from Tune.
[00:10:35] Brian Silveri: So we've got the data feeds from Lead Manager from LEO and from Tune and API calls on the Tune offers Tune goals.
[00:10:44] Brian Silveri: And so we have that correlation back between those IDs and names and campaigns.
[00:10:49] Brian Silveri: And which was the heavy. Was a big, heavy lift for us.
[00:10:53] Brian Silveri: But it's great that you already have this association, Kev. So it's not going to be a lift.
[00:10:59] Brian Silveri: Once we finish these master tables, basically these fluid tables, we're calling them fluid tables.
[00:11:05] Brian Silveri: They'll be available for everybody, including you, to then not have to replicate this disaster of a mapping.
[00:11:13] Brian Silveri: Going forward, databricks will deal with that disaster and add multiple other systems into this mapping.
[00:11:21] Brian Silveri: So yours is just LM to Tune. We're doing everybody into databricks.
[00:11:26] Brian Silveri: So I got playful Adflow, lm, Netsuite and then soon to be ad parlor as well for Vaunt.
[00:11:34] Brian Silveri: So we're going to have everybody in the master advertiser table.
[00:11:38] Brian Silveri: We're going to have everybody in the master campaign table, and that includes Conlin's new vertical associations, which do not equal every other system's vertical association.
[00:11:53] Brian Silveri: So we'll have a fluent vertical assigned as well, so that anyone who wants fluent data can go into these master tables and say, oh yeah, great, here is the advertiser.
[00:12:05] Brian Silveri: And no matter which ID you pick from, it's all going to be linked back.
[00:12:09] KW: Okay. Because that was my next question. How did you guys. We got the avatars. What about the verticals?
[00:12:15] KW: But if you guys are already in motion with that thing. Yeah, Also subvert sub verticals too.
[00:12:21] Brian Silveri: Yeah, so the vertical and. Yeah, sub vertical. Fluent sub verticals. I'll be very specific.
[00:12:28] Brian Silveri: The Conlin definition of verticals and sub verticals. Okay. Not an adflow version. Not.
[00:12:34] Brian Silveri: Not the LM version, just the fluent Conlin version where he did the reorg.
[00:12:40] KW: Okay.
[00:12:43] Brian Silveri: And that's what it's going to show Kev on that advertiser report or the CBUR report when we expand to other bus.
[00:12:50] JC Camacho: Right.
[00:12:52] Brian Silveri: It's everybody. Okay. So that's the goal coming out of Q2 was like, hey, we want to consolidate.
[00:12:59] Brian Silveri: It just happened to take us a lot more effort because we had to keep adding more sources like Apps Flyer.
[00:13:05] Brian Silveri: That wasn't on our list of crap to do.
[00:13:07] Brian Silveri: But all of a sudden we had to find Apps Flyer data and join it in here in order to make stuff work.
[00:13:12] Brian Silveri: So we added like four different sources that were not part of our original purview just to get to this point so we could have some unified linkage at the fluent ecosystem level.
[00:13:23] Brian Silveri: But we're almost done. Team did a great job.
[00:13:30] Venkatesh Mannam: Jesse, if you remember, like on.
[00:13:32] Venkatesh Mannam: On last Thursday or Friday, we discussed about ingesting playful rewards data also.
[00:13:36] JC Camacho: Right. So guys like Kevin or Aliyah. Playful rewards. Right. So are they also.
[00:13:46] JC Camacho: Are like their advertiser IDs are. Are they also flowing through. Through LM or Tuna or. Or.
[00:13:55] JC Camacho: Or do they have their own proprietary advertiser IDs?
[00:14:07] KW: They're their own.
[00:14:09] JC Camacho: Okay. Because. Yeah, because on the. On the data side, for instance, right.
[00:14:13] Brian Silveri: We.
[00:14:13] JC Camacho: They. They haven't provided any advertiser level data
[00:14:17] KW: is events. Right, Right. Aaliyah.
[00:14:21] Alija Collins: What I understand, like, they're embedding the AppsFire link into tune.
[00:14:25] Alija Collins: So then we typically just grab the advertiser ID from Tune.
[00:14:29] JC Camacho: Okay, that's. Yeah, See, that's. That's the. That's the kind of. That's the direction I had given Venki.
[00:14:34] JC Camacho: Right. Like I said, that's. That.
[00:14:36] JC Camacho: I assume that that's what you guys were doing because I know you guys are linking that stuff.
[00:14:42] JC Camacho: But for instance, like any. And then I'm assuming then whatever.
[00:14:47] JC Camacho: Advertiser id, then you just use the tune advertiser ref. To get that information on that.
[00:14:54] JC Camacho: On that advertiser. Correct.
[00:15:01] KW: The Tune advertising.
[00:15:03] Alija Collins: Are we talking about affiliate ID or advertiser ID advertiser?
[00:15:08] JC Camacho: Right now they're just on advertiser.
[00:15:13] Brian Silveri: Just Advertiser.
[00:15:14] Alija Collins: I feel like it just depends. Like the two reports that I have reporting on playful rewards.
[00:15:19] Alija Collins: One is mainly focused on pulling tune data and then they have like separate playful rewards offers inside of the tune platform.
[00:15:27] Alija Collins: But then I also have a separate report that's pulling from Ad Apps Flyer strictly.
[00:15:32] Alija Collins: And we don't use a notion of like advertiser id like they pull in campaigns and then they'll also look at like what's it called?
[00:15:43] Alija Collins: It's called media source on their end.
[00:15:49] Alija Collins: So it like we don't necessarily have a notion of like advertiser ID for the campaign at least that I'm currently using.
[00:16:01] JC Camacho: Is that UA source? No, that's not it.
[00:16:04] KW: Right.
[00:16:06] Alija Collins: No, I know they're looking at like campaigns and then I don't know if they like there's no necessarily like a breakout.
[00:16:15] Alija Collins: Like if it was like I don't know, Moon Active or something. Just as an example. But
[00:16:25] JC Camacho: so the reason I mentioned that one Leah is because. Can you see my screen?
[00:16:29] Alija Collins: Yeah, yeah.
[00:16:30] JC Camacho: So under UA sources, right? We have this, this stuff here, right?
[00:16:35] Alija Collins: Yeah.
[00:16:36] JC Camacho: Are these considered advertisers or affiliates or None.
[00:16:43] Brian Silveri: That's tick tocks.
[00:16:45] Brian Silveri: So I, I would if I'm leaning to that, I'm saying affiliate is my first pass at this JC But I am not the authoritative source on this.
[00:16:55] Brian Silveri: I, I couldn't answer that one for sure.
[00:16:57] Brian Silveri: But I assume Aaliyah in that Playful rewards custom built request report that they have, do they.
[00:17:05] Brian Silveri: Is there a drop down for advertiser for them today?
[00:17:08] Brian Silveri: Like how would he know that it was Disney Like Conlin, how would he know it was Disney for Playful Rewards?
[00:17:15] Alija Collins: It just depends what report they're using.
[00:17:17] Alija Collins: They would only know that it's Disney if they're looking at data from Toon because they have the relationship with the tune offer.
[00:17:27] JC Camacho: So within the, within the playful data they also have this products reference table and then these are the values here.
[00:17:33] JC Camacho: Right. Travel Down Cash Master and all that stuff.
[00:17:37] Alija Collins: That could probably help because that's like offer title and I know that the, the business uses that.
[00:17:44] Alija Collins: I just don't pull that into my Playful Rewards report today.
[00:17:49] Brian Silveri: Yeah, that, that would, that would be advertiser because you got like Candy Crush Saga. It.
[00:17:56] Brian Silveri: It'd be closer. I'll put it that way. It's closer.
[00:17:58] Brian Silveri: It's not like exact mapping to like LM like scopely or you know the other definitions.
[00:18:06] Brian Silveri: But it's as close as you're going to get to who they are.
[00:18:10] Alija Collins: Yeah, you can definitely map those like offer titles back to tune to grab the advertiser name and then join back to whatever table that you guys are building.
[00:18:21] Alija Collins: You'd have to just reverse like engineer that.
[00:18:26] Brian Silveri: The would you use the offers in that case?
[00:18:30] Brian Silveri: So like you know the offers monopoly go you know, in Playful Rewards would you then go to products to try and find that products table to try and find Monopoly.
[00:18:42] Brian Silveri: Go inside there and say, oh, this is that ID for Monopoly.
[00:18:47] Venkatesh Mannam: Go.
[00:18:49] Brian Silveri: Might. Am I thinking of that jc If I'm thinking those two tables correctly?
[00:18:55] JC Camacho: Well, you could. And then that would get you to what the playful rewards ideas for that.
[00:19:08] Brian Silveri: And then Aaliyah, how do you join that? To which. Which field in tune are they using for that?
[00:19:12] Alija Collins: It's called Description. Offer Description, and it's in the Tune Offers table.
[00:19:20] Brian Silveri: Great. And then you're doing a one to one mapping of that data.
[00:19:31] JC Camacho: A lot of fun.
[00:19:33] Brian Silveri: It's great. We love it. But this is what Don tasked us with doing, is to figure out how to get this.
[00:19:40] Brian Silveri: So it's a scalable centralized entity. Got it. What was that Tune, Aliyah, what was the.
[00:19:53] Brian Silveri: What was the tune field and from which table?
[00:19:56] Alija Collins: It's on our end. It's called Tune Offers, but it's the offer description field. And this is like.
[00:20:03] Brian Silveri: Got it. I see it.
[00:20:05] Alija Collins: The AM team can go into tune and they'll update it. Yeah.
[00:20:16] Brian Silveri: All right, so you got. It's not fully populated, but at least it has some.
[00:20:22] Alija Collins: Yes. Yeah.
[00:20:23] Brian Silveri: Got it.
[00:20:24] Alija Collins: The rewards team would have to go into tune and fill out.
[00:20:29] Alija Collins: Fill out the offer description for each offer.
[00:20:33] Brian Silveri: I take the. I guess any team using two would have to do that.
[00:20:39] Alija Collins: Yeah.
[00:20:39] Brian Silveri: So like rewards O2 and Adflow.
[00:20:44] Alija Collins: Yep. I don't know if I think it's the operations team that manages that, though.
[00:20:49] Brian Silveri: Yeah, that. That's an easier thing for us to. To do. So are there any.
[00:20:55] Brian Silveri: Are there any manual mappings that you all are doing to get to these reports?
[00:21:02] Brian Silveri: Like, JC is like, is there any. Or Kevin or Leo, is there any?
[00:21:05] Brian Silveri: Like, here is the spreadsheet of manual mappings that I keep updated every month or refresh when someone tells me something.
[00:21:15] KW: Only when the business wants specifics. Like we.
[00:21:18] KW: We import a lot of data and they may not want to see a lot of data and we're not going to micromanage and be available to, you know, update and hard code a lot of stuff.
[00:21:28] KW: So we let them control that area.
[00:21:30] Brian Silveri: Got it.
[00:21:31] KW: Because sometimes it's a lot of data and they don't want to see everything.
[00:21:35] KW: So we give them that ability. But for this purposes, I don't think that is the case.
[00:21:39] KW: The spreadsheet is not needed.
[00:21:41] Brian Silveri: That's good.
[00:21:42] Brian Silveri: Okay, so like Aaliyah, they're not going in and loading like their own mapping of advertisers for like their create roan report, like commons, like, hey, show me how playful's doing.
[00:21:52] Brian Silveri: And then someone just loaded a Spreadsheet of crap. And then no on.
[00:21:58] JC Camacho: Well, on.
[00:21:59] Alija Collins: I think of like, for the.
[00:22:00] Alija Collins: Let's say, for the rose dashboard, since I pulled in Playful Rewards, I do have like a SharePoint that the rewards team manages that map source string to a source name.
[00:22:13] Alija Collins: So again, share my screen of what that looks like.
[00:22:17] Alija Collins: I don't know if they're actively, like, keeping up with this, but I have this, like, ROAS Dashboard Management SharePoint.
[00:22:26] Alija Collins: And then this is specifically for, like, tune data that comes through that where we have this source string being passed into tune and then they dictate what the source name or who it belongs to.
[00:22:40] Alija Collins: So I don't know if that's relevant
[00:22:42] Brian Silveri: for is, but not for this field. This. This data. So source is also relevant.
[00:22:51] Brian Silveri: And also the word source is different by product. Not everything means the same thing.
[00:22:56] Brian Silveri: So source is another one.
[00:22:57] Brian Silveri: So inside the aggregated ROAS dashboard reports that you have created, you've got advertiser name, and it's got a lot inside there.
[00:23:11] Brian Silveri: Is that just rewards? I can't say rewards. I want to be specific.
[00:23:19] Brian Silveri: Which one of our product lines advertiser names is. Does that make up?
[00:23:26] Brian Silveri: Is that just tune advertiser name, or is that lm or is that a combination of. Of things?
[00:23:31] Alija Collins: It's only Tune advertiser name.
[00:23:34] Brian Silveri: Okay, got it, got it. All right, that makes more sense. So playful does use tune.
[00:23:45] Brian Silveri: So there's like, okay, great. But then there's. And then you're doing that other mapping of.
[00:23:53] Brian Silveri: I don't want to say offer offer to offer. Yeah, it's playful rewards Offer to tune offer.
[00:24:04] Alija Collins: I'm actually not doing that mapping because it's not relevant for the reward.
[00:24:08] Alija Collins: The playful rewards dashboard that I created.
[00:24:11] Brian Silveri: Oh, okay.
[00:24:13] Alija Collins: But if you needed to get to an advertiser name for.
[00:24:17] Alija Collins: From that app's flyer data, then you would just connect it to the tune offers table on offer description and then do like a backwards mapping to grab advertiser name.
[00:24:28] Brian Silveri: Got it. Okay. Yeah, so we're gonna have to do that. Makes sense.
[00:24:36] Brian Silveri: Are there jc, I don't want to commandeer your. The advertisement, but this was really helpful so far.
[00:24:43] JC Camacho: No, this is good. This is pretty much what I wanted to happen.
[00:24:49] JC Camacho: I put the agenda in there, but it was more about getting you guys connected because again, I know, like, everything that most likely I was under the assumption that anything that you guys are trying to do.
[00:25:01] JC Camacho: Aaliyah or Kevin have already probably hit that wall a long time ago and had to find a way to get around it.
[00:25:08] Brian Silveri: Yeah.
[00:25:10] JC Camacho: And again, I don't think this is like the, the one and one and done meeting.
[00:25:14] JC Camacho: I think definitely we, you know, we can have follow ups, you know, and then again.
[00:25:17] JC Camacho: And I wanted you guys to like meet that way again. Becky has a question.
[00:25:22] JC Camacho: He could quickly just, you know, shoot a slack, obviously keep Kevin in the loop.
[00:25:29] JC Camacho: But, you know, but again, I wanted that conversation to happen because I feel like you guys are, you guys are, have hit a couple of walls that again, these guys have already knocked down.
[00:25:39] Brian Silveri: Yeah, this is great. Once we actually have something, we'll do a show and tell for everybody.
[00:25:45] Brian Silveri: Like, hey, welcome to the fluid data universe.
[00:25:49] Brian Silveri: And this is what we've created and this is what it means to you and this is what you have access to, this is what you don't have access to and this is how you get to it.
[00:25:57] Brian Silveri: So we're in the building phase and hopefully sometime early in Q3 we'll have that show and tell with everybody at Fluent and say this is what we did.
[00:26:07] Brian Silveri: And I'm meeting with Conlin at 4 o' clock anyway because he wants to see what his progress is for the cpur.
[00:26:13] Brian Silveri: So he's got very high visibility into us building out these Fluent tables to get his fluent visibility into how we're driving revenue.
[00:26:25] Brian Silveri: And Kevin, that's also why Ryan's driving revenue changes off of the advertiser master report that you built.
[00:26:34] Brian Silveri: Same thing. Two sides of the house.
[00:26:36] KW: Makes sense.
[00:26:37] Brian Silveri: It makes sense, yeah. So just to give you all some color so we're, you're all aligned.
[00:26:43] Brian Silveri: We have two distinct asks for reporting from our co founders.
[00:26:50] Brian Silveri: Ryan wants a micro level view of advertiser performance that he can slice and dice at any dimension off of any attribute.
[00:27:03] Brian Silveri: Kevin and the DE team have built the tables necessary for us to power that so he can see anything that he wants at any time.
[00:27:12] Brian Silveri: Going down to creative level, if he wants to say for a particular creative.
[00:27:17] Brian Silveri: How is it performing in P1 over the last seven days for males over 30 in the state of Iowa? Great.
[00:27:23] Brian Silveri: He can see it. Conlin doesn't care about any of that.
[00:27:27] Brian Silveri: Colin wants a, hey, I want a macro level view of how is Fluent performing?
[00:27:33] Brian Silveri: So when I talk to Disney at their office in New York, I could pull up this report and say, FYI, in June you generated $1.2 million worth of revenue for Fluent based off of this.
[00:27:46] Brian Silveri: Today he can't do that in one report. He has many reports by product line to drive that.
[00:27:53] Brian Silveri: Conlin's like, just show me one report that shows me how the performance is doing.
[00:27:57] Brian Silveri: At the advertiser level and if he wants to campaign because like Cap1, Cap1 cares about Cap1 mobile, Cap1 app and how they're doing for their particular campaigns for cash back versus non cashback.
[00:28:11] Brian Silveri: But that's as far as he wants to go.
[00:28:13] Brian Silveri: Those are two different grains with two different deliverables, all powered off of our master list of tables advertiser campaign and the underlying data that we've all come to to join in different ways.
[00:28:31] Brian Silveri: So it's everything for Fluent and Don tasked DE with getting that done in Q Tip to drive those two reports.
[00:28:40] Brian Silveri: So it's a lot. And now we have to do Trevant in Q3. So it's. That's another thing.
[00:28:49] Brian Silveri: And they've added new portfolios and new data intakes and new systems JC they were already been using before.
[00:28:54] Brian Silveri: And so we've got a lot of discovery to do as to how does Travant play but Minion.
[00:29:01] Brian Silveri: So you got adflow lead manager Pulse, playful rewards to start out of the box and include syndication.
[00:29:09] Brian Silveri: So all of those are there.
[00:29:11] Brian Silveri: The mappings that you all do today inside your roas, reports of like, hey, when the offer tag equals rewards, dts, this goes to rewards over here.
[00:29:21] Brian Silveri: When it equals rewards. Oh, and oh, it goes over here.
[00:29:24] Brian Silveri: All of that fun stuff that you've already built, we used because we needed that relationship so that when Ryan goes into your ROAS dashboard, he sees the same metric when he goes into the advertiser report and he's not like, why the hell are they different?
[00:29:39] Brian Silveri: We can't have that happen.
[00:29:41] Brian Silveri: So that's why all of the work that you all did, we used already because it was foundational.
[00:29:46] Brian Silveri: So thank you.
[00:29:49] Brian Silveri: Does that make sense logically as to where the strategy is and what we did in Q2 and what we're trying to do in Q3?
[00:29:55] Brian Silveri: Oh, yeah, great.
[00:30:00] JC Camacho: Awesome. All right, cool. Anything else?
[00:30:05] JC Camacho: Obviously there's, you know, there's some stuff that obviously are still open, but again, I don't, I don't, I don't think that we.
[00:30:11] JC Camacho: This is a one and done and I think we're good. I think, you know, I think we covered a lot.
[00:30:17] JC Camacho: I, I saw you recording this, so hopefully you guys can go back and see, you know, any unanswered questions.
[00:30:24] Brian Silveri: I did have one question.
[00:30:25] JC Camacho: Jason,
[00:30:28] Brian Silveri: what have you done, if anything team on affiliates across.
[00:30:33] Brian Silveri: Has anyone asked for affiliates as a field? Are we just using tune affiliates? Like, what has you.
[00:30:40] Brian Silveri: Have you done in that space? Because that's the next on our list of stuff to Do.
[00:30:45] Alija Collins: Well, affiliate data also gets passed into like the tune params or I would.
[00:30:53] Alija Collins: They're actually lead manager params because those are what get. Or the tune.
[00:30:57] Alija Collins: The lead manager links get embedded into cake and then we pull the data from those platforms.
[00:31:05] Alija Collins: But also I feel like it's kind of all interconnected.
[00:31:10] Alija Collins: So there's a lot of connections between Tune and lead manager that is reliant on live data.
[00:31:17] Alija Collins: So it like let's say within the context of the ROAS dashboard they can break out by affiliates easily and that's like the pub dev side.
[00:31:27] Alija Collins: But also there's a notion of affiliates that's considered like product in tune that the rewards team understands to be an affiliate.
[00:31:36] Alija Collins: So that all gets passed within like the AF sub fields in tune and
[00:31:42] Brian Silveri: JC is in that. Is that the campaign ref data from lead manager? Basically it nested inside there.
[00:31:49] JC Camacho: It should be in there. Yes. Right.
[00:31:53] JC Camacho: Because all the campaigns that are lead manager are the ones that we capture from that campaign.
[00:31:58] JC Camacho: Referring to table. Right. Ilia. At least lead manager stuff Campaigner.
[00:32:03] JC Camacho: There's a campaign ref table that we. That we bring over on a daily basis.
[00:32:06] JC Camacho: It's a link that, that, that we get from lead manager and it has like, it has vertical in there.
[00:32:12] JC Camacho: It has can. You know, like I think it's the association between campaign and offer name.
[00:32:16] KW: Not reading right is good for mapping to the advertiser.
[00:32:21] KW: It's not good for mapping to the affiliate or the publisher.
[00:32:26] Brian Silveri: Okay, okay.
[00:32:27] KW: Yeah, we have a tune F reference table that identifies those publishers.
[00:32:35] KW: But in tune, in order to map we.
[00:32:38] KW: We break out the actual publisher that's tied because it's embedded within the app sub 2 or absub 1 and we extract it out with the underscore to get at least the publisher or affiliate id.
[00:32:55] KW: Then we map it back to the tune reference, tape the tune f reference table or in this case like Aaliyah mentioned before, cake.
[00:33:04] KW: We have a cake reference table where. Well. Which is soon to be everflow. I think you talk.
[00:33:12] KW: You're talking ahead for future reference after we're done with the advertiser. Right?
[00:33:16] Brian Silveri: Yeah. We're talking the coming weeks. So sometime in July I'm going to start to tackle this.
[00:33:21] JC Camacho: Yeah.
[00:33:22] Brian Silveri: So any logic you've already got on affiliates between systems.
[00:33:27] KW: Yeah.
[00:33:28] Brian Silveri: At your earliest convenience, please kick it over to us. So we're gonna tell you what we're
[00:33:33] KW: gonna have to talk to talk to you about that later because we have to do a transition right now that we're doing right now.
[00:33:38] KW: So whatever logic we give you now is not going to make any sense because it's going to be changed over soon because we're going to Everflow and we utilize reference tables from CAKE to identify publishers and then we use reference tables from Tune and then from that we merge it together to get it.
[00:33:58] KW: So.
[00:33:58] JC Camacho: Because like just looking at the data earlier today, Kev. Yeah, it's definitely going to be.
[00:34:03] JC Camacho: It's going to change.
[00:34:04] JC Camacho: So there's no reason to be to start going through all the CAKE stuff if Everflow is going to be different.
[00:34:09] KW: Exactly.
[00:34:10] Brian Silveri: So what. It makes total sense and I'm in alignment with you all there.
[00:34:14] Brian Silveri: What are we going to do when we're looking at historical data that was coming from cake?
[00:34:21] Brian Silveri: Are we keeping CAKE data for X time and keeping that mapping for like 2025 data as an example?
[00:34:29] KW: So here's. Here's the plan.
[00:34:31] KW: The plan is similar to what you guys are doing right now with tuned advertisers, lead manager advertisers.
[00:34:37] KW: And look, we're doing this. We're going to be doing the same thing here.
[00:34:40] KW: We're going to try to map the likable fields so this way we can keep the history and just append to the existing data that's already in in CAKE so we don't lose any of that history and we could continue and append to that existing table.
[00:34:53] KW: Hopefully that's a conversation J.C. we're going to have later on regarding that.
[00:34:58] Brian Silveri: Okay, got it.
[00:35:01] Brian Silveri: So you got an existing ESK schema and they were like, we're just going to append, hopefully Everflow to this with some sort of mapping.
[00:35:10] KW: Well, right now we have the same situation when it comes to publishers.
[00:35:13] KW: The same issue you have with them not populating the information properly inside of your UI page for.
[00:35:19] KW: So we can get the correct agreements properly in that system.
[00:35:26] KW: If they don't do it properly, then it messes up the same problem I'm having right here.
[00:35:30] KW: We only have a couple of partners and they're saying, oh, you know what's our report is missing data.
[00:35:37] KW: So right now I'm working on that right now, which is that. Which is a headache all by itself because.
[00:35:42] Brian Silveri: Sure.
[00:35:43] KW: And the ramifications of what they have done and it should be one and done. You know, hit the switch.
[00:35:51] KW: All publishers are in.
[00:35:53] KW: It can't be two publishers in and six publishers are going to be in a couple of days in.
[00:35:57] KW: It has to be one and done in order for this to work. So.
[00:36:00] KW: So I have to have that conversation with the business actually later on today.
[00:36:05] Brian Silveri: Do you have A current definition of what publisher means to them, because I have not been able to find one.
[00:36:17] KW: Well, good luck. I mean, when it comes to. I mean, I could reach out to the.
[00:36:25] KW: To the business and see if they could come up with definitions for something.
[00:36:30] Brian Silveri: Kev, I was. I was just wondering if, like, is.
[00:36:33] Brian Silveri: Is publisher used at all today in any report as a field or a filter?
[00:36:39] KW: Absolutely.
[00:36:40] Brian Silveri: Okay, got it. So they're using it. Do. Do we.
[00:36:45] Alija Collins: They just know it's synonymous with Affiliate id.
[00:36:48] KW: Exactly.
[00:36:49] Alija Collins: Depending on who you're talking to.
[00:36:51] KW: But see, we got to be careful, Leah, because it's really that. Need me. Something different in tune.
[00:36:55] Alija Collins: Yes.
[00:36:55] KW: And it means something different in a different platform, but in all sense, we're all talking about Publisher similar to partners in adflow.
[00:37:03] Brian Silveri: Okay, all right, that's helpful.
[00:37:07] Brian Silveri: So then when you all do that mapping between those entities, because they're two distinct entities between Toon and Lead Manager, and say, hey, Lead manager calls them publishers, Toon calls them affiliates, and Batoon also has partners.
[00:37:33] Brian Silveri: So is it. Is it in partners or is it in affiliates, or is it both?
[00:37:39] KW: It's both. Because you know what, it depends on the business Unit. Right.
[00:37:42] KW: So Tunis 2 is encompassing multiple business units. Lead manager is just doing one its own thing.
[00:37:50] KW: So because of that, we use TUNE reference table to establish the partner, partner, publisher, affiliate IDs for those businesses.
[00:37:59] KW: If it relates to O2 and lead manager, we extract that information and link it properly, and we use the reference table from CAKE to reference those.
[00:38:10] KW: Tune is doing multiple things. So we can't just assume the TUNE is strictly for owned and operated.
[00:38:16] KW: It's doing it for playful. It's doing it for.
[00:38:19] Brian Silveri: Yeah, yeah, commerce.
[00:38:22] Brian Silveri: So the reason I ask is because I, I've been living in this ROAS dashboard for a quarter now.
[00:38:29] Brian Silveri: So, like, hey, here are the Tune partner IDs that this particular ROAS dashboard is looking at.
[00:38:36] Brian Silveri: Hooray. Great.
[00:38:40] Brian Silveri: But when you go in there and you look at any one of the things from a partner perspective, I'm assuming you're talking about product.
[00:38:51] Brian Silveri: Tune, like a product. Yeah. So I'm like, all right, is it this, this list?
[00:39:02] Brian Silveri: Is it this list which is the Tune partners?
[00:39:07] Brian Silveri: Or as you mentioned, it's most likely a combination when tune partner ID, whatever the hell it was, I forgot already.
[00:39:16] Brian Silveri: 3299. So if I go in, I do 3299 in here. Great. Fluent CPA wall dynamic as the partner.
[00:39:26] Brian Silveri: But what does that. What does that mean when I click on one of these things and do partner id?
[00:39:33] Alija Collins: Yeah.
[00:39:34] Alija Collins: So in within this report and within the context of Tune, the tune, it's called affiliate ID, which.
[00:39:41] Alija Collins: Yeah, yeah, call changed it to be partner ID. That's just like.
[00:39:46] Alija Collins: We understand that 3299 belongs to rewards, let's say.
[00:39:50] Brian Silveri: Yeah, yeah, yeah, yeah.
[00:39:51] Alija Collins: So it's more of a. It's more of a product ID. And then you have sub F2, which, depending on.
[00:39:59] Alija Collins: Let's say if you were talking to Danny's team or the pub dev team, they would tell you they'd be focused on sub F2 and that's their affiliate ID and that would live in Kate and Cake or Everflow.
[00:40:13] Brian Silveri: Fascinating.
[00:40:14] Alija Collins: And this sub F2 doesn't get passed or. It's not a field within the Tune data.
[00:40:20] Alija Collins: You have to actually parse it out
[00:40:22] Brian Silveri: from the app subfield only on an actual Tune event. It's not a config. Right.
[00:40:28] Brian Silveri: It's an only on the events. Because they're looking for installs with a sub F2. Yeah.
[00:40:35] Alija Collins: And bedroom live data.
[00:40:38] Brian Silveri: Is there a related LM field here?
[00:40:43] KW: Yes, that's what I showed you earlier today when I showed you how to parse it out.
[00:40:47] KW: So I had a list of. Let me share my screen again.
[00:40:53] Brian Silveri: So that sub. App 2 Aaliyah. That. That's just. That's a Tune field. Sub.
[00:41:01] KW: It's a lead manager field. They.
[00:41:04] Alija Collins: So basically, I don't know if this helps color, but when they're creating offers in Tune, they're Grab or Cake.
[00:41:11] Alija Collins: They grab a lead manager link that passes sub 2, sub 1, sub F3, whatever, and that essentially gets posted back into Tune or Cake so that you can extract those data points.
[00:41:26] Brian Silveri: Got it.
[00:41:26] JC Camacho: Okay.
[00:41:28] KW: So I'm trying to find my. My helpful sheet that I had earlier today.
[00:41:40] Brian Silveri: So this spreadsheet is what, actively updated because if they make a new offer, you need to have a link.
[00:41:49] KW: No, this spreadsheet is just an example. I just put this together to give you guys.
[00:41:52] Brian Silveri: Oh, okay.
[00:41:54] KW: Everything is automated through the system, through the database.
[00:41:57] Brian Silveri: Okay, great, great.
[00:41:58] KW: Okay, let me minimize this real quick. Okay, here we go.
[00:42:02] KW: So for publisher ID, we actually use either the sub app 2, the app sub 2 field in tune, or the app sub intune and for various ways to break it out.
[00:42:15] KW: So in the absub, we use the second component of after the underscore.
[00:42:21] KW: If you're going to use the ab sub, you want to use the second component there as well to get absub or the publisher ID related for lead manager, which then we tie back to Cake to get all the information that refers to that publisher, who is the account manager, who is the media buyer, etc.
[00:42:43] KW: Etc.
[00:42:44] Brian Silveri: Which. Which columns today, Kev, in this output includes the cake fields.
[00:42:54] KW: This one.
[00:42:55] Brian Silveri: Oh. Column A is cake fields.
[00:42:58] KW: Column A is.
[00:43:00] Brian Silveri: Yeah, yeah, yeah.
[00:43:01] KW: And this is what the cake field will be called. LM or caveat was called sub F2.
[00:43:07] KW: And this is how we extract it out of.
[00:43:10] Brian Silveri: Okay. And we're still in the process of the Everflow mapping.
[00:43:15] KW: Yes. Which is going to be a nightmare from what I'm saying.
[00:43:18] JC Camacho: Yay.
[00:43:19] KW: We love it. If it wasn't a challenge.
[00:43:23] Brian Silveri: Damn right.
[00:43:25] Brian Silveri: All right, can you just keep Trevor and I on the Everflow dynamic so that we're not doing it twice?
[00:43:34] KW: Yes. Conversation.
[00:43:38] KW: Hopefully once everything's solidified, by the time we're done with advertiser and verticals, we should be in a good place to start talking about publishers or partners.
[00:43:48] KW: And we need to have a unified name for publishers and partners because.
[00:43:53] Venkatesh Mannam: Yes.
[00:43:53] KW: We got what, five different categorizations sub. Have to Publisher, partner, affiliate id.
[00:43:59] KW: It's just very confusing.
[00:44:01] Brian Silveri: Yeah.
[00:44:01] Brian Silveri: Trevor, I think we need to update that EDM entity model to at least show known relations to those entity names.
[00:44:13] Brian Silveri: So like when we say partner, hey, here is an example of partner adflow, partner traffic, partner cake, you know, sub F2, you know, and then that way we could at least say here's what they were are and what our process is going forward so that when there is a fluent partner, master partner table, we have a definition for an entity as a partner.
[00:44:41] Brian Silveri: Even if the mappings are convoluted, which they will be, they don't need to know the mappings, they just need to see fluent partner and this is what you will get.
[00:44:52] Brian Silveri: That's our hope with advertiser and team is like it's fluent Advertiser.
[00:44:56] Brian Silveri: There's one list of advertisers, it's not five by product. Here's your list.
[00:45:03] Brian Silveri: You can then filter by business unit. If you just want to see the ones that apply to business unit.
[00:45:09] Brian Silveri: A great. Have a nice day. Go. Here's your sub list.
[00:45:12] Brian Silveri: But one list of advertisers, one list of affiliates, one list of every entity that we've got campaigns as an example.
[00:45:21] KW: Here's my question. So once we establish the mapping of all advertisers verticals moving forward.
[00:45:28] Brian Silveri: Yep.
[00:45:29] KW: For an objective moving forward, for any new advertiser that comes on board, we need to train every single business unit to say we're going to create one unit.
[00:45:38] KW: Are we going to just create one universal fluent id for that advertiser.
[00:45:45] Brian Silveri: Yes, yes, yes.
[00:45:52] Brian Silveri: And the already established logic that exists is that netsuite is the source of truth for billing when it comes to paying out these people, right?
[00:46:05] Brian Silveri: So netsuite has an id. If netsuite doesn't have id, no one gets paid. End of story.
[00:46:11] Brian Silveri: So using that NetSuite ID as our barrier here, Kevin, I'm like, here is the netsuite slash fluent id which no one needs to see.
[00:46:21] Brian Silveri: Just here's one ID that rules them all.
[00:46:24] Brian Silveri: Even if you go into LM and put in advertiser name as as Disney plus and you go into and you go into ad flow and you do Disney plus Hulu, ESPN as an advertiser versus playful rewards such as Disney, they all map to one NetSuite ID, I.
[00:46:47] Brian Silveri: E. Fluent ID of whatever the hell the name of it just Disney. And that's the id.
[00:46:53] Brian Silveri: And we just show advertiser list is this.
[00:46:57] Brian Silveri: And then we could show linked source really like source advertiser name.
[00:47:03] Brian Silveri: LM underscore Disney plus, whatever you got. You get what I'm saying?
[00:47:08] Brian Silveri: So yes, Kevin, I want to do that for all of the entity types because all of those entities exist across our product lines.
[00:47:19] Brian Silveri: And we can say here's the entity name, advertiser, campaign, affiliate, partner.
[00:47:27] Brian Silveri: Here's one giant table that has all of these mappings with a singular master ID with their associated linked IDs by platform.
[00:47:38] Brian Silveri: If the platforms which I will be pushing hard integrate with netsuite, that makes this even easier.
[00:47:47] Brian Silveri: If they don't even to start de inside databricks will do that heavy lifting for them.
[00:47:57] Brian Silveri: It's fuzzy or fuzzier logic right until NetSuite ID becomes adopted across these systems.
[00:48:04] Brian Silveri: So when someone types in a new name or overwrites an existing name, the ID doesn't change and then cause doing,
[00:48:15] KW: I'm sorry for historical data to be consistent.
[00:48:19] KW: And I think there should be some kind of control of as far as editing names, especially if we reestablish revenue the whole nine yards off of this.
[00:48:28] KW: It should be established and we should be creating a brand new link.
[00:48:31] KW: If it's something new and a different naming convention, then it should have a new ID to be associated with.
[00:48:36] KW: But that's conversation. I. I'm not.
[00:48:40] Brian Silveri: So as an example, I'm sure all of you know so well.
[00:48:44] Brian Silveri: So once when Conlin asked me about ancestry, I'm like, which one, Conlin?
[00:48:50] Brian Silveri: Are you talking about Ancestry or are you talking about Ancestry.com because they're both active and they both have revenue which One do you want to see?
[00:48:58] Brian Silveri: And he's like, well, that shouldn't be. I'm like, well, no shit.
[00:49:02] Brian Silveri: I was like, of course it shouldn't be. But this.
[00:49:05] Brian Silveri: This is what you have when you have manual entry as the only pivot point for these giant entities.
[00:49:11] Brian Silveri: Same hold true for partners and everything else.
[00:49:16] Brian Silveri: So I was like, hey, you have business processes you need to enhance.
[00:49:20] Brian Silveri: We will help you from a data perspective.
[00:49:23] Brian Silveri: But like, the account managers, they didn't even populate account managers for everything.
[00:49:27] Brian Silveri: And I'm like, who do you want me to show you who's doing it? Colin?
[00:49:31] Brian Silveri: He's like, well, they should have it. I'm like, okay, I'll wait until you finish.
[00:49:36] Brian Silveri: But that's the point here. It's like, you've got some work to do from a business process, right?
[00:49:41] Brian Silveri: Technology has to help you, and then data has to help you. So we're working towards that.
[00:49:46] Brian Silveri: And that was what I've conveyed to Don and Terry in my last update. Like, we're getting there.
[00:49:50] Brian Silveri: This is not an easy road. A road we've traveled on before with limited success.
[00:49:57] Brian Silveri: Now we're going all full throttle.
[00:50:03] KW: All right, cool. My bad, guys. I need to add more to this meeting.
[00:50:07] Brian Silveri: No, you're good.
[00:50:08] Brian Silveri: It's all germane to our conversation of don's vision of Fluent 1, and that's what I'm driving on.
[00:50:14] Brian Silveri: Fluent 1 for data, they can't do anything without it consolidated and clear, so we're all in it together.
[00:50:24] Brian Silveri: If you have all have questions on anything, like strategy, what my plans are, please let me know.
[00:50:29] KW: Will do.
[00:50:31] JC Camacho: Great.
[00:50:32] Brian Silveri: Otherwise, thank you for your help. And then keep us in the loop on the Everflow stuff.
[00:50:37] KW: Will do.
[00:50:37] JC Camacho: We'll do. Awesome.
[00:50:39] Venkatesh Mannam: Thanks, team.
[00:50:40] JC Camacho: Thank you, guys.
