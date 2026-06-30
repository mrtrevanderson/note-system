---
title: "DE Daily Standup"
date: 2026-06-29
time: "09:35-10:00"
participants: [Dan Duling, Trevor Anderson]
fellow_url: https://fluentco.fellow.app/meetings/040000008200E00074C5B7101A82E00807EA061DA07C9A5FA178DC01000000000000000010000000AB9154BF9C4AA34B9FC26D395D9AA196/
captured_at: 2026-06-30T12:02:44Z
---

## AI Notes

The data engineering team held their daily standup to review progress on audience pipeline deployments and data quality initiatives. **Model audience pipeline testing** hit a Phase 2 bug that Granth and Joseph are debugging, with validation targeted for completion this week and broad cutover planned for next week. The **declared audience pipeline** was successfully tested end-to-end in UAT and is ready for production deployment once vendor buckets are configured. A critical **date of birth data quality issue** affecting 80 million records was resolved through a hotfix, improving the fill rate from 75% to 99.5%. The team is also migrating **ad group conversion logic** to source from UC Metrics (with hourly refresh instead of 20-minute intervals), requiring validation and Data Science team consultation on potential model degradation impacts. Trevor outlined plans for **persona-based access controls** in Databricks to replace the current over-privileged model, starting with an Audience Solutions analyst group.

### Model Audience Pipeline Testing

- Granth and Joseph encountered a **Phase 2 bug** preventing model audience pipeline progression, requiring API documentation review to identify missing configuration elements.
- The team is no longer blocked by FDSO tickets for model audience work (though one FDSO remains open for declared audience distribution).
- **Validation completion targeted for this week** with broad cutover and full pipeline deployment planned for next week once testing is finalized.
- The bug is confirmed to be in the team's code rather than FDSO-related; seed upload succeeds but processing fails for unknown reasons.

### Declared Audience Pipeline Deployment

