---
title: "In-store - Databricks Schema and Support"
meeting_id: "040000008200E00074C5B7101A82E008000000000E12FBC58002DD01000000000000000010000000D597D021EC569449AA0FBC226E23FCC1"
note_id: 80786144
recording_id: 15046150
date: 2026-06-24
start_time: "2026-06-24T11:00:00-04:00"
end_time: "2026-06-24T11:30:00-04:00"
url: "https://fluentco.fellow.app/meetings/040000008200E00074C5B7101A82E008000000000E12FBC58002DD01000000000000000010000000D597D021EC569449AA0FBC226E23FCC1/"
participants:
  - name: "Aaron Lupo"
    email: "alupo@fluentco.com"
    rsvp: accepted
    organizer: true
  - name: "Jack Hall"
    email: "jhall@fluentco.com"
    rsvp: accepted
    organizer: false
  - name: "Trevor Anderson"
    email: "tanderson@fluentco.com"
    rsvp: accepted
    organizer: false
  - name: "Erin Manchester"
    email: "emanchester@fluentco.com"
    rsvp: accepted
    organizer: false
  - email: "ksreedharan@fluentco.com"
    rsvp: accepted
    organizer: false
  - email: "BSilveri@fluentco.com"
    rsvp: declined
    organizer: false
  - email: "rlabana@fluentco.com"
    rsvp: accepted
    organizer: false
  - email: "SNikam@fluentco.com"
    rsvp: notResponded
    organizer: false
  - email: "RChevale@fluentco.com"
    rsvp: notResponded
    organizer: false
  - email: "KJagtap@fluentco.com"
    rsvp: notResponded
    organizer: false
  - email: "aChatterjee@fluentco.com"
    rsvp: notResponded
    organizer: false
  - email: "BGidwani@fluentco.com"
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
[ ] Fill out the technical specifications ticket with details on Kafka stream setup and data ingestion requirements for the in-store platform.
[ ] Send the technical specifications ticket to Trevor, Kapil, and Bharat for them to fill out with data ingestion details.
[ ] Schedule a follow-up meeting to sync on the remaining technical details and open questions.

## AI Summary

The meeting focused on designing the **data architecture for a new in-store advertising platform** that will integrate with Databricks and use programmatic bidding through DSPs. Aaron Lupo presented the technical architecture to the data engineering team, explaining how the platform will display ads on POS devices at retailers like Walgreens and CVS, using an **open-source Prebid server to conduct real-time auctions** (within ~100 milliseconds) with multiple DSP partners. The system will capture both authenticated and unauthenticated impressions, with authenticated users able to engage with offers via SMS or email, creating a trackable conversion funnel that doesn't exist in traditional in-store advertising.

The team decided to **follow Pulse's existing pattern of pushing event data into Kafka streams** for Databricks ingestion, allowing for flexibility as requirements evolve. Build (the hardware partner) will deploy the first Verifone tablets to stores on **Monday, June 29, 2026**, initially using Pulse's partner channel API before transitioning to the new OpenRTB-based system post-October 2026. The architecture is designed to scale to **10 million transactions daily** within a year, and the Cybage team will handle implementation while Trevor and Kapil provide design guidance.

### Chapters

#### In-store advertising platform architecture
- The platform aims to ingest event data into Databricks in a standardized, cost-effective way, learning from the Pulse implementation and leveraging the team's experience with Databricks. *(0:00:15)*
- The environment will be set up in the **same cluster as Pulse but in a different namespace**, allowing the platforms to be separate tech stacks while able to communicate within the cluster. *(0:02:46)*
- The platform is essentially a **'Pulse Lite'** version - a standardized reimagining of what Pulse would look like if rebuilt today, minus the ad management and partner management complexity. *(0:27:25)*
- The architecture is designed to handle **10 million transactions daily** by this time next year, requiring careful consideration of volume and cost from the start. *(0:27:32)*
- Build will initially use Pulse's partner channel API for low volume, but this approach **doesn't scale beyond Q4 2026**, so the new OpenRTB-based system is being planned for post-October deployment. *(0:39:30)*

#### User experience and engagement flow
- Consumers will see ads on **POS devices at checkout** (like Walgreens) while items are being scanned, starting with an unauthenticated impression when the first item is swiped. *(0:05:01)*
- The platform enables **consumer engagement with in-store ads** (unlike traditional in-store display advertising), allowing users to authenticate with their loyalty number to receive personalized offers. *(0:06:15)*
- When users authenticate, the system makes a **second ad request with known user information** (hashed phone/email, demographics, audience segments) to DSPs who can bid higher CPMs based on user targeting. *(0:06:55)*
- Users can engage with offers via a **'send me the details' CTA** that triggers SMS or email delivery of the offer to their personal device, creating a trackable conversion funnel. *(0:12:59)*
- Even **1% conversion rates from impression to offer redemption** would be valuable, as advertisers currently have no engagement tracking for in-store advertising. *(0:14:13)*

#### DSP integration and bidding process
- The platform will **predominantly fill inventory with external DSP demand** (like The Trade Desk, Amazon DSP) rather than self-sourced demand, as the volume of sessions will exceed available inventory. *(0:07:47)*
- The platform is designed to **remove the dependency on internal demand management** that currently blocks scale, allowing advertisers to access inventory through their existing DSP budgets. *(0:19:42)*
- DSPs will use the **open-source Prebid framework with OpenRTB standard** to conduct auctions, with all connected DSPs bidding and the entire auction completing in approximately 100 milliseconds. *(0:20:38)*
- The **Prebid server will handle auction logic** including bid prioritization, minimum floor prices, request formatting, and user enrichment with data from the Fluent Identity Graph. *(0:23:24)*
- Aaron is evaluating **white-label publisher ad server platforms** instead of building one from scratch, as this configuration layer (similar to what Pulse provides) is a commodity product. *(0:24:27)*

