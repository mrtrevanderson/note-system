---
title: Cybage - DE team standup
date: 2026-06-23
time: 09:05-09:30
participants: [Trevor Anderson, Brian Silveri, Akhil Gonna, Bharat Gidwani, Joseph Ramesh, Kapil Sreedharan, Vani Kaithi, Venkatesh Mannam, Granth Sajjanshetty, Manav Paul, Amit Kumar, Bhagyashree Pamnani, Pranjal Ahire, Samreen Belawat, Samreen Shaikh, Shubham Mandlik, Amit Kumar (Cybage)]
fellow_url: https://fluentco.fellow.app/meetings/040000008200E00074C5B7101A82E00807EA0617D04731FE45E9DC01000000000000000010000000B4E52B2A9B75554886302B59A05B811C/
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
[ ] Joseph to raise PR for BI masking changes (ticket 1317) incorporating Vani's feedback.
[ ] Brian Silveri to schedule meeting this afternoon for squad to review the Segment 2000 registry initiative.
[ ] Vani Kaithi to post the AppFlyer table names in the data engineering channel for naming convention review.
[ ] Brian Silveri to share the minion report with Vani for validation of position and brand fields (ticket 1579).
[ ] Brian Silveri to contact Jason Warnock to obtain the NetSuite API key for data ingestion.
[ ] Brian Silveri to schedule sync meeting with Kapil, Jason Warnock, Bharat, Trevor, and Russell to discuss MV Fluent performance metrics view architecture.

---

The Data Engineering team conducted their regular standup, covering progress on multiple initiatives. Key updates included **Joseph raising a PR for BI masking changes** and beginning validation work for the model loading spike, while **Granth's FTSO tickets were awaiting final sign-off** before production deployment. The team made progress resolving blockers, with Manav and Trevor implementing fixes for FDSO issues in the Audience Solutions workspace.

For the TAM 3.0 release, **Kapil completed the advertiser report** and plans to push it to production today, allowing Kevin to validate before switching from sandbox. The team **decided to hold the vertical dimension change for version 3.1** instead of the current release. Discussions centered on AppFlyer table naming conventions and the architectural approach for MV Fluent performance metrics, with Brian scheduling follow-up meetings to resolve both topics.

### TAM Release and NetSuite Integration

- Kapil completed review comments for the **advertiser report (ticket 1520)** and plans to push the release to production today, representing the main change for version 3.0.
- Once deployed to production, Kevin will be able to validate the advertiser report before switching his main report from sandbox to the new version.
- Venkatesh resolved most review comments from Bharat on the Ontraport PR, with remaining items to be discussed in today's call before deciding on release timing for NetSuite data ingestion.
- The **vertical dimension change will be held for version 3.1** instead of the current release, as planned by Bharat and tagged by Trevor.
- Kapil raised a bug ticket to refresh/run the **Fluent backfill daily** to reduce anonymous user counts, requiring only a config change.

### AppFlyer Integration Development

- Vani implemented the **bronze and silver layers** for AppFlyer integration, proposing table names 'user wise performance' and 'user wise in app event'.
- The team discussed table naming conventions, with Trevor suggesting 'metrics' instead of 'data' for aggregated information (installs, clicks, conversions).
- The team agreed to **spell out 'AppFlyer' in table names** rather than using the abbreviation 'AF', following internal conventions rather than external ones.
- Vani completed work on ticket 1579 to add position and brand fields, validating results against the provided report with successful outcomes.

### Ticket Progress and Blocker Resolutions

- Joseph raised a PR for the **cloud audience ticket** (AS board), worked with Monisha on declared audience segments, and received inputs from Vani for BI masking (ticket 1317).
- Manav and Trevor worked with David to **implement solutions for the FDSO blockage**, creating separate fixes for the SP and Audience Solutions Prod workspace plus test buckets in dev.
- Granth's FTSO tickets (489 and 491) are ready in dev and **awaiting Griggs' confirmation** to deploy to production, expected to unblock today.
- Manav completed work on a large model audience request that should be distributable today pending green light from Virginia, with no blockers remaining.
- Samreen and Pranjal are working on **MV Fluent performance tickets** (four total), having removed two fields not receiving data after discussion with Kapil.

