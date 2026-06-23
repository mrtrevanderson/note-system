---
title: DE Sprint Retro and Planning
date: 2026-06-22
time: 06:05-07:00
participants: [Dan Duling, Trevor Anderson, Brian Silveri, Akhil Gonna, Bharat Gidwani, Joseph Melkin, Kapil Sreedharan, Vani Kaithi, Venkatesh Mannam, Granth S]
fellow_url: https://fluentco.fellow.app/meetings/040000008200E00074C5B7101A82E00807EA061640060797A178DC01000000000000000010000000A54272EE0A813C40AB4FD4B73783F52D/
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
[ ] Brian Silveri to follow up with JC about resolving the tremendous campaign data blocker for gold rewards creation.
[ ] Granth S to add time off (Friday, June 26) to the shared calendar.
[ ] Post a note in the chat about the assignment and triaging process for the Cybage team.
[ ] Brian Silveri to create and assign discovery spikes for Q3 work, including reviewing Q3 epics and PRDs to draft TDD and discovery actions.
[ ] Brian Silveri to schedule a segments planning meeting for Tuesday or Wednesday (June 23-24) with the team to review audience segment work before sprint end.
[ ] Brian Silveri to follow up with DevSecOps and JC regarding open blockers for the team.

---

The DE team held their bi-weekly sprint retro and planning meeting, reviewing current sprint progress, celebrating wins, and planning Q3 work structure. The team received **kudos for excellent work** on the advertiser report, data ingestions, and naming convention standards, while **DE was highlighted as critical path** to Fluent's success across many product lines. The current sprint faced blockers including the **tremendous campaign data issue** and open DevSecOps requests, prompting better collaborative pre-planning with DevSecOps for Q3.

The next sprint capacity was adjusted to **7 points** due to multiple team members taking time off. For Q3, the team's work was organized into **four swim lanes** (Core at 80% capacity, Reporting, Audience Solutions, and Tech initiatives at 20% floor), with **every sprint including stories across all four lanes** to balance the workload. Brian prepared a comprehensive presentation for leadership to determine priorities across the team's extensive cross-team dependencies, acknowledging the current workload is unsustainable at 10 dev resources.

### Sprint Progress Review

- Akhil has one PR in review for column default changes awaiting team approval, with the tremendous data blocker still preventing gold rewards work, and started a new ticket on Friday.
- Granth completed **D1550 exploration** and will create a follow-up ticket for the migration piece. The FDSO ticket remains blocked pending feedback from Trevor or Griggs, though validation can start in dev environment.
- Vani is validating the Power BI report and finalizing the silver layer brand work for **D1577**.
- Venkatesh made changes to the PF that Kapil reviewed on Thursday and is adding status flags from each system. Both tasks will be tagged to the release once approved.
- Joseph updated stories with **2 tasks for this sprint** and 2 completed tasks pushed to develop branch. The DA pipeline has a permission issue for one table that Granth discussed with Greg, expected to be resolved today.
- Trevor is completing the **naming conventions review** with the team later today and formalizing the governance model, which will carry over with added acceptance criteria. He also drafted epics for Q3 tech initiatives to help tag existing backlog stories.

### Sprint Retrospective

- The team received **kudos for excellent work** on the advertiser report (Kapil and Vani), new data ingestions, finalizing naming convention standards, and the audience solutions consulting and streaming pipelines targeting Q2 completion.
- Brian showed that **DE is critical path** to Fluent's success across many product lines, serving as the glue for a giant list of projects that all require data engineering work to succeed.
- The **tremendous campaign data blocker** continues to prevent gold rewards creation, requiring follow-up with JC to resolve.
- **Open DevSecOps requests** were a blocker this sprint, particularly around audience solutions. Trevor will continue following up with Griggs on these issues.
- For Q3, there will be **better collaborative pre-planning** with DevSecOps. Team members should proactively identify potential DevSecOps involvement during discovery and spike work, as they're an on-demand service with limited capacity (one primary body, one helping).

### Next Sprint Planning

- Sprint capacity adjusted from **8 to 7 points** due to multiple team members being out: Bharat for 2 weeks, Manav all of next week, Granth on Friday June 26, and Brian Wednesday-Friday.
- **Time off noted**: Canada Day on July 1st, US holidays on July 2-3, and Trevor out the week of July 6th (next sprint).
- Work reassigned during planning: **netsuite data ingestion** to Venkatesh (requires API key from Jason) and **position view for advertiser report** to Vani due to Manav's absence.
- Trevor and the Cybage team will discuss **triaging and assignment process** tomorrow, likely routing work through their lead for distribution.
- Trevor and Kapil will receive **discovery spikes for Q3 work** to review the Q3 epics and PRDs and draft their TDD and discovery actions.
- Brian assigned sprint tasks to team members, accounting for upcoming time off including Manav's absence and Joseph's holiday availability.
- Joseph volunteered to pick up Manav's tasks during his absence (June 29-July 7) to ensure sprint continuity.
- Joseph has a validation ticket for the model audience pipeline that exists as a subtask nested within one of the sprint stories.
- Bharat is working on a lead manager task with Namat, pending DevSecOps support for testing in Dev. The task is not urgent and prepares infrastructure for future Integral integrations.
- Granth confirmed Greg responded to FDSO 489 comments; the production catalog work appears complete on Greg's end pending Granth's review.

### Q3 Audience Solutions Planning

- Brian consolidated multiple audience segment epics into **one comprehensive epic** to simplify tracking and planning.
- A segments planning meeting will be scheduled for **Tuesday or Wednesday (June 23-24)** with Joseph, Granth(?), and Manav to review Q3 segment registry work.
- Team will either pick up segment tasks early if model pipeline work completes ahead of schedule, or dedicate the next sprint specifically to segments if needed.
- The second Q3 OKR for Audience Solutions is the **audience solutions user interface** (dashboard showing comprehensive audience data), to be planned in the next sprint.
- The segments planning meeting was prioritized for this week to ensure Manav can participate before his time off (June 29-July 7), with deployment planned once the team completes their review.

### Q3 Work Structure

- Q3 work organized into **four swim lanes** using Gantt chart visualization: Core (fluent core initiatives with 80% of 6 devs), Reporting (2 dedicated devs), Audience Solutions (2 dedicated devs), and Tech initiatives (20% protected floor).
- Core lane covers **ROAS phase 2, in-store incentives, identity graph to rewards, and Fluentco one data ingestion pipeline** representing 80% of the team's development capacity.
- Reporting lane includes **advertiser report phase 2, CBUR phase 2, auction explainer, marketing revenue CRM integration**, plus ongoing metric requests from various teams.
- The team has **10 total dev resources** (including the Sybase team) to handle an extensive cross-team workload that Brian will present to Don next week for prioritization decisions.
- Team structure allows flexibility - developers can form project-based pods or pick tasks across lanes, but Brian emphasized the reality of **significant context switching** given the volume of parallel work streams.

