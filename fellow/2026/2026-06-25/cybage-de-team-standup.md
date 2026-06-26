---
title: "Cybage - DE team standup"
meeting_id: "040000008200E00074C5B7101A82E00807EA0619D04731FE45E9DC01000000000000000010000000B4E52B2A9B75554886302B59A05B811C"
note_id: 80890651
recording_id: 15090936
date: 2026-06-25
start_time: "2026-06-25T09:05:00-04:00"
end_time: "2026-06-25T09:30:00-04:00"
url: "https://fluentco.fellow.app/meetings/040000008200E00074C5B7101A82E00807EA0619D04731FE45E9DC01000000000000000010000000B4E52B2A9B75554886302B59A05B811C/"
participants:
  - name: "Trevor Anderson"
    email: "tanderson@fluentco.com"
    rsvp: accepted
    organizer: false
  - email: "BSilveri@fluentco.com"
    rsvp: accepted
    organizer: true
  - email: "AGonna@fluentco.com"
    rsvp: accepted
    organizer: false
  - email: "BGidwani@fluentco.com"
    rsvp: accepted
    organizer: false
  - email: "JRamesh@fluentco.com"
    rsvp: none
    organizer: false
  - email: "ksreedharan@fluentco.com"
    rsvp: accepted
    organizer: false
  - email: "vkaithi@fluentco.com"
    rsvp: accepted
    organizer: false
  - email: "VMannam@fluentco.com"
    rsvp: accepted
    organizer: false
  - email: "gsajjanshetty@fluentco.com"
    rsvp: accepted
    organizer: false
  - email: "mpaul@fluentco.com"
    rsvp: none
    organizer: false
  - email: "AKumar@fluentco.com"
    rsvp: none
    organizer: false
  - email: "bpamnani@fluentco.com"
    rsvp: accepted
    organizer: false
  - email: "PAhire@fluentco.com"
    rsvp: none
    organizer: false
  - email: "sbelawat@fluentco.com"
    rsvp: none
    organizer: false
  - email: "SShaikh@fluentco.com"
    rsvp: accepted
    organizer: false
  - email: "SMandlik@fluentco.com"
    rsvp: none
    organizer: false
  - email: "kumaamit@cybage.com"
    rsvp: none
    organizer: false
    external: true
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
[ ] Granth S to update the FDSO to reflect the corrected S3 bucket naming convention (Audience Solutions def) that matches the production naming standard.
[ ] Check on the model audience S3 bucket creation with SM and loop in Joseph on the status.
[ ] Kapil Sreedharan to analyze and design how to combine different event tables into a unified fact table that powers the Fluent metrics view across all business units.
[ ] Akhil Gonna to reconcile Tremendous campaign payment amounts with source platform (Pulse or Lead Manager) fulfillment costs by mapping campaign names.
[ ] Akhil Gonna to create a placeholder ticket for the gold campaign minion table null fields issue (5 unpopulated fields) to refine when Brian returns.
[ ] Kapil Sreedharan to update the NetSuite IT ticket in the blocked section of JIRA with current status and tracking information.
[ ] Venkatesh Mannam to add the FDSO for creating the new catalog to ticket 1612 for tracking purposes.
[ ] Review with the team and add details to the subtasks for table migrations and source specifications.
[ ] Manav Paul to create a ticket for Lotta May cross-account access setup and coordinate with the team on the instance profile configuration.
[ ] bharatpam to check if the campaign corruption issue in the campaign ref table has been resolved with yesterday's release and report back if it still occurs.
[ ] bharatpam to analyze the tremendous campaigns in the tremendous dev catalog to determine if they can be included in the fluent campaign table and how they map to Pulse and Lead Manager.
[ ] Kapil Sreedharan to create a ticket to review all fact tables and determine if data regeneration is needed due to mapping rule changes.
[ ] Kapil Sreedharan to set up time with Trevor to discuss the fact tables review internally.

## AI Summary

The Cybage data engineering team's standup covered multiple active workstreams and resolved key technical blockers. The team standardized the **S3 bucket naming convention** to 'Audience Solutions' across environments and decided to create a **unified metrics view for all business units** rather than separate views. Development continued on gold table implementations, with **40 out of 71 campaign IDs missing** from the Tremendous reference table creating a blocker for the rewards table.

A critical design decision was made to use **SCD Type 1 for dimension tables** (advertiser and campaign) rather than Type 2, as current requirements only need active records without historical tracking. The team also addressed the **tune click conversion table decommissioning** and committed to following the **DOT naming standard** for all new tables going forward. Multiple code reviews are in progress, and the **advertiser performance table will move** to the fluent_metrics_mv schema.

### Chapters

#### Gold Table Development
- Building the gold rewards table revealed **40 out of 71 campaign IDs were not found** in the Tremendous campaign reference table, which was reported to JC. *(0:07:32)*
- A calculation issue was discovered where **negative payouts result from cancelled orders** (e.g., 4 cancellations at -$75 each plus 1 executed at $75 totaling -$225), creating confusion about refund handling. *(0:07:48)*
- The team needs to **map Tremendous campaign IDs back to source platforms** (Lead Manager or Pulse) to validate payment amounts against fulfillment costs in the original campaign systems. *(0:11:33)*
- Of 71 distinct campaign IDs in the orders table, only **30 can be matched to campaign names** in the Tremendous campaign table, with the remaining 41 preventing proper mapping to source platforms. *(0:13:36)*
- JC is working with the Tremendous team to resolve the missing 41 campaigns, which is critical for completing the gold rewards table development. *(0:15:08)*
- Samreen raised an issue about **five fields in the gold campaign minion table that have been unpopulated** for a long time, creating confusion about their purpose. *(0:15:50)*
- Akhil will create a placeholder ticket describing the null field issue to be refined when Brian returns, as these columns appear to be non-functional placeholders. *(0:16:17)*
- The **campaign status field that Samreen needs** is shown in the Pulse UI but not passed through the configuration field or available in current data sources. *(0:16:51)*
- The best source for active campaigns will be **NetSuite data via the CBOR campaign tables**, which should contain all campaigns actively generating revenue, rather than relying on potentially orphaned Pulse data. *(0:18:52)*

#### SCD Type Implementation
- The team debated between **SCD Type 1 and Type 2** for advertiser and campaign dimension tables, considering historical tracking requirements and downstream complexity. *(0:40:21)*
- Venkatesh explained that when advertiser names change in Pulse (e.g., disney.com to disney co), the **fluent advertiser ID remains the same** rather than creating a new advertiser record. *(0:43:20)*
- Type 2 would require **duplicate fluent advertiser IDs** with active/inactive flags and a separate primary key, plus downstream queries would need to filter for active records only. *(0:47:25)*
- Current requirements from Conlin's dashboard and data science models (Sunmi) only need **active advertisers and campaigns**, with no demand for historical tracking. NetSuite also uses Type 1 approach. *(0:49:17)*
- The team decided on **Type 1 implementation** with the option to add a separate logging table if historical change tracking becomes necessary in the future. *(0:50:12)*

#### Campaign Table Design
- bharatpam asked about handling **digit-only campaign names** that appear to be campaign IDs rather than actual names, particularly for lead manager campaigns. *(0:51:01)*
- A **corrupted CSV file** caused issues in the campaign ref table during ingestion, but this should be resolved with yesterday's release and needs verification. *(0:51:50)*
- The fluent campaign table **joins data from Tune and Lead Manager catalogs**, bringing them together in the Fluentco catalog rather than performing joins within source catalogs. *(0:53:35)*
- **Tremendous campaigns** should also be included in the analysis, with both bharatpam and Akhil tasked to determine how these campaigns map to Pulse and Lead Manager systems. *(0:56:18)*
- Since mapping rules are changing for campaign tables, Kapil will review whether **fact tables need data regeneration** and will discuss this internally with Trevor. *(0:58:33)*

#### CBR Metrics View
- The deployment was completed successfully yesterday, but **two blocker tickets (1459 and 1463) were raised** by the Cybage team requiring design analysis. *(0:02:38)*
- The gold advertiser table is missing the **NetSuite ID field**, which is blocking progress on the CBR work, though other fields are populated. *(0:03:44)*
- After discussion with Brian, the team decided to create **one unified metrics view for all business units** rather than separate views, since selected metrics apply across all BUs. *(0:04:20)*
- Adding a BU column to the fact table may be needed for the unified metrics view, and **combining different event tables (with tune) will add complexity** that requires design analysis. *(0:05:25)*
- Kapil will analyze the logic for bringing all events together into a fact table that powers the Fluent metrics view, with internal discussion to follow. *(0:06:10)*

#### Model Audience Pipeline
- The team discovered a naming mismatch between the dev and prod S3 buckets for model audience, with the **prod bucket using 'Audience Solutions' convention** while dev was different. *(0:01:20)*
- Grant confirmed the code already implements the correct naming convention (Audience Solutions def) to match production, and will update the FDSO documentation accordingly. *(0:01:49)*
- Trevor will follow up with SM (DevOps) on the bucket creation blocker that was discussed with Joseph the previous day and loop Joseph in on the status. *(0:02:17)*
- Manav is working on the **model audience request** (distributing today) and will test and validate the current declare audience pipeline now that device-related blockers have been resolved as of yesterday. *(0:36:09)*
- Dev buckets were created in the DSO ticket for testing purposes, allowing file writes for validation instead of relying only on dry-run mode. *(0:37:14)*
- The **Lotta May integration** requires a different setup involving cross-account access and instance profile configuration, which will be handled separately. TransUnion and Liveram(?) locations are already available. *(0:38:30)*

