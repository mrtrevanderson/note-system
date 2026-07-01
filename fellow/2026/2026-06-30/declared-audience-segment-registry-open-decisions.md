---
title: Declared Audience - Segment Registry Open Decisions
date: 2026-06-30
time: "12:15-12:45"
participants: [Dan Duling, Trevor Anderson, Brian Silveri, Joseph Ramesh, Granth Sajjanshetty]
fellow_url: https://fluentco.fellow.app/meetings/040000008200E00074C5B7101A82E008000000002DC4370FDE07DD010000000000000000100000006977E3265C917040A5FE3025B9560200/
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

The team reviewed open technical decisions for the **Segment Registry pipeline** planned for Q3 2026, working through gaps in the PRD that need resolution before development begins. Key decisions included using the **mailable universe data source** (not raw data), implementing a **single YAML file approach** for tracking segments, and requiring all validations to occur at JIRA ticket creation rather than mid-pipeline. The team agreed to **confirm PII attribute requirements with compliance** (Jeff Richards and Jason) and **contact vendors** (TransUnion, Live Ramp, Loadata) about preferred data delivery formats.

Additional architectural decisions addressed future multi-vendor distribution through Databricks Identity Hub, the need for analytical tables to enable reporting on segment performance, and implementing a human approval gate in the workflow. Joseph will draft vendor communications and update the Confluence documentation with all decisions made during this meeting.

### Segment Registry Q3 Implementation Overview

- The Segment Registry is a **new pipeline** being built off the existing legacy declared audience pipeline, with development work planned for Q3 2026. [01:04]
- Trevor and Joseph identified open questions and decision items in the PRD that need resolution before starting development work, organized as 'gaps' A through F. [01:24]
- The team will share architectural decisions and implementation plans with Virginia and JC as pre-read documentation, with detailed alignment discussions to follow in Q3 deliverable reviews. [09:15]

### Data Sources and PII Handling

- **Gap A** addresses whether to pass raw PII or only hashed PII to vendors (TransUnion, Live Ramp, Loadata). Legal and compliance review is required to determine which identifying attributes can be shared. [02:18]
- Currently declared audiences send **email hash, first name, last name, city, state, address, and zip code**. The team needs definitive compliance sign-off on all attributes rather than assuming current practice is correct. [04:38]
- **Gap B** was resolved decisively to use mailable universe data source rather than raw survey data, which avoids users from pre-FTC compliance periods. [06:04]
- **Gap C** regarding data joins was resolved by confirming that demographic and geographic PII will require joining the user table with survey event table in mailable universe, as segments need demo data like gender. [07:21]

### Vendor Integration and Delivery Format

- **Gap D** concerns whether vendors prefer user-perspective output (user A belongs to segments B, C, D) versus segment-perspective output (segment A contains users 1, 2, 3). Current approach is user-perspective. [10:16]
- Engineering will draft and send a technical inquiry to TransUnion, Live Ramp, and Loadata to confirm their preferred delivery format and whether the hash-only decision affects their ingestion requirements. [11:39]
- Databricks Identity Hub will enable **bi-directional sync of audiences** to activation channels (Meta, etc.) as a feature launching in Q4 2026, requiring the pipeline to track vendor distribution. [20:49]
- By Q4 2026, segments will be delivered to **multiple vendors simultaneously**, necessitating segment storage in Databricks lakehouse rather than just S3 for scalable distribution. [23:21]

### Pipeline Architecture and Storage Approach

- **Gap F** concerned YAML file structure for tracking segments. The team decided on a **single registry file** containing all segments rather than 2,000+ individual files, for better performance and maintainability. [13:56]
- Performance concerns exist around querying **100 million record and 1.7 million record tables** repeatedly for each segment. Materialization strategy needs further technical evaluation. [15:12]
- The team confirmed need for **analytical metric tables** separate from pipeline tables to enable reporting on segment creation, agent attribution, and vendor performance. [16:41]
- Pipeline architecture must support **future reporting requirements** including segment counts, user counts per segment, revenue attribution, and vendor analytics tied back to segment IDs. [19:21]

### Workflow Automation and Validation Process

- The team confirmed using a **Claude skill-based template** where Virginia/account managers input requirements, and the skill automatically creates JIRA tickets only after validating all required fields. [24:29]
- Primary validations (including **1 million record minimum**) must occur before JIRA ticket creation to avoid mid-pipeline failures. The skill should immediately prompt for missing information rather than creating incomplete tickets. [25:56]
- Current back-and-forth in comment sections due to missing information defeats automation purpose. The new approach should **trigger jobs directly** after validation with no human intervention needed mid-pipeline. [27:34]
- The existing monitoring job that checks if segments maintain 1 million records won't be needed for new segments but should run once as a diagnostic on legacy segments to identify those under threshold. [28:33]
- A **human approval stage gate** is required somewhere in the workflow (potentially at PR merge or JIRA status) to prevent automated creation of incorrectly specified segments from freeform text input. [32:12]
- The team will schedule a follow-up meeting when Manav returns (Tuesday or Wednesday of the following week) to review consolidated epics and stories for the segment registry implementation. [34:23]

