---
title: "Venkatesh x Trevor Sync"
date: 2026-06-29
time: "10:30-11:00"
participants: [Trevor Anderson]
fellow_url: https://fluentco.fellow.app/meetings/040000008200E00074C5B7101A82E00807EA061D9BBF66705402DD01000000000000000010000000D99FEF86B50E1843B4BCD270C2E6D1FE/
captured_at: 2026-06-30T12:02:44Z
---

## AI Notes

Venkatesh and Trevor held their weekly sync to discuss recent data pipeline fixes and upcoming work. The primary focus was on resolving two critical issues: a **date of birth data typing problem** that required backfilling 80 million records with a **99.5% success rate**, and a **duplicate records issue in Gold Customer V1** caused by improper deduplication logic for inactive profiles. Venkatesh successfully optimized the FIG data sync performance, achieving **80 million records processed in 20 minutes** using increased batch sizes and partitioning.

The team also reviewed ongoing work including the ad group conversion migration to UCM (which needs rework to handle optimized conversion filters), NetSuite API access requests that are pending with IT, and upcoming release planning with **Release 31 scheduled for early July**. Performance monitoring showed RDS utilization at only **25% during peak sync**, suggesting headroom for further optimization by increasing partition counts from 64 to 128.

### Data Quality Issues

- Lead Manager pushed an update that caused date of birth fields to be typed as strings instead of longs, resulting in null values in the data pipeline.
- Venkatesh implemented a fix and completed the backfill, achieving approximately **99.5% fill rate** for date of birth data.
- The backfilled date of birth data exists in Databricks tables but needs to be resynced to RDS tables alongside the **monthly Experian customer load** to propagate updates to downstream CDP profiles.
- A duplicate records issue was discovered in Gold Customer V1 for inactive customers during Friday's scheduled job run, preventing the planned data sync.
- The root cause was **improper dedupe logic** for inactive profiles where Experian originally sent separate rows (one with only SHA256, one with only MD5) that later got enriched by FIG with complete hashes.
- The fix involved adding drop duplicates logic based on source ID, email MD5, and SHA256 fields to the inactive profile processing code.

### Identity Resolution Logic

- When FIG enriches profiles with both MD5 and SHA256 hashes for the same email, the system **prioritizes SHA256** and marks the MD5-only record as inactive to avoid duplicate profiles.
- Link fluent IDs are computed based on completeness score—profiles with both MD5 and SHA256 populated get priority when linking multiple emails for the same person ID (PID).
- Inactive profiles are assigned their own separate link fluent ID rather than being linked to active profiles, even when they belong to the same person, to maintain data integrity.
- The CDP (also called Identity Graph or IG) merges data from **three sources in priority order**: Experian first, then FIG, then Minion(?), with Gold Customer V1 showing which sources enriched each profile.
- The fix addresses duplicate records in the Gold Customer V1 table caused by primary key (Fluent ID) issues and **does not affect existing FIG or G tables**.

### FIG Sync Performance

- Venkatesh manually triggered the FIG load MySQL job on Friday, which syncs FIG data into the RDS instance server for downstream processes (purpose uncertain but required).
- By increasing rows per upset to **5,000** and using **128 partitions** (default), the sync completed **80 million records in approximately 20 minutes** (15 minutes excluding spin-up time).
- RDS utilization peaked at only **25%** during the Friday sync at 3pm EST, suggesting the infrastructure handled the load well without indexing on the FIG MySQL tables.
- Current IG sync configuration uses 64 partitions and 5,000 records per batch. The team discussed increasing partitions to **128** to improve parallelization and reduce processing time.
- Increasing partitions will add more load per Spark worker but memory appeared healthy in previous runs, making it feasible to test higher partition counts while monitoring worker balance.
- The monthly full load for IG is scheduled for June 29 or 30, which will sync the underlying delta tables from Gold Customer V1 to RDS. The **80 million FIG records** will be included in this sync.

### Ad Group Conversion Migration

- Venkatesh got sidetracked from migrating the ad group conversion job to UCM due to the Friday FIG issue, though dev work is mostly complete.
- Validation revealed a **discrepancy between prod data and new computations** because current ad group conversions use 'optimized conversions' filters (type name = optimized conversion) that UCM doesn't have by default.
- Rework is needed to filter out optimized conversions in UCM to match existing prod logic, though the team needs to better understand what constitutes an 'optimized conversion'.

### Release Planning

- **Release 31 is planned for Monday or Tuesday of next week** (early July 2026), targeting end of the current sprint which closes Friday, June 30.
- Release 30 was deployed on **June 24, 2026** and Trevor updated the release status and documentation, thanking Venkatesh for adding details to Lattice that helped with updates.
- Gold Customer V1 must be fixed before syncing to RDS because the IG API schema tables (Gold Customer, Gold Device, Gold Email, Gold Phone) are loaded directly from it as delta tables.

### Other Updates

- Trevor's FTSO request for NetSuite access remains pending, with concerns that tickets in backlog may not progress without active follow-up.
- The NetSuite catalog creation request for DevOps is currently in backlog with uncertain pickup timeline, requiring follow-up from Trevor.
- The API access request involves IP security requirements and has been sent to IT, with IT mentioning they need to test in their sandbox environment before granting access.

### Action Items

- [ ] Venkatesh Mannam to verify whether inactive profiles are being remapped to a different link fluent ID and report findings to Trevor.
- [ ] Follow up on the NetSuite catalog creation request with the DevOps team.

## Transcript

