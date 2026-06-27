---
title: Akhil x Trevor Sync
date: 2026-06-26
time: 11:00-11:30
participants: [Trevor Anderson, Akhil Gonna]
fellow_url: https://fluentco.fellow.app/meetings/040000008200E00074C5B7101A82E00807EA0617E98E89C6B6D9DC01000000000000000010000000BFCACE4B4A948345884E2BF439AAE751/
captured_at: 2026-06-27T12:02:38Z
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
[ ] Akhil Gonna to discuss payment status logic and calculation methodology with Brian once he returns from vacation
[ ] Akhil Gonna to check Playful rewards data to compare and validate fulfillment records against Tremendous data
[ ] Akhil Gonna to check with Brian on Monday about event mapping ticket and BU classifications table to avoid duplicating existing data
[ ] Akhil Gonna to submit the Lattice summary today

---

Trevor and Akhil held their regular 1-on-1 sync, starting with personal updates including upcoming vacations and Akhil's gender reveal event. The primary technical focus was on **Tremendous fulfillment data integration challenges**, particularly around campaign matching methodology. The team is using **recipient names for fuzzy matching** against Lead Manager campaigns (per JC and Sharita's direction), though this approach created confusion as many campaigns appear to be named after individuals rather than traditional campaign names. They worked through **payment calculation logic**, determining that failed and pending statuses should be filtered out, while cancelled transactions with refunds need to be subtracted from payment totals.

Akhil explored Playful rewards data for cross-validation but encountered challenges with email hashing and limited matching capabilities. The discussion also covered concerns about potential data duplication with the BU classifications table and event mapping work. Trevor announced progress on setting up a foreign catalog for easier access to JC's data.

### Personal Life Updates

- Trevor is taking a road trip on the east coast (Florida to North Carolina) the week of July 6, visiting his brother-in-law's **Green Berets graduation ceremony** after nearly three years of Special Forces training.
- Akhil's **gender reveal event is scheduled for Sunday, June 28**, with friends planning the arrangements as a surprise.
- The team is experiencing a light workload with **half the team out on vacation** during summer break season, with colleagues like Barat taking two weeks off to visit Europe.

### Tremendous Campaign Matching Methodology