### Action Items

- [ ] Joseph to confirm with compliance and legal (Jeff Richards and Jason) which PII attributes (email hash, first name, last name, city, state, address, zip code) can be shared with data vendors for declared audiences. [04:50]
- [ ] Joseph to draft and send message to TransUnion, Live Ramp, and Loadata asking about their preferred format for segment data delivery, with a response deadline of end of next week. [35:47]
- [ ] Update the Confluence document with all decisions made during this meeting, leaving only Gaps A and D as open questions. [36:36] (Trevor Anderson, Joseph)

### Decisions

- The segment registry will **use mailable universe as the data source**, not raw data, for all segment creation. [06:04]
- Segment tracking will use a **single YAML file registry** containing all segments rather than creating separate YAML files for each segment. [14:27]
- All segment validations (including the **1 million record minimum threshold**) must occur at JIRA ticket creation level, not mid-pipeline, to avoid failures during automation. [28:08]
- Pipeline output must **include vendor delivery information** to track which vendors (TransUnion, Live Ramp, Meta, etc.) receive each segment for multi-vendor distribution. [20:22]
- A **human approval gate is required** in the workflow to review and confirm segments before activation, preventing automated creation of incorrect segments. [32:44]
- **Analytical metric tables** will be built to enable reporting on segment creation, performance, vendor attribution, and tie-back to intake requests. [16:41]

## Transcript