### MV Fluent Performance Metrics Architecture

- Kapil raised concerns about creating one combined **MV Fluent performance metrics view** for all BUs (Playful, Adflow, Lead Manager), noting it would require expanding the current fact table.
- The proof of concept dashboard created by Warnock has **separate views/cards for each BU** (Playful, Adflow, Lead Manager) on the same page, suggesting separate metrics views might be easier.
- Brian explained the overall dashboard is an **enterprise-wide snapshot** showing all advertisers across Fluent by BU, requiring alignment on whether multiple views or one entity is preferred.
- Samreen confirmed using the user wise data view Kapil created, noting a slight difference in metrics compared to the Power BI dashboard that requires investigation.

### Decisions

- The **vertical dimension change will be held for version 3.1** instead of being included in the current TAM 3.0 release.

## Transcript

[00:00:19] Brian Silveri: I think we should be able to get started.
[00:00:32] Brian Silveri: Okay, let's get started. Who wants to go first today?
[00:00:43] Brian Silveri: I can go by. All right, Joseph, take it away. Yeah.
[00:00:47] Joseph: Raised a PR for now. The cloud audience ticket yesterday in the AS board and that's interview.
[00:00:53] Joseph: And worked with the Monisha Diara client.
[00:00:56] Joseph: Was helping out with the creation of declared audience segments.
[00:00:59] Joseph: And also for 1317, the BI masking got some inputs from Vani.
[00:01:04] Joseph: So I'll make those changes and raise a PR today.
[00:01:08] Joseph: Also, I'm planning to start the validation today for the model loading spike.
[00:01:12] Brian Silveri: Excellent. Excellent. And this one's still blocked?
[00:01:16] Joseph: Yeah, yeah, it's still blocked. I am following up with Mano.
[00:01:19] Vani Kaithi: Yep.
[00:01:20] Brian Silveri: Great, thank you.
[00:01:22] Manav Paul: Working on it right now. Me and Trevor were working with David yesterday.
[00:01:26] Manav Paul: They're implementing the solution to solve that blockage.
[00:01:30] Brian Silveri: Great news. Love it. Excellent. Thank you. Who's next?
[00:01:40] Granth S: I can go next.
[00:01:41] Venkatesh Mannam: Brian.
[00:01:44] Granth S: Yeah, yesterday I just worked with Griggs on getting the FTSO 489 and 491.
[00:01:50] Granth S: So everything looks good in dev. So you're just waiting a confirmation from me to sign off.
[00:01:55] Granth S: So he's just going to move or deploy those changes to production as well.
[00:02:00] Granth S: So the ticket that I have and blocked will just stay blocked for another day.
[00:02:05] Granth S: Once I get a confirmation from Griggs that everything looks good, then I can move that to in progress.
[00:02:11] Granth S: And at the same time, I'll work with Joseph on the validation piece and create a ticket for the tech debt part from the legacy to the new workspace.
[00:02:22] Granth S: So we'll work on that as well. But no blockers.
[00:02:27] Brian Silveri: Excellent. So that'll be unblocked today. Most likely.
[00:02:31] Granth S: Most likely.
[00:02:31] Brian Silveri: Excellent. Good. Good news. I like good news. Keep the trend going. Who's next?
[00:02:38] Manav Paul: I go.
[00:02:39] Granth S: Brian.
[00:02:42] Manav Paul: Yeah, worked on that FDSO with Trevor and David.
[00:02:45] Manav Paul: As I was saying yesterday, came to an agreement on what the best solution is.
[00:02:49] Manav Paul: They're going to be implementing some fixes for the SP and the audience Solutions Prod workspace and then a separate set of buckets to test in the dev workspace then.
[00:03:00] Manav Paul: Other than that, just worked on that big model audience request.
[00:03:04] Manav Paul: Should be able to distribute today once I get the green light from Virginia, but no blockers.
[00:03:11] Brian Silveri: Excellent. Good news. I will be.
[00:03:14] Brian Silveri: I will be scheduling a meeting this afternoon squad for the reviewing the Segment 2000 registry initiative for us today.
[00:03:23] Brian Silveri: So look out for that meeting later this afternoon.
[00:03:27] Manav Paul: Sounds good.
[00:03:28] Trevor Anderson: Great.
[00:03:29] Brian Silveri: Thank you. Manav. Who's next?
[00:03:36] Kapil Sreedharan: Yeah, I can go. Working on some of the review comments for the advertiser report.
[00:03:43] Brian Silveri: Great.
[00:03:44] Kapil Sreedharan: We'll plan to push that release today.
[00:03:47] Kapil Sreedharan: I think that's the only main change part of the version 3.0 that we have. The other. Yeah. So the.
[00:04:03] Kapil Sreedharan: The Advertiser thing is 1520. Yeah. And then that's that tune advertiser status.
[00:04:10] Brian Silveri: Great. Excellent. Yeah.
[00:04:13] Trevor Anderson: Great.
[00:04:13] Brian Silveri: So he'll have the TAM technically. Done and Excellent. That's great.
[00:04:18] Venkatesh Mannam: Yeah.
[00:04:19] Kapil Sreedharan: Once we push it to prod, I'll let Kevin know he can maybe do some quick validation before he switches over his main report to that from sandbox.
[00:04:34] Brian Silveri: Great. Yeah, he should be able to do that as soon as we.
[00:04:37] Brian Silveri: Technically, he should do it now, but we're just updating it with more information. But fine.
[00:04:43] Brian Silveri: Fair enough. Great work, Kapil Great work.
[00:04:45] Kapil Sreedharan: I. I also raised one buck ticket to refresh the OR run the fluent backfill every day.
[00:04:54] Brian Silveri: Oh, okay.
[00:04:56] Kapil Sreedharan: So that it will reduce that anonymous users count.
[00:05:01] Brian Silveri: Got it. So that's this one.
[00:05:03] Kapil Sreedharan: Yeah.
[00:05:04] Brian Silveri: Okay then.
[00:05:08] Trevor Anderson: All right.
[00:05:08] Brian Silveri: Are you. You're able to take that one on?
[00:05:11] Kapil Sreedharan: Yeah, just config change.
[00:05:14] Brian Silveri: Excellent. Great. Thank you, Kapil Who's next?
[00:05:22] Vani Kaithi: Yeah, I can go expl. So, yeah, for the app Flare 1 implemented the Bronze and silver layer.
[00:05:34] Vani Kaithi: So one thing to confirm with the team, like the table names for those two new tables, I'm thinking like it might be user wise performance and user wise in app event.
[00:05:52] Vani Kaithi: Oh, any suggestions on that table names?
[00:05:59] Brian Silveri: User wise event Makes sense to me.
[00:06:03] Samreen Shaikh: Yeah.
[00:06:04] Brian Silveri: And what the ones like app data?
[00:06:07] Vani Kaithi: Yeah, this has mostly like aggregated data like installs, clicks, conversions, and all.
[00:06:16] Trevor Anderson: The name data here. I brought it up because it was a vague term.
[00:06:25] Trevor Anderson: Maybe metrics might be a better indicator of what is actually in this table.
[00:06:33] Venkatesh Mannam: Yeah.
[00:06:33] Brian Silveri: Vaughn, do me a favor. Can you just post the.
[00:06:40] Brian Silveri: Or I guess just send me the tables in the data engineering channel and I'll also give my opinion because if it's a.
[00:06:48] Brian Silveri: If it's kind of like the minion event data, then it's a combination of stuff.
[00:06:55] Brian Silveri: So we would want to give it a. A more robust name. But again, no one's really using it but us. So.
[00:07:02] Brian Silveri: Trevor, whatever the naming convention that you and Kapil think from an entity perspective, I'm not sure which entity type it fits into.
[00:07:11] Brian Silveri: We should use this as our test case.
[00:07:14] Kapil Sreedharan: This is app Flyer, Right. It's something similar to Tune, you know, for.
[00:07:20] Kapil Sreedharan: It's tuned for mobile apps, right?
[00:07:23] Brian Silveri: That's right.
[00:07:26] Kapil Sreedharan: At least in the table name we have to indicate that it is apps flyer.
[00:07:32] Trevor Anderson: Yeah, make sure you put that apps flyer in the table name instead of. It's just AF right now.
[00:07:39] Trevor Anderson: But I would spell it out.
[00:07:42] Kapil Sreedharan: That's how I think that af. That's how JC calls it.
[00:07:46] Brian Silveri: Yeah. We don't have to follow his convention. Well, let's follow ours.
[00:07:50] Brian Silveri: And Trevor, you get the final sign off from a naming convention perspective. Great, Bonnie.
[00:07:58] Vani Kaithi: And also for 1579 to add the position use Brand.
[00:08:02] Vani Kaithi: Can you share me the report to validate them against?
[00:08:06] Brian Silveri: Sure. Yes, There's a minion report. I'll pull it up for you, Von. Yeah, no problem.
[00:08:11] Brian Silveri: And I posted the other report for you.
[00:08:14] Vani Kaithi: Yeah, I have validated that against it. Looks good.
[00:08:16] Brian Silveri: Excellent. Miracle. All right. Great work, Bonnie. Who's next?
[00:08:26] Venkatesh Mannam: Go ahead.
[00:08:27] Brian Silveri: Thank you.
[00:08:29] Venkatesh Mannam: Yeah, I went through some comments from Bharat on the other interview pr.
[00:08:36] Venkatesh Mannam: So mostly they are resolved and appeal told.
[00:08:39] Venkatesh Mannam: Like we will discuss few things in today's call and then depending on that we can decide whether to release or not.
[00:08:49] Venkatesh Mannam: Anyway, we are ingesting the netsuite data as well. Right. So we can push all at one go.
[00:08:55] Brian Silveri: Oh, that would be great.
[00:08:57] Venkatesh Mannam: Yeah. I mean like who is the point of contact to get the NetSuite API key?
[00:09:02] Brian Silveri: Brand and Jason Warnock.
[00:09:05] Venkatesh Mannam: Okay. Okay. Is it ready or shall I. I mean like shall I ping him for the API key?
[00:09:13] Kapil Sreedharan: Brian, can you contact him?
[00:09:16] Brian Silveri: Of course I can, yeah.
[00:09:18] Kapil Sreedharan: Jason Warnock.
[00:09:19] Brian Silveri: No problem. Yep. That's easy enough.
[00:09:23] Venkatesh Mannam: Yeah.
[00:09:24] Brian Silveri: Yeah. So thank you. Just to. To close the loop here. This vertical dimension to that table.
[00:09:32] Brian Silveri: Is this ready to go out in the today's release or are we holding it for the next release?
[00:09:37] Venkatesh Mannam: End of Sprint like Kapil told. We will hold off the. This change in this current release.
[00:09:44] Brian Silveri: That's fine.
[00:09:44] Kapil Sreedharan: I mean that was also what Bharat mentioned, right? That's the plan. We push it and.
[00:09:51] Kapil Sreedharan: And that's why Trevor created the version 3.1. So you tag him into that.
[00:09:56] Brian Silveri: Yep. No, no problem. I just wanted to confirm just so that I can comms it out. So.
[00:10:01] Trevor Anderson: Great.
[00:10:02] Brian Silveri: Yeah, just tag it to 31 please.
[00:10:04] Venkatesh Mannam: Yeah, yeah, sure thing. Yeah.
[00:10:05] Brian Silveri: Great. Thank you very much, Venky. Any issues or blockers, Venky?
[00:10:10] Venkatesh Mannam: No, no, nothing from here.
[00:10:12] Brian Silveri: Great. Excellent. Okay, who's next?
[00:10:19] Samreen Shaikh: We can go next for the tickets assigned to Bharat. Yeah, he's for one by 11 and 1541458.
[00:10:34] Samreen Shaikh: Yeah, he is working on the review comments which are given by Farat and he has also started on the new ticket.
[00:10:45] Brian Silveri: Great. So both in review then. Would that be a correct statement for 1511 and 1458?
[00:10:54] Brian Silveri: He's just updating the review comments.
[00:10:58] Samreen Shaikh: Yeah, sure.
[00:10:59] Brian Silveri: Great. Excellent. Thank you. And then this one's in progress. Excellent. Yeah, great.
[00:11:05] Samreen Shaikh: Yeah. And then there are tickets assigned to me and Pranjal.
[00:11:13] Brian Silveri: Okay.
[00:11:13] Samreen Shaikh: So these four both are related to the MV Fluent performance.
[00:11:18] Samreen Shaikh: So yesterday we had a discussion with Kapil and we are.
[00:11:26] Samreen Shaikh: We have removed those two fields which we were not getting data for and there are some more changes.
[00:11:35] Samreen Shaikh: So we are working on that.
[00:11:36] Brian Silveri: Okay.
[00:11:38] Samreen Shaikh: Yeah. And there is one ticket assigned to Shubham. Yeah, the.
[00:11:45] Samreen Shaikh: This is done, but we have kept here for testing. And there is one move which he has started on.
[00:11:53] Brian Silveri: Okay, great.
[00:11:55] Samreen Shaikh: Yeah. Ad campaign account manager. So he started on this one.
[00:11:59] Brian Silveri: Great. Yeah.
[00:12:01] Kapil Sreedharan: Brian, just a thing on this. So we are creating this MV Fluent performance metrics, right?
[00:12:12] Brian Silveri: Yes.
[00:12:14] Kapil Sreedharan: The thing is it will be hard to put all the metrics for all the different views in one place like Playful and you know, other bus.
[00:12:27] Kapil Sreedharan: That would mean we'll have to expand the current fact table. It gets complicated.
[00:12:34] Kapil Sreedharan: And now looking at the, you know, the dashboard, right. The.
[00:12:39] Kapil Sreedharan: The proof of concept that Warnock created, they have, you know, separate views or dashboards for each of these.
[00:12:51] Kapil Sreedharan: BU's like playful has its own. Same way Adflow has its own. Lead Manager has its own.
[00:12:57] Kapil Sreedharan: But it's on the same page, but it's a different, you know, card.
[00:13:02] Brian Silveri: Right.
[00:13:04] Trevor Anderson: Right.
[00:13:06] Kapil Sreedharan: Now if that is the case, then it'll be easier for us to create a metrics view for Playful and a separate metrics view for Lead Manager, separate
[00:13:17] Trevor Anderson: for
[00:13:19] Kapil Sreedharan: Adflow that we already have. And then they can just call that metrics view populate their.
[00:13:26] Kapil Sreedharan: Their dashboard.
[00:13:28] Brian Silveri: Well, yeah, let, I think Kapil it might make sense for you, me and Jason to just sync on that real quick because the thing that we just reviewed with Conlin and Katie was that their, their overall dash is a enterprise wide snapshot showing all of the advertisers across Fluent and then it shows by bu.
[00:14:00] Brian Silveri: If the structure is best for us to have multiple views, fine.
[00:14:05] Brian Silveri: And Jason can accommodate that, no problem.
[00:14:08] Brian Silveri: If he wants it as one entity, which is what our original approach was, then we got to figure it out.
[00:14:15] Brian Silveri: So I think maybe just a quick sync between you, me and Jason might solve that.
[00:14:19] Kapil Sreedharan: Yeah, I also include maybe Bharat and Trevor or Russell.
[00:14:25] Brian Silveri: Yeah, of course.
[00:14:26] Kapil Sreedharan: I think it's. It'll be good to get a better sense of how it's going to be used.
[00:14:33] Brian Silveri: Got it. No problem. Okay, I'll schedule that. Thank you. Anything else on the cyber side?
[00:14:46] Samreen Shaikh: Oh no, that's it from us. Thanks.
[00:14:49] Brian Silveri: Great.
[00:14:49] Kapil Sreedharan: And Samreen, you started using the user wise data one it created, right?
[00:14:55] Samreen Shaikh: Yeah. Right.
[00:14:58] Kapil Sreedharan: And how is the metrics does it match with the Power BI dashboard?
[00:15:08] Samreen Shaikh: There is a slight difference, so I'm checking on that.
[00:15:11] Kapil Sreedharan: Okay.
[00:15:12] Samreen Shaikh: Yeah.
[00:15:15] Vani Kaithi: Okay.
[00:15:16] Brian Silveri: Anybody else? All right, thank you all. Please enjoy the rest of your day.
[00:15:26] Brian Silveri: If I don't talk to you, keep up the great work. Thank the team.