- The team is matching Tremendous fulfillment data to Lead Manager campaigns using **recipient names as the campaign identifier** (per JC and Sharita's direction from the BI team), rather than traditional campaign names.
- Tremendous data contains **approximately 9,000 distinct recipient names** that appear to be individual people's names (e.g., Lisa Gardner, Philip Morris) rather than typical campaign naming conventions.
- The Tremendous data is confirmed to contain **only Lead Manager campaigns**, which includes a mix of Playful, Tune(?), and Game Story gaming campaigns like Monopoly Go and Royal Match.
- Investigation revealed that Tremendous also contains traditional campaign names (e.g., 'TSW Commerce Media Generic') alongside the recipient-based naming, with **116 total campaigns** including gaming titles.
- Akhil had previously created a fuzzy match using campaign names, but JC later redirected the approach to use recipient names for matching. The campaign name approach may still be viable for certain record types.

### Payment Calculation Logic

- Tremendous data has two status columns: execution status (executed, cancelled, failed) and reward delivery status (pending, failed, succeeded) that determine final payment amounts.
- For **cancelled transactions**, the payment_subtotal shows the refund amount that needs to be subtracted from the original payout (e.g., $75 executed minus $75 refunded equals $0 net).
- The team agreed to **filter out failed and pending statuses** entirely from payment calculations, as these represent unsuccessful transactions that shouldn't count toward fulfillment totals.
- To calculate total payouts, sum only executed transactions with succeeded delivery status, then subtract any cancelled transactions to account for refunds returned to Tremendous.

### Other Technical Updates

- Akhil identified a potential **data duplication concern** with the event mapping ticket, as a BU classifications table in the Fluent catalog may already contain similar event type mappings for TUNE offers.
- The fact_advertiser_performance_hourly table already includes event type classifications from TUNE, suggesting the new event mapping work might be creating a **subset or duplicate of existing data**.
- Trevor announced that **DevOps is setting up a foreign catalog** that will enable direct access to JC's data, eliminating the current back-and-forth process for checking data availability.

## Transcript

[00:00:00] Trevor Anderson: Pretty easy going.
[00:00:02] Akhil Gonna: Yeah, I know. I think the next week is going to be like.
[00:00:07] Trevor Anderson: I think it's going to be the same next week because yeah. Like half the team is out.
[00:00:15] Akhil Gonna: You know. I just realized I didn't like a two day week. Yeah.
[00:00:24] Akhil Gonna: I didn't know why there are so many out of office but I just realized yesterday is like the last day of this academic year and from today it's like holiday for all the kids.
[00:00:37] Trevor Anderson: Yeah, I'm sure like all the. Everyone's out on summer break now or just started and so.
[00:00:44] Akhil Gonna: Yeah. Right.
[00:00:45] Trevor Anderson: If you're a parent, you know you gotta take your vacation now.
[00:00:51] Trevor Anderson: Well, it's a good time because July, I mean. Right. The fourth of July for the United States. Like we.
[00:00:59] Trevor Anderson: It's two days off.
[00:01:00] Akhil Gonna: Right.
[00:01:02] Trevor Anderson: It's a good time to plan like vacations around that you do a week before the week after. I'm.
[00:01:07] Trevor Anderson: I see I'm out the week after. Same with a few. Same with like Barats out for two weeks. He's going to.
[00:01:15] Trevor Anderson: I don't know where what he's doing but he's.
[00:01:19] Akhil Gonna: Europe.
[00:01:20] Trevor Anderson: Oh he went to Europe?
[00:01:21] Akhil Gonna: Yeah.
[00:01:22] Trevor Anderson: Oh, that's cool.
[00:01:24] Akhil Gonna: Yeah, that's tough.
[00:01:26] Trevor Anderson: Yeah. You're Europe. Summer. I like that.
[00:01:30] Akhil Gonna: Yeah, it's a pretty good time to go.
[00:01:33] Trevor Anderson: I went to Barcelona in August for my. For a honeymoon for a couple weeks. That was pretty fun.
[00:01:43] Akhil Gonna: Yeah, that's good. That's good.
[00:01:46] Trevor Anderson: Yeah, that was fun. I miss it. I want to go back. No, I'm. I'm going to.
[00:01:54] Trevor Anderson: Not next week but the week after. Going on a road trip on the east coast.
[00:02:01] Trevor Anderson: Gonna visit my brother in law but we're starting in Florida and we're going up to North Carolina so.
[00:02:07] Akhil Gonna: Oh nice.
[00:02:07] Trevor Anderson: We're doing like a southern road trip. We're gonna.
[00:02:12] Trevor Anderson: Yeah, we're gonna do some history, you know like go check out the old, old cities.
[00:02:21] Akhil Gonna: So it will be touching Utah or Las Vegas or.
[00:02:26] Trevor Anderson: No, I'm going to the east coast. So Going, going all the way to Florida on the east coast.
[00:02:31] Trevor Anderson: Oh, I'm not, I'm not driving across the country. I'm driving up the coast.
[00:02:38] Trevor Anderson: I would not drive across the country. That's crazy. I would take. That would take weeks.
[00:02:44] Akhil Gonna: Yeah, that's what I thought. Like it's. I thought you'll drive from California to Florida.
[00:02:50] Trevor Anderson: No, I'm flying to Florida.
[00:02:51] Akhil Gonna: Okay. And then take it.
[00:02:54] Trevor Anderson: I'm not gonna drive to Florida. No, that's. I would never do that.
[00:03:02] Trevor Anderson: Yeah, it sounds terrible because then I have to drive through.
[00:03:05] Trevor Anderson: I Have to drive through like New Mexico and Arizona and Texas.
[00:03:12] Trevor Anderson: Yeah, I feel like it would take like five days just to drive through Texas.
[00:03:16] Akhil Gonna: Yeah, I know.
[00:03:23] Trevor Anderson: Yeah. No, I don't want to do, do that. It's only like a six hour road trip from.
[00:03:28] Trevor Anderson: We're going from like north Florida to. And then we're stopping in South Carolina for a few days.
[00:03:37] Trevor Anderson: So we're going to Charleston.
[00:03:40] Trevor Anderson: It's like a historic town and I'm gonna stay there for a few days and then we're gonna go like.
[00:03:47] Trevor Anderson: I think we're gonna visit like Walmart city, maybe like the Outer Banks or something.
[00:03:52] Akhil Gonna: Okay. Yeah. I never heard of it. I know, like the Disneyland this stuff, but not the history.
[00:04:01] Trevor Anderson: Yeah, we're going to.
[00:04:02] Trevor Anderson: So we're flying into Jacksonville, Florida and then we're going to this town called St. Augustine.
[00:04:09] Trevor Anderson: And apparently St. Augustine, Florida is the oldest city in the United States.
[00:04:14] Akhil Gonna: Oh, wow.
[00:04:15] Trevor Anderson: Yes. So it was like one of the first, one of the first, like areas where I don't think America.
[00:04:25] Trevor Anderson: I think like the Spanish. Yeah, the Spanish landed there.
[00:04:29] Trevor Anderson: Like that was the first spot that they landed and made a city there back in, I don't know, like the 7 17, early 1700s or 1600s.
[00:04:41] Trevor Anderson: So hundreds of years ago. But it's the oldest city in the United States, so.
[00:04:47] Akhil Gonna: Yeah, that's cool. That's cool.
[00:04:49] Trevor Anderson: That be cool. See some old forts and cannons and stuff like that.
[00:04:55] Akhil Gonna: Yeah. Even Canadian Canada has a similar place. It's called Canada Birthplace.
[00:05:00] Akhil Gonna: It's like close to east coast Nova Scotia. That's where like I think Spanish or someone found French.
[00:05:10] Akhil Gonna: Yeah, French. Probably French found Canada. It's called Canada Birthplace.
[00:05:15] Trevor Anderson: That's cool.
[00:05:16] Akhil Gonna: Yeah, yeah.
[00:05:18] Trevor Anderson: I like doing some American history.
[00:05:21] Akhil Gonna: Yeah, sometimes.
[00:05:24] Trevor Anderson: But yeah, my, our brother in law is graduating from Green Berets, which is like Special Forces in the army.
[00:05:31] Trevor Anderson: So he just got done with like two, like almost three years of training.
[00:05:36] Akhil Gonna: Oh, nice.
[00:05:37] Trevor Anderson: Past the, the training, they're doing like a, you know, a ceremony and so we're gonna. He.
[00:05:48] Trevor Anderson: He just had a baby and all that, so we're gonna visit him.
[00:05:52] Akhil Gonna: That's cool.
[00:05:53] Trevor Anderson: Yeah, I'm excited.
[00:05:55] Akhil Gonna: Yeah. This weekend is. Is a gender reveal actually this Sunday. So I'm excited for that as well.
[00:06:03] Trevor Anderson: Oh, that's awesome. Oh, that's really cool. You guys not gonna do the surprise?
[00:06:08] Trevor Anderson: You know, like wait until and.
[00:06:11] Akhil Gonna: No, no, not the surprise, but my friends are making the arrangements for this gender reveal.
[00:06:17] Trevor Anderson: Are you doing like a little party or something? Are you gonna have. How are you gonna reveal it?
[00:06:22] Akhil Gonna: I Don't know. My friends are planning, so I don't know.
[00:06:25] Trevor Anderson: You won't even know. They're gonna reveal it to you. They're gonna surprise you.
[00:06:29] Akhil Gonna: Yeah, that's right. Yeah.
[00:06:31] Trevor Anderson: Oh, that's fun. Have you seen all those videos?
[00:06:36] Akhil Gonna: Yeah, I was seeing all those videos and.
[00:06:39] Akhil Gonna: And trying to understand how that works because back in India, they never revealed the gender before the birth.
[00:06:47] Akhil Gonna: So this is something new.
[00:06:49] Trevor Anderson: Oh, yeah, they have like, you pop the balloon and like.
[00:06:53] Akhil Gonna: Yeah.
[00:06:54] Trevor Anderson: Blue or pink confetti comes out or.
[00:06:56] Akhil Gonna: Yeah, I think. I guess. I guess it would be. Yeah.
[00:07:01] Trevor Anderson: Yeah. There's all these, like little.
[00:07:03] Trevor Anderson: I don't know, Western culture has like all these different things, like random stuff that we do.
[00:07:11] Trevor Anderson: It's funny.
[00:07:14] Akhil Gonna: Yeah, that's cool.
[00:07:17] Trevor Anderson: Well, that'll be fun. But yeah, I think next week will be pretty. But he's going as well. Let's see.
[00:07:28] Trevor Anderson: You're pretty much working. You've been. You've been working with this tremendous data.
[00:07:36] Akhil Gonna: Yeah. Yeah. This time on this data, JC have found.
[00:07:41] Akhil Gonna: I mean, it's not very optimized way of getting the campaign, but at least we have some real.
[00:07:49] Akhil Gonna: Actually, I was on that recently. Do you see my screen?
[00:07:57] Trevor Anderson: Yeah, yeah, yeah.
[00:08:00] Akhil Gonna: So this rewards recipient is coming from tremendous and this campaign name is like from lead manager and they both match.
[00:08:15] Trevor Anderson: Campaign name is a name. Is an actual person's name recipient. Yeah.
[00:08:19] Akhil Gonna: I don't know. Yeah, it seems like a person name. Instead of.
[00:08:23] Trevor Anderson: Is that actually is a column called campaign name from lead manager.
[00:08:27] Akhil Gonna: Lead Manager. It's called name. Just name.
[00:08:30] Trevor Anderson: Oh, why'd you name it? Yeah, just this one name.
[00:08:35] Akhil Gonna: Yeah.
[00:08:36] Trevor Anderson: Okay. Why does that look like. Like. Like just someone's name?
[00:08:40] Akhil Gonna: Yeah. In. In few case. I don't know, for some reason, in tremendous. All the.
[00:08:45] Akhil Gonna: I mean, the instruction given by JC or maybe Sharita given like rewards recipient name is the campaign name they're using for fuzzy match.
[00:08:56] Akhil Gonna: In here you can see all 20. Seems like some person's name that does.
[00:09:02] Trevor Anderson: That's on the campaign. That's. I'm confused.
[00:09:10] Akhil Gonna: Yeah. Yeah. But when I tried to match, I was able to find some in lead Manager.
[00:09:17] Akhil Gonna: If you see, I found this Lisa Gardner, for example. I mean, Philip Morris.
[00:09:27] Trevor Anderson: Some of them. Yeah. Some of them are not exactly.
[00:09:32] Akhil Gonna: They're not exactly the same. But in. Yeah. Even in lead manager.
[00:09:39] Akhil Gonna: What I'm trying to say is even in lead manager campaign name, we have like a person's name, someone's name.
[00:09:46] Akhil Gonna: Yeah. Yeah. I don't know why, but yeah, that's how JC have instructed us really?
[00:09:52] Trevor Anderson: There is only 20. Well above. Didn't you only have 20 rows?
[00:09:59] Akhil Gonna: 20 distinct recipient names from Tremendous.
[00:10:04] Trevor Anderson: Well, that's not useful, right?
[00:10:08] Akhil Gonna: Oh, yeah, sorry. Sorry.
[00:10:09] Trevor Anderson: Oh, you're limited. 20. Okay.
[00:10:11] Akhil Gonna: Okay. I think 9,000 something odd.
[00:10:18] Trevor Anderson: Why? I'm confused why it says recipient though. Just seems like whoever received the.
[00:10:27] Trevor Anderson: Are they saying sometimes the advertiser receives a recipe or receives the fulfillment.
[00:10:37] Trevor Anderson: These just look like people's names. Like. Yeah, like just PI. Like whoever's using.
[00:10:41] Trevor Anderson: Like what is tremendous for? Is this for. Is this fulfillment for.
[00:10:49] Akhil Gonna: Yeah, playful. Yeah, this is required.
[00:10:53] Akhil Gonna: Yeah, I think this is required to get how much fulfillment was down, what payouts were done or successful for the campaign.
[00:11:02] Akhil Gonna: From what we came to know, this has only lead manager campaigns. No other BU's.
[00:11:13] Trevor Anderson: Are those gaming campaigns or like what are they? Oh, lead manager campaigns.
[00:11:20] Akhil Gonna: Yeah.
[00:11:21] Trevor Anderson: Do you know if lead manager is.
[00:11:23] Trevor Anderson: Includes playful campaigns in it or is that something completely different?
[00:11:31] Trevor Anderson: Does it have like Monopoly go and stuff like that?
[00:11:33] Akhil Gonna: Yeah, yeah, they have Monopoly.
[00:11:36] Trevor Anderson: So it does. Yeah, yeah, it does. It has this like Royal match. These are playful.
[00:11:44] Akhil Gonna: Yeah. I think lead manager has combination of playful tune. What else? Game story.
[00:11:59] Trevor Anderson: Lots of games. Okay.
[00:12:05] Trevor Anderson: So if you look up like one of those people's names that match, it just shows up in here.
[00:12:11] Akhil Gonna: Yeah.
[00:12:12] Trevor Anderson: Like can you just. You can you just do a wear filter or something like that? Just pull one of those.
[00:12:19] Trevor Anderson: Lisa Gardner.
[00:12:20] Akhil Gonna: The heck?
[00:12:22] Trevor Anderson: That's so bizarre.
[00:12:44] Akhil Gonna: Yeah. Penguin random house area. This.
[00:12:56] Trevor Anderson: I feel like this has to be a mistake. But that's what JC was saying to do. What the heck.
[00:13:10] Akhil Gonna: Yeah, I mean, even for jc, the BI team from lead, my Charita was directing on this
[00:13:19] Trevor Anderson: and they were only using like payout names. Like that doesn't.
[00:13:25] Akhil Gonna: Yeah, this seems like a valid campaign name. Lisa Gardner. But I don't know who created this.
[00:13:37] Akhil Gonna: Sales ID brand.
[00:13:41] Trevor Anderson: Well, why would you have a campaign just be like someone's name?
[00:13:47] Akhil Gonna: I don't know.
[00:13:50] Trevor Anderson: It doesn't seem right.
[00:13:52] Akhil Gonna: Is there any like independent or. I don't know, I'm just a guest,
[00:13:57] Trevor Anderson: but a different type or something.
[00:13:59] Akhil Gonna: Yeah, maybe independent or campaign on social media or something.
[00:14:09] Trevor Anderson: Yeah, maybe it's like a fulfillment campaign or something like that. I don't know.
[00:14:14] Trevor Anderson: They're just like, oh, these are just like strictly to fulfill do fulfillments for or something.
[00:14:23] Trevor Anderson: I don't know.
[00:14:25] Akhil Gonna: Yeah, sorry. I just remembered when I was talking about social media.
[00:14:30] Akhil Gonna: Did you attend the TR meeting the other day? The new. Yeah, yeah, yeah, yeah.
[00:14:38] Akhil Gonna: That was pretty interesting.
[00:14:40] Trevor Anderson: That's the world I came from. I came from Media agency.
[00:14:45] Akhil Gonna: Oh, that's nice. Yeah.
[00:14:47] Trevor Anderson: Yeah.
[00:14:48] Akhil Gonna: And like this I was, I was getting so many. But something called Amaze is the company name.
[00:14:57] Akhil Gonna: It even does the same. What does?
[00:15:00] Trevor Anderson: I guess I. Have you downloaded Playful Rewards?
[00:15:06] Akhil Gonna: No, not yet. No.
[00:15:09] Trevor Anderson: It's really bad. Don't do it.
[00:15:12] Akhil Gonna: Okay. Yeah, I'm not too much into games that now.
[00:15:15] Trevor Anderson: They're like really, they're like very. How do I describe it?
[00:15:22] Trevor Anderson: They're like all those games where there's just like ad pop ups all the time.
[00:15:27] Trevor Anderson: Like app like, you know, like really bad.
[00:15:31] Akhil Gonna: Okay.
[00:15:32] Trevor Anderson: Games on your phone that just are just like scammy.
[00:15:38] Akhil Gonna: I think that's Media solutions.
[00:15:42] Trevor Anderson: Amaze. No. Yeah. Tron is just like a traditional marketing agency, you know, like or ad parlor.
[00:15:55] Trevor Anderson: And so I. It's pretty cool that we have that line of business as well.
[00:16:01] Trevor Anderson: But it's very competitive industry right now.
[00:16:05] Akhil Gonna: Yeah, I know, I know.
[00:16:06] Trevor Anderson: Extremely competitive.
[00:16:07] Trevor Anderson: It's very difficult to have a, a very good, you know, media agency is, you know, that's a challenge.
[00:16:17] Akhil Gonna: Yes.
[00:16:19] Trevor Anderson: Okay. You know what I think might be the case? Is that so tremendous is strictly a fulfillment.
[00:16:27] Akhil Gonna: Yes.
[00:16:28] Trevor Anderson: Data. Right.
[00:16:29] Trevor Anderson: And so maybe what they're saying is like, instead of treating it as like, oh, we have a, we have like a, an ad campaign that we're like marketing Monopoly, go, whatever.
[00:16:50] Trevor Anderson: Instead of thinking about it like that, maybe it's saying, you know, we want to actually have a category that's just strictly for fulfillments and they're just going to have the person's name that we do the payouts for and that's how it's going to come into the manager.
[00:17:08] Trevor Anderson: I wonder if those have like some type of ID or taxonomy that makes them different from the other campaigns.
[00:17:17] Trevor Anderson: All the ones with names in them.
[00:17:19] Akhil Gonna: Yeah, yeah, that's one thing.
[00:17:21] Akhil Gonna: And the other thing which I was mentioning is this piece, you know, we discussed on the other call.
[00:17:26] Akhil Gonna: But maybe once Brian is back, I will discuss with him about the logic.
[00:17:34] Akhil Gonna: So the idea is to get how much reward was paid out. Right.
[00:17:38] Akhil Gonna: In this case, if you see the status here, let me actually get the distinct status just to be.
[00:17:52] Akhil Gonna: Where is this query? Here we go. So tremendous data has 2, 2 columns which gives us the status.
[00:18:05] Akhil Gonna: The first one is telling me whether it's executed, cancel or failed.
[00:18:11] Akhil Gonna: And the second is like reward delivery status.
[00:18:14] Akhil Gonna: Whether the reward is delivery or this has like pending failed and succeeded. Okay. So when I.
[00:18:24] Akhil Gonna: And I want to get the rewards being paid to this email and for One specific campaign ID, when I do that, I found five, five records among this one is executed.
[00:18:40] Akhil Gonna: Let's say the $75 has been paid out and there are four records which say cancelled. I don't know if.
[00:18:50] Akhil Gonna: If I need to subtract right. Or deduct all this 75 sum up and direct. If you see the.
[00:18:59] Akhil Gonna: It says payment refunded. I don't know. It doesn't make sense how. How we take the refund out.
[00:19:10] Trevor Anderson: Oh it looks. It's possible that. So payment subtotal is like okay, tremendous. Pays the person $75.
[00:19:20] Akhil Gonna: Yeah.
[00:19:21] Trevor Anderson: But the transaction was canceled so they refunded 75.
[00:19:28] Trevor Anderson: So that's why it's like those two cancel each other out. 75 minus 75 is zero.
[00:19:33] Akhil Gonna: Oh this gets. Yeah, in this case 75 minus null which is zero is like 75.
[00:19:41] Trevor Anderson: So that means it was 75 was paid out.
[00:19:44] Akhil Gonna: Okay so the grand total here will be 75.
[00:19:47] Trevor Anderson: Right. That's what it seems like. I'm pretty sure.
[00:19:52] Akhil Gonna: Okay, got it is.
[00:19:53] Trevor Anderson: Yeah.
[00:19:54] Trevor Anderson: I'm curious if you look at like executed and failed or something like that status what that looks like is that even one of your distinct items go up real quick to your
[00:20:15] Akhil Gonna: status is executed. Rewards is failed or cancelled. Let me check. See.
[00:20:31] Trevor Anderson: No, it doesn't have that. Oh it does. Executed failed. Yeah, there's a few of them.
[00:20:42] Akhil Gonna: So in this case 75 minus 0.
[00:20:48] Trevor Anderson: Yeah but it failed. So I wonder if it even.
[00:20:52] Akhil Gonna: I think fail doesn't count. I think by default is at zero.
[00:20:56] Akhil Gonna: If I'm not wrong or it's do we considered that payment is like did I even.
[00:21:03] Trevor Anderson: Yeah. Would you even. This is why I'm curious like if you're summing it right.
[00:21:08] Trevor Anderson: Would you say oh we paid out 75 or would you say 0 because it failed?
[00:21:13] Akhil Gonna: Yeah, if it is failed I would make it zero because the payment has not been done.
[00:21:20] Trevor Anderson: Okay, maybe it's not. Yeah, maybe. Can you check all the those camp.
[00:21:26] Trevor Anderson: All those campaigns and see or similarly like how you did below if you check all those records there for that top one and see if there was like another one that tried to go through Campaign 8.
[00:21:55] Akhil Gonna: Yeah, there is so many of my dispute as well. Oh, what I'm trying to do here. It's. Successful.
[00:22:11] Akhil Gonna: Maybe I need to have the recipient email.
[00:22:20] Trevor Anderson: Oh, you have someone specifically. That's a bunch of people. So we need to find the one.
[00:22:30] Akhil Gonna: Yeah, let's take this guy. This guy has two. Has it been teammail?
[00:22:38] Trevor Anderson: Yeah, but we need the one with the failed status. So I don't know if this guy has it.
[00:22:43] Akhil Gonna: This guy has two success. Which means.
[00:22:46] Trevor Anderson: Yeah, you gotta include the email on the top query there as another column.
[00:22:57] Trevor Anderson: Oh, you could do that too.
[00:22:59] Akhil Gonna: Oh, maybe I'll just.
[00:23:12] Akhil Gonna: Okay, okay. There is only one include.
[00:23:16] Trevor Anderson: Include one of those and take out the filter for failed.
[00:23:20] Akhil Gonna: Yeah.
[00:23:40] Akhil Gonna: Okay. For one campaign, he has one executed and one succeeded. Okay.
[00:23:53] Trevor Anderson: Okay. So I think the total here is 75.
[00:23:58] Akhil Gonna: Yeah, right, right.
[00:23:59] Trevor Anderson: So you, I mean cancel failed.
[00:24:03] Akhil Gonna: Yeah. Okay. Okay. See wherever cancel.
[00:24:10] Trevor Anderson: I feel like whenever the status is failed, then you don't even. You just ignore it.
[00:24:15] Akhil Gonna: Yeah.
[00:24:15] Trevor Anderson: Like you probably want to filter those out. Right. For pending as well or pending too.
[00:24:21] Trevor Anderson: Yeah, yeah, interesting. Okay. But yeah, I think for canceled you need to just subtract like.
[00:24:35] Trevor Anderson: Canceled succeeded. Like what the case you had before. Right? Yeah.
[00:24:40] Akhil Gonna: If it is cancel succeeded, I would take the payment total and then it is canceled.
[00:24:49] Trevor Anderson: So the thing is like if you want the sum of like refunds, then you need to take cancel.
[00:24:54] Trevor Anderson: You need to sum canceled, cancel succeeded and look at the total there. Right.
[00:25:01] Trevor Anderson: Because then I'll show you how it was returned back to. Back to tremendous.
[00:25:12] Trevor Anderson: And then, and then for the consumer, like it would be right, it would be.
[00:25:20] Trevor Anderson: I feel like it would only be status is executed like you don't even look at.
[00:25:26] Trevor Anderson: Like you don't look at canceled by any time.
[00:25:30] Akhil Gonna: Do you know where I can compare this?
[00:25:32] Akhil Gonna: Whether this fulfillment is right around the other day couple was mentioning.
[00:25:37] Trevor Anderson: I don't know but apparently there's fulfillment data in Playful too, which I kind of wanted you to just take a look.
[00:25:44] Akhil Gonna: Okay. Yeah, maybe I'll check in the playful to compare at least a few maps.
[00:25:49] Trevor Anderson: Like can we, can we take a look real quick to see?
[00:25:51] Akhil Gonna: Yeah, sorry it's painful.
[00:25:58] Trevor Anderson: Scroll down there.
[00:26:00] Akhil Gonna: Yeah. Playful rewards gold.
[00:26:09] Trevor Anderson: Go to Silver. Silver has everything. I don't know why only gold has a few things.
[00:26:17] Akhil Gonna: User reward. Here.
[00:26:24] Trevor Anderson: I don't know where fulfillment would be under. Maybe it's that one.
[00:26:29] Akhil Gonna: User reward, I guess. State name USD. Is there a username here? App user of ready.
[00:26:50] Akhil Gonna: Or maybe I need to join with the user.
[00:26:54] Trevor Anderson: Cool.
[00:26:57] Akhil Gonna: Silver. Oh, user profile. App uid is there app uid here?
[00:27:11] Trevor Anderson: There's apps.
[00:27:19] Akhil Gonna: App user uuid. Maybe I need to join with that.
[00:27:23] Trevor Anderson: Oh, I see.
[00:27:42] Trevor Anderson: Yeah. The thing is I don't think there's any. Like user profile doesn't have the name.
[00:28:09] Trevor Anderson: You have a pipe. A python. It's area. The thing is. Yeah, user profile I'm looking at. This is.
[00:28:20] Trevor Anderson: Is like an email hash.
[00:28:22] Akhil Gonna: Oh, email hash type. We have raw emails. Oh, email for playful.
[00:28:29] Trevor Anderson: I don't. Thought we would. So how are you supposed to know?
[00:28:44] Akhil Gonna: Do we have names? There is no name. Also, Is it like MD5? So we hash and write.
[00:29:06] Trevor Anderson: I don't know what it is. You could try MD5 or Shaw like,
[00:29:11] Akhil Gonna: but
[00:29:13] Trevor Anderson: I don't know.
[00:29:15] Akhil Gonna: Okay, I'll take a look at it.
[00:29:19] Trevor Anderson: Well, and so what is that? There's payout in the reward. It's USD paid.
[00:29:28] Akhil Gonna: Yeah, USD paid.
[00:29:29] Trevor Anderson: Sorry. Okay, well. And we're only paying the people. Not. Not like. Like on Tremendous. It's.
[00:29:40] Trevor Anderson: Or you were looking at Tremendous before, right?
[00:29:43] Akhil Gonna: Yeah.
[00:29:45] Trevor Anderson: Is there. Is it always someone's name?
[00:29:50] Akhil Gonna: Yeah, there are names.
[00:29:51] Trevor Anderson: Recipient. You know, is there ever a campaign by campaign name? Do you have campaign name here?
[00:29:57] Akhil Gonna: We have campaign id.
[00:29:59] Trevor Anderson: Just campaign id. You don't have the name of the.
[00:30:03] Akhil Gonna: I need to join with something else. But yeah, I can get the name of it. Select.
[00:30:33] Akhil Gonna: Cannot be fine.
[00:30:36] Trevor Anderson: Oh, we miss the table.
[00:30:41] Akhil Gonna: Need manage of. My bad. Okay, the campaign name is TSW Commerce Media Generic.
[00:31:03] Trevor Anderson: Wait, so what were you doing before like that fuzzy match tsw.
[00:31:11] Trevor Anderson: I thought that the only campaigns were like actual just people's names, but it looks like we actually have campaign names in here.
[00:31:17] Trevor Anderson: These you not.
[00:31:20] Akhil Gonna: Oh, yeah, yeah, that was.
[00:31:23] Akhil Gonna: I mean this was done before, but later JC asked us to map based on the recipient name.
[00:31:30] Akhil Gonna: But I also have this fuzzy map somewhere.
[00:31:36] Trevor Anderson: Okay, I feel like that's what we're trying to get right. This. This is the actual campaign name.
[00:31:43] Akhil Gonna: Yeah. Right.
[00:31:50] Trevor Anderson: Now I wonder if that campaign's in playful.
[00:31:54] Akhil Gonna: Yeah, we should have this.
[00:31:58] Trevor Anderson: Oh, we need probably a campaign that's like a gaming thing. Not like. What was that? A bank?
[00:32:17] Akhil Gonna: Yeah, there are no like 116 campaigns.
[00:32:24] Trevor Anderson: Like, is there any gaming ones in here?
[00:32:29] Akhil Gonna: Yeah, Quick game Cash.
[00:32:34] Trevor Anderson: No, I don't know if that's for sure. I know like Monopoly go.
[00:32:41] Trevor Anderson: That's pretty much the only one I know for sure.
[00:32:50] Akhil Gonna: Yeah, monopoly go. Here. Okay.
[00:32:54] Trevor Anderson: Okay. Oh, okay. Someone offers. Is an offer the same as a campaign? Probably not. It has no products.
[00:33:12] Trevor Anderson: What is products in this case?
[00:33:18] Akhil Gonna: I have no idea.
[00:33:19] Trevor Anderson: There's a product stable?
[00:33:21] Akhil Gonna: I think so. I think there should be more data.
[00:33:26] Akhil Gonna: There must be one more products table where we need to join this. But for now we do not have it.
[00:33:38] Akhil Gonna: But from what JC mentioned is this.
[00:33:41] Akhil Gonna: This tremendous campaigns are only from lead manager or lead manager has some playful as well.
[00:33:46] Akhil Gonna: I guess
[00:33:50] Trevor Anderson: that's what I'm trying to link them all.
[00:33:57] Akhil Gonna: Yep.
[00:33:59] Trevor Anderson: Okay.
[00:34:03] Akhil Gonna: Yeah, but yeah, these campaigns this has like. I can maybe try to fuzzy match this one.
[00:34:12] Akhil Gonna: Yeah, this should be fine.
[00:34:14] Akhil Gonna: But there is one more ticket maybe I want to discuss with Brian also once he's back.
[00:34:20] Akhil Gonna: But you remember Bharat was mentioning about this ticket.
[00:34:27] Akhil Gonna: This is to create a event mapping under Fluent catalog.
[00:34:32] Akhil Gonna: But this seems like we already have a table called. What is the table called? BU Classifications.
[00:34:46] Akhil Gonna: This one.
[00:35:12] Akhil Gonna: Oh, it's not on top of my mind, but.
[00:35:19] Trevor Anderson: We'll go back to the. Go back to the ticket.
[00:35:22] Akhil Gonna: Yeah, sure.
[00:35:24] Trevor Anderson: Business units.
[00:35:29] Akhil Gonna: Yeah. This is basically getting all the TUNE offers and trying to find which event it is. Event type.
[00:35:37] Trevor Anderson: There's only like seven rows in this table.
[00:35:39] Akhil Gonna: Yeah, that's right. Yeah. This is more likely finding which event value and bold type.
[00:35:49] Akhil Gonna: I have seen somewhere in some other table that that's being already allocated.
[00:35:54] Akhil Gonna: I thought it's PU classifications. Fluent campaign.
[00:36:06] Trevor Anderson: Now it's probably like.
[00:36:08] Akhil Gonna: Oh, it's.
[00:36:09] Trevor Anderson: I don't know what you're.
[00:36:11] Akhil Gonna: It's in the fact table. Yeah. Fact. Advertiser performance hourly. These are even fetching from tune.
[00:36:20] Akhil Gonna: And this has like the event type. In this case if you see this is called even.
[00:36:26] Akhil Gonna: There are two events even two event six, ten. Something like this maybe.
[00:36:34] Akhil Gonna: Is this table like expanded version of this one?
[00:36:38] Akhil Gonna: Like in this case we have 10 advertisers of this event six. Maybe I need to display in row.
[00:36:48] Akhil Gonna: Row is my question. Yeah, maybe I'll check with Brian. But yeah. Just. Just want to let you know.
[00:36:58] Akhil Gonna: Full name. Yeah. That we already have some events classified in background. Whatever.
[00:37:04] Trevor Anderson: Yeah. I feel like it's a similar.
[00:37:07] Akhil Gonna: Yeah.
[00:37:08] Trevor Anderson: Thing is this too. Huh. But it's. It's long format instead of wide. Why is this only in.
[00:37:20] Trevor Anderson: Is this only in DEV or. No, this isn't prod too.
[00:37:23] Akhil Gonna: Yeah. I think. I'm not sure if this is promoted or still in development.
[00:37:28] Trevor Anderson: No. Why is there only. There's only one table in dev. I mean in prod just be classification.
[00:37:33] Akhil Gonna: I think maybe for next list this would go.
[00:37:37] Trevor Anderson: There's a lot of stuff.
[00:37:38] Akhil Gonna: Yeah. Yeah. I just want to make sure I don't duplicate the data.
[00:37:47] Akhil Gonna: From what I understood my table is like a subset of this table. But yeah.
[00:37:54] Akhil Gonna: I'll double check with Brand on Monday. Okay.
[00:38:04] Trevor Anderson: I also see the naming. It's a little wonky in these dev tables though.
[00:38:15] Trevor Anderson: Like I don't think we need like Fluent underscore how it is right now. There's a Fluent campaign.
[00:38:23] Trevor Anderson: You see that?
[00:38:25] Akhil Gonna: Oh yeah, yeah, yeah. That should be removed. Even. Okay. This. This can have.
[00:38:39] Trevor Anderson: That's fine. The column's fine. But the table name. This is.
[00:38:53] Trevor Anderson: This probably this is what Venki is working on. I think it's still in progress. Yeah, it says SUD2.
[00:39:00] Trevor Anderson: I think we're.
[00:39:03] Akhil Gonna: This is from Signbridge.
[00:39:05] Trevor Anderson: Oh, you're right. This is from Signbridge.
[00:39:12] Akhil Gonna: May I see the top users? Yeah. Okay. Yep. Anyways. Okay.
[00:39:36] Akhil Gonna: Is there anything else or we can wrap it up?
[00:39:40] Trevor Anderson: Oh, yeah, there's. Last thing was just. I'm. I'm working on setting up a foreign catalog right now.
[00:39:47] Trevor Anderson: I think DevOps finally got to it.
[00:39:49] Trevor Anderson: So maybe in the future we'll be able to access JC's data from this catalog, which will be really.
[00:39:57] Trevor Anderson: It'll be much easier to navigate that than how we do it right now.
[00:40:01] Trevor Anderson: There's just a lot of back and forth. I'm like, does this exist? Right. We can check ourselves.
[00:40:08] Trevor Anderson: So that'll be nice. And then if you could submit the Akhil summary for Lattice today, that would be.
[00:40:22] Trevor Anderson: That would be great as well.
[00:40:23] Akhil Gonna: Yep. Sure. Sure, I can do that. Awesome. Okay.
[00:40:29] Trevor Anderson: Well, thanks for. Thanks for chatting. Have a great weekend.
[00:40:34] Akhil Gonna: Yeah, you too. You. Thank you. Take care. Bye.
[00:40:37] Trevor Anderson: Bye.