#### Table Decommissioning Work
- The **tune click conversion table needs to be decommissioned** as three tasks currently populate it, but after switching to the new table source, these tasks are no longer needed. *(0:26:46)*
- The tune click conversion job is failing and **not populating latest data**, causing fact advertiser performance hourly to lack current information. *(0:28:47)*
- Bharat's PR addresses changing the source table pointer from tune click conversions to tune gold events, with all references now pointing to the new source. *(0:29:14)*
- The decommission will be completed in a single PR, including pausing the job, updating YAML files to remove tasks, and moving deprecated job files to a separate folder. *(0:29:36)*

#### Other Updates
- Joseph is working on the summary discussed yesterday and waiting on a **PR from Manav for review**, plus awaiting DevOps resolution on the model audience blocker. *(0:00:51)*
- Grant is reviewing **Joseph's PR and Manav's PR**, plus working on the PII masking PR, with ticket 1612 expected to be ready for review. *(0:20:47)*
- Vani completed code changes for **app engine and position views** based on feedback from Trevor and Bharat, with both now ready for review and validated against the minion report. *(0:21:29)*
- Venkatesh is working on **backfilling fluent IDs in the offer events table (ticket 1619)** by adding a new task to the existing pipeline that runs once daily after the upstream lookup table refreshes. *(0:22:22)*
- Moving date of birth age range inference to gold customer affected **443 million records** and enriched older records that previously lacked date of birth in their profiles. *(0:23:47)*
- samreensh raised questions on ticket **1489** regarding the target catalog schema for fact_auction_candidate table and missing outcome mappings for two reasons (frequency cap breach and one other). *(0:33:49)*
- The outcome mapping table includes **11 reasons** but two additional reasons appear in the data without corresponding mappings, requiring clarification from Brian upon his return. *(0:34:28)*
- Trevor will review the ticket to clarify the catalog destination and acknowledged that the additional outcome mappings may need to wait for Brian's input on the source of the original mappings. *(0:35:05)*
- Discussion about migrating tables from the **central_data_prod catalog** to the fluent catalog, with sources also moving to maintain consistency. *(1:01:22)*
- All new tables must follow the **DOT naming standard** rather than legacy conventions, though the current gold advertiser table uses 'create_date' instead of 'timestamp'. *(0:50:42)*
- Data transformations and joins across catalogs (Tune, Lead Manager) should happen in the **Fluentco catalog**, not within the source-specific catalogs. *(0:55:15)*

### Action Items
- Granth S: Granth S to update the FDSO to reflect the corrected S3 bucket naming convention (Audience Solutions def) that matches the production naming standard.
- Trevor Anderson: Check on the model audience S3 bucket creation with SM and loop in Joseph on the status.
- Kapil Sreedharan: Kapil Sreedharan to analyze and design how to combine different event tables into a unified fact table that powers the Fluent metrics view across all business units.
- Akhil Gonna: Akhil Gonna to reconcile Tremendous campaign payment amounts with source platform (Pulse or Lead Manager) fulfillment costs by mapping campaign names.
- Akhil Gonna: Akhil Gonna to create a placeholder ticket for the gold campaign minion table null fields issue (5 unpopulated fields) to refine when Brian returns.
- Kapil Sreedharan: Kapil Sreedharan to update the NetSuite IT ticket in the blocked section of JIRA with current status and tracking information.
- Venkatesh Mannam: Venkatesh Mannam to add the FDSO for creating the new catalog to ticket 1612 for tracking purposes.
- Trevor Anderson: Review with the team and add details to the subtasks for table migrations and source specifications.
- Manav Paul: Manav Paul to create a ticket for Lotta May cross-account access setup and coordinate with the team on the instance profile configuration.
- bharatpam: bharatpam to check if the campaign corruption issue in the campaign ref table has been resolved with yesterday's release and report back if it still occurs.
- bharatpam: bharatpam to analyze the tremendous campaigns in the tremendous dev catalog to determine if they can be included in the fluent campaign table and how they map to Pulse and Lead Manager.
- Kapil Sreedharan: Kapil Sreedharan to create a ticket to review all fact tables and determine if data regeneration is needed due to mapping rule changes.
- Kapil Sreedharan: Kapil Sreedharan to set up time with Trevor to discuss the fact tables review internally.

### Decisions
- **S3 bucket naming convention** for model audience will be 'Audience Solutions dev Pipeline sieves' to match the existing production bucket naming standard.
- The team will create **one unified metrics view for all business units** rather than separate views per BU, since the Fluent performance metrics are applicable across all business units.
- The **tune click conversion table decommissioning** will be completed in a single PR along with pointing fact advertiser performance hourly to the new source.
- **SCD Type 1 will be used** for advertiser and campaign dimension tables instead of Type 2, with no historical tracking of name changes or attribute updates.
- **Column naming will follow the DOT standard** for all new tables going forward, not the naming convention shown in the current ticket.
- **Tremendous campaigns will be added** to the campaign table analysis to understand how they link with Pulse and Lead Manager campaigns.
- The advertiser performance table will be **moved from central data to the Fluentco metrics catalog** (fluent_metrics_mv schema).

## Transcript

