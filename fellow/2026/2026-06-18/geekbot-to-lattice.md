---
title: "Geekbot to Lattice"
date: "2026-06-18"
start_time: "2026-06-18T10:00:00-04:00"
end_time: "2026-06-18T10:25:00-04:00"
meeting_id: "040000008200E00074C5B7101A82E00800000000D666FFB8E2F9DC010000000000000000100000005DE3CC05B48C654394C975BBB4968C6F"
note_id: 80267623
recording_id: 14659333
fellow_url: "https://fluentco.fellow.app/meetings/040000008200E00074C5B7101A82E00800000000D666FFB8E2F9DC010000000000000000100000005DE3CC05B48C654394C975BBB4968C6F/"
---

# Geekbot to Lattice

## Participants

| Name | Email | Organizer | RSVP |
|------|-------|-----------|------|
| Stephanie McCarthy | smccarthy@fluentco.com | yes | accepted |
| Dan Duling | dduling@fluentco.com | no | accepted |
| Jack Hall | jhall@fluentco.com | no | accepted |
| Trevor Anderson | tanderson@fluentco.com | no | accepted |

## Fellow Notes

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
[ ] Pass feature request to Lattice product team for native Slack bot integration to allow updates to be submitted directly in Slack without navigating to web interface.
[ ] Investigate Lattice's MCP server integration for posting updates to Slack channels and enabling Claude analysis, then report back to the team with findings.

## AI Summary

Stephanie led a discussion about transitioning from Geekbot to Lattice for collecting team updates, aiming to **centralize people data and track AI advancements** across the organization. The team identified key gaps in Lattice's current functionality, particularly the lack of **native Slack integration** for posting updates (unlike Geekbot's seamless in-app experience) and concerns about cross-team visibility and AI capabilities. Dan emphasized the importance of transparency, where updates automatically post to relevant Slack channels for cross-functional awareness, and the ability to use Claude to analyze trends across teams.

The group agreed on a **phased rollout approach**: Lattice updates will be deployed immediately to non-tech teams like finance, while tech teams will continue using Geekbot for daily standups and adopt Lattice only for weekly updates until the Slack integration improves. Stephanie committed to investigating Lattice's new MCP server for Claude integration and passing feature requests back to Lattice. Custom visibility permissions will be configured to enable cross-functional access, with an initial test planned for Dan's commerce media leadership team.

### Chapters

#### Geekbot vs Lattice comparison

- [00:05:38] Jack's team uses **Geekbot for daily standups** and has already set up Lattice for weekly updates, but Lattice doesn't support multiple update cadences or allow configuration of different update sets in the same account.
- [00:06:58] Trevor is currently using both tools personally but hasn't set up team-wide updates yet, though he expressed interest in collecting feedback from his team through Lattice for performance review purposes.
- [00:06:20] Lattice's key advantage is **integration with annual performance reviews**, where it can summarize and compile all update data throughout the year to streamline the review process and provide AI-powered insights.
- [00:14:39] Unlike Geekbot's seamless Slack integration, **Lattice requires users to click through to a web interface** (via Okta login) to submit updates rather than staying within Slack, creating additional friction.
- [00:07:52] The team is open to using Lattice for weekly or bi-weekly high-level updates while maintaining Geekbot for daily standup check-ins, creating a dual-system approach based on update frequency and purpose.

#### Transparency and visibility requirements

- [00:08:27] Dan emphasized that Geekbot's strength is **automatic posting to Slack channels** for transparency, where updates from dev leads post to both the main engineering channel and product channel for cross-functional visibility.
- [00:09:21] Dan uses **Claude in Slack to analyze Geekbot updates** across teams, asking it to review a quarter's worth of updates and flag red flags or trends, which provides valuable leadership insights.
- [00:12:35] Dan requires visibility into commerce media leadership updates (including Katie Mcclanahan, Tim Lukens, Jessica, and others) to understand weekly priorities across the organization, which he currently gets through Geekbot's Slack posts.
- [00:13:07] Stephanie confirmed that **Lattice supports custom visibility permissions** for cross-functional access, allowing leaders like Dan, Jack, and Trevor to view updates from stakeholders outside their direct reporting lines.
- [00:13:38] Dan agreed to test Lattice with the commerce media leadership team (including Patrick) once custom visibility permissions are configured to ensure the transparency requirements are met.