[00:00:00] Trevor Anderson: Do a deep dive into Segment register generated already. So.
[00:00:04] Trevor Anderson: But thank you for going through it and at least raising some concerns for us to review.
[00:00:10] Trevor Anderson: That's what this meeting is about. And so yeah.
[00:00:13] Trevor Anderson: Brian, just to debrief you like Joseph and I last week while you're out we.
[00:00:20] Trevor Anderson: We discussed some items just brought up regarding the Segment Registry work that I think it's only.
[00:00:29] Trevor Anderson: This is directly related to just the declared audience pipeline.
[00:00:35] Trevor Anderson: I'm not sure if this is also relevant to model as well.
[00:00:39] Joseph: No, just for the.
[00:00:41] Brian Silveri: Ricardo, it's only in the source data that could be impacted if we make some of these proposed changes that that could possibly be it.
[00:00:53] Brian Silveri: But yes, just the declared. Sorry, just those audiences. You're correct.
[00:01:04] Trevor Anderson: Right. So yeah, for the new work on how we're building the segment Registry which is going to.
[00:01:08] Trevor Anderson: Which is like a proposed new pipeline build off of the existing legacy declared audience pipeline.
[00:01:15] Brian Silveri: That's right.
[00:01:16] Trevor Anderson: Right. And this is work that we're looking to do in Q3.
[00:01:24] Trevor Anderson: Just went through the existing PRD that exists for that and just rose some questions and concerns and maybe some decision items that we want to nail down before we start development work.
[00:01:38] Trevor Anderson: I already went through some of them and tried to answer some of the things that I might be able to provide guidance on.
[00:01:44] Trevor Anderson: But the. There was just. Yeah, there was some open items that I couldn't answer that needed.
[00:01:50] Trevor Anderson: Maybe you or Virginia might need to provide guidance on these. Right. So happy to.
[00:01:57] Trevor Anderson: Yeah, let's just go through. Let's just jump into it and go through it.
[00:02:00] Trevor Anderson: Unless you guys have any other questions to start off.
[00:02:03] Brian Silveri: Cool. No, I read through it. So we can go right to the questions. Joseph. Yeah.
[00:02:10] Trevor Anderson: So Joseph, I don't know if you want to go through it or I can.
[00:02:12] Joseph: You can go. No problem.
[00:02:18] Brian Silveri: The gap A. Yes. I can't answer that question.
[00:02:22] Brian Silveri: I suspect that the answer will be we should probably move to hash only.
[00:02:31] Brian Silveri: But that is definitely a Virginia slash legal question that that would be like. Yes.
[00:02:38] Brian Silveri: I don't know that we need to be passing raw PII to these vendors.
[00:02:43] Brian Silveri: That's not demographic specific information but that's up to them and what they agree to in the contract which I don't have access to.
[00:02:52] Brian Silveri: So my guess is PII hash only.
[00:02:54] Trevor Anderson: But yeah, please confirm my assumption here
[00:02:58] Dan Duling: was that take that up with compliance Jeff Richards and get. You know.
[00:03:07] Brian Silveri: And Jason.
[00:03:07] Dan Duling: Jeff and Jeff and Jason. Ideally the answer should be it's only hash. We should really like.
[00:03:16] Dan Duling: I know some of our clients work with giving us direct email.
[00:03:20] Dan Duling: We don't like it like everything that we work with, all of our data goes to hash values.
[00:03:26] Dan Duling: Like, you know, sharing PII data just opens up conundrums left, right and center.
[00:03:33] Dan Duling: You know, you know, the hash protects all parties involved. Yeah.
[00:03:40] Brian Silveri: Joseph, one thing I would add as a point of color in the why it matters is be very specific about the PII that we're talking about because we will need to be passing pii, which is information about these people to these vendors.
[00:04:02] Brian Silveri: So some of it we know we need to pass.
[00:04:04] Brian Silveri: The real question should be which one of the spi, you know, special PII data should we be passing?
[00:04:17] Brian Silveri: And not from a how to identify the user perspective. So that should be.
[00:04:23] Brian Silveri: They're identifying attributes, which is email, phone made, which one of these three attributes, an ip, an address.
[00:04:33] Brian Silveri: Should we be including these things? Yes or no? If so, which ones?
[00:04:38] Joseph: Okay, what we currently send for the declared audiences, email hash, first name, last name, city, state address, zip code.
[00:04:49] Joseph: That's it.
[00:04:50] Brian Silveri: Yeah, I, I would just confirm with compliance and legal on those, all of those entities and let them be the definitive answer on them.
[00:05:02] Brian Silveri: And then if, if those other attributes are needed, then we have sign off on them.
[00:05:06] Brian Silveri: Say yes for declared audiences. This is. Okay, great.
[00:05:09] Dan Duling: Yeah, I agree with that.
[00:05:11] Dan Duling: I mean, don't make the assumption because we're doing it now means we're doing it correctly.
[00:05:17] Brian Silveri: Correct. Or, or we did it in the past.
[00:05:24] Dan Duling: Yeah.
[00:05:24] Dan Duling: Because you know, you have to understand even with where we are right now, like, you know, a lot of this was set up by sales.
[00:05:31] Dan Duling: So, you know, it's good if we have questions about various things and we're going to be bringing it up anyways and having legal review it.
[00:05:43] Dan Duling: You know, let's just make sure we get all the, the sign up pieces, you know, in play because end of the day, right, you know, liability falls down to us as well, you know, for, you know, if we violate, you know, some of these FTC consent rules and everything else.
[00:06:04] Brian Silveri: Yeah, Gap B, that's an easy one. Use mail. Mailable universe. Don't use raw period. End of story.
[00:06:16] Trevor Anderson: Said that as well. So same page.
[00:06:21] Brian Silveri: Now what that really means is your issue for Gap C, which is how do you join and get the data that is necessary in mailable universe?
[00:06:34] Brian Silveri: So you're going to need to get to. That information.
[00:06:42] Brian Silveri: But if you just use mailable, I guess, does anything fall out with mailable that you need to join to RAW survey?
[00:06:53] Joseph: Joseph, we do not want the RAW survey because that also has users from like before ftc.
[00:07:02] Brian Silveri: Great. Sorry.
[00:07:04] Brian Silveri: So what I Meant is would any of the demographic or geographic information not be available in mailable universe and forced to join to something else or is it all there and we don't have to worry about this when we picked option B.
[00:07:21] Joseph: Okay, to be clear, we have two tables under mailable, the main tables which are the user table which has the pii, which I meant like first name, last name, so and so and the other table is the server event table.
[00:07:33] Joseph: So only if we join these two we can get arrive at specific users PI.
[00:07:39] Joseph: So if you are not sending PI, we never require this user table.
[00:07:44] Joseph: But still we have certain segments which are like focused into demographics like male,
[00:07:51] Brian Silveri: female, you, you will definitely need this data. Yeah, you will definitely need more of them. Yes.
[00:07:58] Brian Silveri: So we just need to be even with the answer to gap A.
[00:08:04] Brian Silveri: Whatever legal decides you're going to need to build segments based off of demo data which is pii, we already know that, that it's a must do for this.
[00:08:15] Brian Silveri: So yes, you need the data, join it when necessary to whatever tables are necessary to get to that pii, period.
[00:08:40] Trevor Anderson: Just a heads up Brian, like on all these gaps, like I guess what as we're going through this decisioning on it, you know, how do do we want to share that?
[00:08:54] Trevor Anderson: Of course we probably want to share this with Virginia and JC as well.
[00:08:59] Trevor Anderson: You know, how do we want to go about sharing that information?
[00:09:02] Trevor Anderson: Is that something that you want to take care of or something I could bring up in the next stand up or.
[00:09:14] Brian Silveri: Either one is fine.
[00:09:15] Brian Silveri: I think just when I talk about our Q3 deliverables, about audience solutions, we can go into detail about the hows and then share the reference documentation.
[00:09:28] Brian Silveri: Like FYI, here's how we're structuring on building this.
[00:09:32] Brian Silveri: Feel free to go through this because this is a lot to digest and it's not going to be digestible in a meeting.
[00:09:36] Brian Silveri: So it's going to be a pre read.
[00:09:38] Brian Silveri: FYI, here is our segment registry plan for the first half of, you know, first few weeks of Q3.
[00:09:45] Brian Silveri: Here's our plan for the UI slash, I mean reporting. Read through this and we'll align.
[00:09:52] Trevor Anderson: Okay.
[00:09:52] Brian Silveri: I can't imagine that Virginia's gonna be like oh my God, no.
[00:09:56] Brian Silveri: When these answers are basically pretty much just architecturally sound and the other one's compliance.
[00:10:03] Brian Silveri: Agree until we get to Gap E.
[00:10:06] Trevor Anderson: Okay, great. Let's keep going through.
[00:10:08] Dan Duling: Great.
[00:10:09] Brian Silveri: All right. Gap D, I think, I think your recommendation is correct.
[00:10:16] Brian Silveri: You just gotta confirm with the vendors as to A And Joseph, we learned now what we've learned from this previous run of like hey we loaded 25,000 and it took us two days to load the data.
[00:10:29] Brian Silveri: You know, we want to make sure that their format of ingestion matches the performance for what we need.
[00:10:37] Brian Silveri: So like hey, we're thinking of doing this different approach for construction.
[00:10:42] Brian Silveri: Will this fit your APIs for how these things could be loaded? Yes. No. Great.
[00:10:49] Brian Silveri: Which one is your recommendation?
[00:10:52] Joseph: And Brian, I think that's for the model audience which we are talking about the APIs.
[00:10:58] Brian Silveri: So this is completely different. Sorry for the declare my apologies. Yeah.
[00:11:05] Brian Silveri: Then I would just still the same question, go back to TransUnion live ramp and load of May and say hey, which, which method do you want?
[00:11:13] Brian Silveri: Do you have a preference? Does it change anything even with the hash decision?
[00:11:21] Joseph: Okay, so do I be engineers need to reach out to them or like Virginia or like JC would have a call with?
[00:11:33] Brian Silveri: Well, I think we need to formulate exactly what our ask is. That's engineering's responsibilities.
[00:11:39] Brian Silveri: Like hey, we're going to build this thing.
[00:11:41] Brian Silveri: Here's what we're looking at to build and this is the method of delivery as to what we're proposing to change.
[00:11:48] Brian Silveri: Please confirm or you know, if you want to meet to talk about this.
[00:11:52] Brian Silveri: ABC, I think that email is perfectly acceptable to come from engineering because it's an engineering only conversation.
[00:12:00] Brian Silveri: Unless they come back and say this will require some addendum to our contract at which point it goes to Virginia and legal to have that answer.
[00:12:09] Brian Silveri: I'd be shocked if that happens.
[00:12:11] Brian Silveri: But let's engineering draft the request, send it back to those vendors perfectly fine.
[00:12:19] Brian Silveri: Obviously CC, VA and JC because and make sure that they're aware. Yep.
[00:12:25] Brian Silveri: You don't need product to drive a tech decision in this case. This is pure tech.
[00:12:30] Joseph: Okay.
[00:12:31] Brian Silveri: Virginia is not going to care, nor should she a salesperson care on the method of delivery for segment creation.
[00:12:37] Joseph: I understand.
[00:12:40] Brian Silveri: All right. Gap E. Yes. Go to the million.
[00:12:49] Joseph: Okay.
[00:12:56] Brian Silveri: Gap f.
[00:13:04] Trevor Anderson: Can you this one do you want to show. Do you want to explain?
[00:13:08] Trevor Anderson: This is purely how we're storing it in GitHub though. So it's a.
[00:13:13] Trevor Anderson: Joseph, the example you walked me through was helpful.
[00:13:18] Joseph: Okay so here what happens is currently we deliver audience like by user.
[00:13:26] Joseph: So user A falls under segment A, segment B C D.
[00:13:31] Joseph: But this new approach will be like under segment A these all people fall and a segment B these all these many people fall.
[00:13:40] Joseph: So it's like segment perspective is the new approach and user perspective is the current approach which we follow now.
[00:13:46] Joseph: So I wonder which way it's the possibly to move forward with.
[00:13:56] Trevor Anderson: I thought Gap F was mostly concerned about the YAML file creation and how agents are going to track existing audiences that are created.
[00:14:12] Trevor Anderson: And do we need a 1 YAML file per segment or. Right.
[00:14:20] Trevor Anderson: Or have a 1 pending to every time we create a segment. Right.
[00:14:27] Trevor Anderson: And I think that's the way we want to go is that we just have like a registry of all the segments within one YAML file.
[00:14:34] Trevor Anderson: Right. So that way we don't have to scan through 2,000 files. You know, we want 2,000 segments.
[00:14:42] Trevor Anderson: We wouldn't want to have 2,000 files. Right?
[00:14:45] Joseph: Yeah.
[00:14:46] Joseph: And even currently we have all the required details for segments in a single Python file and not under separate YAML files.
[00:14:56] Joseph: But I wonder. That's the reason they came up with this new approach.
[00:14:59] Joseph: They wanted everything as a separate YAML. So that would have its respective SQL query hard coded.
[00:15:05] Joseph: So it just runs on loop for each and every segments.
[00:15:12] Joseph: The only issue is I'm wondering if this is performant because we have two tables.
[00:15:19] Joseph: One is about 100 million and other one is about 1.7 million.
[00:15:23] Joseph: Either we'll have to materialize as a whole and read from there, else it would read like 2,000 times for each segments if we go.
[00:15:38] Joseph: But if it's stored under different separate YAML files, I'm wondering how can we even materialize it and call it under each under the loop?
[00:15:50] Joseph: I'll have to look into it.
[00:15:51] Trevor Anderson: Yeah. This one's purely like a implementation technical item here. So it's not.
[00:15:58] Brian Silveri: Does your choice impact how it would be reportable out?
[00:16:08] Trevor Anderson: Go ahead, Joseph.
[00:16:09] Trevor Anderson: I, I was going to say no because what I had suggested is for reporting we should actually have separate analytical tables that track the segments that are being created.
[00:16:20] Trevor Anderson: I think this, this particular point was concerned like how the agents are going
[00:16:25] Brian Silveri: to be calling to get calling information.
[00:16:30] Trevor Anderson: Yeah, exactly.
[00:16:31] Brian Silveri: Great. I'm completely aligned with you. The Trevor and Joseph.
[00:16:35] Brian Silveri: This is a pure pipeline creation question and maintenance question.
[00:16:41] Brian Silveri: You, you answer it how architecturally it should be sound with the understanding that you will need some sort of analytical metric table construct to report out off of this.
[00:16:55] Joseph: Okay, Makes sense.
[00:17:04] Brian Silveri: So that would be my only say gap Joseph, that we don't have here is that how are we going to report out off of this?
[00:17:14] Brian Silveri: Because that will be the question that Virginia asks.
[00:17:16] Brian Silveri: How are you going to report what you created, what was the agent that created and how does it tie back to the intake request?
[00:17:24] Brian Silveri: That's going to be the end point of like yes, great, you did all this infrastructure, you made all this Cool agents that do all this stuff.
[00:17:31] Brian Silveri: Now you said what, what does that mean? How many segments did I create?
[00:17:35] Brian Silveri: How many people were inside each segment? What was the performance of the segment?
[00:17:40] Brian Silveri: That is our SO and and a funneled thing, if that makes sense. Okay, I get it.
[00:17:46] Joseph: What do you mean by the performance of the segments? Is it the revenue or.
[00:17:50] Brian Silveri: Revenue revenue. How many times did somebody buy this segment?
[00:17:55] Brian Silveri: Who bought it, which vendor did it come from? How many times?
[00:17:58] Brian Silveri: Whatever analytics we get from vendor A, B and C tied Back to segment ID 1234 and Databricks.
[00:18:07] Joseph: I think for this we'll have to meet with the vendors and get an idea of how each of the segments are linked and only then we can arrive at this because we don't have the data right now inside databricks.
[00:18:20] Joseph: I think she's getting it through emails or she's exporting as Excel from the ui, if I'm not wrong.
[00:18:28] Brian Silveri: Well, she's definitely exporting it from the vendor's UI. That's. That's current state for sure.
[00:18:34] Joseph: Okay.
[00:18:36] Brian Silveri: What the second half of Q3's deliverable will be reporting off of this and the other thing.
[00:18:44] Brian Silveri: So prepare it in your mind from an infrastructure perspective.
[00:18:50] Brian Silveri: Even if it's not a deliverable of segment registry, it will be a deliverable of the reporting.
[00:18:57] Brian Silveri: So you know, you're going to have to report off of this.
[00:19:00] Brian Silveri: Make sure you're constructing this in a way that you can report off of it and tie it back.
[00:19:05] Brian Silveri: That's all I'm saying.
[00:19:07] Joseph: Okay.
[00:19:09] Trevor Anderson: Yeah. I think as we're building out these pipelines, we need to.
[00:19:13] Trevor Anderson: Yeah, we need to just make sure like as we're architecting them that we're thinking about the consumer, how they're going to consume the data.
[00:19:21] Trevor Anderson: Like we'll need analytical views of what the pipeline is doing.
[00:19:26] Trevor Anderson: We need tables that are storing that metadata and information that is processing these segments.
[00:19:32] Trevor Anderson: Right. So there's definitely a lot that we can capture currently.
[00:19:37] Trevor Anderson: I know what you said is about like on the actual platform or the provider side what we might not be able to have, but what could be possible in a future state, you know, but there's definitely a lot that can be captured currently as we're building this out.
[00:19:54] Brian Silveri: Yeah, you want to get to. And especially for the output approach, Joseph. And that's the.
[00:20:00] Brian Silveri: So if you scroll down to the nine third bullet, that's going to be critical path because that's going to tell you how you tie that back to that particular segment based off of that vendor.
[00:20:14] Brian Silveri: So you might also Want to ensure that output also includes vendor of delivery.
[00:20:22] Brian Silveri: So you have which vendor? Because in theory a segment could go to two or more vendors.
[00:20:29] Joseph: Okay.
[00:20:30] Brian Silveri: Does that make sense?
[00:20:31] Joseph: Yeah, it makes sense.
[00:20:33] Brian Silveri: Okay. And with Virginia's plan and just so you guys know. Can I share real quick? Joseph?
[00:20:41] Trevor Anderson: I'm sharing, but yeah.
[00:20:43] Brian Silveri: Oh, sorry.
[00:20:46] Dan Duling: So the
[00:20:49] Brian Silveri: plan here, an option that's now out in the universe that we're going to talk about is Databricks Identity Hub sending this thing out to Martech connectors.
[00:21:06] Brian Silveri: So activation channels through databricks.
[00:21:10] Brian Silveri: So Databricks is acting as a bi directional sync of audiences and signals with activation layers with a direct connect.
[00:21:21] Brian Silveri: Joseph.
[00:21:23] Brian Silveri: So Meta, you know, all of these vendors that she could now send out directly segments to via Databricks is going to be a feature inside Databricks.
[00:21:39] Joseph: Okay.
[00:21:40] Brian Silveri: That means when you build this segment id 1234 it's going to be sent to.
[00:21:46] Brian Silveri: She's going to decide, oh, this one is going to enable to load a main transunion in meta. Great.
[00:21:53] Brian Silveri: Have a nice day. We need to track the web of usage across the vendor activation channels.
[00:22:02] Brian Silveri: So that's a piece of data that we're going to have to ensure our pipeline manages not just the single output of segment id.
[00:22:14] Brian Silveri: So Trevor, whether that's, that's a source column or you know, delivery partner ID or partner name as to which one it went to.
[00:22:24] Brian Silveri: So we could attribute delivery of a segment to a entity.
[00:22:30] Trevor Anderson: Yeah. For that cdp, does that data need to be stored and in databricks in order to be distributed?
[00:22:36] Trevor Anderson: Yeah, because right now we're just.
[00:22:38] Trevor Anderson: I think all we're doing is putting this on S3 like these segments. But if the, if, if it.
[00:22:47] Trevor Anderson: I will need to look into this a little more. But if we need to have. Yeah, Lake house. Okay.
[00:22:55] Trevor Anderson: So we'll probably need to think us start thinking about storing these segments in a database so they could be distributed.
[00:23:05] Trevor Anderson: We can endlessly scale up to the amount of.
[00:23:10] Trevor Anderson: I'm not going to solve for that but yeah, I'll think about it.
[00:23:13] Brian Silveri: But yeah, yeah, it's, it's very easy leap in sometime in 2026, most likely.
[00:23:21] Brian Silveri: Joseph, like Q4 there will be a method where a segment gets delivered to multiple vendors.
[00:23:28] Brian Silveri: So we need to now make sure that we have the mental model that that's going to.
[00:23:35] Brian Silveri: That thing will happen. So even if you stay in S3 to start, Trevor. Yeah, fine.
[00:23:44] Brian Silveri: No one's going to complain, but your reporting is going to have to come out of databricks.
[00:23:49] Brian Silveri: You know, whether the BI tool of Choice is irrelevant but you know it's the metric or reporting esque tables of this segment creation is is on our responsibility.
[00:24:00] Brian Silveri: And then Joseph, one thing I am not sure is when this segment registry request comes in for a declared audience are we imagining that this is going to be another intake in JIRA for like go into JIRA put in this form this agent picks it up.
[00:24:27] Joseph: So that's where the point is.
[00:24:29] Joseph: So we'll have to have a template based on what the business creates one so let them not create one.
[00:24:36] Joseph: We create a skill for them.
[00:24:38] Joseph: They just feed in the required informations and then perfect skill load create the respective files and that would trigger the subsequent processes.
[00:24:49] Brian Silveri: Perfect, perfect.
[00:24:51] Brian Silveri: Please add that into either your open questions or not open questions like but that's a thing that they're going to have to do.
[00:24:59] Brian Silveri: So Virginia and the account manager is going to have to build a template in their minds for what information am I going to need in order to to drive a segment creation.
[00:25:11] Joseph: Yeah.
[00:25:11] Brian Silveri: From an authoring perspective even if we're not giving them specific attributes. Thanks Dan.
[00:25:19] Brian Silveri: We want to at least give them the guardrails of things that they should take into account when they're thinking of the authoring of this.
[00:25:27] Joseph: Understand?
[00:25:28] Trevor Anderson: Yeah, yeah.
[00:25:29] Joseph: Because this is the current approach flowchart that have inferred to this is the segment registry approach.
[00:25:36] Joseph: But the only caveat I see here is we have all the validations done after creating the JIRA tickets but I feel that those primary validations to be done before even creating one so we can avoid the.
[00:25:56] Joseph: For example, Virginia can come up with the segment a create a declared audience but that might not have 1 million of records so those should be rejected in the very first so what I thought okay, okay.
[00:26:10] Joseph: Because this is an end to end automation we shouldn't have failures in between either it should complete the entire process or fail in the very beginning, not in the not in the middle.
[00:26:24] Brian Silveri: Yeah, fair, fair, yeah.
[00:26:26] Joseph: And also we can't store all the required information for each segments because initially we used to get about like four or five audiences but nowadays Virginia comes up with 50 and hundreds of audiences so those can't fit it in the JIRA descriptions either.
[00:26:44] Joseph: We'll have to have it as an Excel file and then that should be read from it.
[00:26:50] Joseph: So that can be done through a cloth skill.
[00:26:52] Joseph: She can just read in those required information based on our template which we provide them and then rest the cloth skilled asset and only create a JIRA ticket only if it has the required fields that the pipeline wants and if it's greater than a million, if not, it shouldn't create one
[00:27:12] Brian Silveri: and tell them it's because these X things are missing or you did not exceed a million people.
[00:27:18] Joseph: That should prompt immediately so they can put in those information and then only it can create one.
[00:27:24] Joseph: Because what happens is even I worked like recently, we don't have all the information, it's like back and forth like in the comment section.
[00:27:34] Joseph: So yeah, this one has to be addressed, but I don't think so that is being handled in the approach which Mano has created because all these checks are happening here and then it fails and then it goes back, which is not right, which defeats the purpose of the automation.
[00:27:53] Joseph: So what this should does does is this should directly trigger the job, run it, evaluate it, distribute it, nothing else.
[00:28:02] Joseph: So every validations should be at the JIRA ticket creation level.
[00:28:08] Brian Silveri: I'm with you, I'm completely aligned.
[00:28:11] Joseph: Yeah, and I also see, I don't know if you guys have time, but I can just wrap it into three minutes.
[00:28:18] Joseph: So we have a separate monitoring thing which runs for example every, I don't know which is on Sunday, sorry Monday I guess.
[00:28:26] Joseph: So that refers each and every segments. If it has the required 1 million records.
[00:28:33] Joseph: Do we even need this? Because we shouldn't create a segment if it's less than a million.
[00:28:40] Joseph: So we create everything and we distribute it and then we are going to run a job just to find which are all the segments as less than a million.
[00:28:49] Trevor Anderson: We, we won't need it in the future if there's a requirement going forward in a million.
[00:28:55] Trevor Anderson: But we'll probably need it in the interim.
[00:28:58] Brian Silveri: I, I would suspect that a add on report of like hey, we've implemented the segment registry.
[00:29:09] Brian Silveri: We also ran this diagnostic on the current segments and found segment IDs 1, 3, 5, 7, 9 and 400 are under 1 million.
[00:29:20] Brian Silveri: Option A up to you Virginia. Do you want to rebuild the segments?
[00:29:26] Brian Silveri: If so, go to Claude and rebuild the segments with the same parameters using a million people.
[00:29:31] Brian Silveri: Option B, keep it, you just have less than a million.
[00:29:35] Brian Silveri: Option C, delete it because you don't use it anymore or whatever they are.
[00:29:39] Brian Silveri: It's going to be a product sales decision what to do with it.
[00:29:42] Brian Silveri: We're just going to furnish them with information going forward.
[00:29:47] Brian Silveri: You don't need to worry about this because every segment you create through this process will be a million above.
[00:29:53] Joseph: Okay, yeah, yeah, because.
[00:29:56] Joseph: And also here we have like two, three places where human intervention is required which is not supposed to be because every.
[00:30:07] Joseph: If everything is Validated at the very beginning. We wouldn't require any human interventions here.
[00:30:12] Joseph: It just creates the pr, we merge it and we run the job.
[00:30:15] Joseph: So the only process for this to be just to create the yamls and then that triggers the each task in the pipeline and delivers it, which I believe it should be.
[00:30:27] Joseph: And still have other questions here because here what Mana has mentioned this,
[00:30:34] Brian Silveri: for
[00:30:35] Joseph: example, this requires Virginia's.
[00:30:39] Joseph: I'm not sure if it's Virginia because that's how he has mentioned it.
[00:30:42] Joseph: I hope so it should be an engineer here such as the available status and only then that's get picked up by the pipeline.
[00:30:51] Joseph: So the available status for each and every segments.
[00:30:54] Joseph: So if it's not required, I'm wondering how is this happening here? Because who activates this?
[00:31:00] Joseph: Is there a separate PR be raised for it just to activate it and deactivate it and yeah, those steps.
[00:31:07] Joseph: I still need some insights.
[00:31:09] Brian Silveri: At the very least I would imagine you would want to stage gate approval somewhere in this because especially in the beginning, let's say we run through this and we build our first modeled audience and they go in and the person says to Claude, give me a million people, 50, 50 male, female in you know, California and Texas who answer GLP1 yes, you know, and that's the segment if, if they type it in wrong, it's going to auto go all the way through because whatever they typed in, there's no stage gate for activation here.
[00:31:57] Brian Silveri: That with this new process flow.
[00:31:59] Brian Silveri: So even from a human in the loop perspective, somewhere in step, you know, one to seven here, someone's going to have to say the Virginia or the new account manager.
[00:32:12] Brian Silveri: Yes, this was constructed correctly. Turn on, activate, pick a thing without it.
[00:32:21] Brian Silveri: If Virginia types in the wrong state because it's freeform text, they're not even picking from predefined fields.
[00:32:28] Brian Silveri: They could type in wrong or speak it wrong.
[00:32:30] Brian Silveri: And then Claude's created this thing and now we've got junk.
[00:32:36] Joseph: I believe those steps should be handled here itself.
[00:32:40] Brian Silveri: Brian, I, I don't, I, I agree with you.
[00:32:44] Brian Silveri: From a, a process flow perspective prior to that JIRA automating it, maybe review the JIRA as a thing and then the stage gate of a process flow of approval like the use your workflow status like create or approve or you know, review and whatever have it.
[00:33:03] Joseph: Yeah, we have a gate there because only if an engineer approach we can merge the branch to the main so we obviously have one.
[00:33:14] Brian Silveri: Great. Yeah. It doesn't have to be.
[00:33:18] Brian Silveri: There's no prescription that it has to Be in the pipeline job steps.
[00:33:23] Brian Silveri: As long as there is a human in the loop of I set it to do this.
[00:33:29] Brian Silveri: Please confirm that this is what you're doing. Great. Inside the Claude skill. Super inside jira. Fine.
[00:33:38] Brian Silveri: As long as we have an audit trail so that okay, it this isn't 100% automated with no loop and we're just creating billions of segments of junk.
[00:33:48] Joseph: Okay. Makes sense.
[00:33:52] Dan Duling: Yes.
[00:33:52] Joseph: And I think we can, we can go through with this with Trevor later.
[00:33:58] Joseph: I need still some more time to digest all the epics and stories and
[00:34:02] Brian Silveri: yeah, I, I consolidated all the epics so I tasked Manav with going through to tell me which ones were done.
[00:34:11] Brian Silveri: I don't think he got to it before he went on vacation.
[00:34:14] Brian Silveri: Either way, you guys aren't going to get to them. But I would.
[00:34:18] Brian Silveri: At the very least when you get back, Joseph, let's have a. I think.
[00:34:23] Joseph: Yeah.
[00:34:24] Brian Silveri: When is he come back?
[00:34:25] Joseph: Tuesday I guess or Wednesday.
[00:34:28] Brian Silveri: Let's just schedule something with with the team on Wednesday or Tuesday.
[00:34:33] Brian Silveri: Trevor, even though you're out, I think they can move forward with, with this and please do I, I, I think we could drive on this even with the answers waiting from the vendors.
[00:34:52] Brian Silveri: And maybe we'll just call out Joseph, what which one of our tasks we can build or do that is not waiting on one of these answers, whether it be legal or.
[00:35:04] Joseph: I think that's the first step for us because that decides everything.
[00:35:08] Joseph: If it's user specific, it's going to change completely.
[00:35:12] Joseph: If it's segment specific files, we can just follow what the new approach is.
[00:35:19] Joseph: So we'll have to tweak basically on what the vendor is expecting.
[00:35:24] Brian Silveri: Okay, so the, like the, the agent building or the, or the skill building.
[00:35:31] Joseph: We can pick up the initial ones like the JIRA tickets.
[00:35:36] Brian Silveri: Great. Yeah, we at least are not hard blocked on the answer.
[00:35:40] Brian Silveri: We could at least do something that's just.
[00:35:42] Joseph: Yes, you're good.
[00:35:43] Brian Silveri: Okay. Yeah. So then let's draft.
[00:35:47] Brian Silveri: So when you get that you're great on Thursday, just draft up the the message to TransUnion Live ramp and load of May with the formatting question and get that out and, and give them a due date to respond back by like, hey, we're working on this.
[00:36:08] Brian Silveri: Please give us an answer.
[00:36:09] Brian Silveri: You can give them a week, give them till the end of next week because this week is a holiday for the US Say please give us an answer by the end of next week so that we can continue forward.
[00:36:20] Brian Silveri: If you need a meeting, please reach out and we'll schedule one.
[00:36:25] Joseph: Okay,
[00:36:28] Brian Silveri: great.
[00:36:29] Trevor Anderson: All right. Thank you, Joseph, for this. Yeah.
[00:36:31] Brian Silveri: Thanks, team.
[00:36:32] Trevor Anderson: And thanks, Brian, for helping us go through those.
[00:36:35] Brian Silveri: Of course.
[00:36:36] Brian Silveri: Trevor or Joseph, please go through and put the answers to the things that we decided and update the Confluence document so that we don't have to rehash this when someone else comes.
[00:36:45] Brian Silveri: Like, what was our answer? And open questions should be A and D.
[00:36:57] Trevor Anderson: Yes.
[00:36:58] Brian Silveri: Great.
[00:36:59] Trevor Anderson: Awesome.
[00:36:59] Brian Silveri: Awesome. Thanks, guys.
[00:37:01] Joseph: Thanks, team.
[00:37:02] Trevor Anderson: See you.
