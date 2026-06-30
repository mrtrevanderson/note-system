---
title: "Internal Discussion:  Adflow Self Billing "
date: 2026-06-29
time: "15:00-15:30"
participants: [Sa Ho, Dan Duling, Ryan Perfit, Adrian Stack, Jack Hall, Trevor Anderson, Erin Manchester]
fellow_url: https://fluentco.fellow.app/meetings/040000008200E00074C5B7101A82E008000000006094DEBC37FFDC01000000000000000010000000044A299E696B2E418170866DDACA766F/
captured_at: 2026-06-30T12:02:44Z
---

## AI Notes

The team discussed implementing a **self-billing platform** to automate the current manual invoicing process for Adflow partners, replacing Concur with Topolti as the AP platform. **Databricks was confirmed as the source of truth** for payout amounts, with data flowing through NetSuite for staging and approval before bills are generated in Topolti. The technical architecture will involve **two NetSuite integrations** (one to Pulse for partner IDs, one to Databricks) to enable the monthly billing workflow.

Key concerns were raised about data flow efficiency and whether NetSuite should serve as the central staging area for all three billing sources (Minion/Pulse, Everflow, and vendor invoices) or only as the final endpoint. The team plans to **sign with Topolti in July 2026** and target the **August billing period** (generated early September) for the first automated invoice. An approval workflow within NetSuite will allow the Adflow account management team to review, confirm, and modify amounts before they're pushed to Topolti, addressing previous instances of data mismatches between internal and external platforms.

### Self-Billing Platform Rationale

- The current manual invoicing process causes **significant friction** with Adflow partners not getting paid for months when they don't send invoices or confirm amounts via email, creating payment backlogs and manual work for both internal and external teams.
- Partners currently use two separate workflows: accessing an external platform to confirm payout amounts or connecting with the Adflow team via email, with some partners being reluctant to send bills despite contractual requirements.
- **Concur is being replaced** as it's not the right fit for the automation goals and lacks necessary capabilities for the self-billing workflow the team wants to implement.
- **Topolti was selected** as the AP platform due to its native connectors (like Everflow integration), international ACH payment capabilities (NetSuite requires banks in each country), and overall automation features that will generate payment savings.

### Technical Architecture and Data Flow

- The proposed architecture uses **NetSuite as a staging area** between Databricks/Minion/Pulse and Topolti, with three billing input sources: vendor invoices (traditional), Adflow self-billing (primary focus), and rewards.
- **Databricks serves as the data repository** for all Minion-related setup and event data, while Minion/Pulse is the eventing platform that serves content for the Adflow product line, with Databricks now including partner contract terms that Minion doesn't utilize.
- Everflow (the reporting platform) has **native bill generation functionality** that can push data directly into NetSuite, representing one of the three separate billing source systems.
- Sa raised concerns about **data flow efficiency**, questioning whether NetSuite should serve as the staging middle ground for all three data sources or only as the final endpoint, noting the current two-step approach may not be optimal.
- The billing workflow will operate on a **monthly basis** (not daily), as all partners currently invoice monthly, requiring only a monthly data feed rather than real-time integration.

### Data Fields and Partner Mapping

- Sherif shared a spreadsheet with **required fields for bill generation** (confirmed by Topolti), including field sources, with several fields expected to come from Databricks though exact field naming needs confirmation.
- Data from Databricks will flow into a **custom record in NetSuite** that requires mapping to vendors, with each partner ID (or traffic partner in Minion terminology) expected to map to one NetSuite vendor.
- The partner ID linkage will be handled through **Pulse integrating with NetSuite** to link IDs to partner names, then passing that information to Databricks to ensure data matching and synchronization across systems.
- NetSuite currently has **three RESTlets** (REST-based integrations) that provide advertisers, line IDs, and customers to Minion and other platforms, which were created before Sa's tenure and may need review for this new workflow.

### Approval Workflow and Access Controls