#### Technical integration gaps

- [00:16:29] Geekbot's workflow is streamlined: it prompts users in Slack with questions, shows previous updates for reference, and posts directly to designated channels without any application switching or authentication steps.
- [00:10:22] Dan raised concerns that Lattice may be using an **older AI model (4.2)** compared to current capabilities, where each new model generation represents approximately a 3x improvement in performance.
- [00:10:58] Lattice announced **early access to an MCP server for North America** (as of five days prior to the meeting), which would enable direct integration with Claude for AI-powered analysis of update data.
- [00:15:23] The team identified that allowing Slack bot conversations for updates (similar to Geekbot) would significantly reduce friction and increase adoption, which Stephanie agreed to pass as a feature request to Lattice.

#### Rollout strategy

- [00:15:46] An initial test is planned with **Dan's commerce media leadership team** to validate that Lattice can meet transparency and visibility requirements before broader tech team adoption.
- [00:18:21] Lattice updates will be rolled out immediately to **non-tech teams like finance** who aren't currently using Geekbot, avoiding the need for these teams to wait for Slack integration improvements.
- [00:18:32] Tech teams will receive separate messaging indicating they should **continue with Geekbot for daily standups** and wait for improved Slack integration before adopting Lattice for weekly updates.
- [00:19:38] The updates feature already exists in Lattice and some employees have been using it voluntarily, but the organization is now making it an **official expectation for all employees**, especially to track AI achievements and provide worldwide visibility.

### AI-Detected Action Items

- Stephanie McCarthy: Pass feature request to Lattice product team for native Slack bot integration to allow updates to be submitted directly in Slack without navigating to web interface.
- Stephanie McCarthy: Investigate Lattice's MCP server integration for posting updates to Slack channels and enabling Claude analysis, then report back to the team with findings.

### Decisions

- **Lattice updates will roll out to non-tech teams** (finance, etc.) immediately, while tech teams continue with Geekbot for daily standups and use Lattice for weekly updates until Slack integration is resolved.

## Transcript

