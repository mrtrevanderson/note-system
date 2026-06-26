---
title: "Granth x Trevor Sync"
meeting_id: "040000008200E00074C5B7101A82E00807EA0619545A90461BE2DC010000000000000000100000004A76EF7A808C804FBE50FCD94042134E"
note_id: 80907427
recording_id: 15097608
date: 2026-06-25
start_time: "2026-06-25T12:00:00-04:00"
end_time: "2026-06-25T12:30:00-04:00"
url: "https://fluentco.fellow.app/meetings/040000008200E00074C5B7101A82E00807EA0619545A90461BE2DC010000000000000000100000004A76EF7A808C804FBE50FCD94042134E/"
participants:
  - name: "Trevor Anderson"
    email: "tanderson@fluentco.com"
    rsvp: accepted
    organizer: true
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
[ ] Granth S to document the specific tables required by the analytics team (Virginia, Monisha, new account manager) for Audience Solutions analytical work and add to the governance documentation.
[ ] Granth S to complete the weekly Lattice update before end of day Friday.

## AI Summary

Trevor and Granth held their weekly sync to discuss **data governance improvements** and **catalog structure reorganization**. They decided to implement a clean medallion architecture in the Audience Solutions prod catalog, with only a **gold schema containing three core tables** (marketable users, survey summary, and survey event), rather than replicating the cluttered dev environment. Trevor is establishing an Audience Solutions Analyst persona group for controlled data access and asked Granth to document the specific tables required for analytics work. They also addressed technical issues including AWS instance type selection (settling on R5 instances) and Photon enablement concerns, while successfully testing the model audience pipeline. The team will keep the Audience Solutions Jira board separate from the DE board, with Manav working on restructuring the intake form template.

### Chapters

#### Data Governance and Catalog Structure
- Trevor is creating an **Audience Solutions Analyst persona group** as part of a new governance protocol to control access to schemas and tables required for analytical work, starting with Playful Rewards Prod gold schema since Audience Solutions prod is currently empty. *(0:05:20)*
- Current access patterns are problematic, with analysts like Virginia sometimes using **incorrect tables or sandbox tables for analytics**. The goal is to provision access only to gold schemas as the final data layer, though silver access is temporarily needed during migration. *(0:08:48)*
- For the model audience catalog, only **two schemas are actively needed**: malleable universe and master marketable (containing marketable users, survey event, and survey summary tables), with the rest being deprecated noise that should be cleaned up. *(0:10:04)*
- The marketable users table (built from Lead Manager, FIG identity graph, and demographic data) should remain in the Audience Solutions hub catalog rather than Lead Manager, since it combines **multiple data sources** and will eventually include Playful and other datasets. *(0:15:05)*
- For prod deployment, the team will use the **proper medallion architecture** with a gold schema rather than copying over all the dev schemas. An audit revealed many unused schemas (Master Insights, Backup, Sandbox) with no query history or lineage that can be dropped immediately. *(0:17:03)*
- Granth will document the complete list of tables needed by the analytics team to fulfill analytical work requirements, which Trevor needs when setting up permissions for the new Persona group being created with David. *(0:20:21)*

#### Technical Infrastructure and Pipeline
- Discussion about AWS instance types for Databricks clusters, with Trevor recommending **R5 or R7 instances** (memory-optimized) over M-series (general purpose), ultimately settling on R5 large for the cluster configuration. *(0:00:00)*
- Trevor identified that many jobs have **Photon enabled unnecessarily**, which doesn't provide value in most use cases and should be disabled to avoid unnecessary compute costs. *(0:04:02)*
- The model audience pipeline successfully progressed through validation after a code merge, with **Phase 1 running successfully** including the ramp functionality, though the team was still monitoring the CSV write step (Step 4). *(0:01:18)*
- The marketable users, survey event, and survey summary tables are built from the **JC Subpoena(?) pipeline**, which processes Lead Manager data through bronze and silver layers, and also incorporates FIG (identity graph), demographic data, and as-owned-and-operated external tables. *(0:13:27)*

