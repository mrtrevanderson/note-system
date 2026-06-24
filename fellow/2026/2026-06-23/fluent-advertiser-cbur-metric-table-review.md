---
title: Fluent Advertiser (CBUR) Metric table review
date: 2026-06-23
time: 16:05-16:30
participants: [Trevor Anderson, Brian Silveri, Jason Warnock, Jose Pontes Barros Fernan, Kapil Sreedharan, Bharat Gidwani]
fellow_url: https://fluentco.fellow.app/meetings/040000008200E00074C5B7101A82E00800000000F0C88B230B03DD0100000000000000001000000020D065AC561C6B42934FD79CC0660254/
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
[ ] Brian Silveri to add campaign, advertiser, and account manager name dimensions to the Fluent Advertiser Performance table and deliver to Jose for CBUR reporting.

---

The team discussed data architecture strategies for **Fluent-wide metric tables** supporting CBUR (macro-level) and Advertiser (micro-level) reports. The central question was whether to build BU-specific metric tables or one consolidated table with overlapping dimensions like campaign, advertiser, and affiliate. Jose confirmed that **Domo can efficiently join multiple metric tables** and handle the performance requirements, as issues only arise with billions of rows. Brian clarified that CBUR is designed as a **senior leadership snapshot** with limited shared metrics across business units, while the Advertiser report provides deep drill-down capabilities.

The team agreed to continue using the **Fluent Advertiser Performance table** as the foundation, with plans to add campaign, advertiser, and account manager dimensions. Discussion touched on future Q3 work to consolidate campaigns across platforms using NetSuite as the unified identifier, which will reduce data duplication and improve reporting accuracy across Lead Manager, Pulse, and other systems.

### Data table architecture strategy

- The team is building Fluent-wide metric tables to support **CBUR and Advertiser reports**, with the first version (Fluent Advertiser Performance table) already complete but needing additional dimensions like campaign, account manager, and affiliate.
- Core question: whether to build **BU-specific metric tables** (like Adflow, Playful Rewards, Syndication) with overlapping metrics or one colossal Fluent advertiser metric table with all dimensions across multiple business units.
- Brian asked if **Domo can join multiple metric tables performantly** when loading CBUR data from different BU tables (Trevont, Adflow, Rewards), as the team needs to understand reporting capabilities and optimal table structures.
- Jose confirmed that **Domo is performant enough for joins** and only encounters trouble with billions of rows, which shouldn't be an issue for this use case.
- Jose noted that Domo can create **ETLs to join tables internally**, and even at campaign granularity the row count shouldn't be problematic for performance.
- Trevor and Bharat suggested starting with **granular fact tables that roll up** to macro-level views, allowing a single underlying data source to power both the detailed Advertiser report and the high-level CBUR report without duplicating work.

### CBUR vs Advertiser report distinction

- Brian clarified that **CBUR is macro-level** (no drill-down) while the Advertiser report is micro-level with deep analytics, representing two distinct deliverables with different granularity requirements.
- CBUR's purpose is to provide a **snapshot for senior leadership** when meeting with advertisers like Disney in New York, showing high-level performance across Fluent business units without detailed drill-downs.
- Different BUs have **different metric availability** - Adflow has sessions, Playful Rewards doesn't; Adso has RPT concept, Lead Manager doesn't; Trevont has no installs since nothing is downloaded.
- Conlin defined a **very small number of shared Fluent-wide metrics** (approximately seven total) after lengthy discussions, as teams couldn't initially agree on economics across Fluent business units.
- Brian positioned the reports as complementary: **'dig deep into analytics' via Advertiser report, get a 'snapshot' via CBUR**, pushing back against adding drill-down functionality to CBUR to avoid duplication.

### Campaign consolidation via NetSuite