[00:00:00] bharatpam: Guys.
[00:00:00] Akhil Gonna: Morning team.
[00:00:14] Kapil Sreedharan: Hey, morning team.
[00:00:16] Akhil Gonna: Good morning.
[00:00:36] Trevor Anderson: Okay, does anyone want to kick us
[00:00:39] Akhil Gonna: off
[00:00:42] Joseph: can go through it.
[00:00:43] Trevor Anderson: Awesome, thanks Joseph.
[00:00:51] Joseph: Yeah, I
[00:00:51] Joseph: was mostly working with that summary which we are talking about yesterday, so.
[00:00:56] Joseph: And also I haven't got a PR from Manav today.
[00:00:58] Joseph: We'll review it and just waiting for DevOps on the model audience blocker that we discussed yesterday.
[00:01:04] Venkatesh Mannam: And apart from that, no blockers.
[00:01:07] Trevor Anderson: Okay, let me loop you in with SM this morning. But I did let him know yesterday afternoon in Grant.
[00:01:16] Trevor Anderson: Grant, are you on?
[00:01:20] Akhil Gonna: Yeah.
[00:01:20] Trevor Anderson: So we talking with Joseph and we just wanted to make sure we saw in the FDSO there, you know we were going to create a bucket for the model audience.
[00:01:32] Akhil Gonna: Yep.
[00:01:34] Trevor Anderson: And we saw the naming convention for the existing prod bucket wasn't matching this dev bucket.
[00:01:42] Trevor Anderson: So I think we want to change it to Audience Solutions Dev Pipeline sieves.
[00:01:47] Trevor Anderson: But I wanted to confirm with you first.
[00:01:49] Granth S: No, actually we made that change already in the code. So it is Audience Solutions def.
[00:01:57] Granth S: That matches the prod naming convention. The previous time it was not. So I'll update the fds.
[00:02:05] Akhil Gonna: Thanks for reminding me about that.
[00:02:10] Trevor Anderson: Implemented it yet?
[00:02:12] Granth S: Okay, I'll make a. I'll leave a
[00:02:15] Akhil Gonna: note on the fds.
[00:02:17] Trevor Anderson: Okay, thanks. Yeah, so gonna check on that this morning Joseph and I'll loop you in.
[00:02:24] Trevor Anderson: But yeah, thank you. Okay, great. Is next.
[00:02:38] Kapil Sreedharan: Yeah, I can go. Yeah, we did the deployment yesterday. That. Yeah, that looks okay.
[00:02:47] Kapil Sreedharan: Now there are some block tickets that were raised by the CYBIT team.
[00:02:54] Kapil Sreedharan: I'll create a spike story or we'll need to do some analysis design for some of the questions raised.
[00:03:02] Akhil Gonna: Which.
[00:03:03] Trevor Anderson: Which ones Exactly.
[00:03:05] Kapil Sreedharan: Samreen, you want to cover that? What are the open.
[00:03:13] samreensh: These are 1459 and 1463.
[00:03:22] Trevor Anderson: 459. This is cbr work. Okay. M 146 3. Oh, same thing. Okay. And so what's blocking it?
[00:03:44] samreensh: We needed the. We needed the advertiser table. The gold advertiser table.
[00:03:56] Kapil Sreedharan: The gold advertiser table. Is there.
[00:04:01] Kapil Sreedharan: The only pending thing in there is there's no NetSuite ID but other than
[00:04:10] Trevor Anderson: that
[00:04:13] Kapil Sreedharan: there's also a question around do we. Are we creating separate metrics views for each of the.
[00:04:20] Kapil Sreedharan: Each of the business units.
[00:04:23] Kapil Sreedharan: We had a discussion the other day including Brian and I think we should be able to create one metrics view for all the bus because he.
[00:04:36] Kapil Sreedharan: He selected only the metrics that are applicable for all the bus in this fluent performance metrics.
[00:04:46] Kapil Sreedharan: So we should be also be able to do that.
[00:04:54] Trevor Anderson: Yeah.
[00:04:54] Trevor Anderson: So Kapil are you saying to have a individual or an individual metrics view for each BU or each score.
[00:05:06] Trevor Anderson: No, no, it would be really. Or just one.
[00:05:09] Kapil Sreedharan: It'll be just one.
[00:05:10] Akhil Gonna: It's.
[00:05:11] Trevor Anderson: It's this. It's the one that combines all of them in that report that.
[00:05:15] Akhil Gonna: Yeah.
[00:05:15] Trevor Anderson: Looking at. Right?
[00:05:16] Kapil Sreedharan: Yeah.
[00:05:17] Trevor Anderson: With all the. With all the common metrics that. That are shared between sources.
[00:05:22] Akhil Gonna: Yeah. Okay,
[00:05:25] Kapil Sreedharan: so the. Yeah, for that the. We might need an additional BU column in the fact table.
[00:05:33] Kapil Sreedharan: But I think that would be a new design thing. Might need to look into it. But Samreen, what.
[00:05:40] Kapil Sreedharan: What exactly besides the advertiser table. Are you block?
[00:05:48] samreensh: Yeah. If we are using one table, then again there will be like. We'll have to combine the.
[00:05:57] Kapil Sreedharan: Yeah. The different tables.
[00:06:01] samreensh: With the tune. Yeah. Fact with the tune. That will again be a complexity there.
[00:06:09] Trevor Anderson: Yeah.
[00:06:10] Kapil Sreedharan: I'll do an analysis and I'll let you know.
[00:06:15] samreensh: Okay, sure.
[00:06:17] Kapil Sreedharan: So travel.
[00:06:18] Kapil Sreedharan: They need to understand how exactly to bring all these events together in a factive that will power this fluent matrix view.
[00:06:31] Trevor Anderson: I see. Okay. So we need to. We need to build a. Or think about how. Yeah, the logic for that would work.
[00:06:39] Trevor Anderson: Okay. Okay, great. We'll set up some time internally and discuss. But thank you, Samurine.
[00:07:00] Trevor Anderson: Anything else?
[00:07:02] Kapil Sreedharan: No, that's okay.
[00:07:03] Trevor Anderson: Thank you. Thanks for getting that release out yesterday.
[00:07:11] Akhil Gonna: Two. I'll.
[00:07:15] Trevor Anderson: I'll make sure to close the. This one.
[00:07:18] Akhil Gonna: Yeah.
[00:07:19] Trevor Anderson: To close 30 today. Who'd like to go next?
[00:07:29] Akhil Gonna: Oh, go ahead. I can. Oh, thanks.
[00:07:32] Akhil Gonna: So yesterday I was working on this building gold rewards and then still found 40 campaign IDs were not found and I reported to JC.
[00:07:44] Akhil Gonna: Yeah, probably. But we still have 30 other campaigns. But I was.
[00:07:48] Akhil Gonna: I was building the table and found one more issue where if you open the original ticket 1187 I get for few of them the negative payout.
[00:08:00] Akhil Gonna: If you scroll down to the screenshot. Yeah.
[00:08:06] Akhil Gonna: If you see the result here, it's the same user and the same campaign ID and it's the same date.
[00:08:13] Akhil Gonna: But the payout which I get here is like in negative because the status executed is 175.
[00:08:23] Akhil Gonna: But there are four cancel which are like negative 300. When I do a total it's like negative 225.
[00:08:35] Trevor Anderson: Are you saying in the other table you see negatives or this is in.
[00:08:40] Akhil Gonna: This is the orders table where I need to calculate total payout for each individual user and group by individual user and campaign id.
[00:08:52] Akhil Gonna: And the calculation I'm doing is based on the status here.
[00:08:56] Akhil Gonna: If you see status executed, which means payment is done cancel which means payment is not done or either refunded something like that.
[00:09:09] Akhil Gonna: Yeah. I'm not sure how exactly that works here. I see.
[00:09:20] Trevor Anderson: So you need to see if you're aggregating if you need to include.
[00:09:26] bharatpam: Yeah.
[00:09:26] Akhil Gonna: How the refunds would be calculated is what I need exactly. From our initial.
[00:09:33] Akhil Gonna: Initial conversation executed is like positive 75, whichever cancelled is like negative 75 or whatever the payment subtotal is.
[00:09:44] Akhil Gonna: But if I total up like here we have negative 75, 4 and 1 positive 75, which is like
[00:09:53] Venkatesh Mannam: 3.
[00:09:53] Akhil Gonna: Negative 3, 22, 25.
[00:09:59] Trevor Anderson: Yeah. I'm wondering if these ever will were fulfilled initially like all these canceled ones like.
[00:10:09] Trevor Anderson: Or if it was a posted transaction that never, never actually went through.
[00:10:16] Akhil Gonna: Yeah.
[00:10:17] Trevor Anderson: You know what I mean?
[00:10:18] Akhil Gonna: At the right, if you see rewards delivery status, it says succeeded.
[00:10:23] Akhil Gonna: I'm little confused to what it means. There is status here.
[00:10:27] Trevor Anderson: There's two statuses here.
[00:10:29] Akhil Gonna: Yeah. The extreme right is rewards delivery status, which is succeeded. Yeah.
[00:10:38] Akhil Gonna: Maybe we need to discuss with the Tremendous team and see how they're doing this or how we need to calculate.
[00:10:47] Trevor Anderson: Okay. Is there something we're validating this against?
[00:10:54] Akhil Gonna: Not right now. Maybe we can. Campaign IDs.
[00:10:58] Kapil Sreedharan: Is this lead Manager or Pulse?
[00:11:00] Akhil Gonna: Tremendous. Tremendous.
[00:11:02] Trevor Anderson: Tremendous.
[00:11:02] Akhil Gonna: These are all tremendous.
[00:11:03] Kapil Sreedharan: Yeah, I know, but the campaign IDs which.
[00:11:14] Kapil Sreedharan: Are you able to map these campaign IDs to the Fluence campaign platform?
[00:11:20] Akhil Gonna: No, I haven't mapped them yet, but I mapped with the Tremendous campaign provided by jc.
[00:11:28] Trevor Anderson: Yeah.
[00:11:28] Kapil Sreedharan: So see, Tremendous is a way for us to pay back our customers. Right, the rewards. Right.
[00:11:36] Trevor Anderson: Yes, yes, the attribution platform.
[00:11:39] Kapil Sreedharan: Now, yeah, these campaign IDs are internal tremendous campaign IDs. But these, these are.
[00:11:47] Kapil Sreedharan: This should map to a campaign ID on Lead Manager or Pulse, wherever the actual campaign is, you know, managed.
[00:11:54] Kapil Sreedharan: For example, Monopoly.
[00:11:56] Kapil Sreedharan: If there is a campaign for Monopoly and we are paying out these rewards for Monopoly customers, yet those payments would be here.
[00:12:04] Kapil Sreedharan: But you could, you know, connect it back to the actual campaign that's on.
[00:12:11] Kapil Sreedharan: Let's say if it's on Pulse, there's a fulfillment cost or whatever. Right.
[00:12:15] Kapil Sreedharan: And that should match to what. What it is here.
[00:12:19] Akhil Gonna: Okay. So the mapping would be on the campaign name. Right. This.
[00:12:26] Akhil Gonna: Because the IDs, these ids are specific to only Tremendous. They are building this.
[00:12:31] Kapil Sreedharan: Yeah. So, yeah, you'll have to find that out.
[00:12:33] Kapil Sreedharan: So first thing is to figure out how to map it back to the source platform.
[00:12:41] Akhil Gonna: Okay, understood.
[00:12:43] Kapil Sreedharan: And then. Yeah, get these payment amounts and reconcile with whatever numbers are there.
[00:12:50] Akhil Gonna: Okay. Okay, Got it.
[00:12:52] Trevor Anderson: Yeah. I would check.
[00:12:53] Trevor Anderson: I would do what Pill is saying and check for one campaign in particular, you Know, start.
[00:12:59] Trevor Anderson: Start with one and look for that to see if it comes up in any of these other source platforms like.
[00:13:05] Trevor Anderson: Like Pulse or Lead Manager.
[00:13:08] Trevor Anderson: See if that campaign shows up in there and see if you can see anything like fulfillment costs or anything.
[00:13:14] Trevor Anderson: Any other metric that would help you diagnose what we should actually be including here.
[00:13:23] Akhil Gonna: Okay.
[00:13:24] samreensh: Okay.
[00:13:24] Akhil Gonna: Got it. Yeah, I'll check that out. Yeah. So. Yeah, the. Coming Back to the first issue.
[00:13:36] Akhil Gonna: Among 71 campaigns, there are only 30 campaigns found from the campaign stable.
[00:13:44] Akhil Gonna: The rest 40 are still pending.
[00:13:49] Trevor Anderson: So those. What do you.
[00:13:51] Kapil Sreedharan: What were you.
[00:13:52] Trevor Anderson: What two tables. Were you checking against this one and campaign ref.
[00:13:59] Akhil Gonna: No, no. Tremendous campaign.
[00:14:04] Trevor Anderson: You were checking Tremendous campaign against what?
[00:14:06] Akhil Gonna: Like I. I have a campaign ID here in orders table. Yeah.
[00:14:13] Akhil Gonna: If I need to know the campaign name, I need to join with the tremendous campaign, which is a new table.
[00:14:28] Trevor Anderson: Okay. I was. I was seeing like where. Where were you finding that extra 40
[00:14:36] Akhil Gonna: campaigns in the order stable. In order total, we have like 71 campaign ids distinct.
[00:14:43] Trevor Anderson: I see.
[00:14:44] Akhil Gonna: But only 30 of them. I can find the campaign name. Rest 41.
[00:14:55] Trevor Anderson: Okay. I see. Interesting.
[00:14:59] Akhil Gonna: Yeah.
[00:14:59] Trevor Anderson: So you're trying to figure out that discrepancy.
[00:15:02] Akhil Gonna: Yeah. Right. Without knowing the name, I couldn't map to.
[00:15:06] Akhil Gonna: I mean I couldn't find the exact campaign name, but yeah, Yeah, I know. J.C.
[00:15:15] Akhil Gonna: is trying to check with Sharita Tremendous team.
[00:15:22] Trevor Anderson: Okay, we'll keep digging on that and try to find out why we're missing the missing the campaigns in the campaign table from.
[00:15:36] Trevor Anderson: From orders.
[00:15:38] Akhil Gonna: Okay, sounds good.
[00:15:39] Trevor Anderson: We'll definitely need that resolved.
[00:15:42] Akhil Gonna: Okay.
[00:15:45] Trevor Anderson: Anything else?
[00:15:48] Akhil Gonna: No, for now we could.
[00:15:50] Akhil Gonna: And the issue which soonmi raised yesterday regarding the gold campaign of minion table.
[00:15:58] Akhil Gonna: Yeah, I think that's the data of those five fields which soon we mentioned were not populated from way long back.
[00:16:10] Akhil Gonna: Yeah. Maybe I'll create a placeholder for now ticket we can refine when Brian is back.
[00:16:17] Trevor Anderson: Oh yes, that would probably be a good idea because those are just like floating null.
[00:16:24] Trevor Anderson: Null placeholders.
[00:16:25] Trevor Anderson: And so I can see how it's confusing from her point of view to look at that and say why are these columns in this table?
[00:16:34] Trevor Anderson: They're just going to be all nulls. Right. So.
[00:16:38] Akhil Gonna: Okay.
[00:16:39] Trevor Anderson: Yeah.
[00:16:39] Trevor Anderson: Can you create a placeholder ticket just describing what's going on and then we can probably groom in add details to it on how we want to address it.
[00:16:49] Akhil Gonna: Okay, sounds good. Thanks.
[00:16:51] Kapil Sreedharan: On the Pulse ui, the screenshot that you put Trevor, there is a status, right?
[00:16:58] Trevor Anderson: Yes, there. There's a status of a campaign. Now, I don't know how the status is set.
[00:17:03] Trevor Anderson: I tried to see like as a user can I set the status?
[00:17:06] Trevor Anderson: It doesn't seem like I can actually manually configure the status. I. I'm. Maybe I couldn't find it but
[00:17:17] Akhil Gonna: it.
[00:17:17] Trevor Anderson: There was a status that was shown.
[00:17:19] Kapil Sreedharan: And that is what SUNMI wants, right?
[00:17:23] Trevor Anderson: I think so, yes.
[00:17:25] Akhil Gonna: Yeah.
[00:17:25] Kapil Sreedharan: I guess it should be Pulse should.
[00:17:28] Kapil Sreedharan: If they're not passing to us in the configuration field, they should send it.
[00:17:33] Trevor Anderson: Yeah.
[00:17:34] Kapil Sreedharan: You know we are creating this new CBOR campaign tables right.
[00:17:37] Kapil Sreedharan: That should have all the active campaigns and she should get it in from there.
[00:17:44] Kapil Sreedharan: We should not get it from the events field anymore.
[00:17:49] Trevor Anderson: Yeah. Either Pulse can pass it through the config data, you know or.
[00:17:55] Trevor Anderson: Yeah that's probably the preferred method or status is probably set through some on the pulse side by some like logical sequence.
[00:18:07] Trevor Anderson: Like you know, it needs active,
[00:18:13] Akhil Gonna: you
[00:18:13] Trevor Anderson: know, ad groups or something.
[00:18:15] Akhil Gonna: Right.
[00:18:16] Trevor Anderson: It needs at least one active or one active or needs events in the last 30 days or something like that.
[00:18:26] Trevor Anderson: I don't know. Right. Like there's probably some rule that is set to. To show that it's active.
[00:18:34] Akhil Gonna: Yeah.
[00:18:34] Kapil Sreedharan: I won't be surprised if you know they are even maintaining those fields.
[00:18:38] Kapil Sreedharan: There could be like a lot of campaigns that are, you know, orphaned and all. And that's the thing.
[00:18:45] Kapil Sreedharan: The. The best way to get the active campaigns is you know this whole NET suit thing.
[00:18:52] Kapil Sreedharan: That's where you know if there is an active campaign influence that is making money that is.
[00:18:57] Kapil Sreedharan: You know that's.
[00:19:00] Kapil Sreedharan: It should be on netsuite and our CBUR table would be, you know the final list of all the campaigns that are active on Fluent.
[00:19:11] Trevor Anderson: Yes. And so how are we doing on the netsuite? Are we. We're still waiting on the.
[00:19:18] Kapil Sreedharan: Yeah.
[00:19:18] Trevor Anderson: API.
[00:19:20] Kapil Sreedharan: I. I see one ticket out there that is with netsuite team.
[00:19:31] bharatpam: So.
[00:19:31] Kapil Sreedharan: Yeah, we'll see when we get it.
[00:19:36] Akhil Gonna: Okay. Yeah. Also one last thing is the one in to do 1454.
[00:19:48] Akhil Gonna: We were doing some initial analysis yesterday with Bharat and seems like the table is almost similar to what we only have created.
[00:19:56] Akhil Gonna: Bu classification which has all the events. And this is the new event mapping table.
[00:20:05] Akhil Gonna: Not sure we might. We need this or. Or there is different intention of this.
[00:20:11] Akhil Gonna: But I'll schedule something to discuss more on that ticket.
[00:20:17] Trevor Anderson: Okay, thank you.
[00:20:19] Akhil Gonna: No problem. Yeah, next one. Thanks.
[00:20:23] Trevor Anderson: Okay, thanks Ail. Okay, what's next? I can go next.
[00:20:38] Akhil Gonna: Trevor. Yeah.
[00:20:41] Granth S: Yesterday worked on reviewing one of Joseph pr.
[00:20:47] Granth S: I'm still continuing to review Manav PR and Joseph as another PR for The PII masking.
[00:20:53] Granth S: So we'll take a look at that today and should have 1612 up for review as well.
[00:20:59] Venkatesh Mannam: But apart from that, all good.
[00:21:05] Akhil Gonna: Okay,
[00:21:08] Trevor Anderson: thank you.
[00:21:21] Akhil Gonna: Who's next?
[00:21:25] Vani Kaithi: Yeah, I can go next.
[00:21:27] Trevor Anderson: Okay, thank you.
[00:21:29] samreensh: Yeah.
[00:21:30] Vani Kaithi: For the app engine yesterday, got a few suggestions from Trevor and Bharat.
[00:21:34] Vani Kaithi: So made those changes to the code and it's ready for review. And for the position views as well.
[00:21:44] Vani Kaithi: I have made the code changes and validated against the minion report. It's also ready for review.
[00:21:52] samreensh: Yeah,
[00:21:55] Trevor Anderson: awesome.
[00:21:58] Akhil Gonna: Thank you.
[00:22:01] Trevor Anderson: We'll take a look this morning.
[00:22:03] samreensh: Yeah, thanks.
[00:22:05] Trevor Anderson: Thanks Bonnie,
[00:22:09] Venkatesh Mannam: again.
[00:22:10] Akhil Gonna: One extravagant.
[00:22:11] Trevor Anderson: Awesome.
[00:22:12] Venkatesh Mannam: The NetSuite thing is blocked and regarding 1619 like backfilling of the fluent ids in the offer events table.
[00:22:22] Venkatesh Mannam: So Kapil, like we are planning to add this task into the offer events pipeline itself.
[00:22:28] Venkatesh Mannam: So that way we don't need to manage a separate job.
[00:22:31] Venkatesh Mannam: And this one would be a new task in the same pipeline and it only runs once a day because the upstream fluent ID lookup table would be refreshed at the morning 6 or 7 o'.
[00:22:44] Venkatesh Mannam: Clock.
[00:22:44] Akhil Gonna: Right.
[00:22:45] Venkatesh Mannam: So we added some threshold time to run only once the task rather than ingesting, I mean like running every time.
[00:22:53] Venkatesh Mannam: Because it runs at a frequency of 30 minutes.
[00:22:55] Akhil Gonna: Right.
[00:22:57] Venkatesh Mannam: And yeah, it's in review with the team.
[00:23:04] Kapil Sreedharan: So what would it do?
[00:23:06] Kapil Sreedharan: It will look for all the sessions with null fluent ID in the session events table for the last.
[00:23:14] Kapil Sreedharan: Whatever.
[00:23:15] Akhil Gonna: Yeah.
[00:23:15] Kapil Sreedharan: How long?
[00:23:16] Venkatesh Mannam: How long back so far now we kept four days.
[00:23:23] Kapil Sreedharan: Okay.
[00:23:24] Venkatesh Mannam: Yeah. So it should backfill everything whenever it's missing.
[00:23:32] Akhil Gonna: Okay. Yeah.
[00:23:37] Venkatesh Mannam: And with the yesterday's release Kapil like if you remember, like we moved the date of birth, age range inference after to gold customer.
[00:23:47] Venkatesh Mannam: Right. So we found like almost 443 million records were affected. And earlier records were also.
[00:23:56] Venkatesh Mannam: I mean like the older records which were not enriched got. I mean like got the date of birth in the.
[00:24:03] Venkatesh Mannam: In their profiles and today we somehow we got gold email job failed at the IG API because of weak dedupe logic.
[00:24:13] Venkatesh Mannam: So I added extra dedupe logic which can fix this issue. It's in review with the team.
[00:24:20] Venkatesh Mannam: I raised a bug ticket for that.
[00:24:22] Akhil Gonna: Trevor.
[00:24:25] Venkatesh Mannam: Yeah, the ones.
[00:24:26] Akhil Gonna: This one.
[00:24:27] Venkatesh Mannam: Yeah. Yeah, that's right.
[00:24:33] Akhil Gonna: Okay. Okay. Good fun.
[00:24:42] Trevor Anderson: Thanks for taking care of it.
[00:24:44] Akhil Gonna: Yeah, no worries. Thanks
[00:24:49] Venkatesh Mannam: Kapil.
[00:24:50] Venkatesh Mannam: Like do you have any tracking ticket for the net suit and if I can look at that, maybe if you get busy with other things, I can track out the ticket.
[00:25:07] Kapil Sreedharan: It is a it ticket. Let me see. Where is your. Yeah. I'll put it in the giraffe.
[00:25:21] Kapil Sreedharan: Address that in the block. Do you have that netsuite in block?
[00:25:24] Venkatesh Mannam: Yeah.
[00:25:24] Akhil Gonna: Yeah.
[00:25:26] Kapil Sreedharan: Okay, I'll update that.
[00:25:27] Akhil Gonna: Okay.
[00:25:28] bharatpam: Yeah, thanks.
[00:25:39] Trevor Anderson: Are we. Were we able to create a FDSO for creating a new catalog?
[00:25:45] Venkatesh Mannam: Yeah, yeah, I did create one and Gregory is working on it.
[00:25:51] Trevor Anderson: Okay, great. I'll make sure to.
[00:25:55] Venkatesh Mannam: Yeah, I'll add that FDSO as well into this. Zero.
[00:25:59] Trevor Anderson: Yeah. Can you do that? When did you do that? Yesterday or.
[00:26:04] Venkatesh Mannam: Yeah, yesterday noon.
[00:26:07] samreensh: Yeah.
[00:26:08] Trevor Anderson: Okay. We'll give them a day and I'll just check in on it after that.
[00:26:14] Akhil Gonna: Yeah. Okay, great.
[00:26:23] Trevor Anderson: Did I get everyone?
[00:26:27] samreensh: Yeah.
[00:26:28] Trevor Anderson: Anything else from your side, Samring?
[00:26:31] samreensh: Yeah, yeah, sure.
[00:26:33] bharatpam: For
[00:26:36] samreensh: 1442. That is with Bharat.
[00:26:41] Trevor Anderson: It's with other.
[00:26:44] Akhil Gonna: Okay.
[00:26:45] samreensh: Yeah.
[00:26:46] samreensh: So we have raised a pr, but it is also mentioned there that we have to decommission that tune click conversion table.
[00:26:55] samreensh: So when should we decommission the.
[00:27:01] Trevor Anderson: Is it regarding this ticket here?
[00:27:04] samreensh: Yeah, right. It's mentioned acceptance for the commission of tune click conversions job.
[00:27:14] Venkatesh Mannam: Yeah. Trevor, like to give you a background.
[00:27:16] Venkatesh Mannam: There are like three tasks that populates click conversion final table.
[00:27:23] Venkatesh Mannam: So I asked the Cybeach team to decommission those things because after we switch to the new table, we no longer need to populate them.
[00:27:32] Venkatesh Mannam: Right.
[00:27:36] Trevor Anderson: Okay. So without going into details, the everything that was referencing Qlik conversions.
[00:27:46] Akhil Gonna: Got it.
[00:27:47] Venkatesh Mannam: Yeah.
[00:27:48] Trevor Anderson: Is now coming from Toon Gold events.
[00:27:51] Venkatesh Mannam: Right.
[00:27:53] Trevor Anderson: So this. This has nothing upstream. This is. This is a table.
[00:27:58] Venkatesh Mannam: Yeah, that's a table.
[00:27:59] Akhil Gonna: Right.
[00:28:04] Trevor Anderson: Okay, let me just. I guess I'll check.
[00:28:07] Trevor Anderson: I'll check in the catalog, but we should not see anything that's showing upstream from this in the lineage.
[00:28:16] Trevor Anderson: So just want to make sure that's the case.
[00:28:20] Venkatesh Mannam: Yeah.
[00:28:20] Akhil Gonna: Yeah.
[00:28:21] Kapil Sreedharan: Also, this tune click conversion job is not running or it's failing or do
[00:28:27] Trevor Anderson: we stop it or. Oh, this is just a job. But the actual table. Are we.
[00:28:32] Kapil Sreedharan: There's a job. There's a job that populates this that is failing for whatever reason.
[00:28:38] Kapil Sreedharan: It is sourced from JC's tune table.
[00:28:43] Akhil Gonna: Okay.
[00:28:44] Kapil Sreedharan: Yeah, so we. We'll need to.
[00:28:47] Kapil Sreedharan: Yeah, we'll need to decommission it and then point the fact advertiser performance hourly actually soon because right now it is.
[00:28:56] Kapil Sreedharan: It does not have the latest data because that tune click conversion table is not being populated.
[00:29:03] Trevor Anderson: So in the pr has this been addressed? This one change source table pointer.
[00:29:08] Kapil Sreedharan: Yeah, Bharat has a pr, right?
[00:29:11] Venkatesh Mannam: Yes.
[00:29:11] Akhil Gonna: Yes.
[00:29:12] bharatpam: So in PR we have addressed this.
[00:29:14] bharatpam: The only question is do we make this decommission change in the single PR only or.
[00:29:21] Kapil Sreedharan: Yeah, you can do it in the single pr. Okay.
[00:29:23] bharatpam: Okay, sure.
[00:29:24] Akhil Gonna: Yeah.
[00:29:24] bharatpam: That's the only confirmation I need.
[00:29:26] Kapil Sreedharan: It's basically you'll post the job, right?
[00:29:30] bharatpam: Yes, I pause the job and there are few tasks. YAML file need to get updated to remove some.
[00:29:36] bharatpam: Some of the tasks.
[00:29:37] bharatpam: And also in tune directory we have a Kapil of jobs which will be moved to another deprecated folder.
[00:29:44] bharatpam: So those are a Kapil of changes there.
[00:29:47] Kapil Sreedharan: Yeah, yeah. Since it's part of this ticket, you can do it.
[00:29:54] Trevor Anderson: I think we're okay to do it, right? Yeah.
[00:29:56] samreensh: Yeah.
[00:29:56] bharatpam: Okay, sure. I'll do the change.
[00:29:59] Akhil Gonna: Go ahead.
[00:29:59] Trevor Anderson: Right. We can decommission.
[00:30:02] bharatpam: Sure. Yes.
[00:30:09] samreensh: Yeah. So there is another ticket. 1453, it's with Shubham. Yeah.
[00:30:23] Trevor Anderson: Four, five, three.
[00:30:24] Akhil Gonna: Okay.
[00:30:25] samreensh: Yeah. So it has three subtitles tasks here which we need to start.
[00:30:31] samreensh: But yeah, rebuild the pack table and the change source. So we need.
[00:30:36] samreensh: We would need details here in these subtasks.
[00:30:41] Trevor Anderson: What is this Ad campaign account manager. Is it still waiting on netsuite?
[00:31:00] Kapil Sreedharan: No, no, the account manager is it not. In the tune advertiser table
[00:31:10] samreensh: we are able to get these fields.
[00:31:12] samreensh: I was talking about the subtasks that we are here in the ticket to rebuild the pack table for multiple sources to allow for multiple sources.
[00:31:22] Kapil Sreedharan: Yeah, this one we'll need to look into it.
[00:31:31] Trevor Anderson: So this metrics, we. We want to move to Fluent catalog.
[00:31:35] Trevor Anderson: I'm assuming that one's probably straightforward,
[00:31:41] Akhil Gonna: but
[00:31:42] Trevor Anderson: we can add details there. Change source to point to Fluent data.
[00:31:57] Trevor Anderson: What are the underlying tables that are being used to build this right now? Do you know Samran.
[00:32:06] samreensh: Shubham? Which tables are we using here?
[00:32:16] Pranjal: It's fact the advertiser performance hourly demo.
[00:32:22] Trevor Anderson: Okay, so it's just one. So I guess the idea is that this would.
[00:32:29] Trevor Anderson: Instead of it being this root here central data prod metrics mb it would be fluent metrics mv Fluent advertiser performance.
[00:32:39] Trevor Anderson: The idea is that. And I'd have to check on this the source behind this.
[00:32:45] Trevor Anderson: But the idea is that source would also be influent as well in the Fluent catalog instead of central data and then rebuild fact table.
[00:32:57] Trevor Anderson: I'm not sure. I. For some reason I can't remember what this one was for.
[00:33:03] Manav Paul: But
[00:33:05] Trevor Anderson: I'll review with the team and then we'll add details to these. But yeah, thanks for pointing it out.
[00:33:12] samreensh: Yeah, sure.
[00:33:18] Kapil Sreedharan: Okay.
[00:33:19] Trevor Anderson: Anything else, Samreen on your side?
[00:33:22] samreensh: Oh yeah, just had few questions on 1489 I have added the comment on ticket so anyone could.
[00:33:49] Trevor Anderson: Need clearance on a few questions below. Target catalog schema is unstated.
[00:33:54] Trevor Anderson: Which catalog would fact auction candidate be created? Oh, this is auction explainer ticket. Right.
[00:34:03] Trevor Anderson: Okay. There are two more reasons which need outcome mapping. Frequency cap breach.
[00:34:16] Trevor Anderson: Can you elaborate on this?
[00:34:18] samreensh: Yeah, yeah. So there are reasons here and we. The mapping is given here.
[00:34:28] samreensh: There are 11 reasons for which the outcome is given here if you can see in the outcome mapping.
[00:34:34] samreensh: But there are two more here. I have checked in the data table and the mapping is not there for it.
[00:34:42] samreensh: So maybe if you could add.
[00:34:52] Trevor Anderson: Oh yeah, they're not the outcome.
[00:34:54] Trevor Anderson: Those outcomes that you listed in the comment are not in this table here.
[00:34:59] Trevor Anderson: Yeah, but you're seeing them in the data. Yeah, okay, I got it.
[00:35:05] Trevor Anderson: I'm not sure how Brian got these outcomes.
[00:35:08] Trevor Anderson: Does anyone, anyone else on the team know where these came from? I'll see what I can do. But we might.
[00:35:20] Trevor Anderson: For those outcomes we might have to wait until Brian's back to. To map the additional outcomes.
[00:35:25] Trevor Anderson: I'm not sure where he's gotten these from initially. For the first question about where did the.
[00:35:35] Trevor Anderson: Where does this belong? I'll take a look at this ticket and see if I can help out with that.
[00:35:41] Trevor Anderson: But thank you.
[00:35:45] samreensh: Okay, sure. Thanks. Thanks. That's it.
[00:35:56] Trevor Anderson: Okay, I think I got everyone.
[00:36:03] Manav Paul: Just me, Trevor, I just have.
[00:36:06] Trevor Anderson: Yeah, no, no worries.
[00:36:09] Manav Paul: Just have the model audience request I'm working on going to be distributing that today and then that current declare audience pipeline now that the device stuff is sorted, I'm going to go ahead and test and validate that today.
[00:36:21] Trevor Anderson: Yeah.
[00:36:22] Trevor Anderson: So looks like we might have been unblocked on that since as of yesterday, so hoping that looks like you ran some initial tests on it yesterday.
[00:36:31] Trevor Anderson: Right.
[00:36:33] Manav Paul: I changed some of the code around, address some comments that Joseph had made and then today we're going to go ahead and test it.
[00:36:41] Trevor Anderson: How are you going to go about testing it?
[00:36:45] Manav Paul: Just running the job and seeing. Yeah.
[00:36:49] Akhil Gonna: Okay.
[00:36:51] Akhil Gonna: Sorry
[00:36:51] Joseph: to interrupt.
[00:36:52] Joseph: Do we have a dev bucket for this?
[00:36:55] Manav Paul: I don't think we have a dev bucket yet, but I added like a dry run for now where it shouldn't actually go and send the.
[00:37:03] Trevor Anderson: You should have dead buckets.
[00:37:05] Joseph: Yeah, that's what the ticket is for, right? The dso.
[00:37:08] Manav Paul: I thought it was just to have the.
[00:37:11] Trevor Anderson: I think he created dead buckets in that ticket.
[00:37:14] Akhil Gonna: Oh, okay.
[00:37:15] Manav Paul: In that case then. Okay, I'll take a look and I'll, I'll link those in there.
[00:37:19] Trevor Anderson: Yeah. So you can actually send files to those buckets or write files to test it out.
[00:37:26] Trevor Anderson: So I would, I would try to do that.
[00:37:28] Akhil Gonna: Okay.
[00:37:29] Manav Paul: Do you know how I can go about checking if it was. If they're in there? Like, is there like a.
[00:37:33] Manav Paul: Can I read with the SP or
[00:37:35] Akhil Gonna: how does it work?
[00:37:39] Trevor Anderson: I mean I would put it maybe just add those. The bucket file paths, the path in.
[00:37:48] Trevor Anderson: In the code and when you run it, you know, hoping that it wouldn't throw an error.
[00:37:54] Manav Paul: Yeah, okay, that's fine.
[00:37:55] Akhil Gonna: All right.
[00:37:56] Manav Paul: Yeah. Okay. I'll work on that today and get this done as validated as soon as possible.
[00:38:04] Trevor Anderson: Was it this one? I think so.
[00:38:08] Manav Paul: I believe so.
[00:38:09] Trevor Anderson: Yeah.
[00:38:10] Venkatesh Mannam: Yeah, it is.
[00:38:13] Trevor Anderson: Where is it? This guy.
[00:38:19] Manav Paul: Okay, I see.
[00:38:19] Akhil Gonna: Yeah.
[00:38:20] Manav Paul: And he. Okay, forget the library. I'm going to do.
[00:38:22] Trevor Anderson: All right.
[00:38:22] Manav Paul: That's because. Yeah, I remember now he just needs to. We need to work on the lot of A1.
[00:38:26] Manav Paul: But we can do that separately after this is tested.
[00:38:30] Trevor Anderson: Yeah, so we should have the locations for TransUnion and Liveram.
[00:38:34] Trevor Anderson: I'm not sure what's going on with Lotta May.
[00:38:36] Manav Paul: There's some like cross account access. So I guess what he has to do is make an instance profile.
[00:38:41] Manav Paul: I believe I'll work with him on it, get his insight on it and see what to do.
[00:38:46] Trevor Anderson: Okay. Do we need a separate.
[00:38:50] Manav Paul: I will.
[00:38:50] Trevor Anderson: Yeah.
[00:38:51] Manav Paul: I'll probably make a ticket also. I'll slack him and work on it with him closely.
[00:38:55] Trevor Anderson: Okay. He said a different setup. Okay, interesting.
[00:39:01] Trevor Anderson: But yeah, let's see if we can test out these two so we can move forward on the declared audience pipeline.
[00:39:07] Akhil Gonna: Sounds good.
[00:39:08] Trevor Anderson: Excited to see how that runs.
[00:39:12] Trevor Anderson: If you do, if it is working, let me know because I do want to see how the job runs.
[00:39:19] Akhil Gonna: Sounds good.
[00:39:22] Trevor Anderson: Okay, thanks Manav Yep. All right. Looks like we got through everyone. Well, I don't hear from you.
[00:39:31] Trevor Anderson: Have a great Thursday everyone. Did we get to. Yeah, sorry, go ahead.
[00:39:35] samreensh: Yeah, yeah, I just wanted to mention. Need some inputs on his.
[00:39:45] samreensh: He has put some questions on his tickets.
[00:39:48] bharatpam: Yes, those are 1511 and 1458.
[00:39:51] Akhil Gonna: I mean,
[00:40:00] Trevor Anderson: Sorry, which ones?
[00:40:04] samreensh: No, it's right here.
[00:40:04] Trevor Anderson: This one right here?
[00:40:06] samreensh: Yeah, this one.
[00:40:18] Trevor Anderson: I see.
[00:40:19] Akhil Gonna: Okay.
[00:40:21] Kapil Sreedharan: I don't think we need the SCD2. We are not following that for advertiser. We'll double check.
[00:40:29] Akhil Gonna: Okay.
[00:40:31] Trevor Anderson: Are we deciding not to do that, Kapil?
[00:40:33] Kapil Sreedharan: Yeah, we don't need that in the advertiser table. Well, do we just.
[00:40:38] Trevor Anderson: Do we need it for the other tables? Like campaign?
[00:40:42] Kapil Sreedharan: Yeah. So this is campaign. Yeah, we'll need to check if we need that here. Yeah, we'll need to see.
[00:40:54] Trevor Anderson: I mean, what's your consensus like?
[00:40:56] Trevor Anderson: I mean so type 2 would allow us to keep, you know, retain history on these.
[00:41:03] Trevor Anderson: But the problem is if we're, and I don't know if these would ever be exposed to downstream, like you know, are our stakeholders going to be doing joins on these tables?
[00:41:17] Kapil Sreedharan: This will be used by, you know, those CBUR dashboards. Right. And also if.
[00:41:24] Trevor Anderson: Yeah, but aren't we building a, Are we always building a metrics view on top of those or.
[00:41:33] Kapil Sreedharan: Yeah, so those metrics views would use this table as a dimension table.
[00:41:40] Trevor Anderson: Yeah. But in the metrics view, wouldn't we have a filter on there? Like a, like a where clause?
[00:41:47] Trevor Anderson: Like that's only grabbing active.
[00:41:49] Kapil Sreedharan: Yeah, yeah.
[00:41:55] Trevor Anderson: So in that case, like you know, we could have a scd, you know, we could have a type two that has a history and we only filter for active campaigns or active.
[00:42:06] Trevor Anderson: I Manav, if this is for advertisers too, you know, then we can do the same, same situation.
[00:42:14] Trevor Anderson: The only problem with, with the type 2 is that if it is exposed to anyone downstream, they will have to know that it contains non active campaigns.
[00:42:29] Trevor Anderson: Right.
[00:42:30] Kapil Sreedharan: Yeah,
[00:42:39] Trevor Anderson: You could, you know, we could also do type 2 and then have a view on top of that.
[00:42:42] Trevor Anderson: That was something that was brought up. Right. I guess. What's the reason to not have a.
[00:42:55] Trevor Anderson: Like what are the benefits to having a type one here?
[00:43:07] Trevor Anderson: Like do we need to keep that history is the question. Like do we need to see that?
[00:43:13] Trevor Anderson: I think for the most part it's just the camp, the actual name changing. Right. So
[00:43:20] Kapil Sreedharan: I think in the advertiser for the name changing, Venki handled that.
[00:43:27] Kapil Sreedharan: So like for example, on Pulse, if they change the name, it would not create that fluent advertiser ID.
[00:43:39] Kapil Sreedharan: It would still point to the previous advertiser ID.
[00:43:42] Kapil Sreedharan: Like they could change the name from disney.com to disney co.
[00:43:51] Kapil Sreedharan: And yeah, it should not be a new advertiser. Right. When in this new table.
[00:43:59] Kapil Sreedharan: So it will, yeah, it will still point to the same old id.
[00:44:05] Trevor Anderson: So.
[00:44:05] Kapil Sreedharan: So the name should not really. A name change should not really mean that it's a new advertiser. Right.
[00:44:12] Kapil Sreedharan: An update in the name.
[00:44:14] Trevor Anderson: No, so we should have a fluence like advertiser id. Right. And that should be the unique key. Yeah.
[00:44:20] Trevor Anderson: And if the name comes in differently then we should do a mer, you know, update that record.
[00:44:31] Kapil Sreedharan: Yeah.
[00:44:31] Trevor Anderson: Keep the same fluent advertiser ID and update the actual name.
[00:44:38] Kapil Sreedharan: Yeah, exactly.
[00:44:39] Trevor Anderson: So in a type one only an update would happen. Right.
[00:44:44] Trevor Anderson: But in a type two we would update the existing record as well as. Or we actually.
[00:44:51] Trevor Anderson: Yeah, we would update the existing Record and set it to. Oh yeah, I could see the issue here.
[00:44:58] Trevor Anderson: We'd update the existing record, set it to not active and then add a new row with the new name that shows active but makes it a little bit more complicated and the downstream would
[00:45:21] Akhil Gonna: get affected, right Trevor?
[00:45:23] Venkatesh Mannam: Because earlier the computations may be on the old fluent advertiser id but if you change the fluent ID again, the downstream usage should also be changed, right?
[00:45:40] Trevor Anderson: Yeah, you're right. In a type 2 logic you would probably need.
[00:45:50] Trevor Anderson: You would need to downstream to look for only active records. So.
[00:46:00] Trevor Anderson: And also your fluent advertiser id you need to.
[00:46:04] Trevor Anderson: I feel like you would need in this situation you need another primary, an actual different primary key.
[00:46:09] Trevor Anderson: Because now you're duplicating your fluent advertiser ID essentially because you're not.
[00:46:14] Trevor Anderson: You're now keeping history of a single record.
[00:46:17] Trevor Anderson: So you're saying for advertiser A with fluent advertiser ID 1, 2, 3.
[00:46:23] Trevor Anderson: If this changes, let's say that that gets updated like five times, right?
[00:46:29] Trevor Anderson: Like in, in type one you would only have one row for all those five updates, right?
[00:46:35] Trevor Anderson: And that one row would just get updated. In your type two you'd have five rows.
[00:46:39] Trevor Anderson: But they're going to keep.
[00:46:41] Trevor Anderson: It should keep the Same for each five rows it should have the same fluent advertiser ID.
[00:46:45] Trevor Anderson: Like it should be 1, 2, 3 for all five rows rows.
[00:46:48] Trevor Anderson: However, four of those rows should show a status as inactive and one of them should show active.
[00:46:57] Trevor Anderson: But you're going to have all these duplicate fluent advertiser IDs, right?
[00:47:01] Trevor Anderson: So you need like a new primary key, like just maybe a generated key or something.
[00:47:08] Trevor Anderson: And then in, in the downstream that's connected to a type 2 you need it to.
[00:47:14] Trevor Anderson: To show uniqueness you need to be set to active only, right?
[00:47:18] Trevor Anderson: Or else you're going to have five records.
[00:47:25] Venkatesh Mannam: Yeah, yeah, but it all comes to the downstream requirement, right?
[00:47:30] Venkatesh Mannam: Like suppose for a column like Campaign manager, right?
[00:47:34] Venkatesh Mannam: So if they really wants us to track down the manager change in manager name, then only we should be able.
[00:47:42] Venkatesh Mannam: We should be using this type 2 approach, right?
[00:47:45] Venkatesh Mannam: Otherwise there is no point of using this approach as per my perspective.
[00:47:50] Trevor Anderson: We could do a type one and we could have a separate like log table if we really want to track history.
[00:47:58] Trevor Anderson: So whenever there's a transaction or an update we could log that like yeah, once I get logged those changes, right?
[00:48:06] Venkatesh Mannam: Yeah, maybe once we get the net suit access, right.
[00:48:09] Venkatesh Mannam: We can really understand like how they are tracking those.
[00:48:11] Venkatesh Mannam: If they are tracking the change in manager name or anything. Like that?
[00:48:15] Kapil Sreedharan: No, Netsuit. It's not tracking. It's just.
[00:48:18] Akhil Gonna: Okay.
[00:48:20] Kapil Sreedharan: Type one cd. Yeah. You update the account manager or you know these configs. Yeah.
[00:48:30] Kapil Sreedharan: It's just the latest and that's how they are doing the reporting.
[00:48:34] Venkatesh Mannam: Okay. Okay. Then we should also follow that approach.
[00:48:39] bharatpam: Yeah.
[00:48:39] Kapil Sreedharan: So as of now there is no requirement for having the historical handle like the two requirements for this CBRI tape or the main requirement for CBR table while we are building this is for Conlin's dashboard, right?
[00:48:56] Venkatesh Mannam: Yeah. Yeah.
[00:48:57] Kapil Sreedharan: And that just has a drop down for advertiser that's on the advertiser name.
[00:49:03] Kapil Sreedharan: And they might in the future have.
[00:49:05] Kapil Sreedharan: Let's say they want to also see advertisers for that is managed by certain account managers.
[00:49:13] Kapil Sreedharan: So there'll be account manager name maybe as well.
[00:49:17] Trevor Anderson: Yeah.
[00:49:17] Kapil Sreedharan: So there's no requirement for historical.
[00:49:19] Kapil Sreedharan: And then for Sunmi is like in data science she would also want to get all the advertisers or campaigns that are active and that's all she won't care about.
[00:49:30] Kapil Sreedharan: Like you know, the historicals. That's for the data science models. Right?
[00:49:36] Venkatesh Mannam: Yeah. Yeah.
[00:49:37] Akhil Gonna: Right.
[00:49:40] Kapil Sreedharan: Yeah. CD 2 It would complicate things.
[00:49:45] Trevor Anderson: Yeah.
[00:49:46] Kapil Sreedharan: If we really need it, we could do it as of now. Yeah. We don't have a requirement.
[00:49:53] Kapil Sreedharan: But I guess the thing is also you know we can always expand it right To.
[00:49:57] Akhil Gonna: Yeah, yeah.
[00:49:59] Kapil Sreedharan: In the future.
[00:50:03] Trevor Anderson: So just to just to be clear, Kapil.
[00:50:05] Trevor Anderson: And on the advertiser table, do we have is it type one implementation?
[00:50:10] Venkatesh Mannam: Yeah.
[00:50:12] Trevor Anderson: Okay. Then let's stay with that.
[00:50:15] Trevor Anderson: I think if we really need to track history we could set up a logging like I said.
[00:50:20] Trevor Anderson: Like if we really need to we could say when an update happens like we can write that change to a separate table.
[00:50:31] Akhil Gonna: Yeah. Yep. Yeah.
[00:50:37] Trevor Anderson: Okay. The answer is that column naming follow the we.
[00:50:42] Trevor Anderson: We should follow the DOT standard, not what's in this ticket. Any other questions?
[00:50:52] Trevor Anderson: And we should go through all of these.
[00:50:56] bharatpam: So there is one more. We have this campaign ID in the campaign name.
[00:51:01] bharatpam: So we are avoiding that in the fuzzy logic match. I think number six,
[00:51:13] Trevor Anderson: Lead manager campaigns have some lead manager campaigns have digit only names.
[00:51:22] Kapil Sreedharan: Campaign ID is
[00:51:25] bharatpam: right? Yes. So Those are Campaign IDs in the column Name, Campaign Name or numeric digit ID.
[00:51:38] Venkatesh Mannam: Yeah Bharat, one second.
[00:51:39] Venkatesh Mannam: So I think if you have checked it recently, there is some corruption corrupted file and that's how the campaign rift table got corrupted.
[00:51:50] Venkatesh Mannam: I think that should have resolved it with yesterday's release. Maybe if you can check today.
[00:51:56] bharatpam: Okay.
[00:51:57] Venkatesh Mannam: And if the issue still occurs just Let us know.
[00:52:01] Venkatesh Mannam: Because Kapil, like if you remember the file got corrupted.
[00:52:04] Venkatesh Mannam: The CSU that we try to ingest it for the campaign ref table.
[00:52:08] Venkatesh Mannam: So somehow it created or like it shifted.
[00:52:11] Kapil Sreedharan: It's not the file that corrupted.
[00:52:13] bharatpam: Right.
[00:52:13] Kapil Sreedharan: That was our.
[00:52:14] Venkatesh Mannam: Yeah, yeah. Our process. Ingestion process. Yeah,
[00:52:21] Kapil Sreedharan: yeah. So check the. Yeah. The table should contain proper names now.
[00:52:25] Akhil Gonna: Yeah, okay. Sure, sure.
[00:52:28] bharatpam: Kapil, I will check.
[00:52:38] samreensh: Yeah.
[00:52:38] Kapil Sreedharan: Column naming follow what?
[00:52:40] Kapil Sreedharan: The gold advertiser table that Venky created that should follow the naming convention defined by Trevor.
[00:52:49] Trevor Anderson: It looks like for eight here. It's not quite the case.
[00:52:55] Trevor Anderson: Gold advertiser uses create date instead of timestamp.
[00:52:58] Trevor Anderson: But I think that's the only thing for the most part it should.
[00:53:02] Trevor Anderson: Well, moving forward we should follow the DOT standard.
[00:53:10] bharatpam: And also in the pr, Bharat mentioned that tunelogic will be intune pipeline and the lead manager logic will be in lead manager pipeline.
[00:53:18] bharatpam: So right now we are updating in Fluent campaign. So how we are going to handle that?
[00:53:25] bharatpam: Do we have to do it in future those changes?
[00:53:32] Akhil Gonna: The what.
[00:53:32] Kapil Sreedharan: What kind of logic?
[00:53:35] bharatpam: Basically in Fluent Campaign we are fetching the data from Tune and lead manager.
[00:53:41] bharatpam: So Bharat suggested that in the PR comment that we can apply the logic in the pipeline itself, not externally.
[00:53:52] Trevor Anderson: What do you mean by externally?
[00:53:55] bharatpam: Like in Fluent Campaign, we are fetching data from the tune tables, basically.
[00:54:01] bharatpam: So do we have to apply this logic in Tune pipeline itself?
[00:54:09] Kapil Sreedharan: Are you doing any transformation?
[00:54:14] bharatpam: Not much of the transformation. We are just fetching the data and adding it to the tables and join.
[00:54:20] bharatpam: Not much transformation on that.
[00:54:24] Kapil Sreedharan: So you're joining lead manager and tune.
[00:54:27] bharatpam: Yes, yes.
[00:54:31] Kapil Sreedharan: Yeah, I think that should be here. This isn't the central or fluent, Right.
[00:54:37] Akhil Gonna: This is.
[00:54:38] Trevor Anderson: This one's a fluent table, right? This Fluent campaign. Yeah. So this would be in Fluent catalog. Right.
[00:54:47] Trevor Anderson: And so.
[00:54:48] Kapil Sreedharan: Yeah, yeah, it makes sense. So if you're joining. So Tune would have account manager.
[00:54:53] Kapil Sreedharan: Lead manager probably might not have. Let's say account manager.
[00:54:58] Kapil Sreedharan: You would need to join those two tables, right. In two different catalogs.
[00:55:01] Kapil Sreedharan: Bring it into fluent catalog.
[00:55:03] Trevor Anderson: That's what.
[00:55:03] Kapil Sreedharan: If that's what you're doing, that's fine.
[00:55:05] Akhil Gonna: Yeah.
[00:55:05] bharatpam: Yes, that's what I'm doing as of now.
[00:55:08] Kapil Sreedharan: Okay. Because you, you should not be doing like in Tune catalog.
[00:55:15] Kapil Sreedharan: You should not be joining with the lead manager catalog and bringing the lead manager data in.
[00:55:22] Kapil Sreedharan: You know, those kind of things should happen in this fluent catalog.
[00:55:28] bharatpam: Okay.
[00:55:30] Kapil Sreedharan: Trevor, that's what we are, you know our.
[00:55:35] Trevor Anderson: Yeah.
[00:55:35] Kapil Sreedharan: Leave.
[00:55:36] Trevor Anderson: Leave the tune lead manager as is. Like how. How they are right.
[00:55:41] Trevor Anderson: Now leave them unique to the source and then for. For that. Yeah, if you're.
[00:55:47] Trevor Anderson: That's exact design like in the fluent catalog.
[00:55:50] Akhil Gonna: You're.
[00:55:51] Trevor Anderson: You're joining these sources together. You're bringing them into our. Into the hub. Right.
[00:55:58] Trevor Anderson: So that makes sense.
[00:56:01] bharatpam: Okay.
[00:56:09] Trevor Anderson: Anything else that you wanted to point out here?
[00:56:11] bharatpam: Yeah, one more ticket is there. So one for five feet. Only one, Correct.
[00:56:17] Akhil Gonna: Yes.
[00:56:18] Kapil Sreedharan: Going back to this campaign, are you also including tremendous campaigns here?
[00:56:23] bharatpam: No, not yet. So there is only one column which we are adding.
[00:56:29] Akhil Gonna: Okay.
[00:56:30] Kapil Sreedharan: I think we should also add tremendous campaigns so that we also know how.
[00:56:37] Kapil Sreedharan: Yeah, we also know how that links together.
[00:56:41] samreensh: But.
[00:56:41] Kapil Sreedharan: But that analysis we have to do.
[00:56:44] bharatpam: Okay. Is it available in dev or any other environment as of now. Any tables?
[00:56:48] Kapil Sreedharan: Not yet. We have to do analysis. I think that's what Akil is going to do.
[00:56:51] Kapil Sreedharan: Figure out how tremendous campaigns map to pulse and lead manager.
[00:57:01] Trevor Anderson: Yeah. Is that something Barack can actually look at as well? Like is the tremendous.
[00:57:06] Trevor Anderson: That's in the debt. Is that in a dev. Is that in the tremendous dev catalog right now?
[00:57:13] Akhil Gonna: Yes, down there and I understand.
[00:57:16] bharatpam: Yeah.
[00:57:16] Trevor Anderson: Maybe Bara, you could actually take a look and see while kills looking at that too to see if they would.
[00:57:24] Trevor Anderson: If that source can actually be included in this table. Okay, sure.
[00:57:30] bharatpam: I will look at it.
[00:57:31] Trevor Anderson: There's a tremendous campaign table in the tremendous dev catalog.
[00:57:39] bharatpam: Okay.
[00:57:43] Trevor Anderson: And let us know what you find there. Sure.
[00:57:51] bharatpam: Trevor.
[00:57:52] Trevor Anderson: Okay. I'm sorry, you were gonna mention something else.
[00:58:00] bharatpam: Yeah, just. Yeah, just one Last question on 1458. There is one more question.
[00:58:04] Akhil Gonna: So.
[00:58:21] Trevor Anderson: Since the mapping rules are changing, is the plan to regenerate data in fact performance hourly table depends on those rules.
[00:58:33] Kapil Sreedharan: Yeah, I don't know. I had to review all the fact tables and get back to you on this.
[00:58:42] Kapil Sreedharan: I don't have the answer now.
[00:58:45] bharatpam: Okay,
[00:58:50] Akhil Gonna: I'll.
[00:58:52] Kapil Sreedharan: Trevor, I'll create a ticket for myself to review the fact tables and we can discuss internally.
[00:59:01] Trevor Anderson: Okay. Yeah, if you could set up some time that would be helpful so we
[00:59:06] Kapil Sreedharan: can get through that.
[00:59:08] Trevor Anderson: Yeah. Okay, we'll get back to you on this one.
[00:59:19] Akhil Gonna: All right.
[00:59:19] Trevor Anderson: I know we're well over.
[00:59:21] bharatpam: Did we.
[00:59:21] Trevor Anderson: Is there anything else?
[00:59:26] samreensh: Just one question.
[00:59:27] samreensh: So should we like wait for the updates on the tickets for the blockers as well as the tickets we have pointed out.
[00:59:43] Trevor Anderson: If, if there's anything that like after us going through that, if there's anything that you can move forward on, do that for the things that we need to add comments on still then yeah, we'll.
[00:59:56] Trevor Anderson: We'll wait until we respond on onto those ones, but for like the one for those subtasks, we'll definitely need to add some details on those.
[01:00:12] Trevor Anderson: But yeah, hopefully we were able to answer some questions and move some things along.
[01:00:20] Trevor Anderson: But just let us know if there's any additional questions.
[01:00:31] Kapil Sreedharan: So you can do that analysis of the tremendous campaigns with Pulse and the lead manager campaigns.
[01:00:40] Kapil Sreedharan: You can do the tune changes, decommissioning. Yeah, I think those two.
[01:00:51] samreensh: Okay. Okay, sure.
[01:01:22] Trevor Anderson: Yeah. And then this one can actually be moved to fluent.
[01:01:25] Trevor Anderson: I'm not sure where it is right now if it is in any.
[01:01:28] Trevor Anderson: Is it was this built in a certain catalog in central data. So this can. This can.
[01:01:39] Trevor Anderson: Yeah, this can move to fluent Metrics. Fluent metrics.
[01:01:46] samreensh: Okay. Sure.
[01:01:50] Akhil Gonna: Okay.
[01:02:04] Trevor Anderson: All right, thank you everyone for all the updates. Yeah, I'll hear from you.
[01:02:11] Trevor Anderson: Have a great Thursday and we'll talk to you guys later.
[01:02:17] Akhil Gonna: Bye, guys.
[01:02:18] Venkatesh Mannam: Thanks.
[01:02:18] Akhil Gonna: Thanks.
[01:02:19] samreensh: Thank you. Thank you, everyone.