[00:00:01] Stephanie McCarthy: How are things going with your morning?
[00:00:02] Trevor Anderson: A few meetings this morning, some stand ups, but just getting started.
[00:00:07] Stephanie McCarthy: The usual. Yeah, absolutely. Well, at least it's a long weekend. Looking forward to that.
[00:00:13] Stephanie McCarthy: Do you guys have anything planned with your extra day off?
[00:00:20] Trevor Anderson: We're, we have. My grandpa just had his birthday so the whole family is getting together.
[00:00:27] Trevor Anderson: We're going out to brunch. Yeah. Get to see my parents and grandparents and cousins.
[00:00:36] Stephanie McCarthy: Oh, fun.
[00:00:37] Trevor Anderson: Yeah.
[00:00:38] Stephanie McCarthy: Do you not get to see them very often or is this a regular occurrence for family birthdays?
[00:00:44] Trevor Anderson: We see them, you know, a few times a year, so it's good every time we get to all get together.
[00:00:51] Stephanie McCarthy: Love it. That sounds really fun. Jack, how about you? Anything planned with your extra day?
[00:00:58] Jack Hall: No, I might go out for a bike ride tomorrow, but I gotta see how things shake out. What about you?
[00:01:03] Stephanie McCarthy: Okay, excellent. That sounds fun. I know. Here's hoping that the weather is really nice this weekend.
[00:01:08] Stephanie McCarthy: It's the first day of summer on Sunday, so hopefully we get some sunny weather.
[00:01:14] Stephanie McCarthy: It's actually my birthday this weekend, so that's what I'm doing.
[00:01:16] Stephanie McCarthy: I'm going to get a massage and then my husband and I are going for high tea which should be be a nice treat.
[00:01:24] Stephanie McCarthy: So. Looking forward to that. Hey Dan, how's it going?
[00:01:29] Trevor Anderson: Good.
[00:01:29] Dan Duling: Where are you going to go for high tea?
[00:01:32] Stephanie McCarthy: There's this cute little place in Toronto called the Windsor Arms Hotel and they are known for their extra delicious scones.
[00:01:39] Stephanie McCarthy: They're like fluffy and cloud like and melt in your mouth.
[00:01:42] Stephanie McCarthy: So really looking forward to a sweet treat.
[00:01:46] Stephanie McCarthy: And we might, we'll probably upgrade to the sparkling wine version of high tea versus the tea only.
[00:01:54] Stephanie McCarthy: So you know, it should be nice. What are you doing with your extra day off, Dan?
[00:02:00] Dan Duling: Oh, probably just getting ready to be honest. We.
[00:02:03] Dan Duling: We're going down to Louisville and then we're going with a group of three families to the Smoky Mountains for like four days.
[00:02:15] Dan Duling: How nice. Yeah, no, it should be fun.
[00:02:19] Dan Duling: Looks a little rainy right now, but you know, not really anything you can kind of do about that.
[00:02:25] Stephanie McCarthy: No. Yeah, it's got to wait it out for sure. Well, I hope that, you know, weather's unpredictable.
[00:02:30] Stephanie McCarthy: Maybe it'll pan out in your favor. So my birthday's the Sunday. It's the longest day of the year.
[00:02:35] Stephanie McCarthy: I always love to try and find a patio in the evening, enjoy the sun as much as possible.
[00:02:40] Stephanie McCarthy: But June really is like not quite summertime. Do you know what I mean? It's like hit and miss.
[00:02:44] Stephanie McCarthy: Sometimes it's not a nice day and sometimes it is a nice day and I get to sit out and enjoy the extra long day.
[00:02:50] Stephanie McCarthy: So hopefully for you too, that it'll pan out to be a nice. A nice holiday for you
[00:02:57] Jack Hall: both.
[00:02:58] Dan Duling: The kids love the rain, so the hikes will.
[00:03:02] Dan Duling: Will do rain or shine, actually, that sometimes, especially even with the river crossings and everything, um, it's kind of more fun in the rain.
[00:03:12] Dan Duling: Okay, so sure.
[00:03:15] Stephanie McCarthy: Is that more fun because the river is kind of like moving a little faster and that's exciting or is it because, like, you're already wet so it's not so exciting.
[00:03:23] Dan Duling: You're already wet, so, you know, you don't have to worry.
[00:03:26] Dan Duling: Like, you know, the kids love to, you know, cross the. The fallen tree branches and everything else.
[00:03:33] Dan Duling: And, you know, it's always a bummer when you fall, right. And then you.
[00:03:37] Dan Duling: You're in water, but if you're wet, you know, it's like the first time they get wet, they don't really care anymore.
[00:03:43] Dan Duling: And so it's just kind of having fun with it and, you know, yeah, be. Be safe.
[00:03:49] Dan Duling: But, you know, also, you know, a little live on the edge a little bit so that, you know, love it.
[00:03:56] Stephanie McCarthy: It still sounds exciting.
[00:03:58] Jack Hall: Yeah.
[00:04:00] Stephanie McCarthy: Wow, Dan, that's impressive. Could never be me. I would not.
[00:04:02] Stephanie McCarthy: That does not sound like my idea of a holiday. So that's great. I'm glad. Okay, let's dive in.
[00:04:08] Stephanie McCarthy: I don't want to take up too much of your time, but would love to hear from this group around how and what your thoughts are in terms of leveraging the updates feature in Lattice.
[00:04:21] Stephanie McCarthy: Now, I know you guys are using Geekbot today.
[00:04:25] Stephanie McCarthy: Dan, I know that you and I have visited this conversation probably months and months ago, but Trevor and Jack, I wanted to hear from you guys and is Geekbot a tool that you're using to collect information from your team?
[00:04:38] Stephanie McCarthy: And if so, can I just show you a quick little what updates in Lattice has going just to see if we can kind of compare and contrast and potentially entertain what a switch to Lattice might look like.
[00:04:52] Stephanie McCarthy: What's driving this is that Lattice is sort of our tool of choice for people management, people sentiment, understanding what our employees are up to.
[00:05:02] Stephanie McCarthy: One of the biggest, I guess, tasks that we're looking to accomplish from a HR side and people side is being able to sort of see the kind of collect into one place.
[00:05:13] Stephanie McCarthy: All of the, like, AI advancements that our teams are making.
[00:05:18] Stephanie McCarthy: I know on the tech side, you guys are well ahead of where the rest of the organization is, but through the updates feature, we're Going to be able to collect all of that and be able to bubble it up to leadership in a really clear and succinct way.
[00:05:30] Stephanie McCarthy: So I'll pause there. What do you guys think about Geekbot? Are you using it today? Let's start there.
[00:05:38] Jack Hall: Yeah, we use, I use my team uses Geekbot for the daily standup updates that we give.
[00:05:46] Jack Hall: I also have already set up a weekly thing in Lattice to go on Fridays.
[00:05:51] Jack Hall: But I don't think Lattice lets you either one do things different daily or do multiple different things.
[00:05:58] Jack Hall: At least I couldn't figure out how to configure multiple sets of updates.
[00:06:03] Jack Hall: So I think for my use case right now it looks like it's a yes, I'll use both.
[00:06:09] Stephanie McCarthy: Okay. Yeah, the daily standups and then Lattice maybe for those weekly kind of overarching
[00:06:16] Jack Hall: and then for the for one on ones too. It's all the notes are tied up.
[00:06:20] Jack Hall: So that's because I figure Lattice will eventually do something to help surface those notes once the review periods come around.
[00:06:30] Jack Hall: I'm assuming that they will be doing that.
[00:06:33] Jack Hall: So yes, it figures it'll be easier if I can just get more in there early.
[00:06:37] Stephanie McCarthy: Yes, absolutely. So Lattice will kind of compile all of that.
[00:06:40] Stephanie McCarthy: It'll summarize it, you'll be able to kind of click through.
[00:06:43] Stephanie McCarthy: I know you weren't here when we rolled that out for our year end so we'll have to make sure we do a refresher for our mid year here.
[00:06:49] Stephanie McCarthy: But the more kind of content that's in Lattice, the easier those year end reviews are going to be.
[00:06:53] Stephanie McCarthy: So love to hear that you're leveraging it both ways. Trevor, how about you?
[00:06:58] Trevor Anderson: I'm. I'm leveraging both personally right now. So the team.
[00:07:02] Trevor Anderson: I'm not getting updates from my team right now.
[00:07:05] Trevor Anderson: Although that does sound like a good idea to set up a Geek Bot or have my team post in Lattice so I can have those updates.
[00:07:16] Trevor Anderson: But yeah, using both right now for providing updates on my side of things.
[00:07:24] Trevor Anderson: But yeah, definitely hear from Jack.
[00:07:28] Trevor Anderson: That sounds like a great idea to collect feedback from the team as well. Especially in Lattice.
[00:07:38] Trevor Anderson: Hearing that you guys do the annual reviews through this system, it can leverage that information that's collected throughout the year.
[00:07:49] Stephanie McCarthy: Yeah. But yeah, okay.
[00:07:52] Stephanie McCarthy: So kind of open to the idea of maybe having that weekly or bi weekly collection of that overarching kind of employee updates around what they're working on, but maybe still holding onto the Geek Bot for that daily check in that I know you guys do as part of your process for your Work.
[00:08:05] Stephanie McCarthy: Right. Okay. Love that you're open to it.
[00:08:09] Stephanie McCarthy: Dan, has anything changed since we last saw spoke around potentially opening up Lattice for the updates feature?
[00:08:19] Dan Duling: You know, nothing big I don't think yet.
[00:08:27] Dan Duling: You know the big thing that I like about the updates feature for Geekbot versus Lattice, right Is you know, especially when we look at the other leadership channels as well.
[00:08:38] Dan Duling: But even you know, like I do it for our dev leaders and then it posts into to our main channel and then it also posts into our product channel.
[00:08:48] Dan Duling: So now product also gets to see, you know, some of the initiatives from the leads across.
[00:08:54] Dan Duling: So the transparency is the big thing that I still like from the the Geekbot side.
[00:09:01] Dan Duling: So once Lattice can provide that same transparency in a very easy to access, you know, methodology, I'm totally fine with, you know, going ahead and moving over, but that's just the, the big one that we're kind of missing right now.
[00:09:21] Dan Duling: I would also love and I haven't seen if Lattice has done, you know, I haven't played around too much with their, you know, their solution yet and Claude, but you know, it's great to also have that Claude in Slack have such a seamless integration.
[00:09:38] Dan Duling: So for example, with the Geek Bot, I can go, hey, I want to look at the last quarter, looking at this update, looking at all the title geek bots that come in with this and I want you to look at the quarter and you know, note any red flags or trends that we're seeing across any groups that need to be evaluated.
[00:10:00] Dan Duling: You know, I don't think Lattice is quite at that level yet from what I've seen.
[00:10:07] Stephanie McCarthy: If so like you can ask it to like, hey, look back at my team's updates for the last quarter and summarize for me.
[00:10:16] Stephanie McCarthy: I'm not sure that you can automate that AI ask. Do you know what I mean?
[00:10:19] Stephanie McCarthy: Like you could, you have to go in and like type it into their AI.
[00:10:22] Dan Duling: Yeah, but the other thing is if I recall and I don't, I haven't checked again to see the Gladys is using, I want to say like 4.2.
[00:10:32] Dan Duling: They're using a pretty old model.
[00:10:36] Dan Duling: Not, not saying that, that you know, old models aren't good, but the, the benefit from the new, like each model is basically, you know, I don't know, a 3x factor, I would say.
[00:10:51] Stephanie McCarthy: Yeah, yeah.
[00:10:53] Dan Duling: So, you know, I don't know what model they're using.
[00:10:55] Stephanie McCarthy: I can find out though I'm hopeful that it would have updated since we
[00:10:58] Jack Hall: also it looks like as of the post from five days ago, they have an early access for North America for an MCP server which would then let it connect into Claude.
[00:11:14] Stephanie McCarthy: Oh, love that.
[00:11:15] Jack Hall: We would just have to. I'm sure there's.
[00:11:17] Jack Hall: You have to register for it and I'm sure since it's early access, there's all sorts of probably issues where it's not amazing at everything yet, but.
[00:11:27] Stephanie McCarthy: Okay, well, it's worth testing for sure because all of that update data in the system again will support with sort of the performance reviews and like being able to kind of summarize and bubble it up.
[00:11:37] Stephanie McCarthy: I know cloud can do that as well, but from sort of the organizational standpoint for us to have sight lines into every person across the organization, you know, we feel like this is going to be the best platform for us to be able to have that.
[00:11:49] Stephanie McCarthy: That visibility.
[00:11:52] Jack Hall: Yeah.
[00:11:52] Stephanie McCarthy: Okay, Dan, so you're telling me that we want to make sure that there's a little bit more visibility across teams and that if it connects to Claude, you're able to sort of summarize.
[00:12:04] Stephanie McCarthy: But the cloud piece sounds like it will fix that visibility piece is that, am I understanding that correctly it could pull from Lattice and post it into your Slack channels and then you can leverage Claude kind of the same way?
[00:12:13] Dan Duling: Well, it depends, right?
[00:12:15] Dan Duling: You know, my guess would be that Lattice will restrict the information based upon what you as an individual can't see.
[00:12:24] Dan Duling: So, you know, again, like that one, if I can't see, let's call, you know, let me look at the other channel real quick.
[00:12:35] Dan Duling: If I'm looking at like our commerce media weekly signal, right?
[00:12:40] Dan Duling: If I can't see what Tim Lukens is doing or Katie Mcclanahan or you know, Jessica, you know, other, you know, execration, you know, individuals, it's, it's going to be moot because I won't know exactly what they're kind of focusing on on a week over week basis.
[00:12:59] Dan Duling: Does that make sense?
[00:13:00] Stephanie McCarthy: Like their updates then whenever they put into Lattice with the update feature, you need to have visibility into that.
[00:13:06] Dan Duling: Correct.
[00:13:07] Stephanie McCarthy: You can solve for that.
[00:13:07] Stephanie McCarthy: So Lattice does offer the opportunity to create custom visibility for the update features specifically.
[00:13:13] Stephanie McCarthy: So Dan, I could give you access sort of to the visit the update visibility for your stakeholders.
[00:13:19] Stephanie McCarthy: But same with Jack and Trevor, like if you guys have cross team functional stakeholders that you want to be able to have visibility into their updates, we could create special, like we could create permissions that will allow you to be able to view cross functionally.
[00:13:33] Stephanie McCarthy: So there is that capability.
[00:13:38] Dan Duling: If we can get that set up, I'm happy to start Testing it out and seeing, you know, what kind of adoption we can get with like, you know, with that team.
[00:13:48] Dan Duling: I'm happy to have that.
[00:13:49] Dan Duling: You know, also pull Patrick in on that one just because he's also in that particular channel.
[00:13:53] Dan Duling: So he might have some other insights. But yeah, if we can do that and then if we can even for the.
[00:14:01] Dan Duling: The tech updates, you know, if I could increase that transparency to other teams, I don't have a.
[00:14:07] Dan Duling: As long as the ease of use is. Is there, I don't have a problem with that starting to move.
[00:14:16] Trevor Anderson: Does Lattice have a similar feature where you can post updates within Slack like how we do with Geekbot?
[00:14:22] Trevor Anderson: Right now we post our updates within Slack through Geekbot. So Lattice would be a similar.
[00:14:30] Trevor Anderson: You don't have to go into the application to post updates.
[00:14:33] Trevor Anderson: You can do it through the Slack application.
[00:14:39] Dan Duling: No, you basically what happens is it instead of doing it directly through the Slack app, it will prompt you to take you to the website.
[00:14:53] Stephanie McCarthy: I can show you. So if I'm in.
[00:14:57] Dan Duling: If you're trying to from your phone or something or any other device, it takes you to another web page which you'll have to just make sure that you're signed in through Okta.
[00:15:09] Trevor Anderson: Okay.
[00:15:10] Dan Duling: Yeah.
[00:15:10] Jack Hall: But yeah, if you're taking features to request to Lattice.
[00:15:13] Jack Hall: Yeah, allowing all of the updates to happen directly in to like prompt somebody to do something in Slack and let them use a Slack bot conversation would be awesome.
[00:15:23] Stephanie McCarthy: Yes.
[00:15:23] Jack Hall: Because I think it's just reduces the friction a little bit and gets actually doing 1000%.
[00:15:29] Stephanie McCarthy: I know for the one on one feature you can add one on one bullet points right.
[00:15:33] Stephanie McCarthy: In Slack you don't have to go into Lattice to do it.
[00:15:35] Stephanie McCarthy: But updates are not there yet, so I think I can definitely pass it along and they are very good about taking feedback from clients, so I'll certainly pass that along.
[00:15:46] Stephanie McCarthy: Okay, what I'd love to do is then Dan, let's test with that group that we were sort of initially talking about your, your commerce media lead leadership.
[00:15:56] Stephanie McCarthy: Just in my kind of. I was having, I think I was chatting with you guys.
[00:16:00] Stephanie McCarthy: Maybe not you Dan, but conversations around the Lattice usage people were like, well I don't want to have to do it in Geekbot and in Lattice, so I would kind of love to.
[00:16:10] Stephanie McCarthy: Which makes sense, right? No one wants to do things twice.
[00:16:13] Stephanie McCarthy: So I'd love to, you know, roll out a test to make sure that it's able to work and function the way that we want it to and gain their bias that they're not Doing, too.
[00:16:20] Stephanie McCarthy: So I just want to make sure you're okay with that.
[00:16:23] Dan Duling: Yeah. You know, and just to show you kind of how, like, the Geekbot, you know, update works, right?
[00:16:29] Dan Duling: You know, very streamlined.
[00:16:30] Dan Duling: You know, you select your answer, it tells you basically what you had as your previous update, and then you post.
[00:16:38] Dan Duling: It prompts you for the question, you know, what are your outcomes?
[00:16:42] Dan Duling: And then it just shows what you did previously. And then you can go ahead and post your update there.
[00:16:48] Dan Duling: And then we always have the question on if there are blockers or anything that we need to escalate through the team to get any kind of updates.
[00:16:57] Dan Duling: So you just do it right here within Slack, very seamless.
[00:17:01] Dan Duling: And then that posts directly to our commerce media signal without any friction, so everyone within that channel, you know, can actually see.
[00:17:14] Dan Duling: So, like, that's the. The type of transparency.
[00:17:17] Dan Duling: If we can get the same with Lattice, so we're not having to jump through multiple applications, that's, you know, that would be a huge update if they could expand their Lattice app.
[00:17:31] Stephanie McCarthy: So, quick question for you. That list that you compiled was just for you and your team.
[00:17:35] Stephanie McCarthy: And then each person that you mentioned, like Katie and Patrick and Jess, are all doing their own lists that are getting transferred over.
[00:17:41] Stephanie McCarthy: Okay. Okay.
[00:17:42] Dan Duling: Yeah.
[00:17:42] Stephanie McCarthy: Okay.
[00:17:43] Dan Duling: So, yeah, for mine, it's going to be the compile.
[00:17:45] Dan Duling: You know, it's, you know, compiled from eight different people.
[00:17:48] Dan Duling: That's what my list, you know, basically, you know, gets grouped into.
[00:17:53] Dan Duling: You know, Katie would have, you know, her team based on their initiatives.
[00:17:57] Dan Duling: You know, Tim, you know, PJ from sales, etc. All have kind of their own.
[00:18:03] Stephanie McCarthy: Got it. Okay, let me take this away and see what I can figure out in terms of that.
[00:18:13] Stephanie McCarthy: Whatever we put into Lattice being able to post to Slack, hopefully with that MCP connector, we can make that happen.
[00:18:19] Stephanie McCarthy: And I will report back.
[00:18:21] Stephanie McCarthy: In the meantime, I would like to roll this out for the rest of the or for, like, the finance team, you know, the teams that are not sort of already doing this update feature.
[00:18:32] Stephanie McCarthy: So just as a heads up, I might be communicating that, and we can have a separate message to your group that's already using Geekbot to be like, hey, we're working on transitioning this in more seamless ways, so wait until we kind of get that figured out before starting it.
[00:18:45] Stephanie McCarthy: I just don't want to hold up the rest of the Org while we figure this out.
[00:18:48] Stephanie McCarthy: Does anyone have any issues with that little mixed messaging for your team? Okay.
[00:18:54] Jack Hall: No, like I said, I'm using it anyhow. They'll be like, okay, Cool.
[00:18:57] Jack Hall: Daily stuff, go into Geekbot, weekly stuff, go into Lattice and Perfect.
[00:19:02] Stephanie McCarthy: Okay. And love that you're using a jack.
[00:19:04] Stephanie McCarthy: Love that it's helpful for you and that it's, you know, a meaningful communication tool with your team.
[00:19:09] Stephanie McCarthy: Especially, I think, on the tech side, when maybe you guys are so busy and like, heads down, work is priority.
[00:19:13] Stephanie McCarthy: So love that there's an asynchronous way for you to kind of check in and get that information.
[00:19:19] Stephanie McCarthy: Any other questions, concerns, or anything else we want to talk about regarding this?
[00:19:21] Stephanie McCarthy: As I kind of think through what this rollout looks like.
[00:19:24] Trevor Anderson: Yeah. You're saying that it doesn't exist currently and that you're going to enable something.
[00:19:29] Trevor Anderson: I do see an updates tab here that it already exists on Lattice.
[00:19:34] Stephanie McCarthy: Yes.
[00:19:34] Trevor Anderson: So I was wondering what. What's going to change? What's going to be different?
[00:19:38] Stephanie McCarthy: Good question. So it does exist.
[00:19:40] Stephanie McCarthy: What doesn't exist is sort of the explicit ask to all employees to start leveraging it and using it.
[00:19:47] Stephanie McCarthy: So kind of anyone who's fiddled around with the system or has, you know, kind of figured out what the functionality of it is and has been using it, has kind of done so on their own and just based out of the conversations we had around what Lattice is capable of.
[00:19:59] Stephanie McCarthy: But we're kind of going to say, hey, this is a new feature.
[00:20:01] Stephanie McCarthy: We're going to expect everyone to kind of tuck in and start providing this especially for those AI achievements so that we can have visibility worldwide.
[00:20:11] Stephanie McCarthy: So that's kind of the difference. But functionality? No, nothing's new.
[00:20:14] Dan Duling: Yeah.
[00:20:15] Jack Hall: And Trevor, if you go to that updates page, there's a settings button and that lets you customized frequency, time of day, and also the questions.
[00:20:25] Jack Hall: So if you want different specific questions, you can tune from the default company one.
[00:20:32] Stephanie McCarthy: Yeah.
[00:20:33] Trevor Anderson: Okay. And how does this work like does on. On our. On the team side of things?
[00:20:40] Trevor Anderson: Because I just see team updates and it's blank for me because my team isn't posting updates there, but on their side of things.
[00:20:48] Stephanie McCarthy: Does it show essentially the reminders like in lot? It's right in Slack and via email.
[00:20:55] Stephanie McCarthy: I do believe it's toggled on for your team. So you can.
[00:20:59] Trevor Anderson: So if they go into my updates and add these questions, I will see them on team updates, correct?
[00:21:04] Stephanie McCarthy: Yes.
[00:21:05] Trevor Anderson: Okay.
[00:21:06] Dan Duling: Yeah, great.
[00:21:07] Jack Hall: That's like how if you fill out that update, Dan sees your update.
[00:21:10] Stephanie McCarthy: Correct. It goes up to Dan and Dan sees it.
[00:21:12] Trevor Anderson: Oh, okay, got it. I thought it was just like a personal space. Okay.
[00:21:18] Jack Hall: No, definitely upward visibility.
[00:21:20] Trevor Anderson: Okay, great.
[00:21:21] Stephanie McCarthy: Upward visibility.
[00:21:22] Stephanie McCarthy: And then as I mentioned, we can configure permissioning so that there is some cross functional visibility.
[00:21:29] Stephanie McCarthy: So heads up for that too. Once we kind of again, we'll, we'll figure this out with MCP stuff.
[00:21:34] Stephanie McCarthy: But yeah, what goes in there.
[00:21:36] Trevor Anderson: So that's great. Thank you.
[00:21:38] Stephanie McCarthy: Yeah, of course. Cool. Okay. Well, guys, thank you so much for your time today. I really appreciate it.
[00:21:44] Stephanie McCarthy: Always get to partner with you guys and have a little bit of a chat.
[00:21:46] Stephanie McCarthy: If anything else pops up, if you're using it or you're like, oh, wait a minute, I thought of this, or wait, this is a limitation, please share with me as I'm exploring this other kind of connecting piece and trying to move this forward.
[00:21:58] Stephanie McCarthy: Thank you. Have a good day, guys. We'll chat with you soon.
[00:22:01] Trevor Anderson: Thank you very much.
[00:22:02] Stephanie McCarthy: Thank you. Take care.
[00:22:03] Jack Hall: Bye.
