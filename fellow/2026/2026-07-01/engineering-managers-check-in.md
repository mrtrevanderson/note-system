---
title: Engineering Managers - Check in
date: 2026-07-01
time: "09:30-10:15"
participants: [Dan Duling, Jack Hall, Trevor Anderson]
fellow_url: https://fluentco.fellow.app/meetings/040000008200E00074C5B7101A82E00807EA07012E9FD8F857F3DC01000000000000000010000000DA5E98002F448B48820843F9D2B09AC6/
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
[ ] Create cards for Nimat to work with DSO on scaling down over-provisioned Kubernetes resources to save several thousand dollars monthly.
[ ] Sync up on Ramy's role change and begin the backfill process for the open position.
[ ] Roll out job descriptions for the backfill positions (senior and mid-level roles) for both Canada and US markets.
[ ] Finalize Ramy's role change with Patrick when he returns on Monday, July 6.
[ ] Coordinate with DSO (Griggs or SM) to swap credentials for the partner deletion request processor to resolve the ILM policy creation issue.

---

The meeting covered operational updates and planning across engineering teams. Trevor raised concerns about the **Audience Solutions team needing clearer roadmaps and better communication** between engineers to avoid duplicated work, with Dan suggesting improved meeting cadence and better involvement of JC in card management. Jack reported the recent release is stable and identified an opportunity to **save several thousand dollars monthly by right-sizing over-provisioned Kubernetes clusters**.

The team discussed **Ramy's transition to a principal systems architect role**, which will open backfill opportunities and allow Nimat to step into more of a tech lead position. Dan shared updates from Cannes Lions, including the Built/Beyond rollout scheduled for August and new partnerships with major retailers. On the AI front, the company achieved **87% Claude adoption** and has saved an estimated **$120-180K through workflow automation**, with a Fluent Challenge launching in July to encourage further innovation. A minor technical issue with the partner deletion processor was identified but deemed non-urgent.

### Audience Solutions Team Coordination

- Trevor identified the need for **clearer roadmaps and action items** for the Audience Solutions engineering team, noting engineers sometimes get stuck hypothesizing different paths or duplicate work due to unclear communication. [09:38]
- A new **JIRA board template with required fields** is being designed to improve organization, with better integration into data pipelines to help the team track progress more effectively. [10:45]
- Dan emphasized that **JC should be pulled into planning discussions** since JC was assigned to handle card management and day-to-day requests, while Brian focuses on product initiatives. [11:39]
- Audience Solutions will continue handling **manual data engineering work** (pulling and creating new pools for Liveramp(?)) until full end-to-end automation with quality control is achieved. [12:15]
- Dan suggested adding a **10-minute Audience Solutions standup** immediately after the core data engineering standup (which Brian also oversees) to maintain daily accountability while keeping both teams aligned. [14:00]

### Ramy's Role Transition

- Ramy is transitioning to a **principal systems architect role**, taking on system-level architecture responsibilities previously held by DVH, including ensuring plans across groups work together holistically and documenting how systems interconnect. [20:15]
- **Nimat expressed interest in a tech lead position** when it opens up, which aligns with his current contributions and would allow him to step up from senior engineer to a more principal-level team lead role. [19:34]
- Ramy's move will create a **backfill opportunity** for either a regular engineer or senior engineer, depending on what the market offers, to replace his individual contributor responsibilities on the core team. [21:35]
- Ramy is currently working on the **event processing pipeline architecture for In Store** (Kafka-based), which will incorporate improvements from Pulse 2.0 and allow him to focus on bigger, fuzzier initiatives rather than smaller cards. [22:03]
- Dan will prepare job descriptions for both senior and mid-level roles, posting in both **Canada and US markets** to do an apples-to-apples comparison, though recent US-based hires have generally outperformed in data engineering roles. [24:01]

### Kubernetes Infrastructure Optimization

