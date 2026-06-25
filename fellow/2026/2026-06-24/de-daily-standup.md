---
title: "DE Daily Standup"
meeting_id: "040000008200E00074C5B7101A82E00807EA0618A07C9A5FA178DC01000000000000000010000000AB9154BF9C4AA34B9FC26D395D9AA196"
note_id: 80771283
recording_id: 15040631
date: 2026-06-24
start_time: "2026-06-24T09:35:00-04:00"
end_time: "2026-06-24T10:00:00-04:00"
url: "https://fluentco.fellow.app/meetings/040000008200E00074C5B7101A82E00807EA0618A07C9A5FA178DC01000000000000000010000000AB9154BF9C4AA34B9FC26D395D9AA196/"
participants:
  - name: "Dan Duling"
    email: "dduling@fluentco.com"
    rsvp: accepted
    organizer: false
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
    rsvp: accepted
    organizer: false
  - email: "ksreedharan@fluentco.com"
    rsvp: accepted
    organizer: false
  - email: "vkaithi@fluentco.com"
    rsvp: none
    organizer: false
  - email: "VMannam@fluentco.com"
    rsvp: accepted
    organizer: false
  - email: "mpaul@fluentco.com"
    rsvp: none
    organizer: false
  - email: "gsajjanshetty@fluentco.com"
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
[ ] Review the query changes for ticket 1182 (conversion mappings default to false when null).
[ ] Follow up with Griggs on the FDSO ticket for dev service principal access to S3 bucket for model audience.
[ ] Venkatesh Mannam to create a bug ticket to investigate why offer events process is not properly backfilling fluent IDs, then decommission the manual backfill job once fixed.

## AI Summary

The team held their daily standup with Trevor running the meeting in Brian's absence. The main focus was on **preparing Release 3.0 for deployment**, with discussion about merging branches and moving several tickets (including date of birth enrichment and user data compliance changes) from the planned 3.1 release into 3.0. An extended debate occurred about the **backfill job scheduling**, with the team deciding to run it daily temporarily while investigating why the offer events process isn't properly backfilling fluent IDs. Progress continued on **model audience validation**, though work is blocked pending FDSO ticket resolution for service principal access to S3 buckets. Akhil completed work on Tremendous data integration, addressing retention defaults and preparing to reload the new campaign table schema across environments.

### Chapters

#### Release 3.0 deployment preparation
- Code for the release has been **merged into the develop branch**, and the team is preparing to create the release branch for deployment to production. *(0:10:08)*
- The team cannot follow the normal UAT-to-release promotion path because UAT contains changes intended for Release 3.1, requiring selective merging of only 3.0 changes. *(0:11:03)*
- UAT currently contains two extra changes beyond Release 3.0 scope: date of birth age enrichment for CDP (ticket 1567) and enable user data compliance updates (ticket 1569). *(0:11:29)*
- The team decided to move tickets 1567 and 1569 from Release 3.1 into Release 3.0 since the changes are already in UAT and are minor updates. *(0:12:28)*
- The release is targeted for deployment today, with Kapil managing the branch merging and promotion process. *(0:13:26)*

#### Tremendous data integration updates
- Akhil closed ticket 1182, fixing an issue where **conversion mappings need to default to false when null**, in addition to the previously fixed retention column default. *(0:00:46)*
- JC modified the Tremendous data structure, adding **new columns to the orders table and creating a new campaign table**, requiring reload in sandbox and eventually UAT and production. *(0:01:46)*
- The new Tremendous data comes from two different APIs combined into tables, with Akhil planning to join the orders and campaign tables to create a unified rewards table for downstream use. *(0:03:08)*
- Bharat raised concerns about joining the tables since downstream systems need campaign data separately for the gold campaign table, requiring offline discussion with Akhil to clarify the approach. *(0:04:03)*
- Akhil is working on ticket 1187 (expected completion by end of day) and will start on ticket 1454 tomorrow. *(0:02:01)*