### Cross-Team Dependencies and Prioritization

- Brian is preparing a **comprehensive cross-team project sequencing presentation** for Don next week to determine priorities, value propositions, and resource allocation across all DE-dependent initiatives.
- The team's workload is unsustainable at current capacity - Brian noted **'we do not have a team of 100 people'** and that some initiatives will need to be deprioritized or descoped by leadership.
- Projects vary in their team dependencies - most follow the standard **Pulse → DE → DS → BI** sequence, but some like ROAS follow different patterns (starting with DE and Pulse work, then DS).
- Brian is organizing work into understandable sequences showing how many devs can be devoted to each lane and the interdependencies across the organization.

### Project Tracking and Documentation

- **One PRD per project** that includes requirements for all sub-teams (DE, DS, BI), with each team adding their specific work once requirements are clear.
- Each team maintains their own JIRA board with epics, but projects use **cross-linking and labels** to track dependencies (e.g., BI team uses labels to track DE work they depend on).
- The Pulse idea board allows linking across multiple delivery lanes, and all epics are connected to their parent PRDs for clear traceability.
- Ad hoc reporting requests (like adding country to metrics) are tracked in a **non-initiative work bucket** under the reporting epic to capture DE work outside major initiatives.
- Brian will schedule dedicated time to review Q3 priorities in detail with the team once final prioritization is confirmed, walking through each individual initiative.

### Action Items

- Brian Silveri to follow up with JC about resolving the tremendous campaign data blocker for gold rewards creation.
- Granth S to add time off (Friday, June 26) to the shared calendar.
- Trevor Anderson to post a note in the chat about the assignment and triaging process for the Cybage team.
- Brian Silveri to create and assign discovery spikes for Q3 work, including reviewing Q3 epics and PRDs to draft TDD and discovery actions.
- Brian Silveri to schedule a segments planning meeting for Tuesday or Wednesday (June 23-24) with the team to review audience segment work before sprint end.
- Brian Silveri to follow up with DevSecOps and JC regarding open blockers for the team.

### Decisions

- Q3 work organized into **four swim lanes**: Core (80% of 6 devs for Fluentco initiatives), Reporting (2 devs), Audience Solutions (2 devs), and Tech initiatives (20% protected floor across all team members).
- **Every sprint will include stories across all four work lanes** to balance Core, Reporting, Audience Solutions, and Tech initiative work.

## Transcript