- Nimat discovered the infrastructure is **significantly over-allocated** across Pulse, Public API, node pools, and Kubernetes clusters after reviewing the current provisioning levels. [17:23]
- Right-sizing the clusters could **save several thousand dollars per month**, as it appears resources were scaled up during past performance issues but never scaled back down after the issues were resolved. [17:42]
- Jack will create cards for Nimat to work with DSO on scaling down resources, prioritizing this over more complex auto-scaling implementations since immediate cost savings are achievable through simple right-sizing. [18:08]

### AI Adoption Progress

- The company achieved **87% active user adoption of Claude** (up from 74% in April), with multiple users now creating their own skills and automations using the platform. [28:14]
- Workflow automations have generated an estimated **$120-180K in savings** based on work shifted to offshore resources, demonstrating significant ROI from AI adoption efforts. [28:43]
- **Default AI models were changed to lower-cost options** (Opus 4.8 and Sonnet 5) for general chat and research, while users can still upgrade to higher models when needed for specific tasks. [29:11]
- Jason Warnock's team used AI to completely rebuild the **partner portal** (including partner performance and cross-business unit reports) according to Conlin's specifications, demonstrating end-to-end AI-driven development. [30:02]
- A **Fluent Challenge is launching around July 7** where employees can submit skills or projects automating recurring tasks, with prizes ranging from $3,000 to $8,000 based on impact, with buy-in secured from HR and Finance. [30:32]

### Cannes Lions Updates

- The **Built/Beyond rollout was announced for August** at Cannes Lions, with retail partners increasingly leaning into post-transaction and loyalty programs as key strategies. [26:14]
- The conference strengthened existing partnerships with **Dick's Sporting Goods, Walmart, Rakuten, and Cefas(?)**, with discussions about Rakuten potentially advertising on Post or In Store platforms. [26:54]
- Dan emphasized keeping the **Cefas(?) relationship close** since it's the easiest target for Rock (competitor) to steal, and losing one partner could lead to losing many. [27:17]
- New pipeline opportunities emerged with **Lyft, Kroger, Marriott, Expedia, Roku, and Fox**, with Kroger currently progressing through security review stages. [27:35]

### Release Status

- The recent release appears **stable with only minor, non-urgent issues**, providing confidence that the team can move into the long holiday weekend without major concerns. [17:01]
- New **ILM (Index Lifecycle Management) policies** were added to help trim down growing indexes in Elasticsearch, but some non-admin credentials need to be updated before the policies can be created. [34:09]
- The **partner deletion request processor isn't starting** due to credential permissions issues, but this is non-urgent since the company has 30 days to respond to requests and the queue is still filling normally. [34:52]
- Jack will coordinate with DSO (Griggs or SM) to swap out the credentials, targeting Thursday (July 2) rather than Wednesday due to Griggs being out and Damara dealing with hospital matters. [35:19]

### Action Items

- [ ] Create cards for Nimat to work with DSO on scaling down over-provisioned Kubernetes resources to save several thousand dollars monthly. [18:03] (Jack Hall)
- [ ] Sync up on Ramy's role change and begin the backfill process for the open position. [19:04] (Jack Hall, Dan Duling)
- [ ] Roll out job descriptions for the backfill positions (senior and mid-level roles) for both Canada and US markets. [23:49] (Dan Duling)
- [ ] Finalize Ramy's role change with Patrick when he returns on Monday, July 6. [24:03] (Dan Duling)
- [ ] Coordinate with DSO (Griggs or SM) to swap credentials for the partner deletion request processor to resolve the ILM policy creation issue. [35:14] (Jack Hall)

### Decisions

- **Ramy is transitioning to a principal systems architect role**, focusing on system-level architecture and documentation, which opens a backfill opportunity for his current IC responsibilities. [20:15]
- The **Built/Beyond rollout was announced for August** at Cannes Lions, with retail partners emphasizing loyalty and post-transaction strategies. [26:14]
- **Default AI models were changed to lower-cost options** (Opus 4.8 and Sonnet 5) for general use, reducing costs while allowing users to upgrade when needed. [29:11]
- A **Fluent Challenge will launch around July 7**, offering prizes of $3,000-$8,000 for impactful AI automation projects, with HR and Finance approval secured. [30:43]

## Transcript