#### Data tracking and reporting requirements
- A **unique request ID** will serve as the session identifier (similar to Pulse's offer ID), passed to DSPs and logged throughout the entire user journey for attribution. *(0:10:42)*
- The system will **log all bid responses from DSPs** including bid values, advertiser details, and the winning bid selection for each request to enable performance analysis. *(0:11:11)*
- All downstream events (impressions, engagements, email opens, clicks, conversions) will be **enriched with the request ID** to enable full-funnel attribution back to the original ad request. *(0:11:33)*
- Reporting will focus on **advertiser and creative-level rollups** (image vs. rectangle formats, video performance) rather than campaign-level details, as DSPs manage targeting criteria internally. *(0:30:19)*
- The platform will log **bid request requirements and DSP responses** to enable future optimization, though detailed specifications are not yet finalized. *(0:31:03)*

#### Data ingestion strategy for Databricks
- The data engineering team needs **two types of data**: event/clickstream data (impressions, engagements) and metadata (campaigns, creatives, partners) which should be API-driven for internal apps. *(0:03:51)*
- The team decided to **follow Pulse's pattern of pushing data into Kafka streams** for Databricks ingestion, as this is the standard approach and allows flexibility as requirements evolve. *(0:35:08)*
- Starting with a **raw data feed in Databricks** was agreed upon since there are unknowns ('we don't know what we don't know'), with data curation for reporting to be determined after analyzing the feed. *(0:35:49)*
- There are **two separate data flows**: OpenRTB request/response logging for audit control, and actual ad performance events - these need to be treated as distinct data streams. *(0:37:00)*
- Build (the hardware partner) will call Fluentco APIs and **generate event logs** through those API calls, with Build handling the rendering and device management on their end. *(0:37:47)*

#### Build partnership and implementation timeline
- **Build owns the POS tablets** being deployed to Walgreens stores, with Build tablets communicating to Build servers which then integrate with Fluentco's APIs. *(0:37:27)*
- This is a **brand new implementation for Build** - they have not started building the integration yet and haven't done anything similar previously. *(0:38:25)*
- Build is primarily a **payment processing company** currently focused on shipping new Verifone tablets, with the first device going live in store on **Monday, June 29, 2026**. *(0:38:35)*
- The advertising integration is a **secondary or third/fourth priority** for Build currently, but they are aware of the requirements and will build to Fluentco's specifications. *(0:39:02)*
- Build will initially use Pulse's partner channel API for low volume testing, but the new system must be ready for **post-October 2026** as the current approach doesn't scale beyond Q4. *(0:39:30)*

### Action Items
- Trevor Anderson, Bharat Gidwani: Fill out the technical specifications ticket with details on Kafka stream setup and data ingestion requirements for the in-store platform.
- Aaron Lupo: Send the technical specifications ticket to Trevor, Kapil, and Bharat for them to fill out with data ingestion details.
- Aaron Lupo: Schedule a follow-up meeting to sync on the remaining technical details and open questions.

### Decisions
- Event data from the in-store platform will be **pushed into Kafka streams** following the same pattern as Pulse, with Databricks ingesting from there for downstream analytics and reporting.
- The **Cybage team will handle implementation** of the data ingestion layer while Trevor and Kapil provide design support and guidance on Databricks integration.
- The in-store platform will run in the **same Kubernetes cluster as Pulse but in a different namespace**, allowing the platforms to remain separate while communicating within the cluster.

## Transcript

[00:00:01] Aaron Lupo: Oh, someone else is recording.
[00:00:03] Kapil Sreedharan (1): Perfect.
[00:00:06] Aaron Lupo: Jack Scribe is recording. Love that. Okay, what are we talking about today?
[00:00:15] Aaron Lupo: We are talking about in store and supported needs from data engineering and some advisement on how to move forward best and get data into databricks in a way that you guys want and can work out of and is standardized and all the things and cheap as well.
[00:00:41] Aaron Lupo: So quick introduction, Trevor, I don't think we've officially met yet. Good to meet you.
[00:00:50] Aaron Lupo: Excited to have you on board.
[00:00:52] Aaron Lupo: We have Rahul here as well who is our, our cybage business analyst slash.
[00:00:59] Aaron Lupo: I don't know, you do a bunch of things right well, so I don't want to put a title on you but you know, covering a lot of basis as far as information collecting.
[00:01:09] Aaron Lupo: And so where we're at is trying to just scope out all of the things that we're trying to cover for our in store execution.
[00:01:19] Aaron Lupo: And the biggest piece that we are, you know, want to get right from a foundational aspect is how we are ingesting the event data so that we can essentially report on it effectively and you know, manage it and do all of the things we need to do with data, specifically off learnings that we've had with Pulse and all of the things that you guys are now leveraging with databricks, et cetera.
[00:01:47] Aaron Lupo: So that's kind of the queue up for where we're at.
[00:01:51] Aaron Lupo: I have let me pull up my ticket here that I think I sent you guys as a pre read. Okay.
[00:02:10] Jack Hall: So
[00:02:12] Aaron Lupo: the biggest thing that I know, everyone's strapped with resourcing and trying to do a million things at once.
[00:02:20] Aaron Lupo: And so the goal from how this all works together is I would love for your guys support Trevor and Kapil to help us make sure we're designing the structure the right way and you guys are telling us how to best ingest it.
[00:02:37] Aaron Lupo: And then the Cyvage team will be responsible for actually the implementation layer and getting it over to you from our connected system.
[00:02:46] Aaron Lupo: So DevOps is setting up the environment still.
[00:02:49] Aaron Lupo: It's all going to be within the same cluster as where Pulse is today, but it's going to be a different namespace and so they are going to be separate platforms and tech stacks, but they should be able to communicate within the same cluster.
[00:03:03] Aaron Lupo: Knowing all that I have put together kind of the high level events that we want to be able to capture from a performance perspective and if it helps, I can run through them.
[00:03:19] Aaron Lupo: If you guys have already read them then no need to.
[00:03:22] Aaron Lupo: But the biggest takeaway that I want to get to is best way to get this data into databricks and how you guys would advise that.
[00:03:33] Aaron Lupo: So do you want me to go through these events or were they pretty straightforward?
[00:03:38] Kapil Sreedharan (1): Maybe.
[00:03:39] Kapil Sreedharan (1): I think yeah, I went through, you know, these documents but I think it'll be, you know, this is our first meeting.
[00:03:45] Kapil Sreedharan (1): It'll be better to have a overview of what exactly we're building.
[00:03:51] Kapil Sreedharan (1): So you know, from databricks perspective it's. We're doing analytics, right.
[00:03:57] Kapil Sreedharan (1): And there's only two things we need. So one is the events data, right.
[00:04:02] Kapil Sreedharan (1): The clickstream, these impressions or you know, those kind of data.
[00:04:08] Kapil Sreedharan (1): And then the second one is your metadata, right.
[00:04:12] Kapil Sreedharan (1): What are these campaigns or creatives, the partners, you know, the demand and supply side, how do we get that?
[00:04:19] Kapil Sreedharan (1): And if it is an internal app, then it should be API driven so that we can get these configs or metadata and ingest it.
[00:04:29] Kapil Sreedharan (1): And the other one would be.
[00:04:31] Kapil Sreedharan (1): Yeah, you would push the data through whatever Kafka and we get the real time events.
[00:04:37] Aaron Lupo: Okay, so that's a good place to start.
[00:04:40] Aaron Lupo: Sorry, I've gone over this with many people so I forget that I.
[00:04:43] Aaron Lupo: It needs to be covered again and so appreciate that.
[00:04:48] Aaron Lupo: So this is the high level flow and there's actually some pieces that I still need to add here that are missing.
[00:04:53] Aaron Lupo: But the essential experience that we are building. Actually let me start with the demo. That's.
[00:04:59] Aaron Lupo: Let me visualize it.
[00:05:01] Aaron Lupo: So the consumer experience is I walk up to Walgreens, I put all of the items I have on the table and the clerk swipes the first item that starts the transaction, right.
[00:05:16] Aaron Lupo: And so this represents the device, the built device that will be facing the consumer while the person is swiping.
[00:05:23] Aaron Lupo: This could also be a self service checkout essentially as well, you know, the ability of who is swiping what and starting the transaction is irrelevant.
[00:05:33] Aaron Lupo: This screen is going to be facing the consumer, right? So imagine start swiping.
[00:05:40] Aaron Lupo: The first ad is shown. Now this request goes from the built device, sends it to our server.
[00:05:47] Aaron Lupo: We make a request for offers or for ads.
[00:05:51] Aaron Lupo: This is what's going to connect to our programmatic, Programmatic pipeline that is going to fill this demand.
[00:06:01] Aaron Lupo: So I'll show that in a second. But essentially it renders an ad in this space.
[00:06:06] Aaron Lupo: And the cool thing about what we are trying to build here is that predominantly in store experiences have not been consumer engageable.
[00:06:15] Aaron Lupo: And so they have just been display ads that you can kind of track but not really track engagement from.
[00:06:22] Aaron Lupo: And so what we're trying to do Here is because the user is kind of in this wait state while their items get swiped.
[00:06:29] Aaron Lupo: And there is always a precedence to put in your loyalty number and get some customized experiences.
[00:06:36] Aaron Lupo: The same concept is here. User comes in, they go, yeah, I'm a loyalty member.
[00:06:40] Aaron Lupo: I'm going to put in my phone number to do a lookup.
[00:06:44] Aaron Lupo: I hit enter and it goes and makes a second request. This is just a demo app.
[00:06:53] Aaron Lupo: This is not real, this is a simulator.
[00:06:55] Aaron Lupo: But it goes makes a second request to that same ad server with now known information about the user.
[00:07:01] Aaron Lupo: We now know essentially their hash phone number, their hash email address, some demographic information about them, audience segments.
[00:07:11] Aaron Lupo: Ideally we will want to create some pre cached audience segments that we can then attribute to this user that we will pass along to our programmatic partners.
[00:07:21] Aaron Lupo: And they will say I want to bid $25 CPM on that user and the ties bid wins.
[00:07:30] Aaron Lupo: We render that ad and there's a bunch of stuff that happens behind the scenes to get that ad.
[00:07:35] Aaron Lupo: But essentially that's the user experience. Now
[00:07:38] Trevor Anderson: question is this going to be using the same
[00:07:41] Trevor Anderson: offer recommendation engine that Pulse is currently?
[00:07:45] Trevor Anderson: No using right now? No, it's completely different.
[00:07:47] Aaron Lupo: Yep. So we are predominantly going to be filling this with DSP demand that is not our, our own.
[00:07:56] Aaron Lupo: There will be a component to pull in demand from that we source ourselves, but the amount of volume of sessions we'll have, we don't have enough demand to fill that.
[00:08:08] Aaron Lupo: And so it will be predominantly. This is being built for programmatic demand specifically that will.
[00:08:14] Aaron Lupo: Will come externally.
[00:08:16] Kapil Sreedharan (1): Okay.
[00:08:17] Aaron Lupo: And so they will have their own ranking and bidding logic that they will own and maintain.
[00:08:22] Aaron Lupo: And you know, essentially they're just like, here's what I'll pay for that person.
[00:08:27] Aaron Lupo: And that's, you know, our, our contract with them is pretty simple in that state.
[00:08:33] Kapil Sreedharan (1): Oh, so we just passed them whatever the dsp, the customer profile and they, and they bid on them and show the.
[00:08:43] Aaron Lupo: Yeah, so, so not every DSP will support it, but a lot of them, we will give them a hashed user identifier and they will essentially, you know, send to their own identity graph and know what they know about the user and you know, what their value is for that user and send it forward or.
[00:09:03] Aaron Lupo: And then bid appropriately.
[00:09:04] Aaron Lupo: Now not every dsps will have that and we'll also have some proprietary direct deals with partners or with advertisers that will not go through DSP that will not have that flexibility.
[00:09:15] Aaron Lupo: So we will layer in our own identity graph as part of this and serve up some known user segments.
[00:09:25] Aaron Lupo: But the primary use case will be passing that identifier along and just accepting whatever bid we get back.
[00:09:35] Kapil Sreedharan (1): So in that case then if you're sending to DSPs, we don't manage campaigns and creatives and all.
[00:09:42] Kapil Sreedharan (1): It's all
[00:09:45] Aaron Lupo: correct.
[00:09:47] Kapil Sreedharan (1): And what happens if the user does not log in here?
[00:09:52] Aaron Lupo: Yeah, so those are.
[00:09:53] Aaron Lupo: So the first screen you saw there is what we consider, or we're considering an unauthenticated impression versus an unauthenticated impression.
[00:10:04] Aaron Lupo: And so everything essentially happens the same but we will probably get on average a much lower bid price for those because there is no down funnel tracking that happens.
[00:10:15] Aaron Lupo: They will just be strictly, you know, impression, you know, banner ads in that sense that will not have any engagement layer to it.
[00:10:23] Aaron Lupo: So it, you know, doesn't demand as high of a value from our, from advertisers.
[00:10:30] Aaron Lupo: So kind of bill your earlier question of like, no, we don't need to know the creative, like we're not managing the creative details or anything here.
[00:10:42] Aaron Lupo: But the one constant that will exist throughout this whole process will be the concept of like a request ID id.
[00:10:50] Aaron Lupo: That will be what we consider today is like our offer id. It's like a unique session id.
[00:10:57] Aaron Lupo: It's a unique identifier for that user in front of that POS device at that time.
[00:11:03] Aaron Lupo: That session ID will basically, or that, sorry, request ID will essentially be passed in the request to DSPs.
[00:11:11] Aaron Lupo: We will get back a bunch of bids and we will want to log all of those bids and those values and you know what the advertiser details are around that.
[00:11:20] Aaron Lupo: So we'll get some information back from the DSPS about that. We'll want to log that in databricks.
[00:11:26] Aaron Lupo: So that's something we will want to maintain.
[00:11:28] Aaron Lupo: We will also want to log which one we have determined to be the winner in that request.
[00:11:33] Aaron Lupo: So we can track that.
[00:11:36] Aaron Lupo: And then furthermore, as we as the get further down the event tracking perspective, we will want to say, okay, an event happened or sorry, an impression happened to this request ID to this specific offer ID and we know the winner of that.
[00:11:52] Aaron Lupo: And like all the events then will be enriched further with that single request ID concept, kind of carrying it through so that we can always attribute everything that happens with the request for offers.
[00:12:10] Aaron Lupo: Does that make sense?
[00:12:11] Kapil Sreedharan (1): Yeah. Do you also have time zone where the user is?
[00:12:18] Aaron Lupo: We will, yeah, we can, we will need that probably. Yeah, that's actually good call out.
[00:12:25] Aaron Lupo: We get that from the initial request from the POS device.
[00:12:29] Jack Hall: Yeah, like we should definitely know that because this is all for like a physical device.
[00:12:33] Jack Hall: So we should definitely be able to tell you what time zone they're in.
[00:12:38] Aaron Lupo: Yep, good call out. Any other questions? Okay, so let me continue running through.
[00:12:50] Aaron Lupo: So this is the part that kind of makes us, you know, kind of makes this opportunity really unique.
[00:12:55] Aaron Lupo: Is that because now they've authenticated, we know who the user is.
[00:12:59] Aaron Lupo: There is a call to action button here that says send me the details. They click on that.
[00:13:04] Aaron Lupo: We have an offline process that will be alerted from the device that this user engaged.
[00:13:10] Aaron Lupo: And we will then send them either an SMS or an email, depending on the integration setup, to give them that offer to their device.
[00:13:19] Aaron Lupo: It's essentially like an engaged impression event.
[00:13:24] Aaron Lupo: It doesn't go directly to the landing page, doesn't give them direct access to the deal yet.
[00:13:31] Kapil Sreedharan (1): It's.
[00:13:31] Aaron Lupo: It still needs to get from the POs onto their personal device.
[00:13:37] Aaron Lupo: And so once that happens, then there's a whole nother set of events that come in, which is email, open rate email, click, sms, click.
[00:13:49] Aaron Lupo: And then there is, did they view the page that they actually clicked on? Right.
[00:13:54] Aaron Lupo: Is there any loss between the click event and the actual impression of the landing page?
[00:14:01] Aaron Lupo: And then the final step is did they convert on one or multiple events for the advertiser?
[00:14:08] Aaron Lupo: So that's the whole user experience flow of this.
[00:14:13] Aaron Lupo: And ideally, right, if we have even 1% engagement of this stuff, that's pretty good. Sorry, 1%? Yeah.
[00:14:24] Aaron Lupo: Conversion rate on some of this stuff from email or from view impression here to actually converting on the offer, it's pretty valuable to us.
[00:14:33] Aaron Lupo: And advertisers will pay ideally, I mean, we haven't proven this out yet, but they'll pay for that value that they don't actually have anywhere in store today.
[00:14:46] Kapil Sreedharan (1): And how do you tie from, you know, from this impression that you see on, on this screen to user's device?
[00:14:54] Kapil Sreedharan (1): Right. When they click on the email or the sms, is there a request ID or event id?
[00:15:01] Aaron Lupo: Yeah, that's. That will need to follow through.
[00:15:03] Aaron Lupo: So we will need, in the link that we pass and send via email or sms, we will need to include a concept of unique tracking id, which is, you know, session ID or request ID that will follow that through.
[00:15:19] Aaron Lupo: And then furthermore, when they convert, we will need to catch that ID as well to say this conversion happened.
[00:15:26] Aaron Lupo: And we know all these other things happen too.
[00:15:30] Bharat Gidwani: And this, this SMS or email that is being handled by build or CVS or whatever, right?
[00:15:36] Bharat Gidwani: No, we are not handling that, that part yet.
[00:15:39] Aaron Lupo: We are not sending it to the consumer.
[00:15:43] Aaron Lupo: We, we are sending Them a payload to whatever their service is that tells them what to send or.
[00:15:49] Aaron Lupo: Okay, you know, it's a, it's a partnership there, right?
[00:15:52] Aaron Lupo: Like, hey, this is how we think you should send the email. The email's coming from them.
[00:15:57] Aaron Lupo: But yeah, we're going to work with them to have the template for what that, that context is.
[00:16:01] Aaron Lupo: But yes, they are going to be the physically one.
[00:16:03] Bharat Gidwani: So essentially, so essentially Bilt or CVS or whoever, these, these partners, they are actually integrating our API, this new API that that's being built to do all this.
[00:16:14] Bharat Gidwani: Because this page that you're demoing, this is their page, not ours.
[00:16:20] Bharat Gidwani: There's nothing on this which is ours. Correct.
[00:16:23] Aaron Lupo: We are sending them this image.
[00:16:29] Kapil Sreedharan (1): No, but this image is from the dsp, right? You said the creative is.
[00:16:33] Aaron Lupo: Yes, yeah, sorry, we don't build it, we don't own it. But we are forwarding along. Yes.
[00:16:38] Bharat Gidwani: Okay.
[00:16:39] Bharat Gidwani: The API returns these image parts or whatever and they just display it and they render it and all that.
[00:16:46] Bharat Gidwani: Okay, correct.
[00:16:47] Aaron Lupo: And there is a workflow in the future where we connect this to pulse demand and we have a creative element that is a image.
[00:16:57] Aaron Lupo: Well, we already can support that today that we have an offer type that is in store placement and it competes in the, in the auction and the bid requests, just like DSPs.
[00:17:10] Aaron Lupo: And if it wins, it wins. So it could essentially come from us in the future as well.
[00:17:15] Bharat Gidwani: Okay.
[00:17:17] Kapil Sreedharan (1): Hey, question. Sorry, I'm not fully aware of. I don't fully understand DSPs.
[00:17:21] Kapil Sreedharan (1): But you know, Disney is our partner, right? Advertiser. Right.
[00:17:29] Kapil Sreedharan (1): So do we set up Disney on whatever DSPS that we are engaging here and we set up these campaigns, creatives and all on that dsp or you send, you send these whatever creatives and images to the DSP provider.
[00:17:45] Kapil Sreedharan (1): Is that how it works?
[00:17:46] Aaron Lupo: So DSP is a demand side platform.
[00:17:50] Aaron Lupo: So really what it means is an advertiser can get access to, you know, X number of supply channels, right.
[00:18:00] Aaron Lupo: Consumers via one integration placement. And so like the DSP connects to all of the supply channels.
[00:18:07] Aaron Lupo: The advertiser, who's the buyer just connects to one, one platform and you know, says, I want this is what their budget is.
[00:18:14] Aaron Lupo: And they control all that and they manage that within that that DSP platform.
[00:18:17] Aaron Lupo: So like the trade desk is one of them. Amazon dsp, there's a billion of them.
[00:18:24] Aaron Lupo: And so they are just platforms for like managing budgets across a bunch of different consumer supply channels.
[00:18:34] Aaron Lupo: And so to your question, like Disney, someone like Disney or any other advertiser, they may have budget that is DSP budget of $50,000.
[00:18:45] Aaron Lupo: And then they have another team that does direct budgets.
[00:18:47] Aaron Lupo: They know with someone like US that is $10,000.
[00:18:50] Aaron Lupo: And so there's fully potential that Disney could live in both environments.
[00:18:56] Aaron Lupo: And you know, essentially it only serves up the winner in that scenario.
[00:19:01] Aaron Lupo: They probably wouldn't compete against each other in a lot of places. But it's, it's like a.
[00:19:05] Aaron Lupo: Think of it as a stock market for like the dsp.
[00:19:10] Aaron Lupo: There are buyers for those advertisers that just like are constantly tweaking what their bid is going to be, passing different creatives to the DSPs to really just optimize how much they're spending and the return that they're getting from different supply channels within the dsp.
[00:19:28] Bharat Gidwani: Essentially Aaron, the Disney manages that side of the house.
[00:19:34] Bharat Gidwani: They're not coming to us to manage that creative budget or any of that information right now.
[00:19:41] Aaron Lupo: We are.
[00:19:42] Aaron Lupo: The idea of this is to remove the dependency that we have today on demand management and advertising management.
[00:19:52] Aaron Lupo: Yeah, we, we are blocked by that in a lot of ways for scale right now.
[00:19:58] Aaron Lupo: And so, and you know, we get, we get paid.
[00:20:00] Aaron Lupo: The margins are better when you have direct demand versus going through DSP.
[00:20:04] Aaron Lupo: Like, you know, DSP might take 4% of whatever the bid price is.
[00:20:07] Aaron Lupo: So if it's a dollar, you know, their, their bid now is only 96 cents because DSP takes it.
[00:20:13] Aaron Lupo: So like there's commercial opportunity to get better deals for, and better value for the advertisers going direct.
[00:20:23] Aaron Lupo: But this is predicated on not being a, you know, a sales driven blocker.
[00:20:31] Aaron Lupo: Like we want to go to where the demand already exists and where the budget for these advertisers, they're already spending it.
[00:20:37] Jack Hall: Yeah.
[00:20:38] Jack Hall: And right now the plan is to integrate with these DSPs through an open source project called Pre bid which uses OpenRTB.
[00:20:47] Jack Hall: So like we just, we basically are going to start auctions for each time a request comes through and all the DSPs that are connected in will bid and will follow the standard for how who wins the auction.
[00:21:01] Jack Hall: But like the entire auction happens in a Kapil hundred milliseconds maybe. Correct.
[00:21:09] Kapil Sreedharan (1): We can also do this for Pulse, right? I mean, I'm just.
[00:21:13] Aaron Lupo: Yeah, we could. In the future we could. Yes. This is essentially.
[00:21:19] Aaron Lupo: We didn't know that when we built Pulse, we didn't know there was a standardized structure for bidding that we probably should have evaluated.
[00:21:27] Aaron Lupo: Myself being the main person since I was designing it, but that was before any of you guys were here, so I could have kept that one to myself.
[00:21:37] Kapil Sreedharan (1): I guess this is the future.
[00:21:39] Aaron Lupo: Yeah, no, it's a standard practice that we should get ourselves be within because that's essentially how this thing also grows.
[00:21:47] Aaron Lupo: Right.
[00:21:48] Aaron Lupo: There's going to be agents that bid for these places as well and they need to, you know, operate autonomously in this space as well.
[00:21:56] Aaron Lupo: And you know, we need to live in the standardized world of advertising.
[00:22:02] Trevor Anderson: So are you saying jack off this open source framework we're building, are we internally building our own supply side
[00:22:09] Trevor Anderson: or
[00:22:12] Bharat Gidwani: is that.
[00:22:13] Trevor Anderson: That's kind of how I'm thinking about it.
[00:22:17] Aaron Lupo: We're not building like a supply side platform.
[00:22:20] Trevor Anderson: Yeah, that's what I'm asking.
[00:22:22] Aaron Lupo: No, we're not building that. We are essentially going to be managing.
[00:22:27] Aaron Lupo: So let me go back, let me go to a, another view here.
[00:22:32] Aaron Lupo: So this is the high level data flow of how this works.
[00:22:37] Aaron Lupo: So pos device connects to a built or CBS server that actually sends the request to us.
[00:22:44] Aaron Lupo: We have what's called a publisher ad server, which is kind of that configuration layer that says like, hey, this request is coming in from this IP address, this is the user's hashed email and this is their kind of like channel ID or their, their, you know, essentially their placement within in store.
[00:23:07] Aaron Lupo: And ideally, right, we'll make that, we'll enrich that request further with what we know about the user.
[00:23:13] Aaron Lupo: If we're able to match with our identity graph and build a whole OpenRTB request with that information and send that out.
[00:23:24] Aaron Lupo: The two pieces that we are really focusing on is this previd server, which is, as Jack mentioned, it's open source, we're hosting it ourselves.
[00:23:32] Aaron Lupo: It allows us to build the request as we want, also layer in things we know about the user, the logic and the prioritization of how to select a winner.
[00:23:45] Aaron Lupo: A bunch of different things about that would come in at this level as well, which is like the minimum floor that we want for a bid to have the format of the requests of the offer, a bunch of other things that we get packaged out of here.
[00:23:59] Aaron Lupo: So the Cybage team is currently working on this piece of it and hosting it.
[00:24:07] Aaron Lupo: And that's where we're started.
[00:24:08] Aaron Lupo: And then this Publisher ad server, which is the configuration layer I had thought we might need to build, which is a lot like what Pulse is today, by the way.
[00:24:18] Aaron Lupo: Pulse is a publisher ad server, has a bunch of settings for what's approved, what's not approved.
[00:24:25] Aaron Lupo: But this is a commodity out there.
[00:24:27] Aaron Lupo: And so I'm meeting with a Kapil potential partners to see if we can white label a platform to do this stuff, which just to also give you an idea what it could look like.
[00:24:38] Bharat Gidwani: Right.
[00:24:39] Aaron Lupo: Supply partners. This is kind of the setup of it. They have their own channels beyond pos.
[00:24:49] Aaron Lupo: It kind of has this concept of like a channel id which is basically just like a mapping to the settings.
[00:24:55] Aaron Lupo: So this would kind of be that request from the built orchestration layer.
[00:25:01] Aaron Lupo: Then once we get that in, it says, okay, this request is going to be for these types of ads and these file types, these are the things that are excluded for verticals.
[00:25:15] Aaron Lupo: I don't know exactly all the settings we will have in this.
[00:25:18] Aaron Lupo: But that's the concept is that this ad server will control the enrichment and the build of that OpenRTB request.
[00:25:30] Kapil Sreedharan (1): And these would go to the DSPs where they would apply that filtering on the, on the.
[00:25:37] Aaron Lupo: They wouldn't really need to apply anything.
[00:25:39] Aaron Lupo: We would put it in a big open RTB request that they, their systems like their adapters are already built to integrate with.
[00:25:51] Aaron Lupo: And they would say, okay, I have an offer, like I have an ad that I want to bid, you know, $5 or $25 for that placement and that supply.
[00:26:04] Kapil Sreedharan (1): Okay,
[00:26:13] Aaron Lupo: How much time did I book here? Okay, I only booked half hour.
[00:26:18] Kapil Sreedharan (1): Is it only fig enrichment or are we all. Because I. Our IG API has.
[00:26:23] Bharat Gidwani: This is actually IG API. It's called Fluent Identity Graph. That's how Aaron is labeling it.
[00:26:28] Bharat Gidwani: This is actually the identity graph.
[00:26:30] Aaron Lupo: Yeah.
[00:26:31] Kapil Sreedharan (1): Okay.
[00:26:32] Aaron Lupo: Yeah, not. Not old fig. New. New Identity graph.
[00:26:35] Kapil Sreedharan (1): Sounds good.
[00:26:41] Aaron Lupo: Okay, so we probably need more time, but did this kind of open up the understanding of what we're trying to do?
[00:26:49] Bharat Gidwani: And yeah, and I mean it just looks like, and it looks like we kind of building a parallel pulse platform.
[00:27:01] Aaron Lupo: Minus the ad management.
[00:27:04] Bharat Gidwani: Yes.
[00:27:05] Aaron Lupo: And the partner management.
[00:27:06] Bharat Gidwani: Yeah, but yeah, but again it just. It's a parallel universe of what Pulse does.
[00:27:11] Bharat Gidwani: But yeah, just take taking bits and pieces out of it and doing it.
[00:27:15] Aaron Lupo: Yeah, it's a pulse light. It's a standardized pulse light version
[00:27:24] Bharat Gidwani: that
[00:27:25] Aaron Lupo: the predicated is like what would we do if we did pulse over again
[00:27:30] Bharat Gidwani: and
[00:27:32] Aaron Lupo: think about cost because this is potentially this time next year we could be looking at 10 million transactions a day.
[00:27:40] Kapil Sreedharan (1): Right.
[00:27:41] Aaron Lupo: And so like it's a lot of volume that is going to be coming through the door and how do we, you know, build for that expectation now?
[00:27:52] Bharat Gidwani: Aaron, one thing I would definitely like, if you have subsequent calls, would like to understand the whole concept of campaigns and all because they are now no longer in this.
[00:28:02] Bharat Gidwani: We don't have access to that I guess here.
[00:28:06] Bharat Gidwani: So how are you guys looking at providing value to Disney's or any of the advertisers?
[00:28:14] Bharat Gidwani: Because they can track it on their side because they're setting up the campaign themselves and they can see their return.
[00:28:21] Bharat Gidwani: We are just in a way sending traffic to them.
[00:28:27] Aaron Lupo: We wouldn't have any direct relationship with Disney through a dsp.
[00:28:34] Aaron Lupo: That's not something that we would be leveraging.
[00:28:35] Aaron Lupo: Now what we will get back from the bid requests, you know, we'll know like there is metadata on those responses that says like the advertiser and the other like other information about who is bidding on it that we can put onto our own reporting and understand like hey, these ads are really working and you know these ones are not.
[00:28:57] Aaron Lupo: Right.
[00:28:57] Aaron Lupo: And so like we can then optimize and control and say okay, I want to exclude DoorDash from requests for CBS going forward and there should be a control in the ad manager to do that.
[00:29:12] Aaron Lupo: And so all of that then would update the OpenRTB request so that going forward that request doesn't get sent to DoorDash or is excluded right off the bat.
[00:29:21] Aaron Lupo: And so a little bit is optimization on that side.
[00:29:24] Aaron Lupo: But we will be managing this stuff for our partners. We are less concerned about the demands.
[00:29:31] Aaron Lupo: Like there is gonna be a demand sales team that we have and their job is to get as many as much competition and demand in the network as possible.
[00:29:43] Aaron Lupo: And that's gonna be fluid, right? Sometimes there's gonna be budget, sometimes there's not.
[00:29:46] Aaron Lupo: But there should always be. You know the goal is to have a hundred percent fill rate to our partners.
[00:29:51] Aaron Lupo: Whether that is through dsp, through fluent direct ads or even some, you know, always on ad that is just, you know, 1 cent an impression or like half a cent an impression, something like that.
[00:30:06] Aaron Lupo: But basically a hundred percent fill rate.
[00:30:08] Bharat Gidwani: Okay, so in this reports we just looking at advertiser roll ups, not a drill down of saying which campaigns they're running.
[00:30:16] Bharat Gidwani: Because we have no idea about.
[00:30:19] Aaron Lupo: Yeah, I mean we'll probably get some creative level information.
[00:30:22] Aaron Lupo: I haven't looked into that too deeply yet.
[00:30:26] Aaron Lupo: We'll know like hey, we'll be able to say like square images work better than rectangles or also when we expand this to be video.
[00:30:36] Aaron Lupo: Right. How do those perform differently? And so we also want to know some things.
[00:30:41] Aaron Lupo: We'll try to capture what we can about the creative.
[00:30:44] Aaron Lupo: But yeah, the concept of this campaign has these specific targeting criteria.
[00:30:51] Aaron Lupo: No, that won't really be something we will necessarily know from the advertiser side.
[00:30:55] Aaron Lupo: But our logging logic should say hey, this is what the bid request that fanned out stated.
[00:31:03] Aaron Lupo: These are the requirements for it.
[00:31:04] Aaron Lupo: This is what we got back from the dsps and the connected partners and whatever analyzation we can do off that in the future is going to be valuable.
[00:31:13] Aaron Lupo: But I don't have that spec'd out yet.
[00:31:16] Kapil Sreedharan (1): They should also have the DSPs, right? Like which DSP.
[00:31:21] Bharat Gidwani: Yep.
[00:31:23] Aaron Lupo: Yes, this is, this is like a mock up I was doing before I realized that there are platforms out here that already do this.
[00:31:31] Aaron Lupo: So don't take this, you know, take this with a bit of a grain of salt.
[00:31:35] Aaron Lupo: But yeah, there will, there will be, you know, levels of advertiser or, sorry, DSP advertiser, you know, some information about the creative itself to report on.
[00:31:56] Aaron Lupo: All right. Okay. So wrapping up and we can, I can schedule, you know, a revisit on this.
[00:32:07] Aaron Lupo: I need to kind of know what information would be useful for you guys to know on how to collect specifically event data and how I can, you know, transfer that to the, to the team to make sure and coordinate that.
[00:32:28] Aaron Lupo: I don't need it right now. You know, we're at time.
[00:32:30] Bharat Gidwani: But one quick thing would be, and I don't know if it's you or Raul or someone can look at it, but do we have any samples of what kind of requests or what kind of data you can get from these DSPs when you send the open, open request to them?
[00:32:47] Bharat Gidwani: That could also help.
[00:32:49] Aaron Lupo: Yeah,
[00:32:52] Bharat Gidwani: at the end of the day, you're limited to whatever we get from this, these requests. Right.
[00:32:56] Bharat Gidwani: You're not, you won't be able to.
[00:32:58] Bharat Gidwani: I mean, we might be able to augment some or enrich it to some extent, but not too much, I guess.
[00:33:06] Aaron Lupo: Yeah. So here's one. This is a request.
[00:33:20] Bharat Gidwani: Yeah.
[00:33:20] Bharat Gidwani: Again, we don't have to do it right away, but that would definitely help us kind of dissect this a little more and then how we want to, or what we plan to do with that information.
[00:33:35] Bharat Gidwani: Like the report that you are showing. Right. What kind of reports we expect to see.
[00:33:40] Bharat Gidwani: Again, it doesn't have to be perfect, but what kind of information do we want to aggregate or display to any stakeholders later on from this information?
[00:33:56] Aaron Lupo: Yeah, yeah, I mean, this is.
[00:33:59] Aaron Lupo: I'll try to find something, but like it's, it's well documented out there.
[00:34:04] Bharat Gidwani: Okay.
[00:34:06] Aaron Lupo: So I will try to find one that articulates it well.
[00:34:15] Bharat Gidwani: Yeah.
[00:34:19] Aaron Lupo: Okay. But does. So to that point though,
[00:34:27] Bharat Gidwani: I think we can even take the other approach where. And we just.
[00:34:31] Bharat Gidwani: Whatever data you can pass us, we'll consume it but then we'll curate can be reported correctly.
[00:34:41] Aaron Lupo: Yeah, I guess that's what I'm asking is like what's the first step if I don't have all the details of while this will look in reporting yet, like how do we best get data put somewhere and where does that go and you know, what's the process for that so that we can ingest it and you know, work with it and you know, change it up later.
[00:35:03] Aaron Lupo: I guess that's what I'm kind of looking for.
[00:35:06] Bharat Gidwani: I think as Kapil was saying earlier. Right.
[00:35:08] Bharat Gidwani: We kind of could follow the same pattern that Pulse currently does where they just push data into a Kafka stream.
[00:35:16] Bharat Gidwani: Like the analytics feed that we have. We push this data there and that is ingested in databricks.
[00:35:23] Bharat Gidwani: That could be the first step and then we can figure out what how we curate that data for reporting or any other downstream consumption.
[00:35:32] Aaron Lupo: Okay. Yeah. I mean if that. That works by me, I don't have an opinion on the matter.
[00:35:38] Aaron Lupo: But if that is standard and works, then we can follow that and I could just, you know, have them set that up.
[00:35:49] Bharat Gidwani: Yeah, I think that might be the easiest approach for now. Like because there are a few unknowns here.
[00:35:55] Bharat Gidwani: We don't know what we don't know.
[00:35:57] Bharat Gidwani: So you can start that path and start consuming it in databricks have a raw data feed in there and then we can look at that feed and figure out what we want to do with it and go from there.
[00:36:11] Aaron Lupo: Okay. So if I can.
[00:36:13] Aaron Lupo: I think if you can access this ticket, can I send you this ticket and you just fill that out or like put that in the comments and I can take that and move it forward.
[00:36:26] Bharat Gidwani: Yeah, I think that should be fine.
[00:36:27] Bharat Gidwani: I think me, Trevor or a Kapil one of us can fill that information and. And get you that information.
[00:36:37] Aaron Lupo: I'll attach. I'll send it to all of you and you guys can.
[00:36:41] Kapil Sreedharan (2): And is it built that is sending us the data?
[00:36:45] Kapil Sreedharan (2): Is it from the.
[00:36:46] Kapil Sreedharan (2): Are they is built gonna push this
[00:36:48] Kapil Sreedharan (1): to.
[00:36:50] Kapil Sreedharan (2): Okay.
[00:36:51] Kapil Sreedharan (2): And even for that they're following this
[00:36:53] Bharat Gidwani: open RTB now build is calling our API. Right. So
[00:37:00] Aaron Lupo: there's two pieces, right.
[00:37:00] Aaron Lupo: There's the OpenRTB like logging of the requests and the winners and the responses.
[00:37:06] Aaron Lupo: Like that needs to happen for our own audit control.
[00:37:09] Aaron Lupo: But then there's the actual like ad performance requests or events.
[00:37:14] Aaron Lupo: And those are two separate things.
[00:37:15] Bharat Gidwani: Yeah. Built as like Raikutan or any of these publishers. Right.
[00:37:21] Bharat Gidwani: They are calling our API and we are doing whatever we do internally.
[00:37:26] Aaron Lupo: Yes.
[00:37:26] Jack Hall: Yeah.
[00:37:27] Jack Hall: And then they built is the one that they are shipping the terminals to all of the Walgreens right now.
[00:37:35] Jack Hall: So they own the tablets.
[00:37:37] Jack Hall: So their tablet talks to their built tablets, talk to built servers, which then talk to our servers.
[00:37:42] Bharat Gidwani: Yeah.
[00:37:43] Bharat Gidwani: So technically, I mean, for our purposes, we don't necessarily care what Build is doing because they're integrating with our API, consuming our data and doing whatever they need to do on the rendering side and all.
[00:37:56] Bharat Gidwani: But for these events and all, what we're saying is our APIs that we are building is what is generating these events logs or whatever.
[00:38:08] Kapil Sreedharan (2): Okay,
[00:38:08] Kapil Sreedharan (2): so we should get some
[00:38:11] Kapil Sreedharan (2): request
[00:38:14] Kapil Sreedharan (2): from, or the request,
[00:38:16] Kapil Sreedharan (2): the request data contract from Build.
[00:38:19] Kapil Sreedharan (2): Right? They, they should have some standard API already
[00:38:22] Rahul Chevale: there.
[00:38:23] Aaron Lupo: We're gonna have to give them.
[00:38:25] Jack Hall: Yeah, they have not built this yet. They've not started to build this yet.
[00:38:30] Kapil Sreedharan (2): Is this entirely new implementation for build or have they done something
[00:38:34] Rahul Chevale: similar?
[00:38:34] Aaron Lupo: Brand new.
[00:38:35] Aaron Lupo: So they're, they were supposed to start about two months ago, but they are predominantly a payment processing company.
[00:38:44] Aaron Lupo: And so they are in the midst of shipping out these new Verifone tablets that are their new like payment processing devices.
[00:38:53] Aaron Lupo: And they're starting, you know, I think the first one's in store on Monday.
[00:38:57] Aaron Lupo: And so their focus has been predominantly that getting that to market.
[00:39:02] Aaron Lupo: We are like a secondary, maybe third or fourth priority in their world right now.
[00:39:08] Aaron Lupo: But they're well aware of it.
[00:39:11] Aaron Lupo: They're going to build that UI component and whatever specs we give them to go get that offer.
[00:39:18] Aaron Lupo: And so today they're going to use our partner channel API, which does use pulse inventory, which is fine for an initial step because the volume will be low.
[00:39:30] Aaron Lupo: But going forward that doesn't scale beyond Q4.
[00:39:36] Aaron Lupo: And so this, this all we're planning for and all we're doing is for Pat, you know, post basically October and beyond.
[00:39:44] Aaron Lupo: I mean if we can do it sooner or better. But Bilt has not done anything yet.
[00:39:51] Kapil Sreedharan (2): Okay,
[00:39:56] Aaron Lupo: all right, cool. Thank you all very much. Appreciate the time.
[00:40:00] Aaron Lupo: I'll schedule a follow up to, to sync up on some of these things.
[00:40:06] Trevor Anderson: Thank you very much.
[00:40:08] Aaron Lupo: All right, bye.