- Jason's NetSuite project will help **consolidate the campaign list**, as there are currently over 500 campaigns in Adflow alone with no mapping between systems.
- Currently there's **no correlation between campaigns across systems** - with ~400 campaigns per system across four systems, that's potentially 1,600 campaigns total due to data duplication.
- Brian and Jason plan to work on **campaign mapping in Q3**, starting with Lead Manager and Pulse, using NetSuite as the unifying campaign ID to join platforms together.
- Brian proposes using **NetSuite ID as the unique identifier** across Fluent for entities like campaigns, advertisers, and partners, since these must be built against NetSuite IDs for billing purposes.
- Goal is to eliminate duplication where NetSuite shows 'Disney app run of network Disney plus' and Adflow shows 'Disney mobile run of network Hulu' for the **same campaign** - consolidating to one ID and one representation.

### Decisions

- Jose will **continue using the Fluent Advertiser Performance table** as the data source for CBUR reporting until further notice.

## Transcript

[00:00:10] Brian Silveri: There's jose. Hey, jose,
[00:00:33] Bharat Gidwani: When do you get the. The new hat?
[00:00:36] Jose Pontes Barros Fernan: What?
[00:00:36] Bharat Gidwani: I want
[00:00:40] Jose Pontes Barros Fernan: the. The. The new ad for Travant. I have to go to the office to get it.
[00:00:45] Trevor Anderson: Ah, only.
[00:00:48] Brian Silveri: Only downtown Toronto.
[00:00:50] Jose Pontes Barros Fernan: Only downtown. Oh, man, it's very exclusive. They could send it just a few miles up north.
[00:01:00] Jose Pontes Barros Fernan: No, you have to go downtown together.
[00:01:02] Brian Silveri: Kapil You know where your next adventure is. To school or gear for our squad.
[00:01:09] Trevor Anderson: Yeah. You have to reserve one for me.
[00:01:11] Brian Silveri: Don't go during the World cup games. He's smart.
[00:01:17] Jose Pontes Barros Fernan: Hey. Yes. I mean, yes. Not during games. Games themselves, but when there's no games. It's okay.
[00:01:24] Jose Pontes Barros Fernan: It's okay. Yeah.
[00:01:26] Kapil Sreedharan: Let me know, Jose, when you're going next. Well, I might join.
[00:01:30] Jose Pontes Barros Fernan: Oh, perfect. Yeah. Yeah.
[00:01:33] Kapil Sreedharan: Who's.
[00:01:34] Brian Silveri: Does anyone know who's closest to the office on the D squad? Isn't somebody in Toronto. Yeah.
[00:01:42] Jose Pontes Barros Fernan: Toronto proper.
[00:01:43] Brian Silveri: Yeah, like Toronto proper isn't someone in Toronto proper.
[00:01:48] Jose Pontes Barros Fernan: Kapil, where are you? You're.
[00:01:50] Kapil Sreedharan: Yeah, I'm Toronto proper.
[00:01:52] Jose Pontes Barros Fernan: Yeah. Yeah. Take.
[00:01:56] Brian Silveri: Take the Tr. Take the shuttle in, give you some swag.
[00:01:59] Kapil Sreedharan: I can actually. I bite. So it's like.
[00:02:02] Brian Silveri: Oh, no excuse 25. No excuse. Take the kid. They'll love to see the kid. Just. Did you have that?
[00:02:13] Bharat Gidwani: Get them also get them. There you go.
[00:02:18] Brian Silveri: Maybe, Maybe they got like Tron bibs, Jose. Like, yeah, maybe all kinds of swag. Who knows?
[00:02:25] Jose Pontes Barros Fernan: Well, you. You know that there's a store. There's a store that. That Fluent uses when there's new.
[00:02:33] Jose Pontes Barros Fernan: When there's new swag. Right?
[00:02:35] Brian Silveri: I haven't checked the. The Fluent store in a while. I might have to look at that.
[00:02:38] Jose Pontes Barros Fernan: Yeah. So there's a store and I think, why do that?
[00:02:42] Brian Silveri: Just send Kapil out there.
[00:02:47] Jose Pontes Barros Fernan: But.
[00:02:47] Trevor Anderson: All right.
[00:02:47] Jose Pontes Barros Fernan: My. My. My pet peeve with that. I have to finish. I started now to finish.
[00:02:52] Jose Pontes Barros Fernan: My pet peeve with that is that.
[00:02:55] Jose Pontes Barros Fernan: So there's been like, since I'm around, it's been like five or six campaigns. Right.
[00:03:00] Jose Pontes Barros Fernan: The thing is that you never are. You are never able to spend the entire amount.
[00:03:06] Jose Pontes Barros Fernan: So yeah, you're always left.
[00:03:09] Brian Silveri: Yes.
[00:03:10] Trevor Anderson: Yeah.
[00:03:10] Jose Pontes Barros Fernan: I don't know why they don't just add everything up anyway. End of rent.
[00:03:17] Brian Silveri: It's a fair point. It's a fair point. All right. So Jose, let.
[00:03:23] Brian Silveri: Let the reason I assembled you and if Jason can join, great. But you can answer the questions here.
[00:03:28] Jose Pontes Barros Fernan: Yeah.
[00:03:29] Brian Silveri: So we're trying to build the Fluent wide metric table or tables to support CBUR and the Advertiser report and all the rest of the stuff that we want to have the Most immediate ask that we have is we've built out the first version of our table already and the fluent advertiser performance table.
[00:03:56] Brian Silveri: But we've got more dimensions that we're originally adding or were proposed to add to a singular table.
[00:04:06] Jose Pontes Barros Fernan: Okay.
[00:04:07] Brian Silveri: Like the campaign, the account manager and the affiliate to start. And then there's more.
[00:04:13] Brian Silveri: The question the team has is given that we have now many BU's with many different dimensions that we want to add in there, is it better for us to build BU specific metric tables a la adflow like a playful rewards or a syndication or a pick a trant or with our each individual and overlapping metrics for the ones that are shared and the fields or try and build a colossal fluent advertiser metric table with all of these dimensions like campaign advertiser, affiliate, account manager name and then the list goes on as we expand this list.
[00:05:07] Brian Silveri: And is it is DOMO able to like join the multiple metric tables in.
[00:05:13] Brian Silveri: In a speedy enough way where that is performant so that when they load CBUR and they get they're pulling from the TR Volt table for this and the adflow table for this and the rewards table for this, it can load and sum that or so the team's trying to figure out from a reporting standpoint what can you do and what do we think is the right mix of table structures here to drive on these reporting needs?
[00:05:41] Jose Pontes Barros Fernan: Domo enough for that. Domo is performant enough for that.
[00:05:47] Brian Silveri: Oh, sorry.
[00:05:48] Jose Pontes Barros Fernan: We only start to get into trouble in the billions of rows.
[00:05:52] Brian Silveri: Okay, so Kaplan, Trevor, did I explain that situation correctly and did I miss anything in that regard?
[00:06:01] Kapil Sreedharan: You know, so we were looking at your dashboards. Is this the.
[00:06:10] Brian Silveri: Yep, that's it right there.
[00:06:13] Kapil Sreedharan: And as per this, you know, it says all business unit, all advertisers.
[00:06:19] Kapil Sreedharan: Yeah, this is a CBUR advertisers, Right?
[00:06:22] Brian Silveri: Correct. This is going to be all fluid advertisers. Yes.
[00:06:26] Kapil Sreedharan: And then I guess Jose, for you, you would be expecting one metrics view that has all the data. Right.
[00:06:36] Jose Pontes Barros Fernan: And yes. Or like, like Brian said, one that would make sense to join
[00:06:46] Brian Silveri: if.
[00:06:46] Jose Pontes Barros Fernan: If it has to be to be multiple. I mean even if ideally it would look.
[00:06:56] Jose Pontes Barros Fernan: It would look better if we're talking about the same thing, but it may not make sense to talk about the same thing across all business units.
[00:07:05] Jose Pontes Barros Fernan: Right. Does it make sense to talk sessions and playful rewards or.
[00:07:15] Brian Silveri: The metrics at this highest level are shared and that was Conlin's vision.
[00:07:21] Brian Silveri: They have to be shared metrics in order for this to make it here My, my issue or my issue is the DE's worry is if we add in two more filters here like all campaigns as a dimension and then you add in affiliates or account managers or you know, some other large dimension where we're doing these row calculations times X, is that the right data bricks setup or maybe, maybe it's not.
[00:08:06] Brian Silveri: And we can't have because we have got like gold campaign gold campaigns got campaign from everybody.
[00:08:12] Brian Silveri: And now adding campaign into a dimension in this, in this performance table might not be the right situation here when we're looking at all campaigns even though we can link them, that linking isn't a problem.
[00:08:25] Brian Silveri: It's how do we present it in a way where the data is performant and we're not spending 500 a day to query fluent advertiser performance.
[00:08:38] Brian Silveri: Trevor, check me if I'm wrong.
[00:08:40] Kapil Sreedharan: Brian, is this the all these metrics? Are these the common metrics that applies to all the views in.
[00:08:49] Brian Silveri: Yeah. Yep.
[00:08:53] Kapil Sreedharan: Okay.
[00:08:54] Brian Silveri: It's, it's, it's lightweight on purpose because there's not a ton of shared things going across here.
[00:09:01] Brian Silveri: What there are is available and he wants to see it in one snapshot.
[00:09:07] Brian Silveri: Our job is to show that snapshot in this report.
[00:09:10] Brian Silveri: It is not the advertiser micro report where we're running billions of computations.
[00:09:15] Brian Silveri: It is a macro level view across all BU's. Their only ask was those added dimensions.
[00:09:23] Brian Silveri: Now I guarantee you Colin's going to come back with I want blobbity blobbity blah something in Q3.
[00:09:32] Brian Silveri: It's going to happen. I don't know what the blobbity blobbly is but it's going to happen.
[00:09:36] Brian Silveri: I'm accounting for it.
[00:09:38] Brian Silveri: When I add more dimensions or metrics, what is the right setup so Jose doesn't have to go back and rebuild this damn dashboard when we split this into five different tables.
[00:09:49] Brian Silveri: I much rather do the infrastructure correctly one time.
[00:09:53] Bharat Gidwani: And one one other thing Jose for you as well as is there drill rounds in this like can kotlin or like whoever is looking at it, can they like click on playful rewards and say now show me more information about where the spend is or whatever else.
[00:10:10] Brian Silveri: Their only ask was campaign.
[00:10:14] Bharat Gidwani: So even for campaign let's say if this is, if this is at a campaign granularity.
[00:10:20] Brian Silveri: Sure.
[00:10:20] Bharat Gidwani: They drill down and say now show me more information about how this campaign is performing in adflow from here, from this dashboard itself.
[00:10:30] Bharat Gidwani: Like can they drill down further?
[00:10:32] Bharat Gidwani: Because if they do then we're talking about adding more dimensions at least for those Specific views.
[00:10:40] Bharat Gidwani: Now, because as Jose was also saying, right, for adflow, they have concept of sessions, playful rewards.
[00:10:48] Bharat Gidwani: There is no such thing.
[00:10:49] Brian Silveri: Right.
[00:10:50] Bharat Gidwani: We don't have the information about even though they might have it, but we don't have anything about it and so on.
[00:10:55] Bharat Gidwani: With lead manager, they may not have anything to do with session.
[00:10:58] Bharat Gidwani: That's why sessions are just blank there.
[00:11:00] Trevor Anderson: Sure.
[00:11:00] Bharat Gidwani: RPT and so on. Right. Like Adso has a concept of rpt, lead manager may not have that concept at all.
[00:11:08] Bharat Gidwani: So now if I drill down, am I getting granular there saying no RPT and so on?
[00:11:17] Brian Silveri: No, no. That was not the bounds of this ask for cbur. It was not a drill down.
[00:11:23] Brian Silveri: That was the advertiser report.
[00:11:27] Brian Silveri: Ryan wanted a deep drill down into uber level micro level metrics, which is why I made the distinction between those two Deliverables.
[00:11:38] Brian Silveri: Advertiser is micro, CBUR is macro.
[00:11:41] Brian Silveri: If he wants more data about campaign with creative number seven here on January 7th, that's the micro report.
[00:11:51] Brian Silveri: He wants to take this and then show it to Disney when he meets with Disney in New York and say, FYI, here's how Disney's doing across fluent.
[00:12:01] Brian Silveri: That's the scope of what Conlon wants for this, I think.
[00:12:05] Bharat Gidwani: And I think we are just having similar conversation earlier.
[00:12:08] Bharat Gidwani: Brian Kapil Venky and others, maybe we build a micro stat internally, but then when it's rolled up and aggregated, we just provide a micro view for Jose to consume from.
[00:12:28] Bharat Gidwani: That way if a different view is required in Domo or anywhere else, at least we'll have the information.
[00:12:39] Bharat Gidwani: We don't have to recompute it at that point.
[00:12:44] Bharat Gidwani: Because now we're like, oh, we don't have to go back to drawing board and figure out things.
[00:12:48] Bharat Gidwani: But again, that's an option. Not that we want to go in that direction, but that's an option.
[00:12:52] Brian Silveri: Yeah, I, I would be there.
[00:12:56] Brian Silveri: I would be very hesitant to go to the micro level here because we've already given them the option of that micro at the advertiser report level.
[00:13:07] Brian Silveri: This gives you that and that's the sell that I did to Conlin and to Ryan.
[00:13:11] Brian Silveri: FYI, you want to dig deep into any analytics, go to this advertiser report.
[00:13:17] Brian Silveri: You want a snapshot, you go here. And this is a senior leadership level report.
[00:13:23] Brian Silveri: It is not a micro drill down. I would push back harder on Conlin if he said, oh, let me drill xyz.
[00:13:30] Brian Silveri: You're just duplicating the advertiser report and just go tell Ryan to go do it himself.
[00:13:34] Brian Silveri: Those are two different things. So I would, I would not be leaning towards the micro level and I
[00:13:43] Trevor Anderson: guess to just, to Barat's point is this idea of like if we have the ability to have to start at a granular level with our data, you know, have that as the bottom part and then roll up and aggregate from there, maybe we only have a certain tier of granularity for this report.
[00:14:02] Trevor Anderson: Like, like you're saying, but at least we have the more granular data available.
[00:14:07] Bharat Gidwani: Yeah, like your fact table could be granular but your UCM is more macro level.
[00:14:12] Bharat Gidwani: So that again, from a, from a consumption perspective, it doesn't matter at that point because I have the data now, I can slice and dice it via UCM however we want.
[00:14:27] Bharat Gidwani: So technically, again, we haven't looked at it yet, but technically you can have a fact table which can power the advertiser report and this, this CVOR report again we can look into that as an option.
[00:14:42] Bharat Gidwani: That way we don't have to do the work twice.
[00:14:47] Brian Silveri: I, I leave it up to all of you, but I want to make sure that Jose knows and can support whatever technical architecture we're proposing here.
[00:14:57] Brian Silveri: And it sounds like Jose Domo Ken.
[00:15:00] Brian Silveri: Yeah, I, at the very least to start now, I pointed him towards the Fluent Advertiser performance report because it was structured in this way to drive the macro level economics and that's what it's supposed to do.
[00:15:14] Brian Silveri: So.
[00:15:15] Bharat Gidwani: Yeah, yep, yeah, yeah, that makes sense. That's fine.
[00:15:19] Jose Pontes Barros Fernan: Yeah.
[00:15:20] Kapil Sreedharan: I think Ideally, you know, one fact Table 1 metrics view, you know, will be easier for everyone or at least for Jose.
[00:15:32] Kapil Sreedharan: But my question is this, is this the right, you know, metrics? Like I look at the playful rewards.
[00:15:43] Kapil Sreedharan: Yeah, there is clicks, installs.
[00:15:46] Brian Silveri: It is, it has, it's, it's, it's, it's not.
[00:15:49] Brian Silveri: So that mockup, I think Jose, I don't know where we are with the latest version, but that was the mock up that I presented to them to say here's what I imagined.
[00:15:57] Brian Silveri: It's going to be based off of what Conlin asked for. So this is very specific.
[00:16:04] Brian Silveri: These metrics are very specific to rewards and ad flow. There is no installs on play on.
[00:16:12] Brian Silveri: You know, Trevont doesn't have installs, no one's downloading anything.
[00:16:16] Brian Silveri: So the, that's why this project took so long is because I had to define fluent wide metrics that are shared and repeatable when it's presented to an advertiser, which is a very small number thankfully because we could not agree on the economics across Fluent Conlin had to decide here are the things and it's like seven of them, I think is our total number or something stupid.
[00:16:43] Brian Silveri: Which is great. Less for us.
[00:16:47] Kapil Sreedharan: So that is. So those are the metrics that you put in that in the cbu.
[00:16:54] Brian Silveri: Yeah. So the ones that are.
[00:16:56] Brian Silveri: That they added, which were not originally in the prd, which they added campaign as an example.
[00:17:02] Brian Silveri: Campaign wasn't really there. We added vertical.
[00:17:05] Jose Pontes Barros Fernan: Yeah.
[00:17:05] Brian Silveri: And vertical. Okay, great. You now need campaign and vertical. Okay, fine.
[00:17:11] Brian Silveri: I already built Fluent Vertical and Fluent Gold campaign in prep of knowing I would need it for the advertiser report.
[00:17:20] Brian Silveri: So Hooray. Brian knows what he's doing. We already have the data. Just surface it inside cbur.
[00:17:30] Brian Silveri: So we've got. We've got all the data.
[00:17:32] Brian Silveri: Our issue isn't the data, it's how we want to present it to Jose so that he can digest it and show it on this macro level report.
[00:17:40] Brian Silveri: I don't care what it is.
[00:17:42] Bharat Gidwani: Yeah, I think Jose, as Brian was saying, most likely, at least for those two different reports, the views or the underlying source will be different.
[00:17:53] Bharat Gidwani: Like there might be two different metric views, one for showing advertiser level grain, one as a consolidated cbor.
[00:18:03] Bharat Gidwani: Fluent wide stuff. But internally we might power it from the same tables and so on.
[00:18:12] Bharat Gidwani: That way we don't have to recompute things multiple times.
[00:18:17] Bharat Gidwani: But yeah, for you, most likely it could be different because the grains are different in both the cases.
[00:18:24] Bharat Gidwani: But we'll definitely look into it and see if we can have it consolidated in a single. Single metric.
[00:18:33] Bharat Gidwani: As you said, I think for you it will be easier to consume a single data source. Right.
[00:18:38] Bharat Gidwani: Instead of having one for each view.
[00:18:42] Jose Pontes Barros Fernan: Yeah.
[00:18:42] Jose Pontes Barros Fernan: I mean, we can create an ETL to join them inside Domo, but in terms of granularity, I don't think this is a problem.
[00:18:53] Jose Pontes Barros Fernan: Even going down to campaign, it shouldn't be in terms of rows, it shouldn't be too bad.
[00:18:59] Bharat Gidwani: Okay.
[00:19:03] Brian Silveri: It will get better actually, with Jason's netsuite project, which means we can.
[00:19:12] Brian Silveri: We're going to consolidate the list of campaigns. It'll shrink, which is hooray. Great.
[00:19:17] Bharat Gidwani: How many, how many campaigns are there? If anyone knows, in total there's like 200, 400.
[00:19:26] Brian Silveri: Well, right now inside Adflow, there's over 500 campaigns.
[00:19:31] Bharat Gidwani: Okay, so I think, to be just to be clear, right, right now there is no mapping of campaigns between systems.
[00:19:39] Bharat Gidwani: So if there are, let's say 400 campaigns in each system, you're talking about at least 1600 campaigns across four systems.
[00:19:46] Bharat Gidwani: Because there's no direct correlation between this campaign is the same as the other campaign in.
[00:19:53] Bharat Gidwani: In this system. So. Yeah, so again from when you say granularity, just I want to make sure.
[00:19:59] Bharat Gidwani: Yes, it's not massive data set. But you're talking about duplicity in that data.
[00:20:05] Kapil Sreedharan: Yes, Correlation is what we are building
[00:20:07] Brian Silveri: right, with that cbr if we're building
[00:20:11] Bharat Gidwani: the support for it. But there is no one who's mapping it as yet.
[00:20:16] Brian Silveri: No, that's what Jason and I are going to work on in Q3, starting with lead manager.
[00:20:22] Brian Silveri: I'm going to do Pulse as well.
[00:20:24] Brian Silveri: And that'll tie into NetSuite and NetSuite will be that campaign ID that joins those two platforms together.
[00:20:32] Brian Silveri: And then I want to do the same thing for every other platform.
[00:20:36] Brian Silveri: So at the very least in Q3 we'll have a unifying ID that we can pivot off of to say Campaign NetSuite Campaign ID 1234 is this, this maps to Adflow Campaign 7892.
[00:20:51] Brian Silveri: They're the same and there's no to applicativeness.
[00:20:56] Brian Silveri: Where NetSuite shows Disney app, you know, run a network Disney plus and Ad Flow shows Disney mobile run of network Hulu.
[00:21:08] Brian Silveri: Like this is so stupid. It's the same thing. Same campaign with the same same thing.
[00:21:14] Brian Silveri: But we're showing it twice. We want to show it one time with one ID.
[00:21:18] Brian Silveri: So that's the goal in Q3, but it should be not too bad for these dimensions, which is why I added that ticket.
[00:21:24] Brian Silveri: But you guys tell me we can
[00:21:27] Kapil Sreedharan: discuss that netsuite maybe later when we get data.
[00:21:32] Kapil Sreedharan: But so what I'm thinking is looking at the NetSuite schema at the campaign level, they have the advertiser ID.
[00:21:41] Kapil Sreedharan: So there they are tying this campaign to an advertiser.
[00:21:45] Kapil Sreedharan: So yeah, so if there's a campaign in in Lead Manager campaign in Pulse, that that is type an advertiser in netsuite.
[00:21:54] Kapil Sreedharan: So in netsuite there's one Disney and that Disney is attached to multiple campaigns.
[00:22:00] Brian Silveri: That's right.
[00:22:01] Brian Silveri: But that NetSuite name, that NetSuite ID and NetSuite name isn't always the same yet across the platforms.
[00:22:11] Brian Silveri: Another problem which we've got to tackle.
[00:22:14] Brian Silveri: Okay, so but the NetSuite ID is going to be our ID of record across fluent.
[00:22:23] Brian Silveri: That's how I'm thinking it for these entities that exist in NetSuite Campaign Advertiser partner.
[00:22:31] Brian Silveri: All of those entity types exist in netsuite because they have to be built on build against those IDs.
[00:22:40] Brian Silveri: So great. Let's use that as our unique identifier and tie to those other things.
[00:22:45] Brian Silveri: But either way, because we're right. Time. Jose, thank you. You've given us the answer that we need.
[00:22:51] Brian Silveri: We'll tell you for sure what the structure is.
[00:22:54] Brian Silveri: For now, continue to use Fluent advertiser performance unless we tell you otherwise.
[00:22:58] Jose Pontes Barros Fernan: Okay.
[00:22:59] Kapil Sreedharan: And Brian, are these the. The. The micro metrics?
[00:23:04] Bharat Gidwani: Yeah.
[00:23:05] Kapil Sreedharan: Okay, so this is the final version. It's only handful, right? In person.
[00:23:09] Brian Silveri: That's it.
[00:23:10] Jose Pontes Barros Fernan: Yeah.
[00:23:11] Kapil Sreedharan: Okay. And. And this applies across like Playful rewards, Lead manager and Fluent.
[00:23:16] Brian Silveri: That's right.
[00:23:17] Kapil Sreedharan: Adflow.
[00:23:18] Brian Silveri: Yes.
[00:23:19] Kapil Sreedharan: Okay. Yeah.
[00:23:20] Brian Silveri: Cool.
[00:23:21] Jose Pontes Barros Fernan: Yeah, perfect.
[00:23:23] Brian Silveri: It's. It's not that.
[00:23:26] Brian Silveri: The reason it's a macro level view is because these are things that they only apply across Fluent and.
[00:23:32] Brian Silveri: And it's a small, small list. So. Great. This is what Conway wants to see. We'll show it to him.
[00:23:38] Brian Silveri: Jose, use Fluent advertiser performance.
[00:23:40] Brian Silveri: We'll add campaign advertiser and account manager name in there and we'll get that over to you.
[00:23:46] Jose Pontes Barros Fernan: Perfect.
[00:23:47] Brian Silveri: Great. Thanks, squad. No problem, ct.