[00:00:00] Trevor Anderson: Sa.
[00:00:49] Trevor Anderson: All right.
[00:00:52] Venkatesh Mannam: Yeah.
[00:00:53] Trevor Anderson: How you doing?
[00:00:55] Venkatesh Mannam: So far so good.
[00:00:56] Trevor Anderson: Yeah.
[00:00:58] Venkatesh Mannam: It's been a long Friday for me.
[00:01:01] Trevor Anderson: I know. I'm sorry about that.
[00:01:04] Venkatesh Mannam: I know. No worries.
[00:01:05] Trevor Anderson: No worries. Yeah.
[00:01:09] Trevor Anderson: I was actually having a little bit of trouble understanding, like, the full picture of that.
[00:01:14] Trevor Anderson: Just trying to dig into it some, but I was. I was a little lost.
[00:01:23] Trevor Anderson: So I know that there was that one thing with. It was with date of birth, right.
[00:01:27] Trevor Anderson: Like, it wasn't being populated.
[00:01:31] Trevor Anderson: It was a null because lead manager pushed an update that caused it to be long or to cause it to be a string rather.
[00:01:41] Trevor Anderson: And we were accepting as long. So it was, you know, type typed as null because of that.
[00:01:47] Trevor Anderson: And then you put in the fix. But now you need to backfill data, which I think you did already, right?
[00:01:51] Venkatesh Mannam: Yeah, yeah, yeah, that's right.
[00:01:55] Trevor Anderson: So we have the 90. We have like the 100% fill rate pretty much.
[00:01:59] Venkatesh Mannam: Yeah.
[00:01:59] Trevor Anderson: That's like 9.5 or whatever it was.
[00:02:02] Venkatesh Mannam: Yeah, yeah, but.
[00:02:04] Trevor Anderson: But then you needed to.
[00:02:08] Trevor Anderson: In a related note, you needed to resync all the data or something like that to rds because that was only in the databricks tables.
[00:02:17] Trevor Anderson: Right? Is the backflow.
[00:02:19] Venkatesh Mannam: That's right.
[00:02:20] Trevor Anderson: Yeah. Okay. And then. So I think, See, I kind of get it.
[00:02:26] Venkatesh Mannam: You're okay.
[00:02:29] Trevor Anderson: But then. But then the part I don't understand quite is like, why it wasn't kicked off.
[00:02:37] Trevor Anderson: Like what you need to do to prep, get it prepared or.
[00:02:41] Venkatesh Mannam: Oh, yeah, I can guide you that. So, so those tables that we fix is in fixed schema.
[00:02:50] Trevor Anderson: Table server schema. Okay.
[00:02:53] Venkatesh Mannam: Yeah, big schema. There are few tables which kind of loads it to the downstream cdp. Right? Yeah.
[00:03:01] Trevor Anderson: Bronze tables.
[00:03:03] Venkatesh Mannam: No, in the fixed schema, we have a gold table.
[00:03:07] Trevor Anderson: I thought you said fixed.
[00:03:09] Venkatesh Mannam: Oh, sorry. Fix fig.
[00:03:11] Trevor Anderson: My bad.
[00:03:11] Venkatesh Mannam: Yeah, yeah, so think that same to. I mean, like fake customers. Right?
[00:03:19] Venkatesh Mannam: So right now we load the fake customers into CDP.
[00:03:23] Venkatesh Mannam: Since we fix the above process, that is the FIG records with the updated data for around almost 80 million records.
[00:03:34] Venkatesh Mannam: Right? So the same should sync. I mean, like.
[00:03:37] Venkatesh Mannam: So the same profiles getting updated database should sync the same to underlying RDS tables. Right?
[00:03:45] Venkatesh Mannam: I mean, in the CDP profiles. So what I thought is at the same shot, we load the experience customers.
[00:03:53] Venkatesh Mannam: I mean, like monthly sync. Right. That we do every first of every month.
[00:03:59] Venkatesh Mannam: So I plan to do both of them at same time. I hope you're following.
[00:04:07] Trevor Anderson: Yeah, yeah, yeah, yeah, yeah. I'm
[00:04:12] Venkatesh Mannam: so in the underlying, I mean, like, since those customers getting updated date of birth.
[00:04:16] Venkatesh Mannam: So underlying gold customer V1 and IG APA tables and RDs would reflect the same day, updated date of birth rate.
[00:04:25] Venkatesh Mannam: So I plan to do the Experian customers loading into CDP at the same time at one shots.
[00:04:32] Trevor Anderson: Yeah, yeah. Why couldn't we kick that off on Friday?
[00:04:36] Trevor Anderson: Because you told me we were going to do that on Friday. Right. But then.
[00:04:39] Trevor Anderson: Yeah, yeah, something happened. I gotta go back in our messages. What did that.
[00:04:42] Venkatesh Mannam: Oh yeah. So the problem is there seems to be a small issue.
[00:04:46] Trevor Anderson: So what happened here?
[00:04:48] Venkatesh Mannam: Yeah, in the Gold Customer v1 for inactive customers, there seems to be a small issue which kind of caused me duplicates.
[00:04:57] Venkatesh Mannam: And when I try to sync, I mean like when I try to do that issue kind of popped up at the on the Friday's job run.
[00:05:06] Venkatesh Mannam: So not sure how it ended up those duplicates.
[00:05:10] Venkatesh Mannam: So when I kind of deep dived into the code, there seems to be a small dedupe logic issue which kind of could be present.
[00:05:19] Venkatesh Mannam: I mean like for inactive customers. We never thought this issue is going to come.
[00:05:24] Venkatesh Mannam: So what happened is at the first place experience sent us two rows, one with only SHA, one with only MD5 row and we kind of create fluent ID based on the MD5 and SHA.
[00:05:39] Venkatesh Mannam: And right now we create two fluent IDs.
[00:05:43] Venkatesh Mannam: And at later phase those emails got dropped from the Experian new monthly loads.
[00:05:50] Trevor Anderson: Why? Oh, they just got removed.
[00:05:52] Venkatesh Mannam: Yeah, from the feed.
[00:05:54] Trevor Anderson: Okay.
[00:05:54] Venkatesh Mannam: Yeah, they can get deleted. Right. So those, those records in middle.
[00:06:00] Venkatesh Mannam: I mean like in earlier months we got enriched using the fake profile data. Okay.
[00:06:07] Venkatesh Mannam: At some point Experience in the previous months also sent a record with full shot of 56 and full MD5 record also.
[00:06:17] Venkatesh Mannam: Okay, so in middle of that process, one of the record got inactive and other record is still active.
[00:06:26] Venkatesh Mannam: And with the latest load they kind of removed both the records from Experience data.
[00:06:33] Venkatesh Mannam: So somehow in the inactive profiles, we never had the proper dedupe logic to kind of filter out those records.
[00:06:44] Venkatesh Mannam: Otherwise the current process costs 10, 10 duplicate records for those inactive profiles.
[00:06:51] Trevor Anderson: Wouldn't they. Okay, wouldn't they all be unique if they.
[00:06:55] Trevor Anderson: The way you described it was experience sends three rows.
[00:07:01] Trevor Anderson: One row has MD5 and Shaw, one row just has MD5, one row just has Shaw.
[00:07:07] Trevor Anderson: And then in and subsequent files that Experian sends, those records do not exist.
[00:07:16] Trevor Anderson: So by that logic you guys are marking those records as inactive.
[00:07:21] Venkatesh Mannam: Yeah, that's right.
[00:07:22] Trevor Anderson: But the way you described it, it seems like they would all be unique because of the characteristics of that row.
[00:07:32] Venkatesh Mannam: Yeah, that's right.
[00:07:33] Venkatesh Mannam: So what happened is in the inactive profiles, I mean like for the same luid Right.
[00:07:39] Venkatesh Mannam: I mean, like for the same pid, Sorry, the person ID for the link, Fluent id. So in the.
[00:07:46] Venkatesh Mannam: In the inactive profile checks, we kind of group them again at the PID to mark them inactive, if there are any such cases.
[00:07:57] Venkatesh Mannam: And somehow the internal logic kind of created those duplicates.
[00:08:03] Trevor Anderson: What do you mean by duplicates here? Can you give me an example so I can picture?
[00:08:07] Venkatesh Mannam: Yeah, yeah, I can. Yeah.
[00:08:14] Trevor Anderson: You say you want to mark Trevor as inactive.
[00:08:17] Trevor Anderson: I have three records in your graph in your database, right? Three rows.
[00:08:23] Venkatesh Mannam: Yeah.
[00:08:24] Trevor Anderson: You want to mark each of those three rows as inactive. Right. That's the intended.
[00:08:31] Venkatesh Mannam: I was doing some query at the background.
[00:08:35] Trevor Anderson: You want to mark me as an active. I mean your database. Right.
[00:08:38] Venkatesh Mannam: Yeah, yeah.
[00:08:40] Trevor Anderson: I'm Trevor. My PID is Trevor. Right. I have three rows though, from Experian.
[00:08:47] Trevor Anderson: They all have different primary keys or whatever, right?
[00:08:53] Venkatesh Mannam: Yeah.
[00:08:55] Trevor Anderson: What is the intentional thing you want to do when I don't show up in Experian anymore?
[00:09:00] Trevor Anderson: You want to mark all three rows as inactive?
[00:09:03] Venkatesh Mannam: No. Whichever email that Experian is not sending, we're going to kind of mark your rows as inactive.
[00:09:11] Trevor Anderson: Based off email.
[00:09:13] Venkatesh Mannam: Yeah, based off email.
[00:09:14] Trevor Anderson: So if I have a Gmail and a Hotmail, but Experian is still sending the hotmail, I just want to mark the Gmail as inactive.
[00:09:22] Venkatesh Mannam: Correct? Correct. Yes.
[00:09:25] Trevor Anderson: Okay. Because they're still selling or sending some. Something for me. Okay.
[00:09:30] Venkatesh Mannam: Yeah, that's right.
[00:09:32] Trevor Anderson: And so what's. Yeah, yeah.
[00:09:34] Trevor Anderson: So I guess the question is, I understand that that's intended scenario, but I'm. I'm.
[00:09:40] Trevor Anderson: When you say duplicate logic, I'm getting lost, so.
[00:09:44] Venkatesh Mannam: Yeah, I can explain that. So internally, we never revisited the inactive logic.
[00:09:52] Venkatesh Mannam: I mean, like, that's a huge code that I can say. Let me share my screen for a quick sec.
[00:10:03] Venkatesh Mannam: Screen share option. Let me know if you can see my screen. Yeah, let me pin this column.
[00:10:20] Trevor Anderson: Oh, you can pin columns. That's so nice.
[00:10:22] Venkatesh Mannam: Yeah.
[00:10:24] Trevor Anderson: Thanks for learning a new trick. This is only in the SQL editor.
[00:10:31] Venkatesh Mannam: Yeah, in the SQL editor. Right.
[00:10:32] Trevor Anderson: Oh, I never use this. Okay.
[00:10:34] Venkatesh Mannam: That's probably in the notebooks also. We can do that.
[00:10:36] Trevor Anderson: Oh, you could do it in notebooks.
[00:10:37] Venkatesh Mannam: Yeah, yeah.
[00:10:39] Venkatesh Mannam: So what happened is internally in the inactive logic, it should dedupe on the fluent ID basis.
[00:10:48] Venkatesh Mannam: But for some reason right now, the dedupe logic went on with the source id, email SHA and email MD file.
[00:10:59] Venkatesh Mannam: So it kind of cost me two rows with the same values. All. All of them have same records.
[00:11:06] Trevor Anderson: Even the Fluent id?
[00:11:08] Venkatesh Mannam: Yeah, even the Fluent id because of the grouping based on the source ID email and the shop.
[00:11:14] Venkatesh Mannam: I am defined the shop. So how did I.
[00:11:21] Trevor Anderson: Wait, hold on.
[00:11:22] Venkatesh Mannam: Sorry.
[00:11:22] Trevor Anderson: Yeah, this is in gold customer view one.
[00:11:26] Venkatesh Mannam: Yeah.
[00:11:29] Trevor Anderson: Records are loaded into this from experience, right?
[00:11:33] Venkatesh Mannam: Yeah, yeah.
[00:11:35] Trevor Anderson: Experian doesn't send us two rows.
[00:11:39] Venkatesh Mannam: That's right.
[00:11:39] Trevor Anderson: Okay, so they send us one. What is causing it to be loaded as two here? So it already existed.
[00:11:48] Trevor Anderson: I'm assuming
[00:11:52] Venkatesh Mannam: way back travel.
[00:11:53] Venkatesh Mannam: Like on April 4th we got these records and somehow in June we updated these records and they were enriched using the.
[00:12:05] Venkatesh Mannam: Some. Some of them records got enriched using the fake profiles also.
[00:12:10] Venkatesh Mannam: So they should have only one record. Right. For the same email MD5 shot of 56.
[00:12:16] Trevor Anderson: Yes.
[00:12:17] Venkatesh Mannam: And all of them records are inactive. So somehow in the silver I don't see any duplicates.
[00:12:23] Venkatesh Mannam: Only in the gold. Only only for the inactive profiles. It kind of caused me these duplicates.
[00:12:30] Trevor Anderson: Something hap. Okay, so something happened that loaded duplicate data for some reason.
[00:12:37] Venkatesh Mannam: That's right, yeah. No data issue upstream. No data issue with the experience.
[00:12:43] Venkatesh Mannam: Somehow the process created these duplicates and I added a small fix.
[00:12:49] Venkatesh Mannam: And right now there are no duplicates if I run this.
[00:12:58] Venkatesh Mannam: So the intended process should be something like this. Right. So one fluent ID for one email.
[00:13:06] Venkatesh Mannam: Yeah, yeah. So how I got those records is when I try to.
[00:13:13] Trevor Anderson: Do you know what. Do you know why duplicates were getting loaded into gold?
[00:13:24] Venkatesh Mannam: Yeah.
[00:13:25] Trevor Anderson: What code was doing that
[00:13:29] Venkatesh Mannam: here in the Gold Customer? It's a massive. Where is this inactive?
[00:13:53] Venkatesh Mannam: Yeah, Yeah.
[00:13:59] Venkatesh Mannam: In the inactive silver customer data frame whose emails are inactive or the whose profiles are inactive.
[00:14:09] Venkatesh Mannam: So here it kind of caused me duplicate servers.
[00:14:13] Trevor Anderson: So join.
[00:14:15] Venkatesh Mannam: Yeah, this kind of join caused me duplicates.
[00:14:18] Venkatesh Mannam: So when I added this extra check to drop duplicates based on the source ID to email MD5 and SHA256, it kind of fixed that issue.
[00:14:29] Trevor Anderson: It was joining to where existing data frame. What is that Gold customer reference?
[00:14:39] Trevor Anderson: That's based off the existing table.
[00:14:41] Venkatesh Mannam: Yeah. Existing table, that's right.
[00:14:43] Trevor Anderson: Why would it wouldn't there only be a 1 record in there?
[00:14:48] Trevor Anderson: Why like so if you do a join and there's two records, it means that. There's two records on the join.
[00:15:03] Trevor Anderson: It joins against two.
[00:15:06] Venkatesh Mannam: Yeah.
[00:15:07] Trevor Anderson: Why would there be two? There shouldn't be two though. That's what I'm confused about.
[00:15:11] Trevor Anderson: Like even if you do join, then it should be unique on those. Yeah.
[00:15:18] Venkatesh Mannam: Earlier, these records, right? The. The. The short 256 row and MD5 row. Right.
[00:15:26] Venkatesh Mannam: So they are present as two different rows with same source ID too. Because PID is same for you.
[00:15:37] Venkatesh Mannam: Somehow they Told like you got this profile with two different rows as one row with chart 256 and one row with MD5 and the same profiles got enriched with the FIC profile data because.
[00:15:53] Venkatesh Mannam: Yeah.
[00:15:53] Trevor Anderson: Did they get an email? Did they get a MD5 and Shaw added to the columns?
[00:16:00] Venkatesh Mannam: Yeah, that's right. Yeah.
[00:16:01] Trevor Anderson: Yeah. Oh, okay. That's why. Yeah, that's why.
[00:16:04] Venkatesh Mannam: Yeah, that's right.
[00:16:06] Trevor Anderson: Well, interesting, interesting, interesting, interesting.
[00:16:14] Venkatesh Mannam: So it kind of took me like four or five hours to fix this issue. Like.
[00:16:18] Trevor Anderson: Well, that's confusing. Yeah, that's also. Also. I don't know. Do you if you.
[00:16:26] Trevor Anderson: What, what are you enriching it against? Fig. Are you using email?
[00:16:34] Venkatesh Mannam: So well, first priorities.
[00:16:36] Trevor Anderson: This is from Experian. So you can go to FIG to enrich it. Yeah, it's one place, right?
[00:16:43] Venkatesh Mannam: And there is also millions.
[00:16:45] Venkatesh Mannam: So how we do a priority match here, Trevor is so suppose as I told like there are two rows, one row with SHA and one row with only MD5 row.
[00:16:54] Venkatesh Mannam: Right?
[00:16:55] Venkatesh Mannam: So suppose if FIK comes and says like this Trevor gmail.com MD5 and SHA256 belongs to same row like same person but experiences as a two different row records, right.
[00:17:09] Venkatesh Mannam: So we end up enriching the two profiles with these values that is two different rows with both Shoto 56 populated.
[00:17:21] Venkatesh Mannam: With both MD5 populated.
[00:17:23] Venkatesh Mannam: But the record that we created earlier with the MD5 value as a deterministic to fluent ID. Right.
[00:17:32] Venkatesh Mannam: So we mark that record as false or I mean like we mark that record as inactive profile and we take the SHA 256 as a priority record.
[00:17:46] Trevor Anderson: This is. Yeah, this logic is interesting.
[00:17:50] Trevor Anderson: Yeah, yeah, so I, I hear what you're saying, but it's not driving well with me. That makes sense.
[00:18:01] Trevor Anderson: Just because like you have two profiles. How do you identify that they're the same person?
[00:18:09] Trevor Anderson: Is Experian telling you that like how do you. How do you build the linked fluent id?
[00:18:17] Venkatesh Mannam: Yeah. So suppose ignore this PDF it be travel.
[00:18:23] Venkatesh Mannam: So we never created the these fluent IDs experience centers profile. Something like this.
[00:18:30] Venkatesh Mannam: MD5 is equal to null short of 56 is equal to some value and the other record as MD5 is equal to value short 56s.
[00:18:42] Trevor Anderson: That's fine cd.
[00:18:45] Venkatesh Mannam: And now fit comes in. Now Fig comes and says turned off my series making from Speak.
[00:18:56] Venkatesh Mannam: Now we got records as MD5 is equal to CD. SHA 256 is equal to ABC. Yeah, till now you got this right?
[00:19:11] Venkatesh Mannam: FIG now comes and says like MD5 is equal to CD short 56 is equal to ABC.
[00:19:17] Venkatesh Mannam: So that comes as a source of truth here for us because they believe that they belongs to same customer and we, we create this SHA 256 and MD5 based on the raw email Trevor Yeah.
[00:19:32] Venkatesh Mannam: So here what we do is, since we already created two profiles for the same customer, so what we do is we end up doing this.
[00:19:43] Trevor Anderson: Okay.
[00:19:44] Venkatesh Mannam: And we mark this as active profile because we don't want to put two profiles right.
[00:19:52] Venkatesh Mannam: For the same inactive.
[00:19:53] Venkatesh Mannam: So this one we mark as inactive and this one we mark as active because of the short of six precedence.
[00:20:00] Trevor Anderson: Okay.
[00:20:01] Venkatesh Mannam: Yeah.
[00:20:02] Trevor Anderson: And so, and then you get both the same linked ID but they both have different primary keys.
[00:20:08] Venkatesh Mannam: Correct? Correct. Yeah. In.
[00:20:10] Trevor Anderson: Okay, so it's like a type two almost.
[00:20:13] Venkatesh Mannam: Yeah.
[00:20:14] Trevor Anderson: Where you have like history but you don't really have.
[00:20:19] Venkatesh Mannam: Yeah, that's right.
[00:20:20] Trevor Anderson: Yeah, but you're. Yeah.
[00:20:22] Venkatesh Mannam: For the link fluent ID for these inactive profiles, what we do is we mark their individual record as a inactive link fluent ID also because they, they belongs to inactive and we don't want to recompute their link fluent ID again based on some PID or the person ID or the living unit id.
[00:20:41] Venkatesh Mannam: Right. So the link fluent ID would capture as its own.
[00:20:49] Venkatesh Mannam: So now parent and child belongs to same link fluent affluent id.
[00:20:54] Venkatesh Mannam: Here we compute based on the other factors like if for the if for the same person, if they have any other IDs we use that.
[00:21:03] Venkatesh Mannam: Otherwise we recompute it once again.
[00:21:07] Trevor Anderson: Okay. Yeah, this doesn't describe like multiple different email situation. Right.
[00:21:12] Trevor Anderson: Though that's something else.
[00:21:16] Venkatesh Mannam: Sorry, come again.
[00:21:18] Trevor Anderson: This, this is assuming that the email is the same here though.
[00:21:22] Venkatesh Mannam: Yeah? Yes, that's right.
[00:21:24] Trevor Anderson: But like you still have situations where people have different emails but they're the same person.
[00:21:31] Venkatesh Mannam: Yes. So internally we recompute the link fluent ID based on the person ID. Right.
[00:21:36] Venkatesh Mannam: So suppose for the same person ID PID is equal to 1, 2, 3, 4. Suppose this person has 10 emails and.
[00:21:56] Venkatesh Mannam: Let's see, pid 1, 2, 3, 4 md 5 is equal to efg short of f2 6 is equal to byv. This one is inactive.
[00:22:13] Venkatesh Mannam: ID is equal to. Yeah, Think about something like this table.
[00:22:19] Venkatesh Mannam: So this person is two emails, one with CDE abc.
[00:22:24] Venkatesh Mannam: This one has a profile, this one has different profile and earlier we recomputed at the first place we never had the this complete hash rate we only got because of the fig enrichment.
[00:22:38] Trevor Anderson: Yes.
[00:22:41] Venkatesh Mannam: So this one null. Right, this one null.
[00:22:46] Venkatesh Mannam: So while computing the link fluent iterator, so the first factor that we consider for the same PID is if There are any completeness score for this different emails Completeness score in the sense which profile got the complete MD5 and shot 256 populated for those records.
[00:23:05] Venkatesh Mannam: So in this case only this record got the complete profile, right?
[00:23:09] Venkatesh Mannam: The MD5 record and the SHA256 record.
[00:23:12] Venkatesh Mannam: So for these we compute the link fluent ID as C because it is having the complete profile.
[00:23:22] Venkatesh Mannam: Right, The C Fluent id. So at the first place we compute link Fluent ID something like this to.
[00:23:32] Trevor Anderson: Okay. Oh, okay.
[00:23:37] Venkatesh Mannam: And in the later case once it got enriched cd. So it gets its own link front ID because it.
[00:23:51] Venkatesh Mannam: It was marked as inactive. Now it would be something like this.
[00:23:55] Trevor Anderson: The market Market is inactive.
[00:23:57] Venkatesh Mannam: Oh yeah, inactive sir. That's right.
[00:24:00] Trevor Anderson: Why does it get a different fluent
[00:24:01] Venkatesh Mannam: ID now it is inactive and we don't want to link the inactive profiles to a active link Fluent id
[00:24:13] Trevor Anderson: even though the same person. Do you have a different person id? Do you actually have that record?
[00:24:21] Venkatesh Mannam: These records would still stay in the gold customer V1 Trevor Yeah, that's right.
[00:24:25] Trevor Anderson: When you actually have a column that is like that inactive PID. No PID.
[00:24:31] Venkatesh Mannam: Oh, we call them as a source ID 2. Source ID 2 is internally linked as a. I'll show you. Yeah, okay.
[00:24:50] Venkatesh Mannam: Why it's not responding?
[00:24:55] Trevor Anderson: Weird Scorch now.
[00:25:01] Venkatesh Mannam: Yeah, this one we mark as a PID Person id. Okay.
[00:25:08] Trevor Anderson: What? What? How is it? That's a string. Okay. Yeah, looks like an integer though. Anyways. Okay, go back.
[00:25:28] Trevor Anderson: Okay. Okay, so now you have C of A and C. They're both the same linked Fluent id. They're both active.
[00:25:43] Venkatesh Mannam: Yeah.
[00:25:43] Trevor Anderson: Okay. I.
[00:25:45] Venkatesh Mannam: Okay, so we care about only these two profiles right now the active profiles,
[00:25:51] Trevor Anderson: same person, different emails, but they're both active.
[00:25:54] Trevor Anderson: And when it's like two of the same emails, then you mark the. Yeah, I see. Okay.
[00:26:02] Venkatesh Mannam: Yeah, yeah. Okay.
[00:26:09] Trevor Anderson: It's a little interesting that you changed the linked ID though to a different thing.
[00:26:13] Trevor Anderson: I would think that's like the parent, but the way you have it, it's like almost like they're a different person.
[00:26:20] Trevor Anderson: But I guess you're using source ID 2 as the person basically.
[00:26:25] Venkatesh Mannam: Yeah, yeah.
[00:26:27] Trevor Anderson: So that's like the true parent.
[00:26:36] Venkatesh Mannam: I'm just double checking on the inactive profile.
[00:26:40] Venkatesh Mannam: If I said correct or not,
[00:26:58] Venkatesh Mannam: I'll double check this one.
[00:26:59] Venkatesh Mannam: Trevor, if we are renaming, I mean like if we are remapping this one or not, I'll.
[00:27:04] Venkatesh Mannam: I'll double check the profile and then I'll let you know.
[00:27:07] Trevor Anderson: Okay. Because I almost feel like we wouldn't even. We could keep that SC because it Shows an inactive.
[00:27:16] Trevor Anderson: Like shouldn't that be our thing that tells us like don't, don't. You know, look at this.
[00:27:24] Trevor Anderson: This is the status of it. But anyways, yeah, you let me know.
[00:27:28] Venkatesh Mannam: But yeah, yeah, sure thing.
[00:27:30] Trevor Anderson: This makes sense. So this is for the most part resolved. That's what it seems like.
[00:27:35] Venkatesh Mannam: Yeah, yeah, that's resolved. So the underlying IG API tables are getting prepped.
[00:27:40] Trevor Anderson: And this isn't a problem in identity graph. This is only in figure
[00:27:47] Venkatesh Mannam: which one do that?
[00:27:49] Trevor Anderson: Oh, this is gold customer v1.
[00:27:52] Venkatesh Mannam: Yeah, that's gold customer v1 which
[00:27:57] Trevor Anderson: is enriched by a fake hand. Identity graph or. Yeah, is it Identity graph is pulse, right? Basically.
[00:28:06] Trevor Anderson: Well, it's everything. Never mind.
[00:28:08] Venkatesh Mannam: Yeah, it's everything. That's right.
[00:28:13] Trevor Anderson: Do you say in the gold customer v1 do you say it's enriched by IG? Like, do you.
[00:28:23] Trevor Anderson: When you say enriched like there's. We have the identity graph and we have the fig, right?
[00:28:30] Trevor Anderson: Both are like identity graphs, right?
[00:28:34] Venkatesh Mannam: Yeah. So in the cdp, I mean like we call that as a CDP central data platform. Right.
[00:28:41] Venkatesh Mannam: So here is the place where all the sources get merged or enriched or everything travel.
[00:28:48] Venkatesh Mannam: So our first source of truth is experience and then fig and then minion.
[00:28:54] Venkatesh Mannam: So all these three sources get merged.
[00:28:57] Trevor Anderson: Yeah, but isn't that. That's also known as I ig.
[00:29:01] Venkatesh Mannam: Yeah, ig. That's right. Yeah, we call that as IG or the cdp. Every. Both of them are same. Yeah.
[00:29:07] Trevor Anderson: Okay. And then in that.
[00:29:12] Trevor Anderson: Okay, but okay, Customer view one, it has this column called like enriched or sources or whatever.
[00:29:21] Trevor Anderson: Right?
[00:29:22] Venkatesh Mannam: Yeah, the source names. Yeah, that's all. Yeah.
[00:29:24] Trevor Anderson: So do you have in that column, do you have. You say fit. Okay.
[00:29:31] Trevor Anderson: You say fig, Experian Pulse basically in those. Right?
[00:29:34] Venkatesh Mannam: Yeah, yeah, something like this. So these profiles got enriched by Experian. We.
[00:29:43] Venkatesh Mannam: We somehow inferred the state province information using some process and it's.
[00:29:49] Venkatesh Mannam: It got enriched by FIG also.
[00:29:52] Trevor Anderson: But yeah, is that. Is that list ordered by when it was enriched or.
[00:30:04] Venkatesh Mannam: Yeah, it gets appended whenever a new source get enriched. I mean like added into this profile.
[00:30:10] Venkatesh Mannam: So for some reason if minions somehow added some information to this profile means it gets appended the minion also after this.
[00:30:19] Venkatesh Mannam: Yeah.
[00:30:25] Trevor Anderson: Okay.
[00:30:26] Venkatesh Mannam: Yeah.
[00:30:28] Trevor Anderson: Thank you for going through that with me. I know it was a lot, but really helps.
[00:30:34] Trevor Anderson: Really helps understand.
[00:30:36] Venkatesh Mannam: Yeah.
[00:30:39] Trevor Anderson: So you're basically fixing something that is happening in the gold customer V1.
[00:30:45] Venkatesh Mannam: Yeah, that's right. Yeah.
[00:30:46] Trevor Anderson: Yeah, I see. So that shouldn't actually that doesn't affect really what exists in FIG or G
[00:30:56] Venkatesh Mannam: tables yeah, it doesn't affect anything.
[00:31:02] Trevor Anderson: And then I guess going back to the sync operation. So.
[00:31:06] Venkatesh Mannam: Yeah,
[00:31:08] Trevor Anderson: because you're syncing, you're trying to sync the actual identity graph.
[00:31:13] Venkatesh Mannam: Yes.
[00:31:13] Trevor Anderson: But you're saying you want to fix the gold table first. How is that?
[00:31:18] Trevor Anderson: What's the dependency on those sync tables? Just looking back to the sync.
[00:31:25] Venkatesh Mannam: Yeah. The gold customer v1 is fixed treble with no duplicates.
[00:31:31] Venkatesh Mannam: Before we sync to rds, there is this IG API schema where we just store the tables as it is for just try dump to rds.
[00:31:41] Venkatesh Mannam: Right. So there is this IG API Gold Customer, Gold Device, Gold Email and Gold phone tables.
[00:31:48] Trevor Anderson: Right.
[00:31:49] Venkatesh Mannam: So those are just delta tables which gets straight sync to rds. Right.
[00:31:55] Venkatesh Mannam: So those tables are getting loaded right now.
[00:32:00] Trevor Anderson: And you couldn't do that because.
[00:32:04] Venkatesh Mannam: Because of. Yeah, because of the fluent id. I mean the primary key issue.
[00:32:11] Trevor Anderson: Because you were having more than one record in the gold customer v1. Okay.
[00:32:15] Venkatesh Mannam: Yeah.
[00:32:16] Trevor Anderson: And those. Those Gold email, Gold device, blah blah. Those are split out from Gold Customer V1 or.
[00:32:24] Venkatesh Mannam: Yeah, that's right.
[00:32:25] Trevor Anderson: Okay. So is Gold Customer V1 the actual depend like that's the dependent table. Like those rec.
[00:32:32] Trevor Anderson: Those delta tables come from that gold table.
[00:32:36] Venkatesh Mannam: That's right. Yeah. From Gold Customer. We segregate them into two different tables.
[00:32:41] Venkatesh Mannam: One just for sole purpose of Gold Customer and the other one is Gold Email.
[00:32:47] Venkatesh Mannam: So those two tables are just a segregate of the above Gold customer V1, I would say.
[00:32:57] Trevor Anderson: Okay. They don't come from Silver.
[00:33:00] Venkatesh Mannam: No, no. Just the Gold Customer Weaver.
[00:33:03] Trevor Anderson: I see. Okay, now. Now everything makes sense. Okay.
[00:33:06] Venkatesh Mannam: Yeah,
[00:33:09] Trevor Anderson: that's great. Well, good work there and then. That was a lot.
[00:33:14] Venkatesh Mannam: Yeah. That the FIG issue is fixed ever. So I would like to share that.
[00:33:29] Venkatesh Mannam: So on Friday I did some strange thing.
[00:33:32] Venkatesh Mannam: I mean like, I didn't mention to you what I did is so FIG load MYSQL is there. Right. I'm like.
[00:33:43] Venkatesh Mannam: I'm pretty sure you are not aware of this process. So where we load FIG data into.
[00:33:49] Venkatesh Mannam: Into the same RDS instance server.
[00:33:51] Venkatesh Mannam: I'm also not sure why we are loading this, but for some process is using these tables. Okay.
[00:33:58] Venkatesh Mannam: So I triggered this job run manually on. Yeah, this one. So the default number of.
[00:34:12] Venkatesh Mannam: I mean like you know I told you that There are almost 80 million records. Right?
[00:34:17] Trevor Anderson: Yeah.
[00:34:18] Venkatesh Mannam: In the fixed schema getting updated with the date of birth.
[00:34:21] Venkatesh Mannam: So the underlying table is enriched or like. Sorry. Is getting synced to fig MySQL tables.
[00:34:28] Venkatesh Mannam: I mean the same like IG APA tables. Right.
[00:34:32] Venkatesh Mannam: So I increased the Number of rows per upset to 5,000 for this.
[00:34:39] Venkatesh Mannam: And this default is part default partitions are 128 level in this.
[00:34:45] Trevor Anderson: Okay.
[00:34:46] Venkatesh Mannam: And 80 million records it. It's like synced almost in 20 minutes.
[00:34:56] Trevor Anderson: Okay.
[00:34:59] Venkatesh Mannam: I think we should somehow increase the partitions also in our scenario. Right.
[00:35:04] Venkatesh Mannam: If this makes sense with this sync getting completed in 22 minutes.
[00:35:09] Trevor Anderson: 80 million is not like that much.
[00:35:12] Venkatesh Mannam: Yeah, yeah.
[00:35:14] Trevor Anderson: I mean it's a good amount, but it's not it. Was this against the same serverless infrastructure?
[00:35:20] Venkatesh Mannam: Yeah, that's the idea.
[00:35:25] Trevor Anderson: Does this, the this table doesn't have indexing on it though, right? Or does it?
[00:35:36] Venkatesh Mannam: I never looked into these tables, but let's see. I mean like if you have time. Otherwise I can.
[00:35:43] Trevor Anderson: I have time.
[00:35:44] Venkatesh Mannam: Okay. My sequel,
[00:35:50] Trevor Anderson: it's either I figure this stuff out now or later.
[00:35:57] Venkatesh Mannam: Sorry to do that.
[00:35:58] Trevor Anderson: It's like this type of stuff, like this is like core data data products. So it's like.
[00:36:03] Trevor Anderson: Yeah, either I learn, you know, get more into this now or I figure it out later, you know.
[00:36:11] Trevor Anderson: So it's like needs to happen at some point.
[00:36:14] Trevor Anderson: So might as well take the time now to go dig into it if you have time.
[00:36:34] Venkatesh Mannam: Trying to see where is this RDS sync process.
[00:36:39] Trevor Anderson: Oh yeah, probably you can get it here. So those in MySQL folder, is that for the identity graph?
[00:36:55] Venkatesh Mannam: Yeah, I think these are all for identity graph.
[00:36:59] Trevor Anderson: Yeah, they are. Go to the job and see the central data ingestion main branch.
[00:37:21] Venkatesh Mannam: Load attribute. Mysql load attribute. Okay. Mysql.
[00:37:44] Venkatesh Mannam: Oh yes. Okay. And this is using my SQL connection.
[00:38:08] Trevor Anderson: Source table schema name, table name, what's the table name? The cold attribute.
[00:38:17] Trevor Anderson: That's the table that you're loading to.
[00:38:19] Venkatesh Mannam: Yeah, I mean like there are four different tables. Load attribute, load profile attribute, email.
[00:38:26] Trevor Anderson: This is one of them.
[00:38:27] Venkatesh Mannam: Yeah, this is another one I'm seeing
[00:38:31] Trevor Anderson: where the process execute SQL with params and that's probably an import. Yeah, Base methods. Okay.
[00:38:52] Venkatesh Mannam: I don't think we have any indexes for this. Yeah, that's right.
[00:39:01] Trevor Anderson: Why do we sync these to RDS again?
[00:39:06] Venkatesh Mannam: I think some downstream process is using this server. Some rewards customers.
[00:39:14] Venkatesh Mannam: I'm also not completely sure because like this will like implemented like way before two years ago.
[00:39:26] Venkatesh Mannam: I'm not sure who really is using these tables, but someone is using that customer, email, device, phone.
[00:39:44] Venkatesh Mannam: Yeah, I think we don't have indexes for those fig tables.
[00:39:54] Trevor Anderson: There's the actual load function. What's in here? This is part two.
[00:40:12] Trevor Anderson: Mysql connection execute with parameters MySQL SQL params
[00:40:20] Venkatesh Mannam: version tracking completed version.
[00:40:31] Trevor Anderson: What is it doing? It's just doing. Go up go to the function above here. This one. Execute.
[00:40:41] Trevor Anderson: It's just executing the SQL. That's it.
[00:40:43] Venkatesh Mannam: Yeah. Yeah.
[00:40:46] Trevor Anderson: Okay, so. And then that query that's getting passed here is just a insert statement.
[00:40:58] Trevor Anderson: Well, I guess you can't really tell for sure if there's indexes on it because it's not like rebuilding them or checking it or anything.
[00:41:05] Trevor Anderson: It could be on the DDL table. You wouldn't even know based off this code. But
[00:41:13] Venkatesh Mannam: I didn't see any changed.
[00:41:17] Trevor Anderson: I know, but if it was built like outside of this.
[00:41:25] Venkatesh Mannam: And maybe like. I think only do you have access to check the underlying table server in the.
[00:41:34] Venkatesh Mannam: I think only.
[00:41:35] Trevor Anderson: Yeah. I submitted FTSO a long time ago. But.
[00:41:45] Trevor Anderson: I realized though like with FDSOs, if I'm not like actively following up, they're like are they ever going to get done?
[00:41:53] Venkatesh Mannam: I think when I check the. NET suit also. Right. So it's in backlog. Not sure when they're gonna pick up
[00:41:59] Trevor Anderson: those tasks I guess. Well Dan, That one's like with it though so.
[00:42:11] Venkatesh Mannam: I mean the. NET suit catalog creation for the DevOps.
[00:42:17] Trevor Anderson: Oh, oh, the catalog. The actual catalog. Okay, I could probably follow up on that. But not the.
[00:42:24] Trevor Anderson: The access is something or the. The API access is something else. Right. That's with ip.
[00:42:31] Venkatesh Mannam: Yeah. Seems like security one then since said something to them. Seems like they replied.
[00:42:37] Venkatesh Mannam: They shall be replied to us.
[00:42:39] Trevor Anderson: Yeah, it says sent this request over to it. There needs to be security done first.
[00:42:47] Trevor Anderson: Okay, what's their sandbox environment? Who shall be it?
[00:42:56] Venkatesh Mannam: I'm not sure like what they're trying to say. Oh, I'll get.
[00:43:02] Trevor Anderson: Maybe they don't understand what we're trying to do.
[00:43:05] Venkatesh Mannam: Yeah, yeah.
[00:43:06] Trevor Anderson: Because why did they say we'll. We'll test in our sandbox.
[00:43:10] Venkatesh Mannam: What. Maybe they also have something.
[00:43:17] Trevor Anderson: Some database or something. Or something interesting. Yeah, I guess maybe they. They do have something.
[00:43:30] Venkatesh Mannam: Yeah.
[00:43:34] Trevor Anderson: Okay. I'll follow up on the catalog thing. Yeah, I wonder what's okay. But.
[00:43:42] Trevor Anderson: Okay, back to the processing time though.
[00:43:46] Venkatesh Mannam: Maybe.
[00:43:48] Trevor Anderson: I don't know like did. Did we. Are we able to. Are you going to do any other syncs?
[00:43:53] Trevor Anderson: Because maybe I could actually check the performance. When did that one run?
[00:44:02] Venkatesh Mannam: Let me give that timing.
[00:44:09] Trevor Anderson: Now it's on prod.
[00:44:11] Venkatesh Mannam: Yeah, on prod on Friday evening six estate. So it should be your three o', clock, right?
[00:44:26] Venkatesh Mannam: Yeah, near local.
[00:44:33] Trevor Anderson: I can't find where. Where is our. I thought it was in us. The east one.
[00:44:42] Trevor Anderson: I thought it was an east one, nothing showing up. Oh, this is Billing.
[00:44:46] Venkatesh Mannam: Oh, Nvidia. Yeah, yeah. Monitoring iops.
[00:45:08] Venkatesh Mannam: Utc local. From the sixth pH.
[00:45:22] Trevor Anderson: Oh, I see it jumped up to 24% utilization. This was on the 26th. On the 26th, which was Friday. Right.
[00:45:39] Trevor Anderson: 26 at time 3:00 clock my time is what you're saying. What's 15? Is that 3:00'? Clock? Yeah, that's 3.
[00:46:03] Trevor Anderson: I see a spike at 3. But, but it only has 25% utilization.
[00:46:13] Venkatesh Mannam: Okay, yeah, yeah, I can see 25. That's right.
[00:46:16] Trevor Anderson: So it, I mean it jumped up, but it wasn't like crazy.
[00:46:32] Trevor Anderson: It must be the indexing, I feel like that's causing you these big delays.
[00:46:58] Trevor Anderson: Yeah, I'm saying we all like with partitioning.
[00:47:06] Venkatesh Mannam: Yeah, partitioning the data.
[00:47:07] Trevor Anderson: Yeah. Because what do we have right now? Like 60. And we have, we have like 64 on the, on the IG stuff.
[00:47:15] Trevor Anderson: We have like 64 and 5,000.
[00:47:19] Venkatesh Mannam: Yeah, yeah, yeah.
[00:47:23] Trevor Anderson: Much, much more records. So. Like only I feel like. So what is happening? So Spark engine is.
[00:47:41] Trevor Anderson: We're saying create 64 or we're telling Spark to create 64 batch or partitions.
[00:47:53] Venkatesh Mannam: Yeah.
[00:47:54] Trevor Anderson: And so those partitions are split between the nodes of the Spark cluster and then each node, each 64.
[00:48:01] Trevor Anderson: Each of the 64 is sending 5,000 records at a time.
[00:48:07] Venkatesh Mannam: Yep.
[00:48:10] Trevor Anderson: And we're basically saying, okay, we want to increase the amount of workers that are.
[00:48:15] Trevor Anderson: Or the amount of, you know, requests are going in through.
[00:48:17] Trevor Anderson: We want, we're trying to force the throughput to.
[00:48:21] Venkatesh Mannam: Yeah, yeah,
[00:48:25] Trevor Anderson: parallel. We're trying to increase the parallelization of this. So.
[00:48:37] Trevor Anderson: But we always get to 100% on those, on those loads. Pretty much.
[00:48:42] Venkatesh Mannam: Yeah, yeah, almost like 70, 80 percentage.
[00:48:47] Trevor Anderson: But I think last time I was checking like this, the memory seemed pretty healthy.
[00:48:52] Trevor Anderson: So it almost seemed like we could, we could bump that up.
[00:48:56] Trevor Anderson: Yeah, we could, we could try 120A and on the next run.
[00:49:03] Venkatesh Mannam: Oh, the partitions.
[00:49:04] Trevor Anderson: Right, yeah.
[00:49:05] Venkatesh Mannam: Okay. Yeah.
[00:49:09] Trevor Anderson: Just gotta make sure our spark is healthy, that's all.
[00:49:13] Trevor Anderson: On the database side, the more, the more we increase the partitions, the more load it's going to have on the workers because each worker will have more partitions per worker.
[00:49:27] Trevor Anderson: So just going to make sure there's a balance there.
[00:49:31] Venkatesh Mannam: Yeah, yeah.
[00:49:33] Trevor Anderson: But I think it was fine last time, so might as well increase it to see how that does.
[00:49:41] Trevor Anderson: But that's pretty. Yeah, that's pretty fast though. For 8 million only 20 minutes. That's.
[00:49:47] Trevor Anderson: Yeah, basically like 15 minutes if you take out the spin up times.
[00:49:55] Venkatesh Mannam: Yeah. Spin up time and it's prepping the data for the notes.
[00:50:01] Trevor Anderson: Okay. Anything else that's going on?
[00:50:06] Venkatesh Mannam: Nothing much like just migrating the ad group conversion job to ucucm.
[00:50:12] Venkatesh Mannam: So I guess kind of got sidetracked with the Friday issue and.
[00:50:17] Trevor Anderson: Yeah, yeah, it's a good. It's a valid excuse, though.
[00:50:22] Venkatesh Mannam: Yeah. Yeah.
[00:50:27] Trevor Anderson: Okay. And is Vonnie gonna help out with that or.
[00:50:30] Venkatesh Mannam: Yeah, I told. I mean, like, mostly the dev work is done.
[00:50:34] Venkatesh Mannam: So when I kind of validating those results on Friday, I kind of saw some discrepancy between the results with the existing prod data and with my computation.
[00:50:46] Venkatesh Mannam: And when I was deep diving into that, I found that I was informing that brand as well.
[00:50:51] Venkatesh Mannam: So in the current ad group conversions, they're using the optimized conversions, but at the first place, the UCM don't have those filters.
[00:51:01] Venkatesh Mannam: Right. It kind of counts every click and every conversions. So.
[00:51:07] Venkatesh Mannam: So the rework should be done to filter out those. Optimize conversion.
[00:51:13] Venkatesh Mannam: I'm not sure what really is the optimized conversion.
[00:51:18] Trevor Anderson: Oh, I see that. That specific metric.
[00:51:21] Venkatesh Mannam: Yeah, yeah, that's right. Yeah. So there is this filter type name conversion. Type name. And the.
[00:51:30] Venkatesh Mannam: Those records get filtered using the type name is equal to optimized conversion.
[00:51:35] Venkatesh Mannam: And then only the current goal ad group conversions is counting those records.
[00:51:43] Trevor Anderson: Interesting.
[00:51:44] Venkatesh Mannam: Yeah.
[00:51:47] Trevor Anderson: Okay, well, let me know when you look at. Learn more about that. Figure out more about that.
[00:51:53] Venkatesh Mannam: Sure thing, too. Yeah.
[00:51:54] Trevor Anderson: Do you know about release 31?
[00:51:58] Trevor Anderson: Do you know, did we have a plan for when we were trying to get this out on tracking, like our sprints and where we.
[00:52:10] Venkatesh Mannam: Yeah, end of the sprint. I mean, like, so once we end this print on Friday. Right.
[00:52:14] Venkatesh Mannam: So it should be released either Monday or Tuesday based on the.
[00:52:18] Trevor Anderson: Okay, next week.
[00:52:19] Venkatesh Mannam: Yeah, next week. It should be next week mostly. And we close the 30th.
[00:52:24] Venkatesh Mannam: I see the status is still unreleased for the 330.
[00:52:30] Trevor Anderson: Oh, yeah, I'm gonna do that this morning. But yeah, 30 is out, though.
[00:52:36] Venkatesh Mannam: Yeah, 30 is out. Yeah, that's right.
[00:52:40] Trevor Anderson: I'm just gonna do that right now. Oh, I need to give us. Oh, I released it.
[00:52:51] Venkatesh Mannam: Oh, the release date. I think we don't.
[00:53:01] Trevor Anderson: Oh, I released it, but I think I put the wrong date maybe. Oh, yeah, it says 3.95 days ago.
[00:53:19] Trevor Anderson: What date was it
[00:53:22] Venkatesh Mannam: checking that day?
[00:53:23] Trevor Anderson: It was like the 23rd or the 24th, right?
[00:53:26] Venkatesh Mannam: Yeah, 24th. Yeah, that's right, 24th.
[00:53:28] Trevor Anderson: Okay. Just updated it. I should probably put some notes in there, though.
[00:53:41] Venkatesh Mannam: Yeah, yeah.
[00:53:42] Trevor Anderson: Okay.
[00:53:43] Venkatesh Mannam: And I have added those information in the Lattice as well.
[00:53:49] Trevor Anderson: Oh yeah, I saw that. Thank you very much for doing that. I really appreciate it.
[00:53:53] Trevor Anderson: I use that to, to write my update.
[00:53:56] Venkatesh Mannam: Okay. Okay.
[00:53:57] Trevor Anderson: So thank you. Yeah, that's. Those are going to be helpful for me. So thank you for doing that.
[00:54:05] Venkatesh Mannam: Yeah, yeah, yeah.
[00:54:09] Trevor Anderson: Great. Anything else on my end? I don't think so. I will check on that catalog creation there.
[00:54:25] Trevor Anderson: And then I guess let me know when we're going to do the. The monthly load for ig.
[00:54:35] Venkatesh Mannam: Yeah, it should be today or most probably tomorrow. I'll.
[00:54:40] Venkatesh Mannam: When I start the sync because the underlying table. So every time we do this full sync. Right?
[00:54:45] Venkatesh Mannam: Yeah, a good, good point. I. I just wanted to check on the other thing as well. So the process side.
[00:54:53] Venkatesh Mannam: So earlier we used to do full override on the IG API tables as well.
[00:54:58] Venkatesh Mannam: Trevor, what it does is it kind of truncate the.
[00:55:03] Venkatesh Mannam: I mean like this is all in the data bricks, delta tables that I'm saying.
[00:55:06] Venkatesh Mannam: So what it kind of does is it truncates a whole data and it, it writes the.
[00:55:11] Venkatesh Mannam: Or it loads that full data from upstream Gold customer v1. Right.
[00:55:17] Venkatesh Mannam: So in that case, if I do a truncate and if I load the data into this delta tables and if we try to sync the same to RDS tables.
[00:55:30] Venkatesh Mannam: So I'm thinking like. So if it kinds of. I'm like.
[00:55:35] Venkatesh Mannam: So what I'm trying to say is so if we try to do a full sync, right.
[00:55:42] Venkatesh Mannam: It kind of does a stage swap, right. I mean like the staging table and then it does a swap. Right.
[00:55:50] Venkatesh Mannam: So with that swap, do we see any downtime? If you think of anything.
[00:56:01] Trevor Anderson: Swaps on the FIG side is what you're saying?
[00:56:05] Venkatesh Mannam: No, the IG side. IG APS side.
[00:56:08] Trevor Anderson: So what I'm saying it's not swapping anymore.
[00:56:12] Venkatesh Mannam: Yeah, it's not swiping anymore.
[00:56:15] Venkatesh Mannam: I'm just curious to know like if we kind of do that swap, is it gonna.
[00:56:23] Trevor Anderson: Why would we do that this time?
[00:56:27] Venkatesh Mannam: No, no, I'm not planning to. I'm just checking on that.
[00:56:31] Trevor Anderson: If in theory.
[00:56:33] Venkatesh Mannam: Yeah. If we really do that, what it kind of does, is it going to affect the RDs?
[00:56:37] Venkatesh Mannam: I mean like API performance?
[00:56:41] Trevor Anderson: Like when we do a table swap, it's. It's extremely fast operation.
[00:56:46] Venkatesh Mannam: Yeah, yeah.
[00:56:47] Trevor Anderson: Like it's maybe like a second, you know, or two.
[00:56:56] Venkatesh Mannam: Yeah.
[00:56:58] Trevor Anderson: So that's the downtime. You get like a two seconds downtime.
[00:57:03] Venkatesh Mannam: Okay.
[00:57:03] Trevor Anderson: Or five seconds or whatever, however long it takes just to change the table name reference.
[00:57:09] Venkatesh Mannam: Okay. Okay.
[00:57:11] Trevor Anderson: So yes, if there was a read that happened during that swap, then it would get canceled.
[00:57:17] Venkatesh Mannam: Okay.
[00:57:18] Trevor Anderson: So you actually. You would get downtime, but it was. It's only for a split second.
[00:57:24] Venkatesh Mannam: Okay. Okay. Makes sense.
[00:57:28] Trevor Anderson: So. But those operations are really fast. But we're not doing blue green anymore.
[00:57:35] Trevor Anderson: That's what I thought. Right. Okay. You just were curious about that.
[00:57:38] Venkatesh Mannam: Yeah. Yeah, that's right.
[00:57:42] Trevor Anderson: Yeah. So those should take just a sec. Like. Like to perform that transaction is pretty quick. So.
[00:57:51] Venkatesh Mannam: Yeah. I think during the table step, then it. It will not return any results to the IG API. Okay.
[00:58:09] Trevor Anderson: Okay. But we're not doing that, right?
[00:58:12] Venkatesh Mannam: Yeah. Yeah, that's right. We're not doing that.
[00:58:14] Trevor Anderson: Okay. All right. I think we covered a lot. Yeah.
[00:58:19] Venkatesh Mannam: Yeah.
[00:58:21] Trevor Anderson: Okay. Awesome. Well, thanks for all the updates. Really appreciate it. Yeah, I'll.
[00:58:30] Trevor Anderson: Does it look like we have anything else later today? I don't think so.
[00:58:36] Venkatesh Mannam: I think I have one with today.
[00:58:38] Trevor Anderson: Just Dan.
[00:58:40] Venkatesh Mannam: Yeah.
[00:58:41] Trevor Anderson: Okay, great. Well, I'll talk to you later then. If not tomorrow.
[00:58:45] Venkatesh Mannam: Yeah. Yeah, okay.
[00:58:48] Trevor Anderson: Sounds good. Thank you very much.
[00:58:51] Venkatesh Mannam: Yeah, thanks. See you. Bye.