[00:00:51] iPhone: It.
[00:01:16] Dan Duling: Sam, it.
[00:06:21] iPhone: Hey, Doug, can you hear me?
[00:07:27] Dan Duling: Hey, guys, How y' all doing? Can you hear me?
[00:07:36] iPhone: Hey, dance. Can you hear me? Yeah, yeah, just doing good. Headed to the airport though.
[00:07:44] iPhone: So I'll probably just be with, you know, just on the, on the phone here, so.
[00:07:50] iPhone: But just been listening into this one.
[00:07:54] Dan Duling: Yeah. You know, with us going into, you
[00:07:57] iPhone: know,
[00:07:59] Dan Duling: I know it's, you know, it's a holiday, right, for, you know, Canada.
[00:08:03] Dan Duling: So, you know, I'm sure, you know, that makes it a little bit lighter for, for both of you all, you know, today, you know, we can kind of, you know, keep this one a little, you know, a little briefer.
[00:08:17] Dan Duling: You know, also we're going into the holiday, so I don't want to have a bunch of deliverables that come out of this and you know, end of the day we're not going to look at it until, you know, next week.
[00:08:27] Dan Duling: So I guess high level, any major red flags that either one of you all are experiencing right now or, or, you know, that's kind of come up in the past, you know, week and a half unmuted.
[00:08:45] Jack Hall: My audio was muted somehow. Whoops.
[00:08:54] Dan Duling: Could you hear me? Oh, you could hear me though, right?
[00:08:56] Jack Hall: No, my speakers were muted. That's why I wasn't paying any attention.
[00:09:02] Dan Duling: All right. Yeah.
[00:09:03] Dan Duling: All I was saying is, you know, with, you know, Canada being out on a holiday today, you know, I'm guessing, you know, pretty going to be a lighter day for everyone.
[00:09:11] Dan Duling: So just kind of let's focus on, you know, wrapping up anything.
[00:09:14] Dan Duling: We don't need to talk about new deliverables for this meeting.
[00:09:17] Dan Duling: I was hoping for just kind of getting a pulse on, you know, let's give it two things.
[00:09:22] Dan Duling: You know, a, any red flags that, you know, you are seeing within the team or be anything that is blocking the team that I can, you know, try to push on before we, you know, kick into the, the long weekend.
[00:09:38] iPhone: I think the, you know, there's one thing that's been surfacing as of recently is just, I think we need to find a way on the audience solution side to have a clear roadmap for the, for the developers or the engineering team on that side.
[00:09:56] iPhone: I think I've been seeing, you know, just a few times, like, we can kind of get stuck in this pattern of like hypothesizing or thinking about different paths.
[00:10:11] iPhone: I think having like clear action items is good for them to be able to make progress and work towards something.
[00:10:18] iPhone: And I know this stuff exists, but also, you know, just having clear communication between, between the three engineers as well.
[00:10:29] iPhone: You know, I think a few times I've seen like, you know, someone gets down a path and another one of the engineers is not, you know, aware of the work that has already been done and we have to keep going back and forth or going back to where we started and stuff like that.
[00:10:45] iPhone: But gonna be working with Brian also like just having better organization on their board as well.
[00:10:52] iPhone: I think we, we're designing a template for the, for the new cards on the audience solution side.
[00:10:58] iPhone: You know, having required fields that need to be filled in before something goes onto the JIRA board, being able to pick that up on, into the pipelines on data breaks and, and yeah, just having a better kind of process and operations on that team.
[00:11:14] iPhone: So but other than that everything else has been going well.
[00:11:18] iPhone: They've, you know, we've made a lot of progress on that side. So. Excited to see how Q3 unfolds there.
[00:11:26] Dan Duling: Yeah, I mean one suggestion too is like, you know, when you all are going through things like you know, even the other day when you all had the, the call, you know, make sure JC is, you know, pulled in.
[00:11:39] Dan Duling: Unless you know someone's going to take over the full scrum, you know, management, you know, and that someone I guess would be potentially you on it, you know, make sure that JC is involved because JC was put over that to do that card management.
[00:11:57] Dan Duling: So when, you know, so if you and Brian are having conversations of, you know, Brian's supposed to drive product initiatives, you know, JC is doing, there are day to day requests that still come in.
[00:12:09] Dan Duling: Remember, Audience solutions is not, you know, not everything is going to be, you know, automated.
[00:12:15] Dan Duling: They still have, you know, data engineers that will be, you know, pulling and creating new, you know, pools to, you know, import into load of me and everyone else that's just going to be kind of part of it unless we create a, you know, a full end to end automation that can QC and do everything appropriately.
[00:12:35] Dan Duling: But right now there's still a lot of human pieces that are needed there to validate that data, you know, make sure that they're staying within variances and other things of that nature and just getting, you know, eyes on it.
[00:12:48] Dan Duling: Because when we try to automate it in the past anyway, so far what we've noticed is it comes back and it doesn't perform because there might be, even though the fields there and populated it might not be the right value.
[00:13:01] Dan Duling: So you know, that's going to be a, you know, good management thing to keep an eye on.
[00:13:09] iPhone: Yeah, thank you for that. I think over communication here is going to be important.
[00:13:14] iPhone: You know, there's a lot of different pieces of ownership across different teams here and we all need to, you know, work together to make this product successful.
[00:13:24] iPhone: So.
[00:13:25] Dan Duling: But yeah, you know, I'd start with the cadence.
[00:13:29] Dan Duling: You know, they do the Monday, you know, kickoff meeting, right.
[00:13:33] Dan Duling: To, you know, do card allocation and everything else.
[00:13:36] Dan Duling: You know, it's probably worthwhile doing a, you know, a few follow ups, you know, to that, to, you know, validate with that team or since they do join the data engineering, you know, stand up, you know, it probably makes sense to, you know, and Brian is also over that.
[00:14:00] Dan Duling: Start off with, you know, the core data engineering team in that meeting and then move to, you know, the audience solutions stand up right after.
[00:14:11] Dan Duling: And then just that way we keep that accountability.
[00:14:15] Dan Duling: You know, on a day overday basis it would add, you know, it's going to add 10 minutes to it potentially, but you know, that's going to be a clean way to keep it.
[00:14:25] Dan Duling: And they're already coming to the conversation.
[00:14:29] Dan Duling: So it's just a better organization of how that meeting kicks off, I think.
[00:14:39] iPhone: Yeah, thank you. Appreciate the suggestions there. I think. Yeah, we'll take, I'll take some of that.
[00:14:48] iPhone: We'll apply to kind of our standard meetings that we have right now, I might add, as a secondary follow up because Virginia does run those Monday morning meetings.
[00:14:57] iPhone: I don't want to, you know, interrupt anything that she does currently. Right.
[00:15:01] iPhone: So, but, but yeah, I appreciate.
[00:15:06] Jack Hall: Cool.
[00:15:07] Dan Duling: Yeah. Let me know how I can help as you're kind of going through. Because I think that, you know.
[00:15:13] Dan Duling: Yeah, that.
[00:15:13] Dan Duling: I don't know, I think that will, you know, be beneficial to kind of streamline some of those.
[00:15:21] Dan Duling: I mean, it's a lot better than it was, you know, so I'm happy with, you know, with that because we are making progress basically, you know, day over day.
[00:15:39] Dan Duling: Anything, I guess on that front, then before then you can jump off so that you, while you're on the the road.
[00:15:46] Dan Duling: Anything, Trevor, that I can help out with right now on that front where I can push on.
[00:15:56] iPhone: You know, it was helpful for you to join that segment registry, you know, open discussion items.
[00:16:01] iPhone: I think there's going to be a few more of those moving forward as we go through the requirement documents that we have right now and the PRDs and you know, all the information that you provided to the, to the team.
[00:16:13] iPhone: So, you know, anytime that there is any decisioning that needs to be made.
[00:16:19] iPhone: Well, I'll probably set up some time between all the you know, stakeholders that are involved, but it's good to have your input on those items when they come up.
[00:16:29] iPhone: But yeah, other than that, everything is pretty straightforward and you know, I'll, I'll keep adjusting anything that I see, you know, during these meetings and we can go from there.
[00:16:42] iPhone: Perfect.
[00:16:47] Jack Hall: Awesome.
[00:16:48] Dan Duling: Well, thank you so much and you know.
[00:16:49] iPhone: Yeah, thank you.
[00:16:50] Dan Duling: Good thing to keep on the, the radar. That's, you know, good, good. Pulse.
[00:16:57] Dan Duling: How's everything going for you right now, Jack?
[00:17:01] Jack Hall: It's going well. The release seems stable. There's a few little things, but nothing urgent.
[00:17:11] Jack Hall: And then I did talk with NAMAT this morning and he said he's been looking at some of the applyings like he's been going through and looking at
[00:17:23] iPhone: the
[00:17:25] Jack Hall: like Pulse and public API infrastructure and node pools and all that stuff in Kubes and he thinks he's found that we're fairly over allocated in a lot of places.
[00:17:42] Jack Hall: So I'm gonna probably put some cards together to like have somebody, probably NAMAT work with DSO to get us to scale down some of what we have over provisioned in Kubernetes, because that'll save a couple thousand dollars a month.
[00:18:08] Jack Hall: So there's obviously a lot more we can eventually do towards like the putting in like auto scaling and stuff like that.
[00:18:15] Jack Hall: But right now it seems like it's more important to just go and get our clusters right sized because it feels like it's one of those ones where we must have in the past gone.
[00:18:30] Jack Hall: We're having performance issues.
[00:18:31] Jack Hall: Well, let's throw money at it until we can figure out how to fix them.
[00:18:35] Jack Hall: And then we fixed them and never move the sliders back to where they were before we just left them scaled up.
[00:18:46] Jack Hall: So we should do that because that'll save some money easily. Yeah.
[00:18:54] Jack Hall: And then at some point, I guess next week, you and I need to sync up on the RAMI role change and all and figure out, get started on the backfill that we want to do.
[00:19:09] Dan Duling: Yeah, yeah, I can start. Yeah, that's a good one for us to get done.
[00:19:14] Jack Hall: Yeah, yeah.
[00:19:15] Jack Hall: Because everybody's on board and seems like many people know that this is coming down the pipe for Ramy, so.
[00:19:23] Jack Hall: Okay.
[00:19:25] Dan Duling: They are, they are a group that love to, you know,
[00:19:31] iPhone: so.
[00:19:32] Jack Hall: Yeah, it's not surprising.
[00:19:34] Jack Hall: Nimat's already made it known that if and when a tech lead style position opens up, he's definitely interested, which checks out based on everything he does anyhow, so.
[00:19:51] Jack Hall: Yeah, but it'll be good to be able to.
[00:19:55] Jack Hall: If, if we have Remy stepping Out of the ic, more of of a pulling cards for core roll to get started on the backfill so that way we can get somebody else in to start spinning up because it'll like, you know, it takes a minute.
[00:20:11] Jack Hall: Is this for.
[00:20:12] iPhone: Is this for DVH sort of or.
[00:20:15] Jack Hall: Yeah, okay. It's like it's. DVH's role effectively is getting divided.
[00:20:21] Jack Hall: A lot of his responsibilities are kind of getting divided up of like a lot of the product stuff is now Aaron, I'm running, I'm managing the team, but there's not really anyone to kind of serve as that system level architect to like make sure that like, yeah, all the plans and stuff that everybody pulled together like between all the various groups, like they'll actually work together and have that like holistic view and to do a bunch of documentation on all how all of these systems work and all of that.
[00:20:56] Jack Hall: So that's what we've kind of designed as calling like a principal systems architect for Ramy, because it's a lot of where he was headed anyhow before EVH departure, but yeah, and so when then he moves there, that leaves space on the team for somebody else to like namat to step up into more of the principal role and be that.
[00:21:30] Jack Hall: Be more of a team lead versus just a senior engineer. But it also.
[00:21:35] Jack Hall: Rami moving to this role now opens space for us to backfill with either like a regular engineer or a senior engineer, depending on what we can find in the market.
[00:21:52] iPhone: Yeah, that makes sense. Thanks for breaking that down.
[00:21:55] iPhone: That was not following initially, but yeah, that makes sense.
[00:21:59] Jack Hall: Yeah. Yeah, so that'll. It'll. It'll help.
[00:22:03] Jack Hall: And Remy's had like, like Remy, this Sprint is starting to work on like an architecture for like the event processing pipeline for In Store to like how like it's going to be Kafka based and all that.
[00:22:22] Jack Hall: But like there's still some things that I imagine were thought of for when people were working on Pulse 2.0, but that as you can imagine, most 2.0 projects are always a long tail.
[00:22:38] Jack Hall: And so it's hopefully for like In Store he can come up with the system plan and apply some of the improvements that were already being thought about for Pulse 2.0, but apply them to In Store and we can kind of vet them out.
[00:22:57] Jack Hall: Even though I'm still pretty sure that Cybage is going to do most of the implementation around that, at least right now.
[00:23:06] Jack Hall: But it kind of gives Ramy that type of leeway to start figuring some of that stuff out.
[00:23:14] Jack Hall: If he's not pulling, having to pull the like smaller cards and stuff, he has got more time to work on these bigger, fuzzier initiatives.
[00:23:29] iPhone: Y.
[00:23:40] Dan Duling: Anything. Well, I mean, yeah, I mean so we'll work through that.
[00:23:46] Dan Duling: I'll try to find, you know, maybe I'll have some time, you know, tomorrow actually that I'm gonna, you know, continue to play catch up.
[00:23:54] Dan Duling: So I'll get the. I'll try to have those JDs kind of, you know, rolled out.
[00:24:01] Dan Duling: Patrick's been out, so I haven't been able to move the, you know, changing Ramy's role. But I'll.
[00:24:08] Dan Duling: When Patrick's back on Monday, we can go ahead and you know, finalize that and then yeah, I'm going to outline two different options for you know, a senior role but then also maybe two mid, you know, roles.
[00:24:23] Dan Duling: And then we can also determine if we want to look at Canada or the US for those hires.
[00:24:31] Dan Duling: We'll probably, I typically open them up in both and then that way we can do like an apples to apples comparison or you know, the, the individuals that are coming through.
[00:24:42] Dan Duling: I, I will say I think Canada, you know, has done, you know, is typically better.
[00:24:49] Dan Duling: No better on par on the data engineering side in the past though, you know, for I mean all the new hires since I've started, I've actually brought them in US based just because when we start, you know, looking in the comparison, you know, the people that have come in just out.
[00:25:12] Dan Duling: Outperformed.
[00:25:14] Jack Hall: Yeah, yeah. And yeah, that. I think that that makes sense to see what we can get.
[00:25:18] Jack Hall: I don't think there's a reason why we wouldn't want to post in Canada.
[00:25:22] Jack Hall: Yeah, to just see who's out there.
[00:25:26] Dan Duling: Well, and like, you know, it also get.
[00:25:28] Dan Duling: I'm sure, you know, Rami, Alex and you know, some of those guys have, you know, people that are in their networks as well.
[00:25:35] Dan Duling: So it gives them, you know, the opportunity.
[00:25:39] Jack Hall: Cool.
[00:25:42] Dan Duling: Outside of that, you know, I think if there's anything major, major updates, you know, for you all, I'll get kind of the full update on it.
[00:25:56] Dan Duling: But let's see, I'll give you the pre read real quick.
[00:26:00] Dan Duling: So you know, like can was pretty, you know, pretty good. You know, let's see.
[00:26:09] Dan Duling: There are key takeaways that they shared.
[00:26:14] Dan Duling: They did the built announcement with the beyond rollout in August.
[00:26:20] Dan Duling: They did a lot of information with you know, different retail problems.
[00:26:25] Dan Duling: Retail partners actually leaning into post transaction and loyalty as a strategy.
[00:26:30] Dan Duling: So I think that loyalty is, you've kind of probably heard it.
[00:26:34] Dan Duling: I think you know, in some of those okrs for, you know, how we can change some of our post transaction to also having a loyalty type of component.
[00:26:44] Dan Duling: But I think that was a big ask that a lot of, you know,
[00:26:47] iPhone: is that like incentives too?
[00:26:50] Dan Duling: Yeah, it is. It's different word for incentive.
[00:26:54] Dan Duling: You know, they were able to, you know, deepen, you know, some of the existing partnerships, right, like dsg, which is Dick's Sporting Goods.
[00:27:02] Dan Duling: Walmart, Rakuten, which like we have some interesting ideas for how Rakuten can start advertising on Posta or in store Roku or built and then C OFAs as well built.
[00:27:17] Dan Duling: I still like, I've been talking with Don, like we need to make sure we keep that one really close because that's the easiest one for Rock to come in.
[00:27:26] Dan Duling: And you know, by taking one, they take many. Right.
[00:27:29] Dan Duling: So, you know, really trying to keep that relationship forefront is I think critical.
[00:27:35] Dan Duling: We got some good new pipeline out of it. Lyft, you know, we got, we're moving forward with Kroger.
[00:27:43] Dan Duling: We're going through the security, you know, aspects of that right now.
[00:27:48] Dan Duling: Marriott, Expedia, Roku and Fox.
[00:27:53] Dan Duling: So those are the, some of the big for new pipeline conversations that, you know, I've heard so far.
[00:28:02] Dan Duling: We do have a meeting this afternoon to kind of go over more things and you know, we can, if I hear anything new, I will, you know, share that update with you all.
[00:28:14] Dan Duling: On the AI adoption front, you know, in case you all are curious, you know, another initiative that I'm running, we actually had 87% of our, you know, active users now in Claude.
[00:28:28] Dan Duling: So good increase there, you know, activity up from 74% that was in April.
[00:28:35] Dan Duling: We actually now have, you know, multiple users creating, you know, their, their skills and automations.
[00:28:43] Dan Duling: We've analyzed, we've saved roughly now through some of our workflow automations, about 120-180k is the estimate based upon what we've been able to shift with some of the offshore resources.
[00:28:59] Dan Duling: So that's pretty, pretty great to see. You know,
[00:29:04] Jack Hall: and.
[00:29:08] Dan Duling: We've, we've changed some of the defaults.
[00:29:11] Dan Duling: You know, you all can always, you know, update yours but you know, move to some of the lower, you know, cost items like even, you know, opus 4.8 versus, you know, sonit, you know, 5.
[00:29:24] Dan Duling: Like the 5 is actually cheaper to run.
[00:29:27] Dan Duling: So you know, try like for the, the default for all chats and everything. We've moved to lower models.
[00:29:34] Dan Duling: You can always upgrade it.
[00:29:36] Dan Duling: But there was no reason for us to have people that are just Doing, you know, chat type of research, going up on a higher model, like let's, you know, move them when they need it.
[00:29:47] iPhone: Right.
[00:29:49] Dan Duling: And then we actually like the, the teams, you know, fully building, all with AI, were able to create, you know, some new, you know, advertiser reports.
[00:30:02] Dan Duling: They redid the entire partner portal, including our partner performance and cross, you know, business unit report.
[00:30:11] Dan Duling: So Jason Warnock and his team kind of took that one over.
[00:30:16] Dan Duling: It was previously, you know, under Austin, but revamped it, you know, exactly how Conlin wanted it and you know, did it all, you know, AI, end to end.
[00:30:27] Dan Duling: So that's kind of a cool one. And then yeah, we'll.
[00:30:32] Dan Duling: There'll be a fluentu coming out I think July 7th or so.
[00:30:38] Dan Duling: And then after that there will be kind of a fluent challenge. Right.
[00:30:43] Dan Duling: So where people will be able to start submitting their own skill to automate a recurring, you know, task or, you know, other items.
[00:30:52] Dan Duling: You know, they could also configure a project.
[00:30:56] Dan Duling: You know, there's a few different things that will have. They'll be able to submit it. Submit it.
[00:31:01] Dan Duling: And then we're going to, you know, have a few people judge based upon impact.
[00:31:05] Dan Duling: But you know, that incentive can be, you know, we're going to start it off around 3,000 for the.
[00:31:11] Dan Duling: The person that, you know, ends up winning.
[00:31:13] Dan Duling: But you know, on impact, you know, I think I have the headway up to another five.
[00:31:19] Dan Duling: So if it's a very impactful project, you know, trying to incentivize people to really think through it and you know, submit some, you know, items, you know, leveraging AI.
[00:31:31] Dan Duling: So got HR buy in and the finance buy in. So we'll see how that that kicks off.
[00:31:40] Jack Hall: Cool. Yeah, just I can't remember if it was this week or last week. I built out a.
[00:31:48] Jack Hall: I haven't used it yet for this, but I've tested it. A planning poker shared artifact.
[00:31:58] Jack Hall: Because we had been using Slack messages of like, hey, send me your point in points in Slack.
[00:32:05] Jack Hall: Which gets confusing very quickly to have to like wait for people to send me a Slack message and then figure out what card each one goes to.
[00:32:17] Jack Hall: So I'm gonna use that.
[00:32:18] Jack Hall: I guess not since we are gonna do backlog refinement this week because I'm not here tomorrow, I'll do it, use it probably next week with the team.
[00:32:28] Jack Hall: But those, it's interesting how those shared ones go because like it's shared state dynamically across everybody who's running the artifact.
[00:32:42] Jack Hall: But like I didn't spin up any servers.
[00:32:44] Jack Hall: It's just being shared somewhere within Flawed as like a read, write, shared state.
[00:32:53] Jack Hall: So that's interesting of how it can spin that up and do all of that.
[00:32:59] Dan Duling: Yeah.
[00:33:02] iPhone: Nice.
[00:33:03] Jack Hall: Yeah.
[00:33:13] Dan Duling: Well, I know we're probably over because I've, you know, I was a little late but yeah, if anything else you know, comes up, you know, let me know and you know, enjoy the.
[00:33:24] Dan Duling: The long weekend. Yeah, let me know Jack. If anything comes up though on the the release, you know.
[00:33:31] Jack Hall: Yeah, yeah.
[00:33:32] Dan Duling: Hopefully it seems like we're in a good spot but I was like oh, a little one side of the flirt with a little potential disaster there on a deploy.
[00:33:43] Dan Duling: Like I mean typically it's fine right. It's a midweek but since no one else is going to be around.
[00:33:49] Jack Hall: Yeah, we, we made.
[00:33:51] Jack Hall: I made sure I was looking for anything to pop up that gave me concern to before the release to delay it.
[00:34:01] Jack Hall: But all of the additional testing we did went through. So the only thing I think we.
[00:34:09] Jack Hall: Yeah, the only thing.
[00:34:10] Jack Hall: The biggest thing so far has been the ILM policies added that will help trim down some of the like indexes and all that to continue to grow in elastic, I think or something.
[00:34:28] Jack Hall: Yeah, elastic and some non urgent services don't have the right.
[00:34:37] Jack Hall: You're using different non admin elastic credentials so they can't create those policies.
[00:34:43] Jack Hall: So right now it's like the partner delete.
[00:34:50] iPhone: Whatever.
[00:34:52] Jack Hall: What's that?
[00:34:54] Jack Hall: The partner deletion request processor isn't starting because it can't create the ILM policy.
[00:35:01] Jack Hall: But that processor just processes requests and we have 30 days to respond to any request that comes in.
[00:35:10] Jack Hall: So the queue is still being filled and we'll get DSO to swap out those creds.
[00:35:19] Jack Hall: We would do it today but since this is not that urgent and Griggs is out and it seems like Damara is also out today for hospital stuff.
[00:35:29] Jack Hall: We'll. We'll get Griggs to do it tomorrow.
[00:35:34] Dan Duling: You could ping SM if he is available.
[00:35:38] Jack Hall: Cool.
[00:35:39] Dan Duling: So.
[00:35:40] Jack Hall: Okay. All right.
[00:35:45] Dan Duling: Well thank you all. Off to the next one.
[00:35:50] Jack Hall: Have a good one.
[00:35:52] Dan Duling: Yeah, thanks Jeff.