- An **approval layer is needed** because there have been recent cases of data mismatches between externally facing and internally facing platforms, requiring the Adflow team to have control to review amounts before bills are generated.
- From an **audit perspective**, the calculation logic in Databricks will need proper maintenance controls, and the NetSuite staging layer will require defined approval workflows, though specific approval processes haven't been finalized yet.
- The **Adflow account management team** (like Melissa) will have oversight in NetSuite to confirm amounts, match them to contracts, and flag any needed modifications, which would then get escalated for approval within NetSuite.
- Account managers will need **access to pre-billing records** in NetSuite, similar to the revenue side which uses a custom portal (SuiteLit) rather than direct record access, though there are concerns about ensuring the right people (not BPO staff) enter the data.
- The **volume of partners is much lower** on the AP side compared to revenue, so Sherif doesn't expect account managers to need BPO resources for this workflow, unlike the revenue accrual process.

### Implementation Timeline and Next Steps

- The team aims to **sign an agreement with Topolti in July 2026**, with the goal of getting the self-billing functionality operational as soon as possible after contract execution.
- Sherif has a **call scheduled with Topolti on June 30** (tomorrow from meeting date) that Sa will join, with an invitation extended to other team members if needed to discuss integration requirements.
- Sa requested a **pre-meeting before the Topolti call** to discuss data flow concerns and architecture with Sherif and whoever helped develop the workflow documentation.

### Action Items

- [ ] Brian Silveri to confirm the exact field names in Databricks that correspond to the required billing fields and share with Sherif.
- [ ] Sherif Elzanie to schedule a meeting with Sa (and relevant team members who helped with data flow) before the Topolti call on June 30 to discuss data flow architecture.

### Decisions

- **Databricks will be the source of truth** for payout amounts (not Minion/Pulse), as it contains all Minion event data, setup data, and partner contract terms.
- The team will target the **August 2026 billing period** (to be generated in early September) for the first automated invoice through the new self-billing platform.
- The technical architecture will use **two NetSuite integrations**: one to Pulse (for partner ID linking) and one to Databricks (for payout data), which will cover the needed integrations for the proposed flow.

## Transcript