#### Team Operations and Planning
- Manav is working on a **structured template for the Audience Solutions Jira intake form** to better organize the currently disorganized board and standardize requests for declared and modeled audiences. *(0:21:28)*
- The Audience Solutions tickets will remain on a **separate board from the DE board** despite Dan's suggestion to combine them, as most tickets are segment creation work that should be handled by account managers rather than appearing in engineering standups. *(0:22:35)*
- Granth is taking Friday off (June 26) to enjoy summer but will return Monday, with Trevor requesting the weekly Lattice update be completed before end of day. *(0:23:38)*
- Trevor and Granth plan to schedule time next week to **review the model audience Python code** in detail now that it's working, allowing Trevor to familiarize himself with the actual implementation beyond the initial conceptual walkthrough. *(0:24:15)*

### Action Items
- Granth S: Granth S to document the specific tables required by the analytics team (Virginia, Monisha, new account manager) for Audience Solutions analytical work and add to the governance documentation.
- Granth S: Granth S to complete the weekly Lattice update before end of day Friday.

### Decisions
- The Audience Solutions prod catalog will follow the **intended medallion architecture design** rather than replicating the mixed schemas from dev.
- Audience Solutions prod will contain **only a gold schema** with three actively-used tables: marketable users, survey summary, and survey event.
- The Audience Solutions Jira board will **remain separate from the DE board**, as segment creation work will transition to account managers rather than engineers.

## Transcript