#### Model audience validation blockers
- Granth and Joseph are running validations for model audience but are **blocked by FDSO tickets** that need to grant the dev service principal access to an S3 bucket. *(0:06:09)*
- Griggs acknowledged the FDSO ticket but hasn't provided an update since yesterday; Granth has already provided the necessary query for Griggs to execute. *(0:07:15)*
- Granth expects to complete ticket 1612 by end of day today or latest by tomorrow, pending resolution of the access blockers. *(0:06:40)*
- Joseph is continuing validation work with Granth and has open questions about the segment registry to discuss in the next meeting with Trevor. *(0:08:56)*

#### Backfill job scheduling debate
- Kapil raised a ticket to change the **backfill job from manual/monthly to daily** to ensure session events are properly backfilled with fluent IDs for the advertiser report (which looks back 60 days). *(0:13:47)*
- Bharat noted that the offer events process already has incremental backfill logic that should handle backfilling for the last 1-2 days, making the daily backfill job potentially redundant. *(0:14:47)*
- Kevin reported seeing a **drop in anonymous users** after the backfill job ran last week, suggesting the offer events process may not be catching all cases as expected. *(0:17:43)*
- Venkatesh confirmed the offer events code only backfills new incoming data, not looking back at previous days, which explains why the manual backfill job was finding gaps. *(0:21:11)*
- The team agreed to run the backfill job daily as a temporary solution while Venkatesh creates a bug ticket to fix the offer events incremental process, after which the daily job can be decommissioned. *(0:21:52)*

#### Other team updates
- Manav is working on a model audience request with Bharat to move forward distribution and will review segment registry epics to prepare for next quarter's work. *(0:09:27)*
- Vani raised a PR for app flyer position views (addressing PR comments) and validated the changes against the Minion report with successful results. *(0:22:16)*
- Venkatesh is waiting on a **NetSuite API key from the IT team** (request raised by Jason) before he can begin analysis work; approval process may take some time. *(0:22:54)*

### Action Items
- Trevor Anderson: Review the query changes for ticket 1182 (conversion mappings default to false when null).
- Trevor Anderson: Follow up with Griggs on the FDSO ticket for dev service principal access to S3 bucket for model audience.
- Venkatesh Mannam: Create a bug ticket to investigate why offer events process is not properly backfilling fluent IDs, then decommission the manual backfill job once fixed.

### Decisions
- Tickets 1567 (date of birth age enrichment) and 1569 (enable user data compliance) will be **moved from Release 3.1 to Release 3.0** since the changes are already in UAT.
- The backfill job will **run daily** on a temporary basis while the team investigates why the offer events incremental process isn't properly backfilling fluent IDs.

## Transcript