- Joseph and Manav successfully **tested the declared audience pipeline end-to-end** in UAT on Friday, with distribution functionality working properly.
- The pipeline is **production-ready** and can be deployed once pointed to actual vendor buckets, awaiting final configuration.
- Vani validated Apple injection in UAT and is waiting for confirmation from Kevin (in Tuesday's meeting with Ryan) on whether position view counts should be added as dimensions rather than metrics to enable slicing and dicing.

### PII Masking & Compliance

- Joseph's PII masking code is up for review but deployment is blocked until other systems are ready to avoid masking issues during testing.
- Compliance requires **all PII to be masked** before rolling out Databricks GENIE access to Fluent for Claude, with access restricted to a pilot list until OKTA role-based PII access is finalized.
- The PII masking requirements align with **SOC compliance** standards and will be critical as the team expands beyond central data into Trevor and Kapil's global EDM Databricks setup.

### FIC Process & Data Quality

- Work on ticket **1589 is blocked** for over a week awaiting IT team response on NetSuite API key access needed to ingest NetSuite system data into Databricks.
- A critical issue was discovered where the FIC process ingested **date of birth as null for two years** (affecting 80 million records) due to upstream data type change from long to string.
- A hotfix was deployed improving date of birth **fill rate from 75% to 99.5%**, with RDS sync and Experian monthly sync currently processing in parallel.

### Ad Group Conversions Migration

- Venkatesh is working on tickets **1531, 1530, and 1587** to migrate ad group conversions to source from UC Metrics, with validation changes in progress.
- UCM counts all conversions while current Gold ad group conversions only count optimized conversions, requiring row-level filtering to ensure metrics match.
- The migration will **change refresh frequency from 20 minutes to one hour** (matching underlying UCM table updates), requiring Data Science team consultation on potential model performance degradation.
- Dan requested analysis on actual degradation impact when these metrics are delayed, noting that with Claude's data access capabilities and current limited advertiser inventory (200+ advertisers dominated by 4 in same category), the team should validate realistic impact before assuming 20-minute freshness is necessary.

### Segment Registry & Access Governance

- Joseph documented **five open questions** on Q3 segment registry work requiring decisioning from Brian or Virginia, which Trevor reviewed with Joseph last week.
- Most FDSOs for audience solution work were unblocked last week, allowing the team to move forward on implementation.
- Trevor submitted ticket **1604** for Monisha (first audience solutions team access request) to access Playful Rewards prod catalog in Databricks.
- The team is transitioning from an **over-privileged access model** (read-only and read-write against all catalogs) to persona-based access groups where users only see assets relevant to their work.
- Trevor requested creation of an **Audience Solutions analyst group** from DevSecOps and submitted spike to document current group access state before migrating to new persona-based model.

### Action Items

- [ ] Schedule meeting with Brian (and optionally Dan) to review and make decisions on the five open questions in Joseph's segment registry document.
- [ ] Brian Silveri to follow up on completion of OKTA role-based PII access request required before expanding Databricks GENIE access beyond pilot users.
- [ ] Brian Silveri to sync with Vani after standup to assign priority tasks.
- [ ] Vani Kaithi to pick up stories from Venkatesh's workload on ad group conversions validation (tickets 1531, 1530, 1587).
- [ ] Venkatesh Mannam to slack Brian the list of upstream tables causing the one-hour refresh limit for ad group conversions discussion.
- [ ] Brian Silveri to have conversation with Data Science team to get metrics on model degradation impact from changing ad group conversions refresh from 20 minutes to one hour.
- [ ] Brian Silveri to follow up with IT team on blocked NetSuite API access request (ticket 24010) and slack details to Dan.
- [ ] Brian Silveri to send Trevor the current Databricks roles and access documentation received from Greg.
- [ ] Brian Silveri to review Joseph's segment registry document and schedule sync with Trevor and Virginia to address open questions.
- [ ] Brian Silveri to include Vani in meeting with Ryan to discuss position view counts requirements for Apple injection work.

## Transcript

[00:00:00] Brian Silveri: Oh, right, right. I saw that.
[00:00:08] Vani Kaithi: Okay.
[00:00:10] Brian Silveri: All right, so it should. Trevor should be here.
[00:00:14] Granth S: So
[00:00:16] Brian Silveri: Grant not off today?
[00:00:19] Joseph: No, he's not off.
[00:00:20] Granth S: He's.
[00:00:30] Venkatesh Mannam: Joseph, are you in a meeting till Wednesday, 12 o'?
[00:00:34] Vani Kaithi: Clock?
[00:00:34] Granth S: Oh, my God.
[00:00:35] Venkatesh Mannam: I think that's
[00:00:38] Joseph: okay.
[00:00:40] Brian Silveri: I'm definitely going to host the Outlook training seminar for everybody. All right, let's see.
[00:00:52] Brian Silveri: Just Bonnie, right? Bonnie's left.
[00:00:56] Venkatesh Mannam: She's here. She should be joining.
[00:01:00] Brian Silveri: Great. Awesome. How was everybody's weekend?
[00:01:07] Venkatesh Mannam: Okay.
[00:01:09] Joseph: Same.
[00:01:15] Brian Silveri: Did anybody do anything fun? Months Resting.
[00:01:21] Venkatesh Mannam: Yes.
[00:01:22] Granth S: Yeah, resting.
[00:01:24] Brian Silveri: Good, good.
[00:01:24] Granth S: Welcome. World cup games.
[00:01:28] Brian Silveri: Oh, are there more in Toronto this weekend?
[00:01:31] Granth S: Well, Thursday, Portugal place, which nobody would have expected.
[00:01:36] Brian Silveri: So yeah, yeah, Thursday I believe the
[00:01:41] Granth S: city's gonna shut down.
[00:01:45] Brian Silveri: I expect a massive influx of people there. For sure. So just be careful. Get your supplies beforehand.
[00:01:53] Brian Silveri: Hey, Dan. How's it going?
[00:01:57] Venkatesh Mannam: Everyone Good. Good day.
[00:02:01] Trevor Anderson: Congrats to Canada for winning.
[00:02:05] Venkatesh Mannam: Yeah. Yesterday, Canada
[00:02:10] Granth S: again. That was unexpected win as well, but yeah, take it.
[00:02:14] Trevor Anderson: They're making history.
[00:02:17] Venkatesh Mannam: Lots of stores offered freebies and all dominoes.
[00:02:27] Brian Silveri: Well, you got free dominoes. Yeah.
[00:02:31] Venkatesh Mannam: Some popcorn.
[00:02:34] Brian Silveri: How about that? I hope you all took advantage today. All right, great, great. Now you got lunch.
[00:02:41] Brian Silveri: Excellent. All right, who wants to go first? We got a thin crew here, Dan, so it'll be quick.
[00:02:51] Joseph: Okay, go ahead.
[00:02:52] Brian Silveri: Okay.
[00:02:53] Granth S: Thanks, Joseph.
[00:02:54] Granth S: Yeah, so for me, I've been working with Joseph on getting the testing done for the model audience.
[00:03:00] Granth S: So we ran into some bug, I believe. So we're not able to go past phase two. So, yeah, just.
[00:03:11] Granth S: We'll look into that.
[00:03:13] Granth S: I believe I'll have to take a look at the API doc again to see if I've missed anything.
[00:03:20] Granth S: But yeah, for today we'll just continue to take a look at that and try to fix that and get that pipeline up and running.
[00:03:30] Venkatesh Mannam: So is.
[00:03:31] Brian Silveri: Are we now unblocked based off of the FDSO tickets?
[00:03:34] Venkatesh Mannam: Yes.
[00:03:35] Granth S: Right now we don't have any fdso. So we have all of the.
[00:03:40] Trevor Anderson: We have one, right? We have the one for. Yes, for the bottom. A.
[00:03:46] Brian Silveri: Right.
[00:03:47] Granth S: Oh, okay, never mind. That's for the distribution. That's what declared audience, correct.
[00:03:52] Trevor Anderson: Yeah, that's for.
[00:03:53] Brian Silveri: Declared.
[00:03:53] Granth S: Yeah. But for model Brian, we don't have any. So we're good on that part.
[00:03:58] Brian Silveri: Okay, got it. And then what's our. What's our timing for finishing these up this week?
[00:04:09] Granth S: Hopefully we can get the validations done this week.
[00:04:13] Granth S: So at least by next week we can have the broad cut over and we. We should be able to everything.
[00:04:19] Granth S: Have everything up and running.
[00:04:20] Granth S: So this week the focus Will be just on the validations done and getting the pipeline up and running.
[00:04:29] Brian Silveri: Great.
[00:04:29] Brian Silveri: So Grant, any are we seeing anything else that would block us for this modeled audience deployment other than the testing of those things or.
[00:04:40] Brian Silveri: Joseph, are you going to cover those things?
[00:04:44] Joseph: I think it's the same what Grant mentioned.
[00:04:46] Joseph: We'll have to look into those API logs because we are not sure what's missing there.
[00:04:50] Joseph: Because the seed upload is successful, but it doesn't process for some reason.
[00:04:55] Granth S: But I can confirm it's nothing to do with fdso. It's just a bug in our code.
[00:05:02] Granth S: So we'll just have to take a look at that if that's your question.
[00:05:05] Brian Silveri: Right, got it. Great. Excellent. Thank you, Grant. Thank you. Who's next?
[00:05:14] Joseph: Yep, I can go. Brian worked with Manav on Friday.
[00:05:18] Joseph: Those days we were able to test it successfully for the declared audience pipeline end to end.
[00:05:25] Joseph: So the distribution works fine.
[00:05:27] Joseph: So once the prod is ready, we can point it to the actual vendor buckets and we can run the pipelines.
[00:05:34] Joseph: So that is up and yeah, just looking into this model audience pipeline failures yesterday.
[00:05:40] Joseph: I did run some test, but still that failed. Yeah, just looking into that.
[00:05:47] Brian Silveri: Got it. And nothing else. Okay, got it. Whoa. My monitor's like kicked off and I
[00:05:59] Joseph: hope you saw that message from my DM that I shared the doc regarding the segment registry.
[00:06:05] Brian Silveri: I did, yes. Yeah. And do we need to. To meet to go over that doc once I review it or.
[00:06:13] Brian Silveri: I didn't look at it yet, so I'm sorry.
[00:06:16] Joseph: Trevor, any idea on the segment registry? I think Manavas off. How are we going to cover that?
[00:06:22] Trevor Anderson: Yeah, Brian, just to catch you up.
[00:06:24] Trevor Anderson: So Joseph went through and listed some open questions on the segment registry work for Q3.
[00:06:33] Trevor Anderson: And I went through that document with Joseph last week and there was.
[00:06:38] Trevor Anderson: There was a few open questions that need decisioning on. So.
[00:06:41] Granth S: Okay.
[00:06:43] Trevor Anderson: Something you or Virginia would probably need to make a decision, but there they were listed in the question section.
[00:06:50] Trevor Anderson: So we can.
[00:06:51] Trevor Anderson: I can set up a time for us or you can go through that on your own and get back to us or how.
[00:06:56] Trevor Anderson: Whatever you would prefer there.
[00:06:58] Trevor Anderson: But there was just a few open questions that I think the team needs some decisioning on.
[00:07:03] Brian Silveri: Okay. Dan, do you want to be in on that or. And A and B. Trevor, did you send that to Dan?
[00:07:09] Trevor Anderson: Yeah, we can share with Dan as well. Great. It was just like five questions, I think.
[00:07:14] Trevor Anderson: Yeah, just a couple of those. We'll just need how we want to move forward on, that's all.
[00:07:20] Trevor Anderson: So I can set up some time. Dan, if you would like to be included, you can let me know.
[00:07:25] Trevor Anderson: I can include you as optional.
[00:07:27] Granth S: Great.
[00:07:30] Dan Duling (1): I mean, you can go ahead and include me as optional, but send me over the questions and you know, at least I can just in case I can't get into the call.
[00:07:38] Dan Duling (1): Only because my schedule looks kind of crazy right now.
[00:07:41] Brian Silveri: Now.
[00:07:41] Dan Duling (1): And I don't want to be the roadblock for that. At least I can,
[00:07:45] Dan Duling (1): you know, review the questions and provide some feedback.
[00:07:49] Trevor Anderson: Yeah, that sounds great. I'll set that up.
[00:07:55] Brian Silveri: Great. Thank you. Joseph, any update on the PII one, Are we good for this?
[00:08:03] Joseph: That's up for review.
[00:08:05] Joseph: I think we'll have to deploy this once everything else is set up because if not it'd be running into issues because it would mask everything and we'll have to test first and then we'll push this at last.
[00:08:19] Brian Silveri: Got it. Makes sense. Yeah.
[00:08:22] Brian Silveri: And Dan, we got a request from compliance to make sure that all the PII is masked, especially once we before we roll out the databricks GENIE access to fluent for claude, they want us to make sure every table is mass so that no one could get pii and they would have to go through that OKTA request.
[00:08:47] Brian Silveri: I'll follow up with it because they have the OKTA request for PII by role and they have to finish that.
[00:08:55] Brian Silveri: So right now it's only a pilot list of people who have access to the databricks GENIE connector in CLAUDE before we go.
[00:09:03] Brian Silveri: Why we've got to finish this and this would be included in that as well.
[00:09:06] Brian Silveri: So just want to keep you updated there. Makes sense.
[00:09:10] Dan Duling (1): I'm guessing this is, you know, to keep in line with some of our SOC requirements, even though we're,
[00:09:16] Dan Duling (1): I think we've finalized that. But
[00:09:19] Dan Duling (1): you know, if we have anything that's, you know,
[00:09:22] Dan Duling (1): misleading, it
[00:09:23] Dan Duling (2): will get
[00:09:23] Dan Duling (1): flagged.
[00:09:25] Brian Silveri: Correct. So I, I said we were already in progress because we are.
[00:09:28] Brian Silveri: We had already submitted that request and we already have it in our central data section.
[00:09:32] Brian Silveri: But this will be critical as we expand beyond central data and go into Trevor and couples global EDM databrick setup.
[00:09:41] Brian Silveri: So we'll have to keep applying this as we separate out those entities.
[00:09:47] Brian Silveri: But yeah, we're in a good spot. All right, great, Joseph. Excellent. Who's next?
[00:09:53] Vani Kaithi: Yeah, I can go next.
[00:09:55] Brian Silveri: Go ahead, Moni. Yeah.
[00:09:56] Vani Kaithi: Good morning, team.
[00:09:57] Vani Kaithi: So, yeah, for this appler injection I have tested and validated in the UAT brand and for the position view counts.
[00:10:08] Vani Kaithi: At first we have added them as metrics to our fact table.
[00:10:11] Vani Kaithi: Then as suggestions by the Team, we need to.
[00:10:15] Vani Kaithi: I guess we need to add it as dimension so users can slice and dice the position, the remaining metrics.
[00:10:24] Brian Silveri: Yep.
[00:10:24] Vani Kaithi: Yeah, I guess Kevin is going to check with Ryan on this on Tuesday's meeting.
[00:10:29] Brian Silveri: Correct. And I will confirm.
[00:10:31] Vani Kaithi: Yeah, yeah. Once IT confirmed, we can adjust the code and add it as a dimension.
[00:10:36] Brian Silveri: Right, Perfect.
[00:10:37] Vani Kaithi: So yeah, if there are any priority tasks or anything you would like me to work on next, I can pick those up.
[00:10:44] Brian Silveri: Absolutely. I will sync with you after this, Bonnie.
[00:10:49] Vani Kaithi: Yeah, sure.
[00:10:51] Brian Silveri: Great. Thank you.
[00:10:52] Vani Kaithi: Thank you.
[00:10:54] Brian Silveri: All right, thank you.
[00:10:56] Venkatesh Mannam: Firstly, the 1589 is blocked. I mean like Kabil sent a reminder to IT team.
[00:11:03] Venkatesh Mannam: I'm not sure they haven't responded to us. So it's been blocked from last one week.
[00:11:09] Brian Silveri: Okay.
[00:11:10] Dan Duling (2): And
[00:11:12] Dan Duling (1): sorry, I'll just
[00:11:13] Dan Duling (1): send Tim a note right now. So what's the id?
[00:11:20] Venkatesh Mannam: Give me one sec. So there is an email chain. Dan, I'm not sure if there is an ID in there.
[00:11:27] Brian Silveri: There, there is, hold on. Yeah, I'll send to you. Dan, I got in my emails, I just got too many.
[00:11:36] Brian Silveri: No problem.
[00:11:36] Venkatesh Mannam: Basically Dan, that's like an IT suit API key access to us so that we can ingest into the databricks, the netsuite system data.
[00:11:45] Brian Silveri: Yep.
[00:11:47] Venkatesh Mannam: And on Friday mostly worked with Kapil. Right. Like there seems to be an issue with the FIC process.
[00:11:55] Venkatesh Mannam: I mean especially at the date of birth, date of birth column.
[00:11:59] Venkatesh Mannam: So from last two years our process kind of ingested date of birth as a null field because the upstream source data earlier it was coming as a long field.
[00:12:10] Venkatesh Mannam: But right now it's coming as a string and our kind of process didn't picked up and they kind of populated as nulls and Ryan kind of popped up with this saying that the match the fill rate is almost 75%.
[00:12:26] Venkatesh Mannam: So we kind of fixed the hot.
[00:12:27] Venkatesh Mannam: I mean like we kind of sent a hotfix to the PR and right now the date of birth fill rate is almost 99.5%.
[00:12:37] Venkatesh Mannam: So that's out.
[00:12:38] Venkatesh Mannam: And we're kind of syncing the same to RDS right now since the FIG process kind of touched like almost 80 million records.
[00:12:48] Venkatesh Mannam: And the Experian monthly sync is also in process. So we kind of going two boards at one short go.
[00:12:57] Venkatesh Mannam: So it's in process brand. Like I'll let you know once RDS sync is also done.
[00:13:02] Joseph: And
[00:13:04] Venkatesh Mannam: I kind of working on this 1531, 1530 and 1587 all our InterLink brand.
[00:13:11] Venkatesh Mannam: So basically the ad group conversions, right?
[00:13:13] Brian Silveri: Yeah.
[00:13:14] Venkatesh Mannam: So Kapil kind Of told us like we can go at one go to source conversions and all from the UC metrics.
[00:13:21] Venkatesh Mannam: So kind of validating those changes.
[00:13:27] Brian Silveri: And is this something Von can help you with so you can go faster?
[00:13:30] Venkatesh Mannam: Yeah, yeah, I kind of share the workload with her.
[00:13:33] Venkatesh Mannam: So most of the changes are in place so we need to do some validations.
[00:13:39] Brian Silveri: Got it. Yes, let's do that. Vani, please just pick up whatever story you think is necessary here.
[00:13:45] Vani Kaithi: Yeah, sure man.
[00:13:47] Venkatesh Mannam: And by a brand like current Gold Ad group conversion kind of counts the optimized conversions, I would say.
[00:13:56] Venkatesh Mannam: But UCM counts of kinds of store all the conversions. So just an fia.
[00:14:03] Venkatesh Mannam: The UCM would give more conversion counts.
[00:14:06] Venkatesh Mannam: So for that we are kind of filtering those columns or the rows.
[00:14:12] Brian Silveri: Yeah, because they gotta match.
[00:14:14] Venkatesh Mannam: Yeah, yeah, yeah, that's right.
[00:14:15] Brian Silveri: Yeah. Yep, I would agree. And let me know when you're ready for me to start looking at that.
[00:14:20] Brian Silveri: So I could do some testing and then we can also loop in the BI team.
[00:14:25] Venkatesh Mannam: Yeah, sure thing, Brian. Yeah, we'll do.
[00:14:27] Brian Silveri: Great. Awesome. And I'll just.
[00:14:30] Venkatesh Mannam: The frequency would now go to every one hour brand because the underlying UCM gets updated every one hour.
[00:14:37] Granth S: Right.
[00:14:37] Venkatesh Mannam: So the frequency of GOL AD group conversion would go to one hour just in FYI.
[00:14:43] Brian Silveri: Oh, it's down to be up to an hour. Yes. The underlying tables are running at an hour.
[00:14:50] Brian Silveri: So down from 20 minutes to now to an hour.
[00:14:53] Venkatesh Mannam: That's right. Yeah, that's right.
[00:14:55] Brian Silveri: Okay, got it.
[00:14:57] Venkatesh Mannam: We need to check this with data science because right now they are expecting this job 20 minutes.
[00:15:05] Venkatesh Mannam: Right? Yeah.
[00:15:06] Brian Silveri: And what Venki, just slack me on the side so that I have the list of the upstream tables that are causing the hour limit and then we could talk about it.
[00:15:16] Venkatesh Mannam: Yeah, yeah, sure thing. Yeah, we'll do that.
[00:15:21] Dan Duling (2): I know we asked, but we should also just ask the data science team what are those degradation that we start seeing when these aren't there?
[00:15:35] Dan Duling (2): We know in the past for some of these, even if they degrade for a week at a time, we don't see much of a decrease.
[00:15:43] Dan Duling (2): It's a good number for us to start having.
[00:15:45] Dan Duling (2): And with Claude and the data access that it has, there's really no reason why we can't get to those numbers anymore.
[00:15:53] Brian Silveri: I would agree with you, Dan. Yeah. And before I make changes to that one hour Dan, I'll.
[00:16:00] Brian Silveri: I'll have that conversation with DS again and get metrics to support it.
[00:16:07] Dan Duling (2): Yeah, appreciate that.
[00:16:08] Dan Duling (2): Just because you know, with our current inventory that we have with for, you know, it's Just it would be really interesting to know it's a totally different story if we're talking about, you know, we have 200 plus advertisers right with.
[00:16:23] Dan Duling (2): But when we have basically, you know, a handful and you know, four of them are basically all the same category and that's dominating the market.
[00:16:34] Dan Duling (2): You know, I just want to make sure that we're being realistic with the impact that some of these actually have.
[00:16:44] Brian Silveri: Makes sense.
[00:16:46] Trevor Anderson: Great.
[00:16:47] Brian Silveri: Okay, thank you. The one that's blocked. I'll follow up. Dan, this ticket number was 24010.
[00:16:56] Granth S: Okay.
[00:16:57] Dan Duling (2): You mind just slacking? I just walked downstairs to grab a cup of coffee real quick.
[00:17:00] Joseph: Sure.
[00:17:00] Brian Silveri: Okay, no problem. You got it. All right. Trevor, anything on your side?
[00:17:07] Trevor Anderson: Hey, yeah.
[00:17:09] Trevor Anderson: Last week, you know, we were moving through some of the FDSOs for the audience solution side.
[00:17:14] Trevor Anderson: I for the most part, like we mentioned, most of those got unblocked.
[00:17:18] Trevor Anderson: So we're good to move forward on that.
[00:17:22] Trevor Anderson: I think I submitted quite a few last week as well regarding some of these items here for governance.
[00:17:29] Trevor Anderson: So we are, I think this is the first request where we're getting access request from someone on the audience solutions team, onisha.
[00:17:38] Trevor Anderson: That's the 1604 where she's asking for access to playful rewards prod catalog.
[00:17:45] Trevor Anderson: So what my, what the plan is is to have Persona groups that are structured for different types of access patterns within the databricks environment.
[00:18:02] Trevor Anderson: So what we probably want to see here is an audience solutions analyst group that would have access to the necessary data assets to you know, build the audiences or analyze the data from that line of business.
[00:18:20] Trevor Anderson: So I requested to create, create that group within the databricks environment from our dev devsecops.
[00:18:28] Trevor Anderson: But the spike is regarding current,
[00:18:34] Venkatesh Mannam: the
[00:18:34] Trevor Anderson: current state of groups within the databricks environment and you know, what those groups have access to.
[00:18:41] Trevor Anderson: I think we have a, the previous model was, was a bit over privileged where we just only had like a read, a read only and a read write permission.
[00:18:52] Trevor Anderson: But it was against all catalogs within our environment.
[00:18:55] Trevor Anderson: So I think we're trying to, you know, moving forward, what we're trying to do is have more of a Persona based access.
[00:19:04] Trevor Anderson: So you know, if you, you know, are part of a particular group, you should be able to see the assets that are only relevant to what the work that you're doing.
[00:19:13] Trevor Anderson: So yeah, right now I have a few out for getting access that I need to, to see the current state and then also be able to set up new Personas as well.
[00:19:26] Trevor Anderson: And then you know, add Monisha to to that Persona group and get get her to access that she needs.
[00:19:36] Brian Silveri: Do you. Are you talking about the current setup of what roles we've got?
[00:19:43] Brian Silveri: Is that what you're looking for?
[00:19:45] Trevor Anderson: Yeah, I'm looking for that so I could see who has that, you know, how our access stands as is today and what we need to do to migrate.
[00:19:54] Brian Silveri: I have that for you.
[00:19:56] Trevor Anderson: Okay.
[00:19:59] Brian Silveri: Yes, I got that from Greg. So Trevor, I'll send that to you.
[00:20:02] Trevor Anderson: Oh, thank you very much.
[00:20:05] Brian Silveri: Yeah, the. And before we request. Did you already request the new role?
[00:20:10] Trevor Anderson: I did.
[00:20:11] Brian Silveri: Okay, got it. Okay.
[00:20:14] Trevor Anderson: I requested Audience Solutions analyst group to be created because I don't believe we have based off the existing groups that I was able to see.
[00:20:27] Trevor Anderson: I don't believe we have something like that.
[00:20:29] Brian Silveri: Not could be wrong just for Audience Solutions. No, we would not have a business line specific thing.
[00:20:40] Brian Silveri: We've got an analyst role and we've got a product owner role currently that would probably over privilege because we're not giving it based off of the schema yet which is different.
[00:20:56] Brian Silveri: So yeah, feel free to to look at that. I just sent it over to you if you have let me know. Thank you.
[00:21:03] Trevor Anderson: This helps.
[00:21:04] Brian Silveri: Great. Okay, Joseph, I will look at your thing.
[00:21:09] Brian Silveri: I'll most likely Trevor, require us to sync on it and most and include Virginia.
[00:21:17] Brian Silveri: I will follow up with Dan and I on the NetSuite API access. And Vonnie, I'll let you.
[00:21:24] Brian Silveri: I'll also include you Vonnie on that meeting with Ryan so you can hear from him exactly what he's looking for.
[00:21:28] Brian Silveri: That way there's no mixed messages. Yep.
[00:21:31] Vani Kaithi: Yep. Sure.
[00:21:32] Brian Silveri: Great. Thank you all. Please enjoy the rest of your day if I don't talk to you. Thank you, Dean.
