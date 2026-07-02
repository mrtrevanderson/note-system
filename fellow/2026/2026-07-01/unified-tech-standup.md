---
title: Unified Tech Standup
date: 2026-07-01
time: "07:00-07:30"
participants: [Dan Duling, Adrian Stack, Jack Hall, Arbi Anjargholi, Trevor Anderson, Dave Van Herten, Evan Sendra, Erin Manchester]
fellow_url: https://fluentco.fellow.app/meetings/040000008200E00074C5B7101A82E00807EA07019EF61DDB77F3DC010000000000000000100000005B4EEDE2514D4D479472509650FBA0C3/
captured_at: 2026-07-02T12:10:07Z
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
[ ] Loop Brian into the consolidated PACER report project and coordinate with Jason to ensure no overlap with cross-business unit reporting metrics.
[ ] Brian Silveri to send out Databricks new features and conference announcements to all teams (DevSecOps, Engineering, Product, Audience Solutions) by end of business today.
[ ] Review the Databricks new features documentation and provide feedback on value proposition, desired capabilities, or concerns for contract negotiation planning.

---

The tech team held their weekly standup to share progress updates across multiple departments. The **SDK team is preparing a deployment next week** with PII filtering and migrating the Android SDK to Maven repository to meet CVS security requirements. **Core development released version 1.101** smoothly with ROAS fields (feature-flagged in production) and in-store support enhancements, while starting a new sprint focused on link minification. Data engineering is building a **canonical advertiser dimension** to unify data across source systems for cross-business unit reporting, with AppsFlyer integration and audience solutions pipelines ready for production deployment.

Data science reported **promising results from the deep learning conversion model** and diversity engine tests, with latency issues successfully resolved. The leadership team met with Databricks senior leadership to address April-May platform issues, securing commitments for improved observability and faster escalation processes. The team will review new Databricks features announced at their recent conference to inform upcoming contract negotiations.

### SDK Team Deployment Progress

- The SDK team is **targeting deployment next week** with PII filtering functionality to enhance data privacy controls. [02:47]
- The Android SDK will be **migrated from AWS S3 to Maven repository** because CVS's security team refuses to whitelist the current domain, blocking their ability to pull the SDK. [02:57]
- Active coordination is underway with Dick's Sporting Goods team through email chains with Melissa to progress their integration. [03:28]

### BI Reporting & Data Integration

- **Phase one of Ryan's report is complete** with final components integrated via data engineering collaboration; results tested successfully and ready for production move next week. [03:58]
- Jason continues working with Matt on the CBR report request, currently in the QA phase for data validation and nearing completion. [04:34]
- The partner team order ID integration project is experiencing hiccups that JC is working through with the data ops team to streamline partner data submission and ADWA system ingestion. [04:48]
- The **Everflow transition for owned and operated** is delayed, with a post-holiday meeting scheduled to resolve hiccups; the main challenge is merging partners across systems when one system isn't fully solidified. [05:30]
- A consolidated PACER report is being prepared for Matt in collaboration with Carolyn, with due diligence underway during Carolyn's absence. [06:17]

### Core Development Release

- **Version 1.101 was released smoothly** this morning by Namat, including the final set of ROAS fields and in-store related support features. [07:49]
- ROAS fields are **visible in UAT but feature-flagged in production**, allowing controlled rollout of the new functionality. [08:00]
- A new sprint kicks off Monday with focus on **Q3 priorities including link minification** and related optimization work. [08:08]

### Data Engineering Infrastructure

- Building a **canonical advertiser dimension** that maps advertiser fields from TUNE Lead Manager, Minion(?), and NetSuite into one unified Fluentco(?) ID with standardized vertical taxonomy. [08:43]
- The canonical advertiser dimension will serve as the **foundation for unified cross-business unit reporting**, enabling consistent metrics across platforms. [09:13]
- **AppsFlyer data integration** is included in next week's release, pulling data into the Playful Rewards catalog for performance reporting. [09:22]
- Adding **position view P1 through P4 fields** to the advertiser report as part of next week's release to enhance reporting granularity. [09:44]
- **Two automated audience pipelines** (declared and modeled) built as Databricks programmatic pipelines are ready for production deployment next week. [10:08]

### Data Science Model Testing

- The **deep learning conversion model test is showing promising results**, but the team is holding off on changes until after the long weekend to avoid risk. [10:47]
- **Diversity engine testing continues** with good results; waiting to reach statistical significance on conversion rate impact for the override group before rolling to 100% traffic. [11:09]
- The team is conducting prep work and workshops for new quarter OKR approaches while experiments run. [11:41]
- GB and team successfully **resolved all latency issues**, significantly reducing timeouts in the ranking service despite added complexity. [11:49]

### Databricks Platform Management