[00:00:02] Trevor Anderson: All right, Brian is out of office the next few days here, So I'll go ahead and run this meeting for the next few days.
[00:00:17] Trevor Anderson: Looks like Barat you're out tomorrow, is that still accurate?
[00:00:25] Venkatesh Mannam: Yep.
[00:00:26] Trevor Anderson: Awesome. And then Grant and it looks like you're out on Friday. Okay, sweet.
[00:00:31] Trevor Anderson: Well, happy Wednesday everyone. Shall we get started? Anyone would like to go?
[00:00:37] Akhil Gonna: Yeah, I can go first. Trevor. Thank you. Yeah. So yesterday I have fixed one minor issue. Okay. The top.
[00:00:46] Akhil Gonna: Yeah, so yesterday I did close 1182 where brand have found one concern where actually there is a new call new column added called retention.
[00:00:59] Akhil Gonna: Whenever that is null we need to default to false. That that is fixed earlier.
[00:01:05] Akhil Gonna: But there was one more issue where conversion conversion mappings even when it is null we need to make this as default to false.
[00:01:16] Akhil Gonna: So I made the change and sent it to Brand but seems Brian was away.
[00:01:22] Akhil Gonna: I just closed maybe do you want to take a look at it or.
[00:01:29] Trevor Anderson: Default over retention is set to false now. Oh, so you just asked me to check.
[00:01:34] Akhil Gonna: Yeah, yeah you can. The query is the same. Maybe later when you get chance you can check.
[00:01:40] Trevor Anderson: But yes, I'll take a look when I can.
[00:01:43] Kapil Sreedharan: But thank you.
[00:01:45] Akhil Gonna: Yeah.
[00:01:46] Akhil Gonna: And apart from this, JC also helped us having the new campaign table and seems the orders tables of tremendous is changed.
[00:01:55] Akhil Gonna: I reloaded in sandbox. Maybe we have to reload in UATN prod as well.
[00:02:01] Akhil Gonna: But I'm still working on 1187 which should be ready by end of today. Yeah, no blockers this one.
[00:02:12] Trevor Anderson: So he modified the existing tremendous data. Yeah, right. To include. What was this?
[00:02:22] Trevor Anderson: There was two different APIs or something like that and he included the other one in there.
[00:02:29] Trevor Anderson: So is there a new column that.
[00:02:31] Akhil Gonna: Yeah, there are news new columns.
[00:02:33] Akhil Gonna: There is changes schema for existing orders table and also there is new table for campaign tremendous.
[00:02:41] Akhil Gonna: Yeah, those. Those are the two new changes
[00:02:46] Trevor Anderson: you
[00:02:47] Kapil Sreedharan: already had campaigns table.
[00:02:48] Akhil Gonna: Right. No, Tremendous. This is the new table, Right. Missing campaign.
[00:03:03] Trevor Anderson: Is there a column that tells us which API?
[00:03:08] Trevor Anderson: Because I think what he told me was that now there's going to be two APIs that he's pulling data from until one table.
[00:03:16] Trevor Anderson: So yeah, no, he. How do we identify mention there is a call source?
[00:03:25] Akhil Gonna: Yeah, maybe.
[00:03:28] Kapil Sreedharan: I mean for us it doesn't matter, right? As long as he gives us all the orders and campaigns.
[00:03:33] Kapil Sreedharan: It's just one table.
[00:03:35] Akhil Gonna: Yeah, my plan is to join those two orders and camping table and make it as rewards table.
[00:03:44] Trevor Anderson: Okay, I guess it doesn't matter where it comes from.
[00:03:46] Akhil Gonna: Yeah, I think so.
[00:03:48] Kapil Sreedharan: Yeah, but he just exposes two Tables, right. Orders and campaigns.
[00:03:52] Akhil Gonna: Yes. Correct Kapil Yeah. Okay. Okay.
[00:04:01] Bharat Gidwani: Sorry. Akhil, why are we joining the two tables?
[00:04:03] Bharat Gidwani: I mean we can talk about this later, but here is why we're joining those two tables and creating a single.
[00:04:09] Bharat Gidwani: Because I thought we need campaign separately and orders separately in.
[00:04:15] Akhil Gonna: Yeah, I'm doing those two separate. But for the rewards table we need the campaign data and orders.
[00:04:26] Akhil Gonna: Orders in single table. That's what.
[00:04:29] Venkatesh Mannam: Okay, check.
[00:04:32] Bharat Gidwani: Connect with you later.
[00:04:33] Bharat Gidwani: But yeah, be careful because I thought downstream they need the campaign data separately which is where we are curating the gold Gold campaign table of fluent.
[00:04:48] Bharat Gidwani: But I'll talk to you about that.
[00:04:49] Akhil Gonna: Separately. Yeah. Trevor, in prod only we have orders table as of now. Campaign table is something new.
[00:05:01] Trevor Anderson: Do you have that like in your dev catalog here?
[00:05:04] Akhil Gonna: Not. It's in the external source yet.
[00:05:09] Trevor Anderson: Oh, it's not in any of these catalogs yet.
[00:05:12] Akhil Gonna: No, not yet. Okay. We had before with the CSV file, but that is outdated.
[00:05:18] Akhil Gonna: I need to read from the strip location now.
[00:05:22] Trevor Anderson: Okay.
[00:05:31] Akhil Gonna: Yeah, yeah, that's all for now. Thanks.
[00:05:39] Trevor Anderson: Any. Any. Any update on this one here or.
[00:05:43] Akhil Gonna: 14:54 Chance to work on this yesterday but yeah, I'll start working on this maybe tomorrow.
[00:05:52] Trevor Anderson: Okay, no problem. Thanks Akhil. Anyone would like to go next?
[00:06:09] Akhil Gonna: Next?
[00:06:09] Granth S: Robert? Yeah, yesterday I just worked with Joseph on running the validations for the model audience.
[00:06:19] Granth S: We ran into a few blockers or small minor blockers that I messaged you about that we had to raise the FDSO tickets for.
[00:06:28] Granth S: So yeah, just working with Craigs on that. Hopefully that's done by today. I'll.
[00:06:33] Granth S: I'll connect with them again. But yeah, apart from that everything's looking good.
[00:06:40] Granth S: Oh yeah, I'll should finish up 16:12 before the end of the day or by latest tomorrow.
[00:06:48] Granth S: But yeah, that looking good for that as well.
[00:06:52] Trevor Anderson: Okay. On that fdso, that's for the.
[00:06:58] Trevor Anderson: The dev service principle for modeled audience to access some S3 bucket, right?
[00:07:04] Akhil Gonna: Correct. Yes.
[00:07:07] Trevor Anderson: Okay. Did. Did Griggs provide any update on this? I.
[00:07:15] Trevor Anderson: I think you just told me yesterday that you sent it to him and he acknowledged.
[00:07:18] Granth S: But no, since then I've not received anything. He just let me know. Yeah. That he's working on it.
[00:07:26] Granth S: But yeah, apart from that, nothing else.
[00:07:29] Trevor Anderson: Okay, I'll check on it this morning with you.
[00:07:31] Akhil Gonna: Okay.
[00:07:32] Granth S: I've already given him what he needs to do on the query as well. So he just needs to run it.
[00:07:38] Akhil Gonna: So. Yeah.
[00:07:41] Trevor Anderson: Okay, thank you.
[00:07:44] Venkatesh Mannam: When you get a chance like can you check on the net suit one as well? I think I.
[00:07:48] Venkatesh Mannam: I don't have access to raise 1fds4 maybe if you haven't get.
[00:07:53] Venkatesh Mannam: If you get a chance, please raise one for Net suit as well.
[00:07:58] Trevor Anderson: Oh, you don't. You don't have access to submit an fda so.
[00:08:02] Venkatesh Mannam: Yeah. Yeah, I don't have a.
[00:08:06] Trevor Anderson: Have you tried it through. I think it's through this JIRA Service management here.
[00:08:12] Bharat Gidwani: This should work.
[00:08:14] Bharat Gidwani: It's not that you can't go on the board, if that's what you're trying to do on their board and trying to.
[00:08:20] Trevor Anderson: Yeah, you can't do it through the board. You have to go through your new catalog here.
[00:08:25] Venkatesh Mannam: Oh, okay.
[00:08:26] Trevor Anderson: I'll send you this link in the Zoom chat here. Give that a try. If you're still.
[00:08:32] Trevor Anderson: It still doesn't work, then.
[00:08:34] Venkatesh Mannam: Oh yeah, I can access. Yeah, thanks.
[00:08:36] Trevor Anderson: Okay, great. Great. Thanks, grant. All right, thank you.
[00:08:51] Joseph: Yeah, I was working with the
[00:08:53] Joseph: grant on the validation part
[00:08:54] Joseph: and we'll continue to do that.
[00:08:56] Venkatesh Mannam: And I did review the segment registry
[00:09:00] Joseph: which we talked about yesterday and I do have a
[00:09:04] Joseph: like a few open questions that I would like to discuss
[00:09:06] Akhil Gonna: with you like in the next meeting we have.
[00:09:10] Trevor Anderson: Okay. Yeah.
[00:09:11] Joseph: And apart from that, all good.
[00:09:18] Trevor Anderson: Okay, sounds good. Thanks, Joseph.
[00:09:26] Manav Paul: Go next, Trevor.
[00:09:27] Trevor Anderson: All right.
[00:09:29] Manav Paul: I was working on one of the model audience requests in the AS Bharat going to move forward in the distribution today and we'll take a look at the segment registry epics.
[00:09:38] Manav Paul: Make sure everything is marked correctly as to do or correct or complete and we'll go from there just to get ready for working on that for the next quarter.
[00:09:49] Manav Paul: But no blockers.
[00:09:51] Trevor Anderson: I don't see you on here.
[00:09:53] Manav Paul: Do you have anything not on this board? Probably.
[00:09:57] Trevor Anderson: Okay. It's on the audience solutions board.
[00:09:58] Manav Paul: Yeah.
[00:09:59] Granth S: Yeah.
[00:10:00] Trevor Anderson: Okay. All right, Sounds good. Thank you.
[00:10:08] Kapil Sreedharan: Yeah, I can go. Yeah, still working on the. On the release. I think the.
[00:10:18] Kapil Sreedharan: The code is now in merged into develop.
[00:10:23] Trevor Anderson: Okay.
[00:10:26] Kapil Sreedharan: Yeah, we'll raise a. We'll create a release branch.
[00:10:29] Kapil Sreedharan: I think we cannot move from U18 to release because this UAT has so many other changes.
[00:10:35] Kapil Sreedharan: So we'll have to. Yeah. Merge the. Just these changes into the release branch. Yeah.
[00:10:53] Trevor Anderson: Are you saying normally we go from release to dev to UAT and we're not going to follow that because UAT has other changes?
[00:11:03] Kapil Sreedharan: Yeah.
[00:11:03] Kapil Sreedharan: Normally we push the UAT into release branch and then release to merge, but the UAT has many other changes as well.
[00:11:16] Trevor Anderson: Like what?
[00:11:19] Kapil Sreedharan: I think those things from the next release 3:1.
[00:11:24] Trevor Anderson: Oh, I see.
[00:11:26] Kapil Sreedharan: I think Venki, you. You pushed your changes as well, right.
[00:11:29] Venkatesh Mannam: In uat there Is one extra change Kapil like for the date of birth age enrichment for the CDP.
[00:11:41] Kapil Sreedharan: Is that 1 5, 6, 7.
[00:11:44] Venkatesh Mannam: Yeah, that's right. Yeah.
[00:11:46] Kapil Sreedharan: Is that the only thing?
[00:11:48] Bharat Gidwani: Yeah, that's the only thing.
[00:11:49] Venkatesh Mannam: Yeah.
[00:11:52] Kapil Sreedharan: Okay, then we should be able to
[00:11:56] Venkatesh Mannam: push from UAT and I think like a small changes from Bharat as well. Like.
[00:12:04] Venkatesh Mannam: Yeah, enable user data compliance. Yeah, that one.
[00:12:08] Trevor Anderson: Do you. Do we want to move these to 30 then if we're you, you let me know Kapil But I was gonna
[00:12:15] Bharat Gidwani: ask today because I. I saw those changes moved to uat but if they are already there, the.
[00:12:22] Bharat Gidwani: The minor updates so we should be fine pushing these out.
[00:12:28] Kapil Sreedharan: Okay. Yeah. Then we can move it to 3 0.
[00:12:32] Trevor Anderson: Okay, I'm gonna move them.
[00:12:34] Kapil Sreedharan: Yeah.
[00:12:38] Trevor Anderson: Okay.
[00:12:39] Venkatesh Mannam: And the other one as well. Yeah, the last one.
[00:12:44] Trevor Anderson: Oh yeah, this one. Okay.
[00:12:49] Venkatesh Mannam: Yeah, yeah. 1451 changes. I'm intentionally not merging those changes.
[00:12:54] Venkatesh Mannam: I'm waiting for the net suit ones. Yeah.
[00:12:57] Kapil Sreedharan: Okay, so my change is currently in dev. I'll raise a. Well, we cannot merge from dev to uit. Right.
[00:13:06] Kapil Sreedharan: So I'll raise a PR from my feature branch to uat.
[00:13:12] Venkatesh Mannam: So I think dev to UIT is clean now maybe. Yeah, it's clean now. Yeah.
[00:13:18] Kapil Sreedharan: Okay.
[00:13:19] Venkatesh Mannam: Yeah.
[00:13:24] Kapil Sreedharan: Okay, cool. Yeah, we'll.
[00:13:25] Venkatesh Mannam: We'll do that.
[00:13:26] Trevor Anderson: Okay. We'll try to get the release out today. Yeah. Awesome. That sounds great. Thank you.
[00:13:41] Trevor Anderson: Is this one new?
[00:13:47] Kapil Sreedharan: Oh yeah. This is the backfill job.
[00:13:49] Kapil Sreedharan: Yeah, we'll also push it today is just changing the schedule of this job.
[00:13:57] Trevor Anderson: What happened? Was it running out of order or.
[00:14:00] Kapil Sreedharan: No, right now it's manual.
[00:14:02] Trevor Anderson: Oh, it is.
[00:14:03] Manav Paul: Okay.
[00:14:05] Kapil Sreedharan: Or every month.
[00:14:11] Trevor Anderson: Okay. So you just run this.
[00:14:12] Trevor Anderson: You've been running it manually, but we can run it every month on a schedule.
[00:14:17] Trevor Anderson: So what we're changing into or.
[00:14:19] Kapil Sreedharan: Yeah, yeah, this one we.
[00:14:21] Kapil Sreedharan: We want to run it every day so that it can backfill the session events with the fluent id.
[00:14:33] Bharat Gidwani: This is already. This is already done in the. In the pro. In the offer events process.
[00:14:42] Akhil Gonna: Right.
[00:14:47] Bharat Gidwani: I think this is already taken care of the back.
[00:14:51] Bharat Gidwani: Anytime there's null, we I think look for last two or three days worth of data and backfill that in the events.
[00:14:59] Bharat Gidwani: I'm not wrong.
[00:15:00] Venkatesh Mannam: In the.
[00:15:00] Bharat Gidwani: In the existing exhibition. This.
[00:15:02] Bharat Gidwani: This job was just to say if for whatever reason that thing skips a day or two and like if it fails and it skips it.
[00:15:12] Bharat Gidwani: That's why this process was created.
[00:15:14] Bharat Gidwani: The existing offer events job should take care of backfilling yesterday's data.
[00:15:23] Kapil Sreedharan: But what about the last 60 days?
[00:15:27] Bharat Gidwani: We don't need 60 days like that offer events every day, every 20 minutes or so.
[00:15:33] Bharat Gidwani: When it runs, it backfills for last a day or two worth of data. So it should be all caught up
[00:15:45] Kapil Sreedharan: because for the, for the advertiser report, they are.
[00:15:49] Kapil Sreedharan: They're looking for, you know, last 60 days, right?
[00:15:54] Bharat Gidwani: No, no, but it should already be there. Like there shouldn't be anything missing.
[00:15:58] Bharat Gidwani: This backfill should not fill anything in there.
[00:16:02] Bharat Gidwani: This should not be required because the back the existing minion offer events.
[00:16:09] Bharat Gidwani: It sees if there are any events which have null fluent IDs and if we have them, it backfills it for I think last one or two days, if I remember it correctly.
[00:16:20] Kapil Sreedharan: Yeah, but what about the older events? Like I came in last month.
[00:16:25] Bharat Gidwani: Yeah, but your ID is generated the same day.
[00:16:29] Bharat Gidwani: The next day when we run the CDP process, it gets generated. So we should have all. That's our thing.
[00:16:36] Bharat Gidwani: It should already be working fine. There shouldn't be any captures.
[00:16:42] Bharat Gidwani: This process was done or added so that if for whatever reason there are few which gets left over, it'll catch up.
[00:16:50] Bharat Gidwani: But it shouldn't be like required every day.
[00:16:56] Kapil Sreedharan: Okay. Okay, got it. Yeah.
[00:16:57] Trevor Anderson: Yeah, this has a look back.
[00:17:00] Bharat Gidwani: Yeah, that's what like if this was. This was added to see if for whatever reason we missed anyone.
[00:17:06] Bharat Gidwani: For whatever reason, maybe the job failed for a day and we are fixing it and it skipped that window.
[00:17:13] Bharat Gidwani: This will catch those up. But I mean this should be a very small percentage of users, if any.
[00:17:20] Bharat Gidwani: So this is just catch all situation. That's what this job is supposed to do.
[00:17:26] Venkatesh Mannam: Not.
[00:17:27] Bharat Gidwani: This is not supposed to be once a month then. Yeah, that's why it was once a month.
[00:17:32] Venkatesh Mannam: That.
[00:17:33] Bharat Gidwani: Okay, if I, if you miss anything,
[00:17:35] Venkatesh Mannam: it will do this.
[00:17:36] Bharat Gidwani: But okay, I'll.
[00:17:38] Kapil Sreedharan: I'll check.
[00:17:39] Bharat Gidwani: So you should check if there is anything that.
[00:17:43] Kapil Sreedharan: Last week after running it, running this job, Kevin said he saw a drop in anonymous users. I'll.
[00:17:52] Kapil Sreedharan: I'll check. We'll see.
[00:17:56] Bharat Gidwani: Because I don't remember when we enabled the incremental process because if the incremental is not running.
[00:18:02] Bharat Gidwani: Yes, there'll be gaps because I ran
[00:18:04] Kapil Sreedharan: this yesterday manually and then before that it was fun on 19th or something.
[00:18:09] Kapil Sreedharan: If you check the timestamp.
[00:18:11] Kapil Sreedharan: And after running that, I think Kevin mentioned that he saw some drop, but that is odd.
[00:18:23] Bharat Gidwani: Yeah. If that is happening, we should, we should see why that is happening.
[00:18:26] Bharat Gidwani: Because the offer events should be backfilling the info. It should not be.
[00:18:33] Bharat Gidwani: Should not be required to drop the. Or should not be required to run this Every day.
[00:18:40] Kapil Sreedharan: Yeah.
[00:18:43] Trevor Anderson: Looks like this has been ran every.
[00:18:48] Trevor Anderson: But even every other day or every Kapil days for the past few months.
[00:18:52] Kapil Sreedharan: I think in the. In the job. In this job it'll. It shows right. How many was backfills.
[00:18:58] Trevor Anderson: So does it. I see it. No, no, it doesn't.
[00:19:07] Kapil Sreedharan: Number of effect that this is. This looks empty, but it's not.
[00:19:12] Trevor Anderson: Oh, maybe this one. Maybe. Let me go to a. It's just a data type.
[00:19:25] Kapil Sreedharan: Okay.
[00:19:31] Bharat Gidwani: Yeah. I.
[00:19:32] Bharat Gidwani: I mean if this is actually doing something like if it is filling up some information for people we were missing, we should definitely add a bug to figure out why Offer Events is not doing it.
[00:19:46] Bharat Gidwani: Maybe there's something missing there because both of them are supposed to do the same thing.
[00:19:53] Bharat Gidwani: Look at at least that incremental process.
[00:19:56] Bharat Gidwani: The SLO Offer Events is supposed to run it for last one or two days to at least catch up any.
[00:20:04] Bharat Gidwani: Any of those missing or any of the users who were generated new.
[00:20:09] Bharat Gidwani: But yes, it could happen that if that user was shown came in very long ago and we just generated for some reason it would be missing.
[00:20:21] Bharat Gidwani: But technically that should not happen anymore. This was an old.
[00:20:26] Bharat Gidwani: I mean any old data should already be populated.
[00:20:29] Trevor Anderson: Yeah.
[00:20:29] Kapil Sreedharan: Yeah, makes sense. Okay, I'll. I'll. I'll do some validation and we'll see.
[00:20:36] Kapil Sreedharan: But yeah, so for now then we don't need this.
[00:20:42] Bharat Gidwani: Yeah, a Kapil If you're validating it, if you find issue, definitely maybe add a bug there because it is most likely something wrong in the offer events.
[00:20:54] Kapil Sreedharan: Yeah.
[00:20:54] Bharat Gidwani: Because that should. Should be doing this right now.
[00:20:58] Kapil Sreedharan: Yeah.
[00:20:59] Venkatesh Mannam: I was going through the code, but seems like we have a process to backfill whatever the data frame that we get into the whatever events that we get on every job run.
[00:21:11] Bharat Gidwani: I thought it even looks at the last day also the backfill.
[00:21:17] Bharat Gidwani: If it is only looking at the new data, then yes, what Kapil is saying makes sense.
[00:21:22] Bharat Gidwani: It will not catch up or it will not do anything on the last. But maybe.
[00:21:27] Bharat Gidwani: Maybe that's what we should do in there. So then a Kapil What you're saying makes sense. It.
[00:21:32] Bharat Gidwani: You will have to run this one to look at the events from yesterday because what Venki is saying it is only looking at the new data that is coming in.
[00:21:42] Bharat Gidwani: Not maybe not the whole thing.
[00:21:45] Akhil Gonna: But.
[00:21:46] Bharat Gidwani: But when he just confirmed that if that is the case, then what Kapil is saying that ticket will be required.
[00:21:52] Venkatesh Mannam: Yeah.
[00:21:52] Kapil Sreedharan: Okay. So then for now we'll implement this.
[00:21:55] Venkatesh Mannam: This.
[00:21:56] Kapil Sreedharan: Running this every day and then winky create a bug ticket to address that and then we can decommission this job.
[00:22:02] Venkatesh Mannam: Yeah, sure thing. Yeah.
[00:22:06] Trevor Anderson: All right, thanks guys. All right speed round before our next stand up before our unified stand up.
[00:22:13] Trevor Anderson: Go for it, Bonnie. Yeah.
[00:22:16] Vani Kaithi: So for the app flaring yesterday raised
[00:22:19] Bharat Gidwani: a PR and addressing few PR comments on that.
[00:22:24] Vani Kaithi: This is for the position views.
[00:22:27] Akhil Gonna: Okay.
[00:22:28] Vani Kaithi: Yeah I have just raised a PR for this.
[00:22:31] Bharat Gidwani: I have added and validated against Minion report.
[00:22:35] Vani Kaithi: It looks good.
[00:22:39] Akhil Gonna: Okay.
[00:22:39] Trevor Anderson: I in position view. Okay. Yeah. Is that. That's all? Okay, awesome. Thank you. And then Venki, any.
[00:22:51] Trevor Anderson: Any update?
[00:22:54] Venkatesh Mannam: Yeah, I'm waiting on for an ED suite API key from Bran. He told like.
[00:23:00] Venkatesh Mannam: I mean like Jason confirmed that right now it's with IT team.
[00:23:05] Venkatesh Mannam: It seems not sure like how we are tracking over that request if you have any information Trevor.
[00:23:15] Venkatesh Mannam: So once I get that API key maybe I can start playing around the net Suit data and see like through some analysis on the netsuite data.
[00:23:28] Trevor Anderson: Has this happened before? How do we normally. I mean I'm sure I could just email.
[00:23:32] Trevor Anderson: We could just email the the team about it. Any suggestions?
[00:23:40] Kapil Sreedharan: Is this Net Suit access?
[00:23:42] Trevor Anderson: Yeah, Netsuite API access.
[00:23:44] Kapil Sreedharan: Yeah, Jason raised a IT ticket. Oh he did already the finance team. So yeah, I think
[00:23:54] Akhil Gonna: so.
[00:23:54] Trevor Anderson: I'll have to wait on that.
[00:23:55] Akhil Gonna: Yeah, we'll have to wait on that.
[00:24:00] Kapil Sreedharan: Okay. I think he also said that it might not be. It'll take some time to get it.
[00:24:03] Kapil Sreedharan: It's to get those approved.
[00:24:07] Trevor Anderson: Okay. So this might be waiting for a while.
[00:24:12] Trevor Anderson: I'm just going to write a comment real quick but let's jump to the unified standup.
[00:24:18] Trevor Anderson: But thanks everyone.
[00:24:25] Kapil Sreedharan: All right guys, thanks.
[00:24:27] Trevor Anderson: Thanks Jim.