[00:00:00] Dan Duling: One updated their
[00:00:06] Dan Duling: status
[00:00:08] Dan Duling: of their projects or their stories. Snow maybe so not sure. Let's go through it.
[00:00:24] Dan Duling: Dan Dilling is scratch. Okay, let's see. Kiel, how are we doing here?
[00:00:32] Akhil Gonna: Yeah, the one in review, I made the change and reached the pr.
[00:00:37] Akhil Gonna: It's just the change to make the columns default to false. Once the team approves, maybe I can.
[00:00:45] Akhil Gonna: I can close the ticket. But apart from that, the tremendous is still blocked.
[00:00:51] Akhil Gonna: And the other ticket I just started on Friday.
[00:00:54] Dan Duling: Got it. Okay, great. So I'll carry over. Thank you. Okay, who's next?
[00:01:04] Dan Duling: Oh, go ahead, Grant.
[00:01:07] Granth S: Yeah, actually for D1550, the work, I guess the exploration is done for that.
[00:01:15] Granth S: I think I have already spoken with you about this last week, Brian.
[00:01:19] Granth S: So I'll just create a follow up ticket to work on the migration piece.
[00:01:24] Dan Duling: Great.
[00:01:26] Dan Duling: So this one is done?
[00:01:27] Granth S: Yes.
[00:01:28] Dan Duling: Yeah. Great. Okay.
[00:01:31] Dan Duling: And these, this one's still blocked off of the DevSecOps.
[00:01:37] Granth S: Yes, the FDSO ticket. I haven't heard back anything from Trevor or from Craig's.
[00:01:43] Granth S: So I'm still assuming that's still unblocked.
[00:01:47] Granth S: But we can still start the validation piece from the dev environment.
[00:01:52] Dan Duling: Yeah, got it. And then is 1224 that I think
[00:01:57] Granth S: the one that's unblocked is the.
[00:02:00] Dan Duling: The task of this one. Okay, got it.
[00:02:03] Dan Duling: Yeah. Okay.
[00:02:05] Dan Duling: And then Joseph, you've got this one and is that still. Is that not yet started or is it this sprint?
[00:02:16] Dan Duling: Okay, got it.
[00:02:18] Dan Duling: All right, thank you.
[00:02:20] Dan Duling: Okay, great.
[00:02:20] Dan Duling: So those will cut over. Great, thank you. Okay, who's next? Anybody?
[00:02:36] Vani Kaithi: Yeah, I can go ahead.
[00:02:38] Joseph: Lonnie.
[00:02:43] Vani Kaithi: Still working on this. So D1577.
[00:02:48] Vani Kaithi: I'll validate with the playful Power BI report and finalize the silver layer brand.
[00:02:54] Dan Duling: Great. Great work, Mike. Thank you. Right, thank you.
[00:02:59] Venkatesh Mannam: Yeah, Brian, like 1451. Kapil mentioned that we got the.
[00:03:04] Venkatesh Mannam: I mean like we were trying to pull the NetSuite data as well.
[00:03:07] Dan Duling: Right.
[00:03:08] Venkatesh Mannam: So are we holding on these changes until we pull the NET suite and analyze the data?
[00:03:13] Dan Duling: No, no, the netsuite is in the next story.
[00:03:16] Dan Duling: So we have a subtask in that NetSuite story to update this. So let's just close this one out, deploy.
[00:03:24] Venkatesh Mannam: Yeah.
[00:03:24] Dan Duling: And then add netsuite as a follow on I have a place.
[00:03:28] Venkatesh Mannam: Yeah, sure. Yeah, I made some new changes to this pf, so seems saw those changes on Thursday.
[00:03:38] Venkatesh Mannam: So once we approve this, it should be good.
[00:03:41] Vani Kaithi: Great.
[00:03:42] Kapil Sreedharan (1): So right now it's null, right?
[00:03:44] Venkatesh Mannam: Yes.
[00:03:46] Dan Duling: Yeah, yeah, that's it. Great.
[00:03:55] Dan Duling: And then this tune one, is that done?
[00:03:59] Venkatesh Mannam: Yeah, it's done. Just adding the status flag from each and every respect to systems.
[00:04:04] Akhil Gonna: Right.
[00:04:05] Venkatesh Mannam: Yeah, that's.
[00:04:06] Dan Duling: Yeah. Great.
[00:04:07] Dan Duling: Add that. Tag it to the release for me.
[00:04:10] Dan Duling: Venki.
[00:04:10] Dan Duling: I guess both of them.
[00:04:11] Venkatesh Mannam: Yeah, sure.
[00:04:12] Dan Duling: Yeah. Thank you. Right. Thank you. Who's next?
[00:04:24] Joseph: I can go.
[00:04:24] Dan Duling: Brian, Go ahead. Just.
[00:04:27] Joseph: Yeah.
[00:04:27] Joseph: I've updated the stories and I have two tasks for this Sprint and the other two tasks I have put it to done.
[00:04:37] Dan Duling: Great.
[00:04:39] Dan Duling: Awesome.
[00:04:40] Dan Duling: Is this in a release?
[00:04:43] Joseph: It's already been pushed to the develop branch and that's how we are evaluating the other pipelines.
[00:04:49] Joseph: The DA pipeline. So we have another. Not an issue.
[00:04:53] Bharat Gidwani: It's like we are.
[00:04:54] Joseph: The SP is not having a required permission for one table.
[00:04:59] Joseph: So I think Grant has already spoken to Greg and that should be done by today.
[00:05:05] Dan Duling: Great.
[00:05:06] Joseph: Yeah,
[00:05:10] Vani Kaithi: great.
[00:05:13] Dan Duling: Thank you. As we carry over. Who's next?
[00:05:29] Granth S: Trevor,
[00:05:31] Dan Duling: you've got some stuff.
[00:05:39] Trevor Anderson: Good morning. We're going through the second half.
[00:05:42] Trevor Anderson: Just going through the rest of the review on the naming conventions later today.
[00:05:50] Trevor Anderson: Been Friday was doing some planning for Q3 for the tech initiatives.
[00:05:59] Trevor Anderson: I've written of some epics that describe each of the different areas. So I can.
[00:06:06] Trevor Anderson: I can put those up on the board so we can start tagging or associating stories to them because I think there's some existing stories in the backlog that go to these different groups.
[00:06:18] Trevor Anderson: So up to you. I'm not sure if you want to do that or.
[00:06:21] Dan Duling: I can go ahead, feel free.
[00:06:25] Dan Duling: Yeah.
[00:06:25] Trevor Anderson: Okay, great.
[00:06:28] Dan Duling: I have not looked at those PRDs yet, Trevor, but yeah, that should not stop you.
[00:06:35] Dan Duling: Awesome.
[00:06:36] Dan Duling: Okay. And then are these two stories carrying over or is this naming convention? I think it was.
[00:06:44] Trevor Anderson: That's done. We're just going through with the team. And then the governance model. I'm yeah.
[00:06:51] Trevor Anderson: Currently building that out right now. Although the model's done.
[00:06:57] Trevor Anderson: It just needs to be, you know, formalized and written up.
[00:07:03] Dan Duling: Yeah.
[00:07:04] Dan Duling: So let's.
[00:07:04] Dan Duling: Let's just add your acceptance criteria for this one, Trevor, and then you can carry it over and then we can call it done once you're happy with the formalized pattern.
[00:07:16] Trevor Anderson: That sounds good.
[00:07:17] Dan Duling: Great. Okay.
[00:07:20] Dan Duling: All right, Barad, I think the only ones for you are in blocked.
[00:07:24] Dan Duling: So. Good. That's a carryover. Yep.
[00:07:27] Dan Duling: Okay, great. Excellent work, team.
[00:07:34] Dan Duling: And let's get started for the retro.
[00:08:00] Dan Duling: All right, ready?
[00:08:02] Dan Duling: Everybody have access.
[00:08:05] Dan Duling: All right, get it.
[00:13:09] Dan Duling: Does anyone need more time?
[00:13:16] Akhil Gonna: Nope, I'm good.
[00:13:21] Dan Duling: Great. Let's review.
[00:13:40] Dan Duling: Okay, great.
[00:13:44] Akhil Gonna: Great work.
[00:13:44] Dan Duling: A Kapil of team on the advertiser report. Bonnie and Kapil excellent job. Ryan is happy.
[00:13:52] Dan Duling: We've Got a path forward and hopefully Kevin releases this to production and we can call this successful in this quarter.
[00:14:00] Dan Duling: So excellent job team. Well done. Great teamwork on the new data ingestions. Yes.
[00:14:07] Dan Duling: These keep happening and that's expected.
[00:14:09] Dan Duling: So great job rallying especially during the sprint to figure out how to get it, what to do and working on those pathways.
[00:14:17] Dan Duling: So well done team. Finalizing naming convention standards we followed in code.
[00:14:23] Dan Duling: Great.
[00:14:24] Bharat Gidwani: And that
[00:14:26] Dan Duling: links to a here and another one that we have down here.
[00:14:36] Dan Duling: So can't remember where the heck was this one.
[00:14:41] Dan Duling: So excellent job team. It's going to go a long way for making sure that whoa.
[00:14:50] Dan Duling: This goes okay.
[00:14:52] Dan Duling: Sorry about that. And kudos to the audience solutions team and consulting and streaming pipelines.
[00:15:00] Dan Duling: Yes. Hopefully we deploy this Sprint and we can call it successful as Q2 deliverable.
[00:15:10] Dan Duling: That should be top of mind of removing any blockers to get this done.
[00:15:13] Dan Duling: So we can call that successful and have a very happy audience solutions and team, including Terry Schulke because he's very invested in audience solutions.
[00:15:25] Dan Duling: So great work, team. I also, I didn't add it, but you're all doing a great job.
[00:15:34] Dan Duling: I know I'm throwing a lot at you with a lot of changes mid Sprint. So please keep up the great work.
[00:15:42] Dan Duling: Everyone's counting on us and you're all doing a great job. So just want to give a kudos to you guys.
[00:15:47] Dan Duling: I'm getting a lot of great feedback and you're doing really well.
[00:15:53] Dan Duling: I want to show you something just so not to scare you all, but I do want to show you the giant list of stuff that is given to us here.
[00:16:11] Dan Duling: Can you all see my screen?
[00:16:13] Dan Duling: Yeah.
[00:16:14] Dan Duling: Okay, so this is a partial list of just the projects that are in our path, how they're linked to the other teams and how many of these all require D E D E D E D E D E D to get done like everything and that doesn't include these and the ones that we've got for our own tech.
[00:16:47] Dan Duling: So we are the glue for all of these things at Fluent across many product lines and many giant things.
[00:16:59] Dan Duling: So keep up the great work. You all are doing really well.
[00:17:05] Dan Duling: We are critical path to Fluent success, so please keep it up.
[00:17:11] Vani Kaithi: Okay.
[00:17:12] Dan Duling: What didn't go well? Tremendous campaign data is still blocking to create gold rewards.
[00:17:19] Dan Duling: Yep.
[00:17:21] Dan Duling: And that's a follow up with jc, correct?
[00:17:25] Akhil Gonna: Yes. Yeah.
[00:17:26] Dan Duling: If I remember correctly.
[00:17:27] Dan Duling: Okay, I'll take that action item.
[00:17:29] Dan Duling: So but hopefully we get that resolved and we can get everything linked out.
[00:17:36] Trevor Anderson: Also, something not mentioned but probably worth noting is open FDSOS on. On this.
[00:17:41] Trevor Anderson: I definitely was a blocker for this sprint, which I'll continue to follow up on with Griggs on mostly revolving around what's going on with audience solutions and our.
[00:17:56] Trevor Anderson: Yeah.
[00:17:56] Dan Duling: Appointments and that is this action item assigned to me which I've got for Q3.
[00:18:06] Dan Duling: I already talked to Dueling and Griggs about this and we'll have a better collaborative pre informed scenario.
[00:18:17] Dan Duling: So Trevor, especially you Kapil and Barat, please, as you do the discovery of the spike items and your tech things, think about DevSecOps.
[00:18:30] Dan Duling: Even if you're like have a slight inkling that they might be involved in some capacity, add them.
[00:18:37] Dan Duling: It's better to take it away rather than add it later.
[00:18:41] Dan Duling: And that's what we had to do for audience solutions.
[00:18:42] Dan Duling: So
[00:18:45] Dan Duling: not they're an on demand service, but their on demand capacity is one body and the other body is helping because not even supposed to.
[00:18:55] Dan Duling: So key things there. But yes, definitely held us back. Still almost there. But we could do better.
[00:19:09] Trevor Anderson: Yeah. And that's probably goes into. How could we do things differently?
[00:19:12] Trevor Anderson: Probably more proactive planning with DevSecOps. Like you mentioned, they're an on demand group.
[00:19:24] Trevor Anderson: But you know, I think we can look at our upcoming items that are going into future sprints and see if there's any future planning that can be done with them.
[00:19:39] Dan Duling: Great. Okay.
[00:19:41] Dan Duling: Do you want me to add another one, Trevor, for how we could do it differently or does that cover this one down here?
[00:19:48] Dan Duling: Oh, that.
[00:19:49] Trevor Anderson: That definitely covers it. Just wanted to mention it, but yeah.
[00:19:52] Dan Duling: Awesome.
[00:19:53] Dan Duling: All right, great job.
[00:19:57] Dan Duling: Okay,
[00:19:59] Dan Duling: welcome to the last sprint.
[00:20:02] Dan Duling: Ish.
[00:20:03] Dan Duling: Slight carryover into the new quarter. Is anybody off starting today through July 3rd?
[00:20:14] Bharat Gidwani: Yeah, Brian, I. I am out
[00:20:19] Dan Duling: of this week. All right, so that's also.
[00:20:24] Akhil Gonna: There is a holiday for Canada, folks, on July 1st.
[00:20:31] Dan Duling: Yes. And the US is out second and third.
[00:20:39] Dan Duling: Oh, very weird. Don't ask me why you guys are. Oh, God.
[00:20:51] Dan Duling: Trevor, go ahead. Who's that?
[00:20:54] Trevor Anderson: That wasn't me.
[00:20:55] Dan Duling: Oh, somebody else, sorry.
[00:20:58] Granth S: No, I'm off on this. This Friday 26th.
[00:21:04] Dan Duling: That's a. And that's about it.
[00:21:06] Trevor Anderson: Grant, can you put that on my calendar?
[00:21:08] Granth S: Yes, I'll put that on the calendar.
[00:21:10] Trevor Anderson: Yeah, it looks like you're out too, Brian.
[00:21:14] Dan Duling: I am out. Yes. But you all don't need me. Yeah, so I'm out Wednesday, Thursday, Friday.
[00:21:23] Dan Duling: I'm back Monday, Tuesday, Wednesday, which you guys are out that Wednesday and then for holiday, Thursday, Friday.
[00:21:32] Dan Duling: So.
[00:21:32] Trevor Anderson: And I'm not out this sprint, but next sprint I'll be out following the week of the 6th. July 6th.
[00:21:42] Trevor Anderson: So that's not this sprint, but next sprint, I'll. I'll remind as we get closer to it.
[00:21:48] Dan Duling: Party.
[00:21:52] Dan Duling: Okay, so as we go through these points, please just let me know if we're over capacity.
[00:22:00] Dan Duling: I think A, we're going to go from 8 to 7 because we're off. Barat, you're off how many days?
[00:22:12] Bharat Gidwani: Yeah, I'm off like two weeks.
[00:22:15] Dan Duling: Okay, great.
[00:22:16] Dan Duling: So nothing for Barat and. Okay, great.
[00:22:24] Dan Duling: Let's get started.
[00:22:25] Dan Duling: So I carried over everything. Let's see
[00:22:33] Dan Duling: here. No,
[00:22:38] Dan Duling: I'm assuming he didn't start on this, given he was blocked. So I'll keep it as a 3. Akil for this one.
[00:22:46] Dan Duling: Still a one once we get unblocked. Or do you need more or unknown?
[00:22:52] Akhil Gonna: Yeah, maybe I do because I need to create new tables for that.
[00:22:56] Dan Duling: Okay, great.
[00:22:58] Trevor Anderson: Also, sorry, Brian Manov. Looks like Manov is out next week. All of next week.
[00:23:03] Dan Duling: Okay, got it.
[00:23:05] Dan Duling: I won't assign anything else to him. So he's got that one story. Okay, great. So this is a two.
[00:23:12] Dan Duling: Joseph, still a one for this.
[00:23:16] Dan Duling: Great.
[00:23:17] Dan Duling: Bonnie. Oh, sorry, Venki,
[00:23:25] Dan Duling: Just one. Great.
[00:23:31] Dan Duling: Trevor, still three on this one.
[00:23:35] Trevor Anderson: Yeah.
[00:23:37] Dan Duling: Okay, great.
[00:23:38] Dan Duling: Feel free to tag that to your new epic that you'll create.
[00:23:41] Dan Duling: Thank you. All right.
[00:23:45] Dan Duling: For this one?
[00:23:46] Akhil Gonna: Yeah. This one. Just one is fine. I only changes the default to null. It's.
[00:23:52] Akhil Gonna: The PR is already raised here.
[00:23:55] Dan Duling: Great. All right.
[00:23:58] Dan Duling: I saw that the Cybage team raised a PR for this, so I don't know what your team PR review is. What?
[00:24:08] Dan Duling: Make sure we look at that closely. Very closely. I'll leave their points untouched. For this one.
[00:24:20] Akhil Gonna: Yeah. This was just Friday.
[00:24:23] Dan Duling: All right, got it.
[00:24:24] Dan Duling: Sorry. So, three, four. All right.
[00:24:27] Dan Duling: That's pretty much it for you.
[00:24:28] Dan Duling: K. Yeah.
[00:24:30] Akhil Gonna: I have one more. I created a bug ticket which is a one story pointer I added just now.
[00:24:39] Dan Duling: What is it? Do you know?
[00:24:40] Akhil Gonna: Yeah, One second. It's 1601.
[00:24:55] Dan Duling: 1601.
[00:24:57] Akhil Gonna: Yeah. 1601. Oh, did I add in some other board? No, I added in keyboard.
[00:25:07] Dan Duling: All right, that's good. That's good. Hold on. Yeah, no, Got it.
[00:25:18] Dan Duling: One second.
[00:25:24] Dan Duling: Yeah. It's weird, right? Okay.
[00:25:28] Akhil Gonna: Yeah, this is the bug which we found on last Friday.
[00:25:36] Dan Duling: Got it. Okay, no problem. Yeah.
[00:25:41] Dan Duling: And that.
[00:25:42] Dan Duling: That'll.
[00:25:43] Dan Duling: You okay to take that on?
[00:25:45] Dan Duling: Yeah, yeah. Okay, great.
[00:25:52] Dan Duling: All right, so, Keely, you're done.
[00:25:55] Dan Duling: All right. Go through this. All right.
[00:26:02] Dan Duling: All right, Manav's got this, which she
[00:26:04] Dan Duling: does not have time to do, so correct to sign this to somebody else. All right?
[00:26:13] Dan Duling: Ingest the netsuite data. Who wants to take on the netsuite data?
[00:26:21] Venkatesh Mannam: Yeah, you can send me.
[00:26:23] Dan Duling: Great. Thanks, Venki.
[00:26:26] Kapil Sreedharan (2): We'll need the API key for that from Jason, right?
[00:26:32] Dan Duling: Yep, yep, yep. All right.
[00:26:36] Dan Duling: Vani, do you want the position view for the advertiser report?
[00:26:39] Vani Kaithi: Yeah.
[00:26:42] Dan Duling: Thank you. All right. All right. So Akil is done.
[00:26:47] Dan Duling: Venki, it's five plus one.
[00:26:53] Dan Duling: Yeah, you're pretty much done. So maybe this bug here, Venki. Maybe. Yeah. All right. All right.
[00:27:04] Dan Duling: So that covers you. Great. See?
[00:27:13] Dan Duling: Oh, yeah.
[00:27:15] Dan Duling: We definitely gotta add this.
[00:27:19] Venkatesh Mannam: Okay.
[00:27:25] Bharat Gidwani: To me. I mean, I don't know if it'll get unblocked by the time I'm heading out, but.
[00:27:33] Bharat Gidwani: Yeah, that might be something that Vani can take a look at.
[00:27:39] Dan Duling: Yeah, I'll move this to the next Sprint, and if it gets unblocked, I'll bring it in.
[00:27:47] Dan Duling: We've got other stuff here.
[00:27:49] Kapil Sreedharan (2): What was blocking it?
[00:27:53] Bharat Gidwani: The tables. The campaign and advertiser tables are not there.
[00:27:59] Bharat Gidwani: And I think campaign does not have tremendous data in it yet, so.
[00:28:03] Kapil Sreedharan (2): Okay.
[00:28:04] Dan Duling: That's right.
[00:28:05] Bharat Gidwani: Plus, there's some calculation things, so I have a draft PR for it already, I think. But.
[00:28:10] Bharat Gidwani: Yeah, it doesn't. It's not complete. Like, it doesn't. It's missing some calculations and whatnot.
[00:28:16] Dan Duling: All right, so, Bonnie, you should be good.
[00:28:18] Dan Duling: Now, you've got your apps flyer data and the position review report.
[00:28:24] Dan Duling: So I think you're good.
[00:28:26] Dan Duling: Venki, you're good. Akil, you're good.
[00:28:29] Dan Duling: Right?
[00:28:29] Dan Duling: You're out.
[00:28:31] Dan Duling: So. Christ. All right, so that means the other squad's gonna have to do this work. All right, so,
[00:28:41] Dan Duling: Trevor, any thought process on how you want this stuff assigned to the Cyber team?
[00:28:52] Trevor Anderson: Yeah, that's actually a good question. I'm not sure.
[00:28:56] Trevor Anderson: I'll bring it up with them tomorrow on how we want to do our triaging and assigning to the.
[00:29:03] Trevor Anderson: To the team, I'm assuming you could probably give it to their lead. They can.
[00:29:08] Dan Duling: Yeah.
[00:29:09] Trevor Anderson: We could talk about it tomorrow.
[00:29:11] Dan Duling: All right, so let's do that. I can't remember who it was, who their lead is. Do you.
[00:29:19] Trevor Anderson: Do you remember Capitol at all them?
[00:29:22] Dan Duling: Thank you.
[00:29:31] Trevor Anderson: I'll just post a note in the. In the chat today, please.
[00:29:35] Dan Duling: All right.
[00:29:36] Dan Duling: Everything's assigned.
[00:29:44] Dan Duling: Good.
[00:29:45] Dan Duling: All right, Kapil And, Trevor, I will be giving you both spikes for discovery, for Q3 work so that you guys could read through the Q3 epics and PRDs and start to draft out how you're going to do your TDD and Discovery actions.
[00:30:10] Dan Duling: So I'll Add those and then add
[00:30:12] Dan Duling: them to the sprint for each of you,
[00:30:17] Dan Duling: if that's okay.
[00:30:19] Dan Duling: Great.
[00:30:19] Dan Duling: Okay. Is it just a particular.
[00:30:27] Trevor Anderson: Are they for all. Is this for all of the Q3 goals? Are they?
[00:30:32] Dan Duling: Well, you won't get through all. You won't get through all of them
[00:30:36] Trevor Anderson: because I know it'll be. Might come later in the.
[00:30:40] Dan Duling: Yeah. So as soon as I've done this, we have time. I'll go through the.
[00:30:44] Dan Duling: The sequence here with everybody.
[00:30:47] Trevor Anderson: Okay.
[00:30:47] Dan Duling: And that'll make.
[00:30:48] Dan Duling: That'll make life a little bit easier. Okay.
[00:30:53] Dan Duling: All right.
[00:30:54] Dan Duling: Did I get everybody? Joseph and Grant, since man of most likely will be out.
[00:31:07] Dan Duling: And Grant, you're off a day and then Joseph, you're only off the holiday. Yes.
[00:31:14] Dan Duling: Okay.
[00:31:15] Dan Duling: You're find that.
[00:31:18] Joseph: What's that? The Manav's tasks to me, because I'm not sure if you can pick that up. Yeah.
[00:31:24] Dan Duling: All right. Thank you, Jess.
[00:31:26] Joseph: And also, I have a validation ticket. I don't see it here for the model audience pipeline.
[00:31:33] Dan Duling: Is it a subtask?
[00:31:35] Joseph: Yeah, it was a subtask.
[00:31:37] Dan Duling: Then it'll be nested inside one of the.
[00:31:39] Joseph: Okay, okay.
[00:31:41] Dan Duling: Stories that were dropped.
[00:31:43] Dan Duling: Okay, great. Okay.
[00:31:49] Joseph: And sorry, how are we planning for Q3 segment registry for audience solution planning?
[00:31:58] Dan Duling: Yeah, we. We should meet this.
[00:32:05] Dan Duling: This Sprint because ideally we'd be picking up and doing that work a little bit more.
[00:32:11] Dan Duling: But we have to finish and deploy our model pipelines thing.
[00:32:18] Dan Duling: So I will schedule a specific audience segment. And audience. I'll just do segments.
[00:32:27] Dan Duling: We'll go through the segments. I consolidated all the epics. There was too many.
[00:32:31] Dan Duling: I made them one big epic. So you guys can go through and we'll do that. I'll include J.C.
[00:32:38] Dan Duling: and Grant and Joseph will. Will go through that with Manav as well.
[00:32:44] Joseph: Okay.
[00:32:45] Dan Duling: So I'll. I'll schedule that for us before we. Before the Sprint end.
[00:32:52] Dan Duling: And then next Sprint, we can either, if we're done early, hooray. We'll pick up the tasks.
[00:32:57] Dan Duling: If we're not done, then we'll have that as our dedicated sprint.
[00:33:01] Dan Duling: Next Sprint.
[00:33:04] Dan Duling: Good question.
[00:33:05] Dan Duling: And then next Sprint, we'll go through the Q3 second OKR for audience solutions, which is the audience solutions user interface, which is a misnomer because it's basically show me everything.
[00:33:21] Dan Duling: So that'll be the other Q3. Q3 deliverable for audience solutions.
[00:33:27] Dan Duling: So I know you guys are light on tasks here, so I'll schedule that this week.
[00:33:36] Dan Duling: So maybe like Tuesday or Wednesday.
[00:33:38] Dan Duling: Joseph.
[00:33:39] Dan Duling: And then that way once you guys are happy and we're ready to Deploy.
[00:33:42] Dan Duling: We can start to pick up those segment stories because Manav has also started that work, so I want to make sure he's here.
[00:33:49] Akhil Gonna: Okay.
[00:33:50] Dan Duling: Does anybody know when he's out? I don't have it on my calendar.
[00:33:57] Trevor Anderson: Who. Who is this?
[00:34:00] Dan Duling: He.
[00:34:01] Trevor Anderson: He's out all of next week.
[00:34:03] Dan Duling: Is he in this week?
[00:34:04] Trevor Anderson: Yes, he's out the 29th. Yeah.
[00:34:07] Dan Duling: Through the third. Got it.
[00:34:09] Dan Duling: Then I will definitely get it done tomorrow.
[00:34:12] Trevor Anderson: Seventh, but yeah, got it.
[00:34:16] Dan Duling: Okay. Any questions on the stuff assigned to the Sprint? Anybody else?
[00:34:25] Dan Duling: Anybody not clear on what we're trying to do?
[00:34:29] Bharat Gidwani: And sorry, there's one more task on the guru board that I'm working on right now with namat.
[00:34:36] Bharat Gidwani: We'll see if I can get that deployed this week. If not, we'll do it once I come back.
[00:34:43] Bharat Gidwani: That's related to the lead manager stuff. So nothing urgent. Tracking the
[00:34:55] Trevor Anderson: referral.
[00:34:56] Dan Duling: Yeah.
[00:34:58] Dan Duling: Oh, this is for the new. Yeah, this is for the. Oh, yeah, yeah. Definitely not required here for us.
[00:35:09] Dan Duling: Super, super nice to have because.
[00:35:14] Dan Duling: Yeah, again.
[00:35:14] Bharat Gidwani: Yeah, I just wanted to let you know that this.
[00:35:16] Bharat Gidwani: I have changes in there, but I'm waiting on something from DevSecOps to even test this in Dev.
[00:35:25] Dan Duling: Do you want me to drop it from the Sprint produce?
[00:35:30] Bharat Gidwani: I right now? Just leave it.
[00:35:33] Dan Duling: Just leave it there.
[00:35:34] Bharat Gidwani: I'll work with Namar to see if he. If he can get it done. If not.
[00:35:39] Venkatesh Mannam: Yeah, I'll drop it.
[00:35:40] Dan Duling: Yeah. Do not stress. No on this one. It's definitely way.
[00:35:45] Dan Duling: Yeah, in it.
[00:35:46] Dan Duling: And we're going faster than the project,
[00:35:50] Dan Duling: so
[00:35:52] Bharat Gidwani: this would at least just preppers for anything like lead manager or anything else that comes in future for Integral.
[00:36:00] Bharat Gidwani: So.
[00:36:00] Dan Duling: Yeah, you're right.
[00:36:00] Bharat Gidwani: This is not. It's not like a urgent thing or anything.
[00:36:04] Dan Duling: Yeah.
[00:36:04] Dan Duling: Thank you. Please don't stress either one of you on that one. Okay, great. Here is.
[00:36:11] Dan Duling: All right, Sprint is started.
[00:36:14] Dan Duling: Great.
[00:36:14] Dan Duling: Any questions? Team on the Sprint.
[00:36:20] Dan Duling: All right, then I'll use the last 15 minutes here to go through Q3 as a plan and then you guys
[00:36:31] Dan Duling: can start to ask me questions on Q3. Yeah.
[00:36:42] Dan Duling: Okay. So there is a lot of work on our plates, as you saw. I'll go through each one.
[00:36:55] Dan Duling: And what do you guys see right now?
[00:37:02] Trevor Anderson: Gantt chart.
[00:37:03] Dan Duling: Great. Okay.
[00:37:05] Dan Duling: So we are basically splitting for visualization purposes are team into multiple swim lanes Core which covers all of the fluent core initiatives.
[00:37:28] Dan Duling: For example, ROAS phase two in store incentives, our identity graph to rewards and our fluent one data ingestion pipeline.
[00:37:41] Dan Duling: That is 80% of our work of our six devs including cybers.
[00:37:49] Dan Duling: Then there's a dedicated lane for reporting that's straight up two devs running the reportings teams asks of Q3.
[00:38:02] Dan Duling: So advertiser report phase two I E the Ryan report CBUR phase two which is the Conlin Report Auction explainer coming from Data Science Marketing Revenue which and CRM integration which is coming from our marketing team and then a whole bunch of make me new metrics for stuff requests.
[00:38:23] Dan Duling: I'm going too fast. I'll circle back but I just want to give you the lates then Audience Solutions.
[00:38:30] Dan Duling: We're dedicating two devs to Audience Solutions to do their priority work. First is the segments.
[00:38:38] Dan Duling: Second is their solutions reporting dashboard which is their fancy word of a ui.
[00:38:46] Dan Duling: Then we've got our tech initiatives which is a protected floor of 20%.
[00:38:53] Dan Duling: So then all the team will be working on these based off of what Trevor wants to do in which sequence.
[00:38:59] Dan Duling: But I use these as a visual representation of those.
[00:39:04] Dan Duling: So every sprint will have some sort of tech related initiatives.
[00:39:10] Dan Duling: What that means is that every sprint is going to have stories across these four lanes.
[00:39:20] Dan Duling: Does that, does that make sense everybody?
[00:39:27] Akhil Gonna: Yes, Brian.
[00:39:28] Dan Duling: Okay.
[00:39:29] Bharat Gidwani: And just one thing that I'm kind of confused.
[00:39:33] Bharat Gidwani: The numbers that you have for devs, that kind of, at least in my mind it doesn't seem to add up.
[00:39:39] Bharat Gidwani: But like if I, if I am having work across all the, all the lanes.
[00:39:47] Dan Duling: Yep.
[00:39:49] Bharat Gidwani: The resource, number of resources that you have here, I mean we don't have 16 devs.
[00:39:56] Bharat Gidwani: Unless I'm missing someone.
[00:39:57] Dan Duling: No, no, no. So that's great question. So it's 6, 7, 8, 9, 10, 10 devs.
[00:40:04] Dan Duling: This is not, this is inclusive of that. So including our cybage team, these are our dev resources.
[00:40:13] Dan Duling: 10 dev resources 18, 20.
[00:40:14] Bharat Gidwani: Okay, never mind.
[00:40:18] Dan Duling: No, it's, it's fine.
[00:40:19] Dan Duling: It's not in as intuitive as read it but I, I added this just so that when I present next week to Don, he wants to know why does this not get priority or that not get priority which then goes into this disaster of.
[00:40:42] Dan Duling: Here is the cross team project sequences as you can see of all of the projects across all of the teams in the sequence that they need to be in based off of those teams.
[00:40:58] Dan Duling: It is crazy how much de work he has we have assigned to us and I've got to give him what the priority sequencing of these things are and what the value propositions of doing each one of these things and which teams are involved in doing these things.
[00:41:21] Dan Duling: Because as you said Barat, we do not have a team of 100 people.
[00:41:28] Dan Duling: So doing all these things is not possible. So someone's going to get a no in one of these things.
[00:41:33] Dan Duling: And Don really has got to have to weigh in as to whether or not we should be doing these things or the scope or priority or whatever it is.
[00:41:44] Dan Duling: So that's the biggest thing here, is the sizing of each one of these things.
[00:41:51] Dan Duling: And how much of a priority is it to fluent? And then how much work does DE have to do to support it?
[00:41:59] Dan Duling: There's a lot. A lot. And he's got to be sequenced in the right order.
[00:42:04] Dan Duling: So as you guys all know, most of our stuff has the right feeding of pulse to us, to ds, to bi.
[00:42:12] Dan Duling: That's the normal sequence. But not all these follow this normal sequence like Roas.
[00:42:16] Dan Duling: Roas started with DE with some pulse work, now des.
[00:42:22] Dan Duling: So this is going to be a lot of work for us as a unit.
[00:42:28] Dan Duling: And there's going to be a lot of moving parts, which is why I've been trying to split it out into some sort of understandable sequence of these things that we're going to try and do across the org and how many devs I can devote to each one of these lanes.
[00:42:53] Dan Duling: Without even going into details about each one of these deliverables. This makes sense.
[00:43:02] Dan Duling: Everybody does. Does anyone have questions on this? Anything that jumps out at them, it's okay to ask.
[00:43:11] Dan Duling: Yeah.
[00:43:12] Akhil Gonna: Also the number of devs which you put in is there like dedicated.
[00:43:16] Akhil Gonna: This specific developer is dedicated to the specific lane or.
[00:43:22] Dan Duling: Good question. No, no, I don't think so.
[00:43:29] Dan Duling: I'll leave that up to Trevor and how you guys want to do that.
[00:43:33] Dan Duling: But
[00:43:35] Dan Duling: we have not been as prescriptive of those kind of pods, Akhil.
[00:43:41] Dan Duling: And given that we've got a lot of things, it might be a project pod, could be an option like, hey, I'm doing this thing, I'm going to work on all the things inside this thing.
[00:43:53] Dan Duling: Or you guys can just grab all the things you want. But there is a lot.
[00:43:58] Dan Duling: So context switching across this list is a real thing.
[00:44:03] Dan Duling: So however you all want to assemble from an Avengers standpoint here, you know, you could all get your own Avengers name and then figure out how you're going to tackle these things.
[00:44:14] Dan Duling: I'm all for it. There's no wrong answer. But the work is placed on us here, squad.
[00:44:21] Dan Duling: And Don is expecting us to deliver. So how you want to do it? We get to decide, but we gotta do it.
[00:44:29] Akhil Gonna: Okay.
[00:44:30] Dan Duling: Yeah.
[00:44:30] Kapil Sreedharan (2): Is this gonna be an epic for each of these, like option?
[00:44:34] Dan Duling: Oh, yes, yes.
[00:44:36] Kapil Sreedharan (2): One Parent epic. And then there'll be like. Or is there one PID for across.
[00:44:44] Dan Duling: There's one PRD for each one of these deliverables, each one of these projects. That is correct.
[00:44:50] Kapil Sreedharan (2): Like deprd or. It's a.
[00:44:53] Dan Duling: It'll be a. A single PRD and it will include all of the sub teams that need to do their work.
[00:45:02] Dan Duling: So there should only be one PRD for projects.
[00:45:06] Dan Duling: So as an example for incentives, Aaron's got the PRD for incentives.
[00:45:11] Dan Duling: We will add our piece once we know what the hell that piece is. So will DS and bi.
[00:45:19] Dan Duling: Same for in store, same for, you know, like identity graph. You know, I write roas, I wrote cbor.
[00:45:27] Dan Duling: I wrote Segment Registry. We Dan wrote Advertiser report. I wrote Auction explainer. DS wrote.
[00:45:36] Dan Duling: Each one of these has their own prd. One prd.
[00:45:40] Dan Duling: And there should be one TDD to go with that Bharat, Good questions.
[00:45:49] Kapil Sreedharan (2): So is this one epic and we'll have no you.
[00:45:54] Dan Duling: I will be writing a single epic. But that's a good question. Here, I'll show you what I did.
[00:46:00] Kapil Sreedharan (2): But would that epic apply for all the teams and then.
[00:46:04] Dan Duling: No, no. Each team has their own JIRA board with their own epics.
[00:46:14] Kapil Sreedharan (2): Like, you know, the B. The BI team will be dependent on some of our.
[00:46:19] Dan Duling: Absolutely. Absolutely.
[00:46:21] Kapil Sreedharan (2): How would they track it?
[00:46:23] Dan Duling: So inside. Good question. So inside the pulse, basically, this is our idea board.
[00:46:31] Dan Duling: Each one of these has the ability to link across multiple delivery lanes.
[00:46:38] Dan Duling: So they can track and they can. We also added labels.
[00:46:42] Dan Duling: So for those of you who have been working on the BI stuff or advertising report.
[00:46:49] Dan Duling: Where is it Here?
[00:46:53] Dan Duling: All the stories have a label that the BI team uses to direct to their stuff so they know what's in flight, what's linked.
[00:47:03] Dan Duling: And JIRA lets us cross link across epics and projects.
[00:47:10] Dan Duling: Good question.
[00:47:15] Dan Duling: What other questions do you guys have?
[00:47:17] Kapil Sreedharan (2): So every task we have, there'll be a label that would point to this idea board.
[00:47:25] Dan Duling: Well, not the label. I've already linked all the epics to each one of those boards. So that's.
[00:47:34] Dan Duling: That'll be inside here. Each one of these PD boards has a PRD associated with it.
[00:47:40] Dan Duling: And each one of those has, whether it's driven by a PRD or an MPO or a jira.
[00:47:47] Dan Duling: And each one of our product managers has their own list here. So Karen's got hers. Here's my prd.
[00:47:55] Dan Duling: She's linked here.
[00:47:57] Dan Duling: She's linked her item and then she'll link to my item so that we're all clear on what the Deliverable is.
[00:48:07] Dan Duling: Good.
[00:48:07] Dan Duling: Questions.
[00:48:08] Dan Duling: Okay, lots of work.
[00:48:11] Kapil Sreedharan (2): One more. Now, what about these ad hoc requests or some new initiative came in.
[00:48:18] Kapil Sreedharan (2): Would that also go here? And that's not part of the Q3.
[00:48:23] Dan Duling: Yes. So like for example, where did I have. I have individual reporting tasks as a.
[00:48:36] Dan Duling: As a question, as a thing, a bucket.
[00:48:39] Dan Duling: It'll be non initiative work and it will be assigned to reporting because it came in as a request of like, oh, I want to add in country to the metrics so that I can report off of them.
[00:48:56] Dan Duling: Well, in order to do that they need de work.
[00:48:59] Dan Duling: So we have our little bucket of reporting requests that we will support and that will be assigned to that epic.
[00:49:07] Dan Duling: Every one of these. If you guys add new epics, I'm okay with it.
[00:49:10] Dan Duling: I just need to know so I can link it back to its parent thing.
[00:49:15] Dan Duling: So you do not have to only stick with my one epic.
[00:49:18] Dan Duling: But I will be creating a single EPIC for every initiative and then all of you will be creating the stories to drive off of that.
[00:49:28] Dan Duling: I won't be creating stories anymore.
[00:49:46] Dan Duling: The other question.
[00:49:47] Dan Duling: I'll be scheduling time for us to review the Q3 priority once I actually have Q3 priority.
[00:49:55] Dan Duling: And I'll go through each individual item with all of you. So don't worry.
[00:50:04] Dan Duling: Okay.
[00:50:06] Dan Duling: Any other questions? If you do. If you want to talk to me individually, you know where to find me.
[00:50:14] Dan Duling: Otherwise, Trevor, I'll follow up with DevSecOps and JC on those open blockers.
[00:50:22] Dan Duling: You all that just keep doing what you're doing.
[00:50:27] Granth S: Brian.
[00:50:28] Dan Duling: Yes.
[00:50:29] Granth S: Call up before for the FDS of 489. Greg's already responded to that.
[00:50:35] Granth S: Sorry, that went to the other items on Outlook. So I'll just take a look at that.
[00:50:40] Granth S: I'll just take a look at his comments.
[00:50:42] Dan Duling: Great. All right.
[00:50:45] Dan Duling: Awesome.
[00:50:46] Trevor Anderson: Was for regarding the production catalog. Okay.
[00:50:53] Trevor Anderson: Did he say it was complete and needs to be tested or what was the
[00:51:00] Granth S: just still in pending. You just need some confirmation from me.
[00:51:07] Granth S: So I'll just take a look at that on his end. It looks like everything's done.
[00:51:12] Dan Duling: So wait.
[00:51:16] Granth S: I just wanted to give you a heads up Brian on that.
[00:51:19] Dan Duling: Great.
[00:51:21] Dan Duling: Joseph, I know Manav was also blocked on an fdso, so you might want to follow up with him on 1342, the one that we just assigned to you.
[00:51:31] Joseph: Sure.
[00:51:32] Dan Duling: I don't know the status of that
[00:51:33] Dan Duling: fdso, but I will follow up. Okay. Okay.
[00:51:39] Dan Duling: Anybody else? All right, thank you all. Enjoy your day. If I don't talk to you. Thank you, Dean.
[00:51:50] Trevor Anderson: Thanks everyone.