[00:00:00] Sherif Elzanie: To do here is essentially automate in a nutshell.
[00:00:05] Sherif Elzanie: We're just trying to like automate what we're currently doing manually via receiving bills and just try to implement what we're calling a self billing platform or self billing workflow.
[00:00:21] Sherif Elzanie: So anyway, we'll get into more into like what, what that looks like and we haven't signed an agreement, we haven't done anything yet.
[00:00:29] Sherif Elzanie: And we just want to get aligned from a technical standpoint before we, we make any commitments.
[00:00:37] Sherif Elzanie: All right, so any, any questions before we kind of move forward here?
[00:00:44] Sa Ho: Hey, Sherif this is Sa.
[00:00:45] Sa Ho: Yeah, we could get like a quick introduction because I have not met most of these people as well.
[00:00:51] Sherif Elzanie: Oh cool. Yeah, that, that'd be great as well. So I don't know, we could just go around and introduce.
[00:01:01] Brian Silveri: Sure, go. Sherif why don't you give yourself you started and then pick somebody else?
[00:01:08] Sherif Elzanie: Oh yeah, yeah. I introduced myself a little bit, but I'm Sherif I work on the accounting side.
[00:01:13] Sherif Elzanie: So I manage everything. Payment related accounts, payable expenses, all that good stuff. So Brian,
[00:01:24] Brian Silveri: Brian Silvery.
[00:01:25] Brian Silveri: I'm the principal product manager supporting the data engineering group here at Fluent and I report to Adrian
[00:01:34] Sa Ho: Aaron
[00:01:36] Erin Manchester: Erin Manchester.
[00:01:38] Erin Manchester: New old to Fluent. I was here previously. Been back for about three weeks now.
[00:01:43] Erin Manchester: I'm product manager of the Pulse platform.
[00:01:48] Erin Manchester: I can pass to Jack after that.
[00:01:50] Jack Hall: Yeah,
[00:01:51] Jack Hall: I am Jack. I'm the
[00:01:54] Jack Hall: engineering manager for
[00:01:56] Jack Hall: Pulse.
[00:01:58] Jack Hall: I started a few just a few months ago.
[00:02:02] Jack Hall: Trevor, you want
[00:02:03] Brian Silveri: to go?
[00:02:03] Trevor Anderson: Yeah,
[00:02:04] Trevor Anderson: yeah, sounds good. Nice to meet you guys.
[00:02:07] Trevor Anderson: Nice to meet everyone. I'm Trevor.
[00:02:10] Trevor Anderson: I'm also a new engineering manager on the data engineering side.
[00:02:16] Trevor Anderson: Started around April and
[00:02:20] Trevor Anderson: supporting our data engineering team
[00:02:23] Trevor Anderson: here at Flume.
[00:02:31] Sherif Elzanie: Dan,
[00:02:36] Dan Duling: In case someone doesn't know who I am.
[00:02:38] Dan Duling: I'm Dan, you know, our interim cto, but over oversee our, you know, engineering teams for all the adflow side as well as some of the others.
[00:02:50] Dan Duling: But you know, wanted to get this group together just because we know that with the replacement of, you know, concur, we do have some, you know, work that potentially might need data engineering and data and our core engineering teams and product teams to kind of work together to figure out a few needs.
[00:03:14] Dan Duling: So I thought it might be a good idea for Sherif to go ahead and give an overview of what's, you know, looking to happen and then let us take this back and figure out how it can work into our workflow and then also, you know, figure out any kind of timing as it, you know, depending on the timing, it might impact some of our current, you know, okrs.
[00:03:39] Dan Duling: Depending if this is a Q3 or Q4 initiative.
[00:03:43] Dan Duling: So we haven't made any, you know, you know, decisions on how we would do the integration.
[00:03:50] Dan Duling: But you know, we could potentially integrate directly with the, the Pulse platform or we could actually do the full integration directly out of Databricks.
[00:04:01] Dan Duling: So it just kind of depends on what we hear, what those needs are.
[00:04:05] Dan Duling: So that's why I have the whole group together. Any questions before kicking off?
[00:04:15] Brian Silveri: Thanks, Dan.
[00:04:15] Sa Ho: Questions, but I haven't introduced myself. Hi everyone, My name is SA.
[00:04:19] Sa Ho: I am your NetSuite technical lead.
[00:04:23] Sa Ho: I am typically involved in anything involving NetSuite and since this is Sherif Netsuite is involved.
[00:04:32] Sa Ho: My goal since I started last year is to make sense of our varied integrations.
[00:04:39] Sa Ho: And this one has a lot of integrations it looks like.
[00:04:44] Sa Ho: I'm sure we'll be chatting more in the future about all that fun stuff.
[00:04:50] Sherif Elzanie: Thank you, Saad. Mike, are you there?
[00:04:59] Dan Duling: I'm here.
[00:05:00] Mike Jacoby: Hey everybody.
[00:05:01] Mike Jacoby: Well,
[00:05:01] Mike Jacoby: is this not working? Let's see.
[00:05:04] Mike Jacoby: There we go.
[00:05:05] Mike Jacoby: Hey buddy. I'm the controller for Fluent.
[00:05:08] Mike Jacoby: I'll just be listening in. If you guys need me, let me know.
[00:05:11] Mike Jacoby: Nice to meet everybody.
[00:05:14] Sherif Elzanie: All right, thanks Mike. So let me get this going.
[00:05:22] Sherif Elzanie: So just to give everyone kind of like a brief overview of why we're changing, we'll just go over this very quickly.
[00:05:30] Sherif Elzanie: Main reason is around a lot of the friction that we've gotten on the adflow side specifically around invoicing.
[00:05:42] Sherif Elzanie: We have, you know, there's been a couple cases where partners just don't get paid for months just because they don't send us invoices and, and, or confirm over email.
[00:05:53] Sherif Elzanie: So this is just cause like a backlog of payments and a lot of like in general just like manual work from both internally and externally.
[00:06:07] Sherif Elzanie: And then yeah, like I was alluding to, we currently have two workflows that adflow partners could send their bills.
[00:06:16] Sherif Elzanie: We have, you know, I believe they either have access to like an external facing platform or they confirm or payout amounts and, or connect with someone from the adflow team to confirm how much they're able to invoice.
[00:06:36] Sherif Elzanie: They send us either a bill or they confirm over email.
[00:06:40] Sherif Elzanie: Some of the partners have been like very reluctant for some reason to send us bills.
[00:06:45] Sherif Elzanie: So we also have a separate workflow built out and we have that built into their contract where it gets sent invoice and then third, I mean concur is just not the right fit for us.
[00:06:56] Sherif Elzanie: We're just kind of fed up and you know, just overall it's not A good fit for like what we're trying to do and what we're trying to automate in the future.
[00:07:11] Sherif Elzanie: So I'll just go to like my vision for, you know, hopefully what we could integrate here.
[00:07:19] Sherif Elzanie: I main focus is on the second one. I'll just touch really quickly. We have two other inputs for bills.
[00:07:26] Sherif Elzanie: One is vendor invoices, which is the traditional invoicing that we have today.
[00:07:30] Sherif Elzanie: And then as the others on the reward side. But we'll focus in on this second item right here.
[00:07:37] Sherif Elzanie: So I guess I shared the, the doc, the workflow doc the other week.
[00:07:44] Sherif Elzanie: But mainly the vision here is to use netsuite as to feed databricks and or minion pulse to netsuite where we'll you know, establish like a staging review phase and then push that data into Topolti to generate the actual bills.
[00:08:11] Sherif Elzanie: So we'll get more into that.
[00:08:12] Sherif Elzanie: But any initial questions, concerns on, you know, what we're looking to do there?
[00:08:22] Sa Ho: Yeah, a few questions.
[00:08:24] Sa Ho: So I know you said we're focusing on the second box, but the first box and the third box, do they not go into netsuite until at the very end?
[00:08:34] Brian Silveri: Correct?
[00:08:34] Sherif Elzanie: Yeah, Both of those two are the endpoint.
[00:08:38] Sherif Elzanie: Like NetSuite just to record the actual like payable or payment.
[00:08:44] Sa Ho: Could you pull up that process flow that you shared?
[00:08:48] Sherif Elzanie: Yep, I could show that in.
[00:08:54] Sa Ho: Okay, so here at the top you can see NetSuite and Green System of Record.
[00:09:00] Sa Ho: But in the second column it says data bricks, source of truth.
[00:09:03] Sa Ho: Your system of record is your source of truth. So which one is it?
[00:09:08] Sherif Elzanie: I guess it's like source of truth. What I meant by that was more about like which platform.
[00:09:14] Sherif Elzanie: And I guess this is like more of a discussion point in terms of like which platform should be the source of truth for payout amounts.
[00:09:24] Sherif Elzanie: And I, I don't know.
[00:09:26] Sherif Elzanie: I know that was like something that we were a little bit unsure about whether that should be minion or databrick.
[00:09:33] Brian Silveri: Databricks. I can tell you now the answer is databricks.
[00:09:37] Sherif Elzanie: Gotcha. And what's, and what's the relationship between the two? Like, I, I don't understand.
[00:09:41] Sherif Elzanie: I'm not sure in terms of like.
[00:09:44] Brian Silveri: Sure. So databricks is the data repository of all minion related data.
[00:09:56] Brian Silveri: So that's setup data and event data.
[00:09:59] Brian Silveri: Minion in and of itself is an eventing platform that serves up the content and can set that up for our adflow product line.
[00:10:10] Brian Silveri: So they send all of their data to us.
[00:10:13] Brian Silveri: They also have reporting, which is what was being used historically to say oh, FYI, here's how much revenue a particular partner had or advertiser or what have you.
[00:10:22] Brian Silveri: We've now made it so that databricks is the source of truth.
[00:10:25] Brian Silveri: That includes the partner contract terms which, which Minion does not currently utilize.
[00:10:32] Brian Silveri: So that is why databricks would be the source of truth for all partner related revenue entities going forward, in my opinion.
[00:10:42] Sa Ho: Okay, so if databricks is the source of truth for Minion Pulse appflow, what is the source of truth for Everflow and the third one, vendor invoices, what
[00:10:53] Sherif Elzanie: is the source of truth? I mean everflow is like the reporting platform, right?
[00:10:59] Sherif Elzanie: So whatever is being reported in there is like considered the source of truth, I guess.
[00:11:06] Sherif Elzanie: But they have like a native function where we're able to like generate bills and push that data into NetSuite.
[00:11:13] Sa Ho: Okay.
[00:11:14] Sa Ho: And then that means NetSuite is not the system of record, it is the separate systems that are the system of record because they are the source of truth.
[00:11:22] Sa Ho: So then is that why we have this approval at the orange boxes?
[00:11:27] Sa Ho: Is that why we're having that all of those in the topality platform, because it's coming from three separate sources as opposed to going all three going into netsuite as the staging, which is the green box in the second column where we could do these approvals, the three tier three step reconciliation that needs to happen.
[00:11:52] Sa Ho: Is that the reason why?
[00:11:54] Sa Ho: Because it's very inefficient that some of the data is going into netsuite in the second column and then some of the data is not.
[00:12:02] Sa Ho: But eventually everything ends up in netsuite.
[00:12:05] Sherif Elzanie: So yeah, I think the thought behind that was that we were originally thinking about plugging databricks or having some sort of customization to push that data to PULTE directly.
[00:12:20] Sherif Elzanie: But there was like some concerns around approvals. Right.
[00:12:25] Sherif Elzanie: Like I'll get into this in a bit but basically there's times where potential, like I think there was a case recently where there was like some sort of data mismatch between what was in the platforms and what was, sorry, what was externally facing versus what was internally facing.
[00:12:45] Sherif Elzanie: So we'd want some sort of, we'd want to allow some sort of control with approvals to like the adflow team to like in those cases.
[00:13:00] Sherif Elzanie: Right.
[00:13:01] Sherif Elzanie: Like if there's something wrong with like what's coming from databricks, obviously we'll have like approvals built in.
[00:13:06] Sherif Elzanie: But ultimately we want to use Topoli Azer AP platform just because of like all the like automations that they have on their Side like the native connectors between like everflow.
[00:13:20] Sherif Elzanie: Right.
[00:13:20] Sherif Elzanie: Like NetSuite doesn't have anything native that ISO that could plug or generate bills directly into the system and then also just like the payment savings.
[00:13:31] Sherif Elzanie: So you know, there's like we're able to issue international ACH, which we can't do in NetSuite.
[00:13:38] Sherif Elzanie: In order to issue international ACH, you need to have a bank in each of those countries that you're paying to and we pay especially on the reward side to like multiple countries.
[00:13:49] Sherif Elzanie: So there's just like a lot of reasons why we chose Topolti.
[00:13:54] Sherif Elzanie: But yeah, essentially we just needed some sort of middle ground between databricks and Topolti and this was an idea that we, we briefly discussed that we want to like hash out to see if it's like feasible.
[00:14:11] Sherif Elzanie: It didn't need to be netsuite, but I, I just. Yeah, that's.
[00:14:16] Sa Ho: So I still have plenty of questions and concerns around the data flow of it all.
[00:14:20] Sa Ho: But I think we can come back to another time not take up the whole call for that.
[00:14:24] Sa Ho: So thank you for that.
[00:14:26] Sherif Elzanie: Yep, no problem.
[00:14:27] Sherif Elzanie: And I think I sent you that sheet which has like all the different fields that we're looking to and this is like confirmed by Topolti, this is what they need and this is what I think we could source it from.
[00:14:39] Sa Ho: So yeah, I took a look at that and I mean like I said we can discuss this another time.
[00:14:44] Sa Ho: But my opinion right now is either NetSuite is that middle ground that hosts all three data or it's only at the end like it's inefficient and data wise doesn't make sense for it to be doing this two step tango thing.
[00:15:01] Sherif Elzanie: God. Okay, yeah, we could definitely discuss that
[00:15:05] Dan Duling: I guess, you know, just a question since it's new for me.
[00:15:09] Dan Duling: I mean this is a, an issue that we have today with Concur anyways. Right.
[00:15:14] Dan Duling: Like that inefficient process is going on right now. Correct.
[00:15:21] Sherif Elzanie: It's I guess inefficiencies in, in very different ways in that there's no data flowing to nets. We.
[00:15:27] Sherif Elzanie: It's all, I'm sorry to concur. It's all like manual bills that we're getting via PDF.
[00:15:33] Sherif Elzanie: So I guess a different kind of problem. But yeah, that's, it's.
[00:15:41] Sherif Elzanie: There's no data flowing to concur right now.
[00:15:43] Sa Ho: Yeah.
[00:15:44] Sa Ho: So this is establishing a new process, a new flow to address some of those manual concerns in addition to improving the workflow itself for the AP team
[00:15:56] Sherif Elzanie: and, and also Just one thing to notice that. Sorry, this, this flow right here is on a monthly basis.
[00:16:02] Sherif Elzanie: We don't need it on a daily basis. All partners right now invoice monthly. So, you know, it's.
[00:16:10] Sherif Elzanie: I wouldn't, you wouldn't need to expect like a daily feed or something like that, but just wanted to throw that out there.
[00:16:28] Sa Ho: Okay. Yeah, I think we can jump back into the other.
[00:16:31] Sherif Elzanie: Yeah, cool. So, yeah, so this is kind of what I was referring to with, with sa.
[00:16:38] Sherif Elzanie: Like I sent over a bunch of different fields that I think we'll need actually that I know will need to generate the bills and where I expect those, those fields to come from.
[00:16:54] Sherif Elzanie: So there's just like a few of these fields that are coming from databricks.
[00:16:59] Sherif Elzanie: I don't have the exact naming of all these fields, but you know, if there's any way we could, you know, get confirmation on that, like whenever we do push those through.
[00:17:14] Sherif Elzanie: But that this is usually like based, you know, the basic amounts of the basic things that we're looking at.
[00:17:25] Brian Silveri: That's easy. And I can take that action.
[00:17:28] Sherif Elzanie: Okay, perfect.
[00:17:33] Sherif Elzanie: And then from the netsuite side, you know, basically what we were looking at before was that custom record.
[00:17:40] Sherif Elzanie: Right. Where all this data from databricks we're hoping is going to be able to flow through.
[00:17:47] Sherif Elzanie: So that will, you know, require us to map it to a vendor. Right.
[00:17:54] Sherif Elzanie: So that's also something that I think I brought up in a later slide is I guess I don't know if that, that mapping has to.
[00:18:04] Sherif Elzanie: Is going to exist on the netsuite side or if it's something that we could input on the databrick side.
[00:18:10] Sherif Elzanie: But there is a unique identifier for
[00:18:13] Brian Silveri: each,
[00:18:16] Sherif Elzanie: I guess partner ID or partner.
[00:18:19] Sherif Elzanie: I know you guys at least and Minion, because I'm not as familiar with databricks, I know that you roll, roll it up until like a traffic like each partner ID could roll up into like a traffic partner, if I'm remembering correctly.
[00:18:36] Brian Silveri: But
[00:18:38] Sherif Elzanie: yeah, I think for the most part one part traffic partner will map to one vendor. Vendor on.
[00:18:46] Sherif Elzanie: In netsuite. Okay, so we went over this.
[00:18:56] Sherif Elzanie: Sorry, I'll pause if there was like any questions because I know we were like going back and forth on this.
[00:19:05] Sa Ho: I mean, I definitely have a lot of questions. Yeah, so yeah, we can discuss it separately.
[00:19:12] Sherif Elzanie: Gotcha, gotcha.
[00:19:20] Sherif Elzanie: So I guess looking at, you know, just like what we're kind of looking at from like an audit perspective, I think you mentioned Brian, in terms of like the calculation and logic in databricks and how that's being maintained.
[00:19:39] Sherif Elzanie: You know, that will definitely be in scope for something, you know, in terms of like something we'll need to ensure is, you know, being properly maintained from like an audit perspective because everything just becomes in scope at this point.
[00:19:55] Sherif Elzanie: Um, so I know we'll, we'll talk through this a lot and you know, whether or not we go end up moving forward with this but you know, netsuite staging, that'll be a big layer as well in terms of like getting the right approvals.
[00:20:09] Sherif Elzanie: We haven't nailed down the specifics in terms of how things are going to flow in terms of, you know, approvals or modifications or anything like that, but that's something to consider there.
[00:20:21] Sherif Elzanie: And I'll leave these well to Pulte.
[00:20:24] Sherif Elzanie: You know, obviously we'll build in the workflows there and then from the system of record netsuite side, it's like, you know, it's pretty b.
[00:20:34] Sherif Elzanie: It's pretty much what we're doing currently with NET with concur. Any questions there?
[00:20:45] Dan Duling: I think the, the big thing that we'll just need to make sure we get to Aaron and Brian, you maybe they can work with you and so on is just going to be what, you know, data fields you're going to need output, you know, from the system.
[00:21:01] Dan Duling: And then because you know, we can, we can facilitate that from the start of this and then let you all run through the netsuite to pulte and back to netsuite, you know, flow.
[00:21:15] Dan Duling: But that's going to be the big thing that we'll just need is what are the fields that you're going to need to accomplish with this?
[00:21:22] Sherif Elzanie: Yep. Yeah, I have a good idea.
[00:21:23] Sherif Elzanie: I just don't know what they're called, but I'll, I'll circulate something so we could identify that.
[00:21:29] Dan Duling: You know, like you said, Aaron and you know, Brian and the other teammates too can probably, if you, you know, decide if you just do a quick meeting to go over that they can walk through and figure out what those fields are for.
[00:21:45] Sherif Elzanie: You gotcha.
[00:21:53] Dan Duling: What is the timing for this right now?
[00:21:59] Sherif Elzanie: I think we're just looking to get something signed as soon as possible in July and then from there, you know, we'll start scoping out more.
[00:22:13] Sherif Elzanie: So on the Topoli side, we obviously want to get the self billing side up and running as soon as possible, but yeah, it's what we're.
[00:22:26] Sherif Elzanie: We're hoping to sign something in July.
[00:22:31] Brian Silveri: Okay, and then when are you looking to have the first invoice date done?
[00:22:39] Brian Silveri: I guess I trying to work backwards from that date so I can plan properly here.
[00:22:46] Sherif Elzanie: I'd say for probably like August billing period.
[00:22:51] Brian Silveri: Okay.
[00:22:53] Sherif Elzanie: Which would get generated in beginning of September.
[00:22:56] Brian Silveri: Yep, got it. Thank you.
[00:22:59] Sherif Elzanie: Yeah, no problem. I'll touch on this really quickly, but I kind of alluded to it before.
[00:23:09] Sherif Elzanie: We're also thinking about, you know, that workflow within NetSuite so that there is like some level of control.
[00:23:15] Sherif Elzanie: We want some sort of oversight from when the adflow team sees that amount, could confirm it, match it to like the contract, make sure we're good there.
[00:23:25] Sherif Elzanie: And then if there's any modifications that.
[00:23:27] Sherif Elzanie: That gets like escalated within NetSuite to whoever needs to see it.
[00:23:34] Dan Duling: And in that world you're thinking like the account management team. More like the Melissa's.
[00:23:39] Dan Duling: And they have eyes on it.
[00:23:41] Brian Silveri: Got it.
[00:23:42] Sherif Elzanie: Yeah.
[00:23:42] Sherif Elzanie: And then that gets asked if they make any modifications that gets escalated for approval from there.
[00:23:49] Sa Ho: So where do you expect them to
[00:23:50] Sherif Elzanie: be making these modifications in NetSuite in that I guess we could call like pre billing stage.
[00:24:00] Sa Ho: So these account managers have access to whatever these records would be.
[00:24:05] Sherif Elzanie: Yeah, they should.
[00:24:06] Sherif Elzanie: I'm not sure if Melissa, but I think, you know, we do something similar on the revenue side now where it's like, I don't know if they have access currently, but do they have access
[00:24:18] Sa Ho: now or they will need access in the future?
[00:24:21] Sherif Elzanie: I'm not entirely sure. They might need access.
[00:24:25] Brian Silveri: Okay.
[00:24:25] Sa Ho: So on the revenue side they do not directly touch the records. We have a.
[00:24:31] Sa Ho: In NetSuite terms, it's called a suite lit, but everybody calls it a portal.
[00:24:34] Sa Ho: So it's just a custom webpage that we created. We slash our partners.
[00:24:41] Sa Ho: That allows them to see the data in a certain way so they don't directly interact with the actual records itself.
[00:24:47] Sa Ho: However, caveat. I don't know how many of them actually do what they need to do on the portal.
[00:24:53] Sa Ho: I think they have their BPO person doing it. So there's also that.
[00:24:58] Sa Ho: And one of the things that we on the financial side really need to work on is the person that doing the work needs to be the person who will face the consequences of entering poor data.
[00:25:10] Sa Ho: Just keep that in mind, please.
[00:25:12] Sherif Elzanie: Got it. And what do you mean by that? Like in terms of if they.
[00:25:16] Sa Ho: So by that, I mean the ams should be the ones that are accruing the revenue in the portal, not the BPO person.
[00:25:24] Sherif Elzanie: Got it. Okay. Yeah.
[00:25:25] Sherif Elzanie: I mean I would expect, I wouldn't expect them to use BPO at all because like the volume is like a lot less compared to the revenue side.
[00:25:33] Sherif Elzanie: So I mean the volume in terms of like the number of vendors are managing, so I wouldn't expect them to, to use any BPO resources, but I got you on that.
[00:25:46] Sa Ho: Okay.
[00:25:47] Brian Silveri: Yep.
[00:25:55] Sherif Elzanie: So yeah, so I guess from, from, you know, just in, in general we're really just trying to make sure that, you know, I, I forwarded this on to, to Pulte as well.
[00:26:07] Sherif Elzanie: I have a call with them tomorrow, so. I know you're joining that. I know.
[00:26:15] Sherif Elzanie: Is, is there anybody else who needs to be on that call or if there's any questions?
[00:26:20] Sherif Elzanie: I, I feel like for the most part you guys are interfacing with NetSuite so the connection is really on the NetSuite side, but I'm more than willing to, you know, add people to the call as needed.
[00:26:35] Brian Silveri: I think really it's just two integrations with netsuite.
[00:26:39] Brian Silveri: One to Pulse for the partner ID to be inside Pulse and then one to Databricks.
[00:26:46] Brian Silveri: And I think that would cover the integrations here with your proposed flow and have the data match and sync.
[00:26:53] Brian Silveri: So that question D. I think you had, Sherif is like, how do you know the NetSuite ID?
[00:26:58] Brian Silveri: We'll have Pulse integrate with NetSuite to link ID to partner name and then pass that to databricks.
[00:27:06] Sherif Elzanie: Okay. Okay.
[00:27:10] Jack Hall: You're saying
[00:27:11] Jack Hall: that
[00:27:12] Jack Hall: record gets created or modified in netsuite
[00:27:15] Jack Hall: and Pulse
[00:27:16] Jack Hall: receives that as like a web hook event or something and doesn't update
[00:27:22] Brian Silveri: either direction? Jack, Because I don't know.
[00:27:24] Dan Duling: I.
[00:27:25] Brian Silveri: This is really a partner success team process.
[00:27:27] Brian Silveri: Question is, when CVS signs their contract and we create a new partner in NetSuite and in Fluent, I'll just say influent.
[00:27:37] Brian Silveri: What's the process? I think we just need to harden that process, Jack, and say here is what you do.
[00:27:43] Brian Silveri: You go to Pulse and you create an impulse and it will create an ID in NetSuite or you go to NetSuite and it will create an ID and pulse something that there is a process that you do at one time.
[00:27:53] Brian Silveri: It's linked and people are not replicating the same ID or name in four different platforms.
[00:28:00] Sa Ho: So in NetSuite we have looks like three restlets.
[00:28:03] Sa Ho: That's a REST based integration that's kind of like a self serve minion or whatever platform would ping it.
[00:28:12] Sa Ho: And in this case it looks like it provides advertisers, line IDs and customers.
[00:28:18] Sa Ho: Currently those integrations were created before I got here, so I don't know the specifics.
[00:28:25] Dan Duling: As you say, Brian, I think we already have you know, those pieces in place, so nothing that we'd have to do or change on that side.
[00:28:32] Dan Duling: This is just more whatever we need to add in for the databricks aspect.
[00:28:38] Brian Silveri: Perfect.
[00:28:45] Sherif Elzanie: Okay. Any other questions or concerns? Any questions?
[00:28:59] Sa Ho: So I guess, Sherif let's get some. A meeting possibly before we meet with the Palti tomorrow.
[00:29:08] Sa Ho: If that works for your schedule with you, me and whoever helped you with the data flow.
[00:29:16] Sherif Elzanie: Got it. Yeah, that. Yeah, it was. I mean, Dan came up with this. You know, we.
[00:29:21] Brian Silveri: We.
[00:29:21] Sherif Elzanie: Me and Dan kind of talked about it, but like I feel it was, you know, we're. I.
[00:29:25] Sherif Elzanie: I built all the like workflow docs and stuff like that. So.
[00:29:31] Sherif Elzanie: Yeah, whoever needs to be on that call, we. We could talk through it.
[00:29:34] Brian Silveri: Okay.
[00:29:35] Sa Ho: Yeah, you could get that set up, please?
[00:29:38] Sherif Elzanie: Yeah, let's do it.
[00:29:41] Brian Silveri: Great. Thank you all. I appreciate it. Sherif and team. Yep.
[00:29:44] Sherif Elzanie: All right.
[00:29:46] Sa Ho: Thanks everyone.