[00:00:00] Trevor Anderson: Yeah, the r5.
[00:00:03] Granth S: Okay.
[00:00:03] Trevor Anderson: Oh, that uses r7.
[00:00:06] Granth S: Okay. Ip1. I can. Okay. I can.
[00:00:14] Trevor Anderson: Probably just use the R5s or R7.
[00:00:18] Trevor Anderson: I think the R classification is like the memory optimized ones, but I'm not sure what the M classifications for.
[00:00:27] Granth S: Okay. So I mean usually it's for like general purpose. It's just. Yeah, it's small cluster. So.
[00:00:35] Granth S: But okay, if you say if you want me to use our files like bigger compute, then sure I can.
[00:00:42] Trevor Anderson: Well, no, no, not bigger, just a different compute type. You could just use a regular.
[00:00:46] Trevor Anderson: Is it just an extra large or something like that? Yeah, yeah. Is that the smallest one on the R's?
[00:00:51] Trevor Anderson: I think so.
[00:01:04] Trevor Anderson: Where's the list? Come on. Databricks. Yeah, I go to. I just go and create a drawing.
[00:01:18] Granth S: Okay. So okay, sweet. So he's just saying it got merged and he's asked me to retry it.
[00:01:26] Granth S: I mean maybe that's not even my
[00:01:28] Trevor Anderson: chest workout because it didn't get to it yet. Oh, it just started.
[00:01:31] Granth S: He did. Yeah, he got it.
[00:01:33] Trevor Anderson: So there we go. It just started.
[00:01:36] Granth S: Yeah, so he's saying. Yeah, they just merged it. So I'll have to give it a. Might work this time.
[00:01:48] Granth S: It might just.
[00:01:51] Trevor Anderson: It's doing it right now. No structure or is it.
[00:01:55] Granth S: Is it fast? Yeah. Yep. I think that work. Yep. Sweet.
[00:02:04] Trevor Anderson: I don't think it has tried to write yet. Right. That's the last part of this, I think.
[00:02:09] Granth S: Try validate. Okay.
[00:02:19] Trevor Anderson: What is write CSV Step four. That's where it needs it, right?
[00:02:25] Granth S: Yes. Okay, so we're still waiting on that one. Okay.
[00:03:07] Trevor Anderson: Yeah, you could just use this one R5 large.
[00:03:10] Granth S: Okay.
[00:03:11] Trevor Anderson: The MD ones. General purpose.
[00:03:17] Granth S: Yeah,
[00:03:27] Trevor Anderson: Yeah, just keep it, keep it with the.
[00:03:35] Trevor Anderson: So it's like the R7, just a new version of the memory optimized.
[00:03:39] Trevor Anderson: I don't even see it depends on the sizing. And just used the R5 large. And. I don't know that everyone.
[00:04:02] Trevor Anderson: I've been seeing this and I need to look into it, but I think a lot of jobs have photon enabled, which I don't even know why we're using photon in a lot of cases.
[00:04:14] Trevor Anderson: It doesn't seem like it should actually going to be doing anything.
[00:04:21] Trevor Anderson: So probably just make sure that that's checked off or not using it. You're not using it in this one.
[00:04:28] Granth S: Right.
[00:04:30] Trevor Anderson: So that's good. You didn't check it, but yeah, just don't use photon.
[00:04:33] Granth S: Yeah.
[00:04:41] Trevor Anderson: Well, hopefully this works.
[00:04:45] Granth S: I'm assuming crossing my fingers. Yeah, well, because.
[00:04:51] Granth S: Okay, so I can see from the code that he merged in.
[00:04:57] Granth S: So, yeah, that has the correct Granth So I'm assuming there's a bit of a delay. There could be.
[00:05:04] Granth S: There might not be. Oh, yeah. Let's keep our famous box. That's the only thing I can say.
[00:05:14] Trevor Anderson: Okay. A few questions that I have for you.
[00:05:20] Trevor Anderson: So, yes, I'm creating a Persona group for Audience Solutions per new governance protocol that I'm trying to set up.
[00:05:35] Trevor Anderson: And so this Persona group would be like Audience Solutions Analyst, and it would have Granth to all the tables that are required or schemas that are required to do analytical work on Audience Solutions assets.
[00:05:52] Trevor Anderson: I'm just going to call it Data Assets.
[00:05:56] Trevor Anderson: Now I need to know, like, what specific schemas should this group have access to?
[00:06:04] Trevor Anderson: And right now it's a little bit confusing. Like, you know, we don't have everything in.
[00:06:10] Trevor Anderson: In the prod catalogs.
[00:06:12] Granth S: Right.
[00:06:13] Trevor Anderson: Like I look here, Auditing Solutions Prod. There's. There's nothing in here.
[00:06:18] Granth S: Audience Solutions. Well, because that's. I think. Well, that's because it's a new.
[00:06:27] Granth S: Still a new workspace. Right. So I'm assuming it's still.
[00:06:32] Granth S: Because that's what the ticket is for the migration purposes. I gotta.
[00:06:36] Trevor Anderson: Yeah.
[00:06:37] Granth S: So yeah. What goes into. So for.
[00:06:39] Trevor Anderson: Right. So for right now, I'll have to probably do something. The dev catalogs, right?
[00:06:46] Granth S: Yes.
[00:06:47] Trevor Anderson: But for playful. Playful should be good. I could do prod. Right. Okay. So I kind of just need to know.
[00:06:56] Trevor Anderson: So, like. So like, should they have access to the gold schema and Playful Rewards Prod.
[00:07:03] Trevor Anderson: Right, that should. That should be one.
[00:07:08] Granth S: And Playful Rewards.
[00:07:10] Trevor Anderson: Okay. Because Monisha, I think Monisha was it Monisha.
[00:07:14] Granth S: No, it wasn't.
[00:07:15] Trevor Anderson: Yeah, she asked me. She asked me, like, I need access to something.
[00:07:20] Granth S: Okay. I remember the. Yeah, I remember the message you had asked us.
[00:07:24] Trevor Anderson: And I forget what she was asking access to. Let me pull that up. I have a ticket. I have a ticket.
[00:07:45] Trevor Anderson: This table. Damn.
[00:07:46] Granth S: Okay,
[00:07:49] Trevor Anderson: look at this ticket. We need, like, I need to set up required fields.
[00:07:55] Trevor Anderson: Not even asking for, like a specific
[00:07:58] Granth S: thing, just the entire thing. Mate. Okay.
[00:08:05] Trevor Anderson: I'm not sure what she's looking for in this catalog. Do you have any idea?
[00:08:13] Trevor Anderson: She's looking to create a modeled audience. So I'm not sure what table she would use to.
[00:08:18] Trevor Anderson: To do that because I know there
[00:08:21] Granth S: was a discussion going on with Virginia. I'm probably using Playful Rewards.
[00:08:29] Granth S: Yeah, I'm drawing a blank on this one. Yeah. Yeah, we'll have to ask.
[00:08:42] Trevor Anderson: So what I want to do, though, the. The. The goal is to have go you know, per the.
[00:08:48] Trevor Anderson: The new naming conventions is to say we only want to provision access to gold schemas. Correct.
[00:08:54] Trevor Anderson: So like that should be our final data. Like it right now doesn't look to be the case.
[00:09:01] Trevor Anderson: Like all this stuff. I'm not sure this is all. All in silver right now.
[00:09:05] Trevor Anderson: Like, I'm not sure if the plan is to have a gold version of this. Right.
[00:09:14] Trevor Anderson: But for right now, I probably have to give access to silver and gold. Right.
[00:09:18] Trevor Anderson: But ideally it would just be gold and then she probably needs stuff.
[00:09:20] Trevor Anderson: Or this group also probably needs access
[00:09:23] Granth S: to this catalog or model audience. Oh, okay. For. Okay. Because I. Sorry. Okay.
[00:09:29] Granth S: I thought you were pointing to as owned and operated.
[00:09:32] Trevor Anderson: So I don't. Maybe this. This one too for now.
[00:09:36] Trevor Anderson: Like, I don't know if there's anything that needs to be in here for model audience.
[00:09:40] Granth S: No, they don't need access to that even.
[00:09:45] Granth S: Honestly, there's a bit of, like I said, the deprecation or that in here.
[00:09:50] Trevor Anderson: In here. Like, there's a lot of junk too, I'm assuming. Right.
[00:09:53] Granth S: Yeah. That will be cleaned up.
[00:09:56] Trevor Anderson: So for right now is do I need to give access to the whole catalog or is only certain of these being used right now?
[00:10:03] Trevor Anderson: Right.
[00:10:04] Granth S: Right. For now, I think just malleable universe and master marketable should be good rest.
[00:10:10] Granth S: I don't think she needs. Okay.
[00:10:14] Trevor Anderson: So what I want is like, it to be. Yeah. Cleaner on their end.
[00:10:19] Trevor Anderson: So like I've seen a couple times, Virginia is like, I'm looking at this table and anyways, it's the wrong table that she shouldn't be looking at.
[00:10:28] Trevor Anderson: Right. This should just be easy.
[00:10:30] Trevor Anderson: There should be like we should just have the final version of the data. Right?
[00:10:35] Granth S: Yeah.
[00:10:36] Granth S: For example, I got to know like, I think yesterday or day before yesterday, there's a table and sandbox that she uses for analytics purposes.
[00:10:45] Granth S: I'm like, what?
[00:10:47] Trevor Anderson: Exactly.
[00:10:48] Granth S: That makes no sense.
[00:10:50] Trevor Anderson: But it's just confusing on their end because they have.
[00:10:53] Trevor Anderson: They see all this and they're like, well, you know.
[00:10:56] Granth S: Exactly. Okay. Yeah. Yeah. Okay.
[00:11:01] Trevor Anderson: So I'm gonna do that for now, but we'll want to probably clean this up.
[00:11:04] Trevor Anderson: Like what I was just talking about was with Manav was like gold marketable users and survey answers or survey summary survey vendor.
[00:11:19] Trevor Anderson: One of these are the two that we actually use right now for modeled for model audience.
[00:11:26] Granth S: We use user. I think we use survey event and survey summary. Or I think we use question answers. Okay.
[00:11:38] Granth S: And other thing also just to let you know, phase one ran successfully. So where fast it has the Ramp.
[00:11:48] Granth S: I just saw that. So really, I. I know. Hopefully now this, this goes all the way.
[00:11:57] Trevor Anderson: That's really cool.
[00:11:58] Granth S: Okay, so.
[00:12:11] Granth S: Okay, so the Master Market Gold, my credible survey event.
[00:12:16] Granth S: So they use survey event, Marketable users and service summary. I think. Yeah, it's just. Yeah.
[00:12:30] Granth S: Only these three should be good.
[00:12:33] Trevor Anderson: Yeah. But the. So what are these built off of right now? Like how are these tables created?
[00:12:40] Granth S: These all come from JC Subpoena. From what? Last I know.
[00:12:50] Trevor Anderson: So are these Lead Manager only data or do we join two other sources to create these?
[00:13:02] Granth S: So for example, let me just share my screen. Let me know if you can see my screen switching.
[00:13:14] Trevor Anderson: Switching. No, try again.
[00:13:27] Granth S: Okay, so this is JC Zatina.
[00:13:31] Granth S: So survey demographic, tbase id, survey questions, survey answer, Bronze silver.
[00:13:41] Granth S: And then these all come to this, the marketable users, Marketable survey event, Question answers and question and survey summary.
[00:13:51] Granth S: So to me, question a survey summary is just question answers and server. Then let's.
[00:14:00] Granth S: So yeah, it is Lead Manager Pipeline Pipeline and aspect pipeline.
[00:14:07] Trevor Anderson: All right, so it uses fig. Okay.
[00:14:09] Granth S: Yeah.
[00:14:11] Trevor Anderson: And so big uses fig. FIG is built off Lead manager too though. No. Or does it. Does FIG use other stuff?
[00:14:27] Trevor Anderson: I thought FIG was just Lead Manager Identity graph. Basically,
[00:14:33] Granth S: as far as I remember, Identity Wallet, this is demographic as owned and operated external tables.
[00:14:48] Trevor Anderson: Okay. My question is, should this be in Lead Manager catalog at this table,
[00:14:55] Granth S: the marketable users?
[00:14:58] Trevor Anderson: Yeah. Marketable users. Yeah. Like per the hub in, you know, hub and spoke design.
[00:15:05] Trevor Anderson: Like our, our goal is to say, well, if this is a table that's originating from the source and it does not include any other data, it should just live in the catalog in which it is coming from.
[00:15:17] Trevor Anderson: But if it's combining data from multiple sources, then it would belong in like an Audience Solutions Hub catalog.
[00:15:25] Trevor Anderson: Like how it is now.
[00:15:28] Granth S: So you're saying we don't need. Okay, so right now it's.
[00:15:32] Granth S: You're saying it's not in the correct catalog.
[00:15:35] Trevor Anderson: All right, well, it might be, but I need to, I'm trying to figure out if it's, you know, combining data from other sources or in the future if it's going to be combining data from multiple sources to create a marketable universe.
[00:15:48] Trevor Anderson: I feel like the answer is later because we're saying, oh, we want to include playful.
[00:15:53] Granth S: Yeah.
[00:15:53] Trevor Anderson: In the. In this marketable users. We want to include, you know, other stuff as well.
[00:15:58] Trevor Anderson: So I mean we can keep it in, we can keep it in Audience Solutions catalog. And that's a hub, right?
[00:16:06] Trevor Anderson: There's two hubs, Fluent and Audience Solutions.
[00:16:09] Granth S: Correct.
[00:16:10] Trevor Anderson: Right. Audience Solutions is For the Audience Solutions line of business.
[00:16:16] Granth S: Right.
[00:16:17] Trevor Anderson: And but we'll want it in medallion. Like right now it says oh gold marketable users.
[00:16:25] Trevor Anderson: We need a gold schema with we need users events, yada yada.
[00:16:32] Trevor Anderson: And that gold schema that would have it basically be the same tables, just the design is different.
[00:16:39] Granth S: Okay.
[00:16:43] Trevor Anderson: So we'll need to think about that a little bit like and planned it out for this quarter to make that change because it's all over.
[00:16:50] Trevor Anderson: It's a little bit of a mix up right now.
[00:16:53] Granth S: Yeah. Okay.
[00:16:55] Trevor Anderson: Whatever's going in the prod, I want to make sure because prod is empty right now, it's clean. Right.
[00:16:59] Trevor Anderson: I don't want all these schemas from dev to show up in prod.
[00:17:03] Trevor Anderson: Like I'd rather have us use the intended design and move forward with that.
[00:17:08] Granth S: Nope, makes sense.
[00:17:09] Trevor Anderson: So yeah, at a minimum let's make sure that these are not going into prod. Like all these schemas.
[00:17:16] Trevor Anderson: No, definitely.
[00:17:17] Granth S: I mean again like I said, we don't need if going forward if we're going from the. If you're.
[00:17:24] Granth S: And if you're creating audience solutions analysts, we don't need that noise to be in there.
[00:17:29] Trevor Anderson: Yes.
[00:17:30] Granth S: So now I agree with you. I agree with you.
[00:17:36] Trevor Anderson: So for audience solutions prod, right now we probably just want a gold schema in there.
[00:17:41] Granth S: Right.
[00:17:41] Trevor Anderson: And then we could probably move like the tables that we're using into that schema like users marketable users and marketable survey summary and survey event.
[00:18:00] Trevor Anderson: Right, right into that schema and that's all we would need to have in there.
[00:18:04] Trevor Anderson: Unless there's other stuff too. Right.
[00:18:06] Granth S: I don't see the need to be on other stuff. Yeah.
[00:18:11] Granth S: Because those are the three that they actively use or we've actively used. So rest, I think.
[00:18:19] Granth S: And that's just noise. Yeah.
[00:18:21] Granth S: Previous, like I said six months ago, this was all just plugging holes everywhere. So.
[00:18:27] Trevor Anderson: Yeah, yeah, I get that.
[00:18:30] Granth S: So yeah, no, I agree with you. Let's. We'll just have what's needed and broad rest.
[00:18:38] Granth S: Don't need to be there.
[00:18:39] Trevor Anderson: And you did a bit of an audit on this already, right?
[00:18:43] Granth S: Yes.
[00:18:43] Trevor Anderson: Okay.
[00:18:47] Granth S: Some of the schemas, for example in Audience Solutions Dev you go to like for example you took Master Insights, Backup Sandbox, all of these.
[00:19:00] Granth S: Nobody's ever queried them. There's no lineage whatsoever. It's just unnecessary noise basically.
[00:19:10] Granth S: So those can, we can drop them. Right. Right off the bat. We don't need to do anything with them. Yeah.
[00:19:20] Trevor Anderson: I'll take a look again at that doc. And just see again.
[00:19:24] Granth S: But yeah, just in that doc, I'll just make a change about adding a analytics schema to.
[00:19:36] Granth S: Or I guess this analytics schema is.
[00:19:39] Trevor Anderson: No, not a schema, just the group.
[00:19:41] Granth S: Oh, sorry, the group. Yeah,
[00:19:44] Trevor Anderson: yeah, that's in the group would have access to. Right. This schema or this.
[00:19:51] Trevor Anderson: To read from this schema or this schema.
[00:19:53] Granth S: Right, right. Because the.
[00:19:57] Granth S: I guess the Annette, the table that Virginia uses for analyzing in the sandbox.
[00:20:07] Trevor Anderson: You know what, that would actually be really helpful if you could just like for audience solution data assets, like what is required from the team from Virginia Monisha, this new account person that's coming in.
[00:20:21] Trevor Anderson: Right. Like what tables do they need to fulfill the requirements of just doing analytical work for.
[00:20:32] Trevor Anderson: For this line of business? Okay, you just throw them in there.
[00:20:37] Granth S: Okay, I'll. Okay, I'll get that in the doc as well.
[00:20:42] Trevor Anderson: I will need that when I get to. When I finally get this Persona group. I'm working with David on it.
[00:20:48] Trevor Anderson: When I get it created. Yeah, I will. That will be.
[00:20:51] Trevor Anderson: My next step is actually granting the permissions to the. To those tables.
[00:20:58] Trevor Anderson: So if you could do that in the meantime, that would be really helpful.
[00:21:02] Granth S: No, for sure. I'll get on that as well. I can scope that out in this ticket.
[00:21:09] Trevor Anderson: Yeah. Do you need a ticket for that?
[00:21:11] Granth S: No, no, I can add it to 16 as well as it's just a part of it, so no need to create another.
[00:21:17] Granth S: I'll just tackle it in 16 to it.
[00:21:24] Trevor Anderson: I just created a new ticket for Manav, which. Which was.
[00:21:28] Trevor Anderson: He's going to work on a structured template for the audience solutions. Jira Intake.
[00:21:34] Granth S: Yeah. Okay. Oh, okay. Yeah. For declared and model.
[00:21:38] Trevor Anderson: Yes. Because right now there's. That board is just like all over the place. So we.
[00:21:44] Trevor Anderson: I was like, let's start with a. Let's redesign what we want the intake form to look like.
[00:21:52] Trevor Anderson: And then I could figure out. I was trying to figure out who the owner of the space was and
[00:21:57] Granth S: who manages it, but I think previously Dan was mentioning something about, you know, like combining basically.
[00:22:05] Granth S: All right. In the DE board, he wanted to show the audience solution tickets as well.
[00:22:12] Granth S: Because, for example, sometimes, you know, Manav in the standup says, or Manava.
[00:22:16] Granth S: For me, we just say, like, we're working on some tickets in the audience solutions pool.
[00:22:21] Granth S: And then for you or Brian or anybody won't really have. Won't really know what's happening. So.
[00:22:29] Trevor Anderson: Yeah, I don't. No, no.
[00:22:33] Granth S: Do you want to keep that separate?
[00:22:35] Trevor Anderson: Well, because most of the tickets are like create segments Right, right.
[00:22:39] Granth S: But then that's the thing. If you want. If you don't want.
[00:22:43] Trevor Anderson: I don't really want that visibility. Like.
[00:22:46] Granth S: Okay.
[00:22:46] Trevor Anderson: I want to actually keep it separate. Like, because moving forward, this isn't.
[00:22:51] Trevor Anderson: You're not going to be saying that. Like, you're gonna. It should just be dev work, you know, like.
[00:22:56] Granth S: Yeah, okay, makes sense. Because that board should be handled by the account manager and not.
[00:23:00] Trevor Anderson: Exactly. Exactly.
[00:23:01] Granth S: Okay, that makes sense. That.
[00:23:03] Trevor Anderson: Yeah. So right now you might.
[00:23:04] Trevor Anderson: We might be saying, oh, yeah, we're manually creating these, but like, hopefully sometime in this quarter that that won't be happening anymore.
[00:23:14] Granth S: Right. No, makes sense.
[00:23:17] Trevor Anderson: Okay.
[00:23:18] Granth S: Okay.
[00:23:19] Trevor Anderson: I was like, ah, don't. We don't want those tickets anymore. Very on the D board. It's already enough.
[00:23:30] Granth S: Yeah. No, okay, makes sense.
[00:23:35] Trevor Anderson: You're out tomorrow.
[00:23:38] Granth S: Yes.
[00:23:40] Trevor Anderson: Just tomorrow.
[00:23:41] Granth S: Just tomorrow. Back on Monday.
[00:23:44] Trevor Anderson: Okay.
[00:23:45] Granth S: Enjoying the summer as much as possible.
[00:23:48] Trevor Anderson: Yes. All right, well, hope you have a nice long weekend.
[00:23:53] Trevor Anderson: One thing that you could do before you sign off today is if you could do the weekly.
[00:23:59] Granth S: Oh, yes, Lattice. Yes.
[00:24:01] Trevor Anderson: Weekly lattice update. Yeah. Before you're out this weekend and.
[00:24:07] Granth S: Yeah, sounds good.
[00:24:10] Trevor Anderson: Anything else? I'm glad that this. Next week. Let's go through. It looks like this is all working.
[00:24:15] Trevor Anderson: The model, the audience Python.
[00:24:17] Trevor Anderson: If we can set some time next week to actually go through the code, I want to learn.
[00:24:23] Trevor Anderson: I just want to familiarize myself more with the actual code itself.
[00:24:28] Granth S: Yeah, for sure.
[00:24:29] Trevor Anderson: And the steps that are happening.
[00:24:31] Trevor Anderson: I know you did the initial walkthrough without the code and everything, but now I kind of want to.
[00:24:36] Trevor Anderson: It's working, so I want to go through it.
[00:24:38] Granth S: Okay, sounds good. Hopefully before I sign out, we're able to go through all the phases.
[00:24:46] Granth S: Yeah, I'll keep an eye out and update you accordingly. Awesome. Okay, let's go travel.
[00:24:55] Trevor Anderson: All right, have a good one.
[00:24:57] Granth S: You too. All right.