- Dan, Trevor, and Brian met with **Databricks senior leadership** to address the multitude of platform issues experienced in April and May that impacted data science and themes(?). [12:15]
- Databricks leadership was remorseful and committed to **implementing observability** to prevent future incidents and providing faster escalation processes going forward. [12:34]
- Databricks requested an impact number to calculate service credits and asked for a list of most important models to add extra observability (answer: all endpoints). [12:57]
- Brian will distribute information on **new Databricks features** announced at their annual conference last week for all teams to review and identify value propositions for upcoming contract negotiations and cost planning. [13:25]

### Action Items

- [ ] Loop Brian into the consolidated PACER report project and coordinate with Jason to ensure no overlap with cross-business unit reporting metrics. [07:02]
- [ ] Brian Silveri to send out Databricks new features and conference announcements to all teams (DevSecOps, Engineering, Product, Audience Solutions) by end of business today. [13:25] (Brian Silveri)
- [ ] Review the Databricks new features documentation and provide feedback on value proposition, desired capabilities, or concerns for contract negotiation planning. [13:39]

## Transcript

[00:00:00] KW: Sa.
[00:00:33] Brian Silveri: Hey, Namat
[00:00:36] Dexter Sue: This.
[00:00:36] Brian Silveri: This might be a four person stand up. So
[00:00:43] Namat Katawazi: we.
[00:00:44] Brian Silveri: We might be able to cancel. But I did want to get a status update on the release.
[00:00:50] Namat Katawazi: Yeah, yeah, of course.
[00:00:52] Brian Silveri: All right. Hey guys, I actually totally forgot that
[00:00:57] Daniel Hernandez: Canada had off today.
[00:00:59] Brian Silveri: And.
[00:00:59] Brian Silveri: And don't forget, gentlemen, US is off the next two days, so you're not going to see your Canada brethren until Monday.
[00:01:11] Brian Silveri: In case you didn't see that in the. We have two days off for July 4th.
[00:01:18] Trevor Anderson: Thank you. Fluent.
[00:01:21] Jack Hall: Yeah.
[00:01:21] Brian Silveri: So please enjoy your long weekend.
[00:01:24] Namat Katawazi: They took all my holidays away when I moved to Portugal, they said I wasn't allowed anymore.
[00:01:30] Brian Silveri: So you're just working tomorrow. Good for you.
[00:01:33] Namat Katawazi: Yep. Seven days a week, actually.
[00:01:35] Brian Silveri: Excellent. All right, Jack. I've got a ton of stuff I need him to do.
[00:01:40] Namat Katawazi: A lot of weekends. It was terrible.
[00:01:44] Brian Silveri: There's lots of stuff. I expecting amazing productivity coming from.
[00:01:47] Dexter Sue: You
[00:01:50] Namat Katawazi: may go the other way.
[00:01:54] Brian Silveri: All right. All right, we've got. I think we got everybody that was on US side.
[00:02:04] Brian Silveri: Trevor and I are the only DE people left. Okay, that's covered.
[00:02:09] Namat Katawazi: I would have thought Baxter would have hopped on. He was messaging me this morning.
[00:02:14] Brian Silveri: He's here. I don't know where he's at, but he's here.
[00:02:18] Namat Katawazi: Yeah.
[00:02:20] Brian Silveri: All right.
[00:02:35] Brian Silveri: All right, why don't we get started? Who wants to go first?
[00:02:43] Daniel Hernandez: I could do the updates for the said team.
[00:02:46] Brian Silveri: I don't.
[00:02:47] Daniel Hernandez: Yeah. So SDK team we're targeting doing a deployment probably next week with the PII filtering.
[00:02:57] Daniel Hernandez: And in addition to that, we are going to be pushing the Android SDK to Maven's repository.
[00:03:04] Daniel Hernandez: Currently we have it hosted on our AWS S3 bucket, but CVS's security team doesn't want to whitelist our domain.
[00:03:15] Daniel Hernandez: So by pushing it to Maven they should be fine and they could pull it from there.
[00:03:20] Daniel Hernandez: So that's what we're working through to get that out there. And yeah, everything else is going well.
[00:03:28] Daniel Hernandez: We're also working with the Dick's sporting goods team and Melissa through the email chains trying to get that going.
[00:03:39] Dexter Sue: Great.
[00:03:42] Jack Hall: Excellent.
[00:03:43] Brian Silveri: Good work. Okay, who wants to go next? Everyone.
[00:03:50] KW: I can go next. And I'll. I'll kind of paraphrase for the candidate team as well. So we.
[00:03:58] KW: We've actually finalized one of the reports for Ryan.
[00:04:02] KW: Phase one, if you want to call it that, or phase two.
[00:04:05] KW: We just got important component in their position working with the data engineering team.
[00:04:13] KW: So I was able to put it in last night, tested the results and everything looks good.
[00:04:17] KW: So I got your note, Ryan. So yeah, I would Suggest probably next week.
[00:04:21] KW: We move that hopefully Ryan just commented he's happy with the results, so that's good.
[00:04:30] KW: Second, I know Jason.
[00:04:34] KW: Jason is still working with Matt on his request for the CBR report that he's working on.
[00:04:40] KW: So that's still in progress.
[00:04:42] KW: Look like it's closer to completion, but they're in a QA process of the data right now.
[00:04:48] KW: I guess the other component is what's on our end as far as helping out the partner team.
[00:04:53] KW: We are actually working with the partner team on order IDs from the.
[00:05:00] KW: From the actual partners, bringing in their data into our entire system, so.
[00:05:03] KW: So we could be more knowledgeable for their order IDs. That's still in play.
[00:05:08] KW: There's some hiccups that JC is working through with the data Ops team, so I'm not exactly sure what those hiccups are, but he was trying to help out the situation where to get to make it easier for both the partners to send their information and for the ADWA system to be able to ingest that data properly.
[00:05:30] KW: We're working with the Everflow team for the transition.
[00:05:33] KW: For the owned and operated side, some things are still a little delayed on their end.
[00:05:38] KW: We're gonna have a meeting after the holidays to ensure what those hiccups are and come up with a process to bring that data in a more streamlined fashion.
[00:05:48] KW: The only issue that with that is we have some partners that are in there, some partners that are not in the other system.
[00:05:54] KW: And it's, you know, a headache to really try to merge those two entities together, especially when one of them is not truly solidified.
[00:06:02] KW: So we're going to work through that.
[00:06:04] KW: Work through that in the meantime and come up with the interim process to bring in the.
[00:06:09] KW: Those other vet partners. So this way reporting is not disrupted.
[00:06:14] KW: And I guess the last piece is we're working, right.
[00:06:17] KW: Not running Matt, he's looking for a consolidated PACER report that was constructed by my team.
[00:06:25] KW: We worked with Carolyn on this and I'm going to do some due diligence on that and get that to Matt for him to view on his end in the absence of Carolyn and one of my other employees.
[00:06:38] KW: That's it. From the BI reporting side.
[00:06:40] Brian Silveri: Kevin, can you from a. Two things.
[00:06:45] Brian Silveri: One, let me know if you need products help with the Everflow issues.
[00:06:54] KW: It's more. It's not even an issue. It's more of a process rollout. Yeah.
[00:07:02] Brian Silveri: And then for that PACER report, loop me in.
[00:07:06] Brian Silveri: And Jason, because I don't want it to overlap with the cross Business unit report thing which might have the same metrics.
[00:07:17] KW: No, this right here is a totally different report. It's a patient report. A unified patient report.
[00:07:26] Brian Silveri: I know it. Okay, great, great, great.
[00:07:31] Brian Silveri: Yeah, I just want to make sure because I've been looking at that automated pacer a lot lately.
[00:07:36] Brian Silveri: So if you need our help on that or if you think it overlaps in any way, please let me know.
[00:07:42] KW: You got it.
[00:07:43] Jack Hall: Great, thank you.
[00:07:45] Brian Silveri: Okay, who's next?
[00:07:49] Jack Hall: I can go. We this morning Namat got the release version for 1.101 out and everything seems smooth.
[00:08:00] Jack Hall: That includes a bunch of stuff like the last set of ROAS fields though those are currently hidden in production.
[00:08:08] Jack Hall: They're visible in uat, but we just feature flagged them in production and a bunch of other in store related support stuff got put in there and we're kicked off a new Sprint Monday.
[00:08:25] Jack Hall: So we're starting to dig into some of the work we need to do this quarter like link minification and whatnot.
[00:08:37] Brian Silveri: Great.
[00:08:40] Trevor Anderson: I could just give a quick update for on the data engineering side.
[00:08:43] Trevor Anderson: So right now we're building out a canonical advertiser dimension.
[00:08:50] Trevor Anderson: And so what we're looking to do is just map the various advertiser fields from across our different source systems here at Fluent.
[00:08:56] Trevor Anderson: So things like TUNE Lead Manager, Minion, netsuite, we're looking to map all the advertiser dimensions from these different platforms into one canonical Fluent ID with a standard vertical taxonomy.
[00:09:13] Trevor Anderson: And so yeah, this will be, you know, the foundation for our unified Cross business unit reporting.
[00:09:22] Trevor Anderson: Another thing that's going to be part of our next release here coming up next week is going to be we're bringing Apps Flyer data in.
[00:09:31] Trevor Anderson: So we, yeah, we made a connection to AppsFire and we're pulling in data into the playful rewards catalog as well for playful performance reporting.
[00:09:44] Trevor Anderson: And one more thing is we adding the position view P1 through 4 to the advertiser report.
[00:09:52] Trevor Anderson: And so yeah, I'll be upcoming as part of the upcoming release next week. So is there anything else?
[00:10:04] Trevor Anderson: I think that's, I think that's the main things coming up might be a
[00:10:08] Brian Silveri: few other things and we should be deploying our audience solutions infrastructure changes next week as well.
[00:10:17] Trevor Anderson: Oh yes, we have two automated pipelines, one for declared audience, one for modeled audience creation.
[00:10:26] Trevor Anderson: They're databricks pipelines that do the audience building as programmatic code in a databricks pipeline.
[00:10:35] Trevor Anderson: So we're excited to see those in production.
[00:10:39] Brian Silveri: Yep. Great. Wants to go next.
[00:10:47] Dexter Sue: Good. Next, Brian. All right, so for data science, we have two experiments still running.
[00:10:53] Dexter Sue: We have our deep learning conversion model test tests are running.
[00:10:58] Dexter Sue: So far results are looking promising but we're not going to make any sudden moves right before the long weekend.
[00:11:04] Dexter Sue: So that'll be a priority for next week. Will be suddenly designed to roll that across more traffic.
[00:11:09] Dexter Sue: We also still have our diversity engine testing going again. So far results are looking good.
[00:11:16] Dexter Sue: We just want to run it for a bit longer to make sure we get to stat sig in terms of the impact on conversion rates for our override group.
[00:11:29] Dexter Sue: So we're hoping over the weekend we'll get statsig and then we can roll that across 100% of traffic and and ship that okr.
[00:11:38] Dexter Sue: Then outside of that it's just prep work for the new quarter.
[00:11:41] Dexter Sue: So just doing workshops and our approaches to those OKR items.
[00:11:46] Jack Hall: Great.
[00:11:47] Dexter Sue: That's about it.
[00:11:49] Namat Katawazi: I think you forgot one thing. You guys fixed all your latency issues over the week.
[00:11:57] Namat Katawazi: The timeout went way, way down. You guys basically fixed it all. So good job.
[00:12:02] Dexter Sue: Yeah, GB and A SHEETA were doing a lot of work to try to bring down latency.
[00:12:06] Dexter Sue: We've we add a lot more complexity to the ranking service. So glad to hear Nat. Glad to hear.
[00:12:15] Brian Silveri: And we, we also met with the databricks senior leadership yesterday, Dan and Trevor and I and addressed the multitude of issues that we experienced in April and May that impact the data science and the theme.
[00:12:34] Brian Silveri: So they were remorseful and have things moving forward for lans to have observability in place so that we don't experience these type of things going forward and
[00:12:47] Trevor Anderson: hopefully a faster escalation on their end as well.
[00:12:53] Dexter Sue: Yeah. Great. Do we ever get a credit? Did that ever end up happening?
[00:12:57] Brian Silveri: They Dexter, they did ask for an impact number so that they could figure out what that credit would look like.
[00:13:05] Dexter Sue: Oh we gave a number.
[00:13:07] Brian Silveri: Just send it to me and I'll kick it over to them.
[00:13:10] Trevor Anderson: They also asked about the most important models that we have to add extra observability on them.
[00:13:16] Trevor Anderson: So that might be something we want to provide.
[00:13:19] Brian Silveri: The answer is all of them. All the endpoints, all of them.
[00:13:25] Brian Silveri: FYI I'll be sending this out to everybody but I'll be sharing the databricks new features and what they sent out at and presented at their annual conference last week.
[00:13:39] Brian Silveri: Please go through those because we want to figure out if anyone sees any value proposition for those capabilities, features, products that they have launched so that we can take into account for our new contract negotiation that we're doing and our expected cost and spend so I'll be sending that out today by the end of business today to everybody.
[00:14:05] Brian Silveri: I want everyone to look at it. So it'll be DevSecOps engineering product.
[00:14:10] Brian Silveri: I also send it to Audience Solutions because there's stuff there for them as well.
[00:14:13] Brian Silveri: So please don't hesitate, like, oh, this looks cool. I want. I want to learn more.
[00:14:17] Brian Silveri: Or I think this might help us. Or this makes no sense. Don't want to use this. Yeah.
[00:14:23] Brian Silveri: So I'll send that out today, and you can send me that feedback. All right. Anything else, Dan?
[00:14:35] Brian Silveri: Anything on your side?
[00:14:37] Dan Duling: No, nothing on my side today.
[00:14:40] Dan Duling: You know, hope everyone, you know gets to wrap up today and enjoy the long week.
[00:14:50] Brian Silveri: Great. Thank you all. Please enjoy your long weekend.
[00:14:54] Brian Silveri: Thanks, everybody.
