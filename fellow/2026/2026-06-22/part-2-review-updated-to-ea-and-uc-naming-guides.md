---
title: Part 2: Review Updated to EA and UC Naming Guides
date: 2026-06-22
time: 09:00-09:30
participants: [Trevor Anderson, Brian Silveri, Bharat Gidwani, Manav Paul, Joseph Melkin, Granth S, Akhil Gonna, Venkatesh Mannam, Vani Kaithi, Kapil Sreedharan]
fellow_url: https://fluentco.fellow.app/meetings/040000008200E00074C5B7101A82E00800000000CAED0DDD5BFFDC01000000000000000010000000F23843906F523D4894397D8DB29C9884/
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
[ ] Schedule a meeting with Joseph, Manav, and Granth to review the existing audience solutions design and evaluate how it maps to the new medallion schema structure, including discussion of Live ramp and Transunion catalogs.
[ ] Set up a separate conversation to discuss the pattern for accessing S3 (whether to use volumes or direct S3 access, and how to handle external data sources).
[ ] Granth, Manav, and Joseph to review the code skills in the central data repo and copy them over to the audience solutions repo if applicable, tailoring them as needed.

---

The team reviewed comprehensive **data engineering standards for Unity Catalog**, establishing guidelines for schema structure, naming conventions, documentation requirements, and technical configurations. Key decisions included mandating **Bronze, Silver, Gold, and Metrics as the four approved schemas** (with custom schemas requiring peer review), requiring **business-focused comments** on all Silver, Gold, and Metrics tables, and standardizing on **snake_case column naming** with mandatory audit columns. The team also addressed volume storage organization by medallion layer, established **primary key and constraint standards**, and determined that UCM tables, online tables, and feature store tables will live in the Metrics schema.

The discussion covered migration planning for audience solutions to align with the new medallion architecture, with a deployment scheduled for end of sprint. Additional topics included retention policy adjustments for infrequently-updated tables, the deprecation of the central_data_prod catalog, and strategies for handling external data sources. Trevor will schedule follow-up meetings to discuss audience solutions design mapping and S3 access patterns.

### Schema Structure and Custom Schemas

- Some existing schemas (like ops and ML) were built for specific purposes like anomaly detection and log capture, not following bronze/silver/gold patterns. These schemas captured RPS metrics and other operational data.
- Data science assets currently live across multiple catalogs and don't operate in a medallion architecture. The team is considering consolidating them into a **dedicated data science schema** rather than a separate catalog.
- **Bronze, Silver, Gold, and Metrics are the approved schemas** that can be created as needed. Any additional custom schemas require peer review and team approval, with discussion of business justification required.

### Comments and Documentation Requirements

- **Comments are required** on tables in Silver, Gold, and Metrics layers, including both table-level descriptions and inline column comments. Bronze layer comments are optional but recommended.
- Comments must be **business-focused, not technical descriptions**. They should make sense from a stakeholder's perspective using the data, not from a data engineering perspective.
- Team members can reach out to Brian for help identifying column meanings or descriptions. Technical columns like 'create_date' can have standardized comments noting they are audit columns.
- Bronze tables are exempt from comment requirements since stakeholders won't use them directly - they're strictly for data engineering understanding of source data.

### Table Naming Conventions Overview

- Every table needs an **entity name** and may require a domain designation when multiple subsource systems contain the same entity type (e.g., users from Sprout Social vs Facebook vs Instagram).
- **Domain** is defined as a subcomponent of a source system - a distinct product surface or subsystem within a single source. Some sources are monolithic and map cleanly to one catalog, others contain several different products.
- **Version suffixes (e.g., '_v1') should not be permanent solutions** - they're for iteration on existing tables. Once tested and approved in production, changes should be applied to the primary table and the versioned table deprecated.
- **Fact tables** use 'fact_' prefix, belong only in Gold schema, and should include date grain as suffix (e.g., '_daily'). Fact tables may require additional descriptions in the name to clarify purpose.
- **Dimension tables avoid 'dim_' prefix** and are treated as standard entity tables. Prefixes for special table types include 'online_' for sync tables, 'feature_' for feature tables, 'mv_' for metric views, and 'v_' for standard views.

### Column Naming Standards

- **Snake_case is required** for all column names (lowercase with underscores), avoiding camel case entirely.
- Complex data types like structs should be used to logically group related columns, with arrays and maps used on an ad hoc basis where needed.
- **Audit columns are mandatory** for silver and gold tables: created_at and updated_at timestamps must be included on all records.
- **IDs must be entity-qualified** (e.g., customer_id, campaign_id) - bare 'id' or 'guid' columns without entity context are not allowed.
- The team established standards around naming conventions and will adjust the document as new patterns emerge during Q3 development.

### Primary Keys and Constraints

- **Primary keys should use integer type** when possible for join performance optimization, with GUID/string type as fallback where integer isn't feasible.
- Constraints in databricks help with **lineage tracking** but are not enforced - they provide warnings only when violated.
- **Primary key constraints should be defined** in Unity catalog to improve data asset tracking and make key relationships visible in the databricks UI.
- Additional constraints like not null and defaults should be added on a **per-use-case basis** when the team is confident about column requirements.
- The **'rely' option** on foreign key constraints enables databricks to optimize query performance by understanding primary-foreign key relationships, reducing costs.

### Audience Solutions Migration Planning

- Audience solutions currently has schemas like 'malleable_universe' and 'livermdf' that don't align with the central data structure. The team needs to determine if these become part of Gold schema or separate catalogs.
- The **new deployment at end of sprint** changes audience solutions to point to Databricks, making it a logical time to implement the new structure with proper access controls and permission granularity.
- **Gold marketable universe table** (combining lead manager and Playful(?) data) will belong in the Fluent catalog. Distribution tables for Live ramp and Transunion(?) may remain as separate schemas or domains.
- Live ramp, Lotame(?), and Transunion(?) tables are **essentially platinum-level tables** - they are created and shipped to external marketplaces, not ingested data from those platforms.
- Initial thinking was these external platforms warranted separate catalogs, but since the team is serving data **to** (not from) these platforms, they likely don't need entire catalogs - schemas within the medallion structure should suffice.

### Volume Storage Strategy

- Legacy pipelines stored checkpoints in a central shared volume location, but the new approach creates **managed volumes within each catalog schema** rather than external mounts.
- Checkpoints are now created as managed volumes in the schema itself (e.g., customer schema), while the old shared_volumes location is an external S3 mount accessible outside databricks.
- The team aligned volume organization with **medallion architecture**: ingestion sources in bronze schema, checkpoints in the layer where they're used (bronze/silver/gold).
- The data_sources folder structure may be phased out entirely in favor of pointing directly to S3 locations, as currently done for playful data and lead manager.
- Further discussion needed on volume migration strategy, particularly for handling vendor-uploaded files and managed CSV files in GitHub.

### Catalog Migration Challenges

- External data sources like Experian are currently mounted in central_data_prod as external volumes with **SFTP access for vendors**, requiring careful migration planning.
- The central_data_prod catalog will eventually be deprecated, requiring all data sources to move into their respective source catalogs (Experian, Melissa, etc.).
- Migration will take time due to dependencies, particularly for externally-mounted volumes where vendors are actively uploading data via SFTP.
- Current approach for playful and lead manager data is to point directly to **S3 locations** without mounting as volumes, which may become the standard pattern.
- Team wants to move away from centralized data storage and **place each source in its own catalog** (Experian with Experian, Melissa with Melissa), but needs to resolve S3 access patterns first.

### Table Properties and Configuration

- **'Cluster by auto' is recommended** for all tables unless a measured access path justifies using an explicit clustering key for performance optimization.
- **Default retention policy is 7 days** for time travel in Databricks. Tables updated infrequently (e.g., monthly) need adjusted retention policies to maintain history between updates, or there will be no time travel data available.
- **Retention policy adjustments** should be applied during table creation using table properties in the DDL statement, not as separate alter statements after the fact.
- The team will add an **addendum covering volumes, functions, and models**. Currently there are some functions for PII and similar use cases, but not many across the system.

### Code Skills Documentation

- Code skills exist in GitHub in the central data repo to help developers follow the naming patterns and standards discussed.
- Bharat recommended **copying and tailoring the code skills** for the audience solutions repo, potentially using Claude to adapt them based on existing code.
- Team should follow the naming conventions when developing new work in Q3, and **bring up any issues** that don't fit so the document can be adjusted to maintain consistency across Unity catalog.

### Action Items

- Trevor Anderson to schedule a meeting with Joseph, Manav, and Granth to review the existing audience solutions design and evaluate how it maps to the new medallion schema structure, including discussion of Live ramp and Transunion catalogs.
- Trevor Anderson to set up a separate conversation to discuss the pattern for accessing S3 (whether to use volumes or direct S3 access, and how to handle external data sources).
- Granth S, Manav Paul, and Joseph to review the code skills in the central data repo and copy them over to the audience solutions repo if applicable, tailoring them as needed.

### Decisions

- **Any purpose-built schema** beyond Bronze, Silver, Gold, and Metrics requires peer review and team approval before creation.
- **Table and column comments are mandatory** for Silver, Gold, and Metrics layers; Bronze layer comments are optional but recommended.
- Comments must be **business-focused and outward-facing** for stakeholders, not technical descriptions for data engineers.
- **UCM tables, online tables, and feature store tables will live in the Metrics schema**; Gold schema will only contain fact tables and entity-based dimension tables.
- The new naming convention applies **on a go-forward basis only**; existing legacy naming conventions should remain until a formal migration plan is developed.
- **Dimensional attribute tables will not use 'dim_' prefix** and will be treated as standard entity tables.
- **'Cluster by auto' is recommended** for all tables unless a measured access path justifies an explicit clustering key.
- Tables with **update cadences longer than the default 7-day retention** (e.g., monthly updates) should have adjusted log retention policies to maintain time travel history.
- **Volumes will be organized by medallion layer**: ingestion sources in bronze schema, checkpoints in the schema where they're used (bronze/silver/gold).
- **Audit columns required** for all silver and gold tables: create and update timestamps must be included on records.
- Bronze metadata column will use **snake_case naming** (_metadata) instead of camel case (_bronzeMetadata).
- **Primary keys should use integer type** when possible for join performance optimization, with GUID/string type as fallback where needed.
- **Primary key constraints must be defined** in Unity catalog for improved data asset tracking and lineage visibility.

## Transcript

[00:00:00] Trevor Anderson: With these items,
[00:00:03] Bharat Gidwani: bronze, silver, gold patterns or anything. It just some data that we are collecting.
[00:00:10] Bharat Gidwani: One of the instance was we built a.
[00:00:12] Bharat Gidwani: Now it's disabled but we had built a anomaly detection at one point for. For RPS and all that.
[00:00:24] Bharat Gidwani: And there are like certain schemas here which is like ops, ML.
[00:00:27] Bharat Gidwani: Like if you look at ML and ops they are. They were like built by us.
[00:00:34] Bharat Gidwani: They were purely capturing some log information for us. They are not. Yeah, there's no.
[00:00:44] Bharat Gidwani: It's not like a like a bronze or anything.
[00:00:47] Bharat Gidwani: So some of these things could happen and this is, this is one of those examples
[00:00:55] Trevor Anderson: and I know you know some of the Data Science assets they live within other catalogs as well.
[00:01:00] Trevor Anderson: And and then there's also some of these other ones right. Like this model deployments catalytic.
[00:01:07] Bharat Gidwani: They started using this data science model deployment now.
[00:01:11] Bharat Gidwani: But yeah but they I think the older, the older model older pattern was click underscore API click underscore model prod API whatever.
[00:01:25] Bharat Gidwani: Like these were earlier ones. I think they're moved away but it still might be using this and so
[00:01:33] Trevor Anderson: I mean it might make sense to have their own data science schema schema or schemas if there's a need to actually have different schemas but we can consider having a specific data science schema where their tables, tables, models, volumes could live.
[00:01:56] Trevor Anderson: Okay, but I kind of just kept it open for now.
[00:02:00] Trevor Anderson: Like I think the best case is to actually have it their own layer at the schema level instead of a catalog level because they.
[00:02:12] Trevor Anderson: Because of the fact that Data science doesn't operate in a medallion architecture.
[00:02:15] Trevor Anderson: It wouldn't make sense to have almost their own catalog with.
[00:02:21] Trevor Anderson: Okay, could be the case after discussion but this is a specific use case that needs to be.
[00:02:28] Trevor Anderson: I think some some investigation work needs
[00:02:32] Bharat Gidwani: to be done before Essentially Essentially we are saying and maybe might want to add a note here where we should have some conversation if somebody is creating a a custom schema for XYZ reason we should like it should be reviewed.
[00:02:50] Bharat Gidwani: Like why why a custom schema is being created.
[00:02:53] Bharat Gidwani: Maybe it's a business need but but at least we should should discuss those.
[00:02:59] Bharat Gidwani: I think that's what I think we are getting to. Right?
[00:03:03] Trevor Anderson: Yes so any any like purpose built schema needs to be reviewed by the team before moving forward.
[00:03:09] Trevor Anderson: We can't just go and create additional schemas. The this list here Bronze, silver, gold metrics.
[00:03:16] Trevor Anderson: These are approved schemas. So these can be used as needed and created as needed.
[00:03:23] Trevor Anderson: But any additional schema will need to be peer Reviewed by the team first and then approved before creating it.
[00:03:33] Bharat Gidwani: Makes sense.
[00:03:47] Trevor Anderson: Thank you. I think the last thing here was comments.
[00:04:01] Trevor Anderson: So comment adding comments and I think this is in the data objects as well.
[00:04:08] Trevor Anderson: But it's a requirement for us like you know, as teams are going to be as stakeholders are going to start using our data assets.
[00:04:19] Trevor Anderson: We definitely want comments on the gold and metrics layers as well as silver.
[00:04:28] Trevor Anderson: So it's a requirement for us to
[00:04:29] Bharat Gidwani: add
[00:04:32] Trevor Anderson: descriptions on the tables that live within silver, gold and metrics layers.
[00:04:40] Trevor Anderson: And this includes the table description as well as inline comments on the columns as well.
[00:04:49] Trevor Anderson: I think bronze is optional but definitely recommended as it's going to be our kind of source records.
[00:05:01] Bharat Gidwani: One thing Trevor, just highlighting it is we should make sure they are business comments, not technical descriptions.
[00:05:10] Bharat Gidwani: I think sometimes we just add technical descriptions to these, to these columns.
[00:05:17] Bharat Gidwani: So maybe we should avoid those.
[00:05:21] Trevor Anderson: Yeah, that's a good point.
[00:05:23] Trevor Anderson: We, we want the comments to be outward facing like we, we want them to make sense from a stakeholder using this data resource and to be this table to make sense from their point of view.
[00:05:43] Trevor Anderson: Right. Rather than from a data engineering perspective. Especially in the serving layer.
[00:05:48] Bharat Gidwani: Right, correct. And yeah, if they are obviously we may not know all of them.
[00:05:52] Bharat Gidwani: So yeah, reach out to Brian if, if you need help in identifying what the column means or what the description should be.
[00:06:04] Bharat Gidwani: But yeah, like if, if they are purely technical columns like create data, whatever we can standardize it to say these things should have some, a standard comment like it's audit column or whatever.
[00:06:21] Bharat Gidwani: But anything else should have. Yeah
[00:06:28] Brian Silveri: and I think we've done a pretty good job of that so far.
[00:06:30] Bharat Gidwani: Yeah, yeah we have. I just wanted to make sure it add. It's added here as well. That way we don't.
[00:06:38] Brian Silveri: Yep.
[00:06:39] Bharat Gidwani: Don't miss out on that Trevor.
[00:06:41] Brian Silveri: We putting that on a layer level, are we requiring comments?
[00:06:48] Trevor Anderson: Yeah. Silver, Gold and metrics must carry table level comment and inline comment on every column.
[00:06:55] Brian Silveri: Great.
[00:06:56] Trevor Anderson: Bronze is exempt but recommended.
[00:07:00] Trevor Anderson: So if we have the time to add comments on bronze tables it's only going to help us but not a strict requirement because stakeholders will not be using bronze tables in any capacity.
[00:07:12] Trevor Anderson: So you know, it's strictly for de, you know, understanding the data.
[00:07:24] Trevor Anderson: Yeah, I think that covers everything on schema level. One thing or actually one more thing.
[00:07:39] Trevor Anderson: Here is the metric schema. So right now our UCM tables will live in this metric schema.
[00:07:48] Trevor Anderson: However, we have some online tables as well in feature stores and this could be a place for those tables to live in this schema I'm.
[00:08:03] Trevor Anderson: I'm not sure where our online tables live right now. Are they?
[00:08:08] Trevor Anderson: Well actually we don't have a medallion schema structure so they probably just live as their own, you know table.
[00:08:15] Trevor Anderson: Right. So it would make sense to have them in in the metric schema here.
[00:08:20] Trevor Anderson: So moving forward we, we would the online tables and any future underscore tables would belong in the metric schema.
[00:08:29] Trevor Anderson: Gold would only have back tables and entity based dimension tables. Makes sense.
[00:08:44] Trevor Anderson: I'm going to move to data objects.
[00:08:46] Trevor Anderson: I think we started covering off on this last time but so quick recap on this. So data objects cover.
[00:08:54] Bharat Gidwani: Sorry Trevor, before go forward the we were mentioning the permissions will only be applied to to gold and platinum platinum layers.
[00:09:07] Bharat Gidwani: Right. I mean silver sometimes it'll be exposed to stakeholders or other downstream process.
[00:09:13] Bharat Gidwani: It will be the golden. The metric schema they will always be exposed to downstream. Correct. Okay.
[00:09:25] Bharat Gidwani: Because the reason I'm saying that is with this segregation of online tables in the metric views, the underlying gold table still will be in the gold layer.
[00:09:36] Bharat Gidwani: So we just need to make sure the models can access both of them.
[00:09:40] Bharat Gidwani: Sometimes there's some confusion, but from a permission standpoint we'd love to make sure golden metric is exposed to the models because these will refer or reference the gold tables.
[00:09:55] Bharat Gidwani: So data science will need access to both these schemas. Okay.
[00:10:04] Trevor Anderson: Yeah. I mean unless we bring online into gold. I think regardless both of these are almost the same.
[00:10:15] Trevor Anderson: Platinum will be slightly more refined version of Gold but for the most part this will be.
[00:10:21] Trevor Anderson: These two schemas will have refined data for business ready consumption.
[00:10:27] Trevor Anderson: So I'm not concerned about access at either level here.
[00:10:33] Trevor Anderson: If we need to expose both of these access then that's fine.
[00:10:36] Trevor Anderson: I'm more concerned about access at the silver and bronze layer because it doesn't have all that business contextual information applied.
[00:10:44] Brian Silveri: Yep.
[00:10:44] Bharat Gidwani: Okay. Makes sense.
[00:10:48] Brian Silveri: Trevor, is there a plan as part of this or maybe that's part of the deliverable that we're going to have a migration plan for our existing catalog and then how those objects that are using this know to migrate to this because obviously the pointers would greatly impact the data science and the BI team here.
[00:11:15] Brian Silveri: Just curious if that's part of your deliverable or this is just like going forward.
[00:11:20] Brian Silveri: This is how we're going to do it and we're going to figure out migration as a separate deliverable.
[00:11:24] Trevor Anderson: Yeah.
[00:11:24] Trevor Anderson: So yeah right now this is completely on a go forward basis and migration plan will be a bigger effort that we'll plan out during this next quarter here on what is going to be the easy lift.
[00:11:42] Trevor Anderson: Items that can just simply migrate over what are all the data objects that have strong dependencies, models that are actively using it, downstream dashboards or reports or things that need to be migrated.
[00:12:00] Trevor Anderson: Right. So we'll have to build out a migration plan that supports this.
[00:12:08] Trevor Anderson: So yeah, good reminder for the team like this all this is on a go forward basis.
[00:12:13] Trevor Anderson: Any new greenfield entity will definitely want to follow this.
[00:12:18] Trevor Anderson: But if you're working out of an existing naming convention or legacy naming conventions, just keep that as is until we develop a migration plan for it.
[00:12:39] Trevor Anderson: Table names we covered some stuff before.
[00:12:43] Trevor Anderson: Every table will definitely need an entity name and we talked about entities and I called it domains at the time and we could probably choose a different word.
[00:12:53] Trevor Anderson: I just trying to describe the idea of if we have a source system like Ad Parlor, there could be multiple sub sources within AdParlor and these source systems could have.
[00:13:12] Trevor Anderson: If we're using an entity like user, for example, like we could have two subsource systems that both have users in them.
[00:13:21] Trevor Anderson: And so we'll definitely want to denote what the subsource system is within the table name.
[00:13:31] Trevor Anderson: So we know this comes from Sprout Social, this comes from Facebook, this comes from Instagram.
[00:13:41] Trevor Anderson: These users come from these different sub systems and I'm calling that domain.
[00:13:45] Trevor Anderson: But if domain doesn't make sense then we could change the name there. It's more about the idea.
[00:14:04] Trevor Anderson: So domain. Yeah, subcomponent of a source system.
[00:14:07] Trevor Anderson: A distinct product surface or subsystem within a single source.
[00:14:11] Trevor Anderson: Some sources are monolithic and map cleanly to one catalog, but others can contain several different products within a source.
[00:14:31] Trevor Anderson: Versioning in the table name.
[00:14:33] Trevor Anderson: So in certain cases, such as model deployments, a version of an existing table is required.
[00:14:41] Bharat Gidwani: Quick note around this. Does this apply to even audience solutions work that.
[00:14:48] Bharat Gidwani: That the team is working on or is this only for central data right now or anything related to fluent?
[00:14:56] Bharat Gidwani: I know they. On the audience solution side there is.
[00:14:59] Bharat Gidwani: There are a lot of schemas and I don't know if it fits the same plan fits there.
[00:15:08] Trevor Anderson: Ideally what we would like to do is have our, you know, the whole data engineering team operate out of this structure.
[00:15:18] Trevor Anderson: You know, audience solutions it should.
[00:15:22] Brian Silveri: Yes, they're part of us.
[00:15:24] Trevor Anderson: We want medallion architecture at the schema level for audience solutions.
[00:15:30] Kapil Sreedharan: Okay.
[00:15:31] Bharat Gidwani: Like for example, I gave.
[00:15:32] Bharat Gidwani: And maybe it will help if we have examples for that as well here, for example, they have I think Malleable Universe as a schema in there now does that, does that now become part of the goal tables or the goal schema in a new structure or is that a different catalog or whatever like some examples there that way?
[00:15:56] Bharat Gidwani: I think it might help solidify how these guys are migrating things
[00:16:06] Kapil Sreedharan: again.
[00:16:06] Bharat Gidwani: We don't have to do it right away all of them, but some structure around that as well.
[00:16:12] Bharat Gidwani: Like even livermdf I think.
[00:16:14] Bharat Gidwani: I mean I'm just looking at the catalog right now and there are similar or similar or different schemas that we don't have on the central data side.
[00:16:25] Brian Silveri: I think that is probably Trevor the let's say easiest but the most logical evolution given that the new deployment that we're going to do the end of the sprint for audience solutions changes everything to point the databricks anyway.
[00:16:41] Brian Silveri: So you might as well have the structure as part of another deliverable in Q3 for them to do this.
[00:16:47] Brian Silveri: And then we can deal with the access controls as part of audience solutions to make sure that it's clear and delineated so that people aren't over permissioned like Virginia and she's just permissioned to the right level of granularity.
[00:17:05] Brian Silveri: Just how I see it. I'll defer to all of you.
[00:17:08] Trevor Anderson: No, that makes sense. And so yeah, Joseph. Yeah. Manav and Grant will definitely.
[00:17:16] Trevor Anderson: I'll want to book a time and I've been wanting to do this to review the existing and I know we've done an audit of what needs to migrate and what doesn't to the new catalogs but we'll definitely want to evaluate the current design versus the new, you know, this new structured design that we have here and see how to how that legacy pattern will look in this new design and see what makes sense and what doesn't make sense.
[00:17:48] Trevor Anderson: I think yeah. Involves a whole different conversation around that because the use case is different.
[00:17:56] Trevor Anderson: But I think the plan is to have it fall underneath this pattern here.
[00:18:04] Kapil Sreedharan: So for audience solutions in order to do that that gold marketable universe. Right.
[00:18:10] Kapil Sreedharan: So right now they are building it from JC's table tables.
[00:18:18] Kapil Sreedharan: Now when we ingest all those lead manager or we already have those. Right.
[00:18:25] Kapil Sreedharan: We also need that lead manager users table we would be creating this gold marketable universe table Right in the fluence.
[00:18:36] Kapil Sreedharan: I guess in the fluence catalogs catalog.
[00:18:39] Trevor Anderson: If it, if it joins and combines data.
[00:18:42] Kapil Sreedharan: Yes, yeah, yeah it joins. It's basically lead manager data plus playful.
[00:18:51] Kapil Sreedharan: So it'll be in fluent catalog. And then for audience solutions there they create these.
[00:19:00] Kapil Sreedharan: What was that Live ramp Tu. I guess they, they said the Distributions. Right, distributions.
[00:19:06] Kapil Sreedharan: I guess they, they have these catalogs for that maybe that could be also called as domains.
[00:19:15] Kapil Sreedharan: But yeah, this is something that we
[00:19:17] Bharat Gidwani: need to look into.
[00:19:18] Kapil Sreedharan: But yeah, it makes sense to follow the existing what they have like live ramp and TU catalogs within audience solution and the gold marketable universe that's influent
[00:19:32] Bharat Gidwani: catalog.
[00:19:33] Trevor Anderson: Yes, yes, definitely. And so we'll need to talk about these Live ramp lot of may.
[00:19:45] Trevor Anderson: Transunion schemas that exist right now because maybe there needs to be a clear delineation.
[00:19:51] Trevor Anderson: These actually need to be separated permission boundaries and things like that.
[00:19:56] Trevor Anderson: Maybe it does warrant additional schemas in that case.
[00:19:59] Trevor Anderson: But for the most part I think everything needs to follow this medallion structure in the schema level.
[00:20:05] Kapil Sreedharan: Yeah, if you think about the medallion structure, those live Ram Lotima Pu, those false.
[00:20:11] Kapil Sreedharan: Those are really platinum tables. We create those tables that we ship it out to the marketplaces.
[00:20:19] Kapil Sreedharan: Those are platinum tables.
[00:20:25] Trevor Anderson: Yeah, I probably just need some more information on them like what they're actually used for and stuff.
[00:20:32] Trevor Anderson: But as it seems or as it sounds like they're just.
[00:20:35] Trevor Anderson: We're just storing data of what we have provided to these platforms. Right.
[00:20:39] Trevor Anderson: So in that case it just sounds like a platinum or a metrics level table. Right.
[00:20:48] Trevor Anderson: I'll set up some time though and we'll discuss further on that. Thanks for bringing it up.
[00:20:57] Bharat Gidwani: Yeah, I think the way, I mean if I, if you'll just take it verbatim, we talking about creating new catalogs for them instead of schemas because they are from a different source or going to a different source.
[00:21:11] Bharat Gidwani: I think that's what we think initially.
[00:21:14] Trevor Anderson: Right. And if we are ingesting data from these platforms, then they. Would it be a different catalog?
[00:21:18] Trevor Anderson: I think.
[00:21:19] Bharat Gidwani: Okay.
[00:21:19] Trevor Anderson: I think that the nuance here is that we might not be ingesting data from these platforms, but rather just have a schema that we're serving data to which I don't, I wouldn't say warrants an entire another catalog for that case.
[00:21:34] Bharat Gidwani: Right, okay, makes sense.
[00:21:37] Bharat Gidwani: Yeah, definitely review it and then we can identify what, where they should live in.
[00:21:45] Trevor Anderson: Yes. For versioning.
[00:21:53] Trevor Anderson: So this was a, you know, example that we, or a pattern that we saw in our existing unique catalog where, you know, in certain nuance cases we'll have an actual version number as a, as a suffix.
[00:22:11] Trevor Anderson: And so yeah, in certain cases will.
[00:22:14] Trevor Anderson: This will be, you know, something that is required to avoid breaking a production consumption of an asset.
[00:22:24] Trevor Anderson: And so if there is a case where we do need a version of a table, we'll just have a suffix as version number, for example, v1.
[00:22:34] Trevor Anderson: And the idea is like this shouldn't be a permanent solutioning, so it should just be for iteration on existing tables.
[00:22:44] Trevor Anderson: So once tested and improved in production, we should apply the changes that were made to the version number to the primary table and then deprecate the version table.
[00:22:55] Trevor Anderson: We shouldn't see long lived versioning in our Unity catalog, but we can use it as a lifecycle pattern, if you will.
[00:23:10] Trevor Anderson: I think right now we do have an underscore v1, but ideally what would happen is we would make those v1 changes on the primary table and then get rid of the V1 table and have the whoever's using the B1 right now, you know, revert back to the primary table.
[00:23:37] Trevor Anderson: Facts and dimensions we talked a little bit about this.
[00:23:40] Trevor Anderson: So fact tables belong in the gold schemas only. We use fact underscore prefix to denote fact tables.
[00:23:48] Trevor Anderson: You know, a pattern that I like to see is also on fact table specifically will denote the date grain of the fact table as a suffix.
[00:24:03] Trevor Anderson: And fact tables may also require additional descriptions in the table name to describe its purpose.
[00:24:08] Trevor Anderson: And so we'll have to consult with the team on additional descriptions as part of fact table naming conventions dimension tables.
[00:24:18] Trevor Anderson: So right now we'll, you know, we want to avoid dim underscore for dimensional attribute tables.
[00:24:23] Trevor Anderson: These will just be treated as standard entity tables, other prefixes and suffixes.
[00:24:31] Trevor Anderson: So online pre for prefixes online underscore will be used to denote online tables are synced like race tables.
[00:24:42] Trevor Anderson: I think online tables was the legacy naming for database. It's now called sync tables.
[00:24:46] Trevor Anderson: But we'll still keep the online underscore to denote this type of table.
[00:24:52] Trevor Anderson: Feature underscore will be used to denote feature tables, MV will be used to denote unity catalog metric views and V will be used to denote views which we might have limited use cases for standard views right now, but if there is one, then we want to say the underscore for that type of object suffixes.
[00:25:20] Trevor Anderson: So daily or date grain underscore date grain for the fact table dimension or fact table date grain for intermediate table.
[00:25:33] Trevor Anderson: So this came from the, you know, intermediate table used for transformation or per performance stager between steps will append underscore in.
[00:25:45] Trevor Anderson: And we want to avoid underscore raw or underscore clean or any other type of suffix.
[00:25:51] Trevor Anderson: If there's, you know, a den number, you know, a change that we need to make to this naming convention guide, then we'll consult with the team and then add it As a new option, a new suffix option moving forward.
[00:26:09] Trevor Anderson: But for right now, avoid any other suffix type on the table.
[00:26:15] Bharat Gidwani: Naming.
[00:26:21] Trevor Anderson: Clustering. So it's recommended that we use Cluster by on all the tables. Cluster by auto.
[00:26:32] Trevor Anderson: Unless a measured access path justifies an explicit clustering key.
[00:26:39] Trevor Anderson: We talked about comments already, but comments are required on the silver, gold and upstream schemas.
[00:26:45] Trevor Anderson: Bronze is optional, but recommended. How are we doing on time here?
[00:26:55] Trevor Anderson: Do you guys all have a little bit more time to get through this?
[00:27:00] Trevor Anderson: I just want to get to the columns here. If you need to drop. That's. That's okay.
[00:27:06] Bharat Gidwani: Yeah, I
[00:27:07] Akhil Gonna: think we can
[00:27:07] Bharat Gidwani: go.
[00:27:08] Trevor Anderson: Okay, great.
[00:27:09] Bharat Gidwani: Thank you.
[00:27:13] Trevor Anderson: Retention policy. So you know the default retention policy.
[00:27:17] Trevor Anderson: So this is like time travel and Dave bricks, right. Default seven days.
[00:27:22] Trevor Anderson: You know, certain tables we only update once a month. Right.
[00:27:25] Trevor Anderson: So we'll actually not have any history by the time we do the new, you know, we reprocess new data into that table.
[00:27:34] Trevor Anderson: So for those cases, for like monthly tables, you know, things that don't updated it too frequently, we'll want to adjust the retention policy on those tables
[00:27:46] Brian Silveri: to
[00:27:46] Trevor Anderson: something more appropriate for the table update cadence.
[00:27:53] Trevor Anderson: And so we'll have to alter the table properties to provide a longer log retention policy for that.
[00:28:04] Akhil Gonna: Just to confirm, this will be run using our ad hoc SQL script or do we maintain in the GitHub somewhere this alter commands.
[00:28:15] Bharat Gidwani: I think when you're creating a new table, if you know you.
[00:28:20] Bharat Gidwani: You only are going to refresh this once a month or whatever, we should apply this automatically.
[00:28:25] Bharat Gidwani: Don't, don't wait on doing it all the way later.
[00:28:31] Akhil Gonna: Okay, got it.
[00:28:33] Bharat Gidwani: But if again, if you have to do it after, after the fact, then yeah, use that, use that process.
[00:28:41] Akhil Gonna: Understood.
[00:28:42] Akhil Gonna: So if it is like a monthly or whatever the job which is as frequency more than a week, then we update.
[00:28:49] Akhil Gonna: Yeah, the table property. Okay, thanks.
[00:28:53] Trevor Anderson: Yeah.
[00:28:54] Trevor Anderson: And actually the ddl, I'm not sure if you can actually set the table properties in the DVL creation.
[00:29:03] Trevor Anderson: You probably can. That's something I would need to check on.
[00:29:06] Bharat Gidwani: I think we. We do create or we do apply certain properties already. This is. This should be.
[00:29:13] Bharat Gidwani: And I mean no different.
[00:29:16] Trevor Anderson: Shouldn't be. Yeah.
[00:29:17] Kapil Sreedharan: Okay.
[00:29:18] Trevor Anderson: So yeah, you can. You don't have to.
[00:29:21] Trevor Anderson: I guess what I was saying is you don't have to perform a separate alter statement. Right.
[00:29:25] Trevor Anderson: You could just do it on the create statement. Okay, great. Okay.
[00:29:33] Trevor Anderson: So that kind of, that covers, you know, all the data objects.
[00:29:37] Trevor Anderson: I think it mostly covers table and view definitions.
[00:29:40] Trevor Anderson: I'll definitely want to write an addendum to this that covers volume functions and models.
[00:29:51] Trevor Anderson: Do we have a lot of functions that exist? I. I haven't really checked.
[00:29:56] Bharat Gidwani: I think we have for PI and some of these other things where we have functions, but not a lot of them.
[00:30:08] Trevor Anderson: Okay.
[00:30:09] Trevor Anderson: But we'll definitely want some naming style guide for functions as well as I think volumes and models, they're going to follow similar but different naming convention.
[00:30:22] Trevor Anderson: So different types of data objects. Right. But model warrants discussion with data science team.
[00:30:28] Trevor Anderson: And we can probably handle these internally. Volume and function volumes.
[00:30:34] Bharat Gidwani: We definitely use a lot functions, not all the time.
[00:30:40] Trevor Anderson: Okay.
[00:30:41] Bharat Gidwani: Checkpoints and all they do they. They do use volumes.
[00:30:44] Trevor Anderson: A volume.
[00:30:45] Bharat Gidwani: Right. Okay.
[00:30:48] Trevor Anderson: And how is the naming structured for volume right now? Is there. Right.
[00:30:53] Bharat Gidwani: Okay. So I think there are two different things.
[00:30:55] Bharat Gidwani: One is obviously the naming and then where the volume is supposed to reside.
[00:31:01] Bharat Gidwani: So I think checkpoints in the, in the legacy pipelines we have checkpoints in a central location.
[00:31:09] Bharat Gidwani: But now we have switched away.
[00:31:11] Bharat Gidwani: And if you go, if you go to the schemas, I can, I can show you how that looks like.
[00:31:18] Bharat Gidwani: So there's a shared location in this shared volume. No, not shared. Sorry, shared volumes.
[00:31:27] Bharat Gidwani: So earlier the old pipelines used to dump every checkpoint here as a folder.
[00:31:33] Brian Silveri: But.
[00:31:33] Bharat Gidwani: And I think that the naming. There's no prefix or anything.
[00:31:39] Bharat Gidwani: It's just a name of the table or the entity that you're looking at.
[00:31:44] Bharat Gidwani: So that's how the names of the volumes are. This is all data that database managers.
[00:31:51] Bharat Gidwani: But now I think what we've done moving forward is we don't use this at least for checkpoints.
[00:31:58] Bharat Gidwani: We use it in the catalog itself. So like if I think the cold.
[00:32:03] Bharat Gidwani: The customer might be a good example to go to the customer schema.
[00:32:15] Trevor Anderson: Oh, I saw.
[00:32:17] Bharat Gidwani: Yeah, the schema itself in there. Yeah. So this seed. So now we create the volumes here.
[00:32:24] Bharat Gidwani: So instead of creating it under that other location, they are managed volumes instead of.
[00:32:30] Bharat Gidwani: And that the shared volume schema that you see there, that's actually external mount. It's.
[00:32:37] Bharat Gidwani: It's accessible via S3 bucket. This is managed by databricks.
[00:32:43] Bharat Gidwani: So you can't, you can't access this directly in. In aws. The other one you can.
[00:32:51] Bharat Gidwani: So yeah, so yeah, there I think the assumption is for checkpoints we definitely use the catalog or schema based volumes.
[00:33:01] Bharat Gidwani: Now like, like this, this one.
[00:33:05] Bharat Gidwani: But for the actual data itself, like if I'm ingesting some raw CSV or parquet file or whatever and it's manually like we manage that us, we put it in that shared volumes, the external mount.
[00:33:22] Bharat Gidwani: So if you go to the share shared volumes again there's a data source, there's a data sources at this folder.
[00:33:30] Bharat Gidwani: This is where we loaded from.
[00:33:34] Bharat Gidwani: The idea was that the DevSecOps team will need to expose may need to expose this to external sources.
[00:33:43] Bharat Gidwani: Like for example Experience, right? The Experience external folder here.
[00:33:49] Bharat Gidwani: This is actually mounted on SFTP and exposed to Experian and they load the data here directly and we ingest it from here.
[00:33:59] Bharat Gidwani: So this is an external location and obviously there are two things.
[00:34:04] Bharat Gidwani: One is external location for us and exposed to the vendor.
[00:34:11] Bharat Gidwani: So this data source, I mean anything that we manage and is a CSV or parquet file currently we put it in this location.
[00:34:22] Bharat Gidwani: Somewhere in this location.
[00:34:25] Bharat Gidwani: But volumes are predominantly now moving towards the checkpoints volume as a. As a path.
[00:34:36] Trevor Anderson: So I feel like this fits cleanly with the medallion structure.
[00:34:39] Trevor Anderson: Like if it's a ingestion source then we'll want it in the bronze schema as a volume.
[00:34:46] Trevor Anderson: If it's a checkpoint, have it in the schema that that checkpoint is being used for. Right?
[00:34:52] Trevor Anderson: So like if it's a bronze table that using a checkpoint for then have it have that volume in the bronze schema.
[00:35:01] Trevor Anderson: If it's a silver base checkpoint then in the silver schema, so on and so forth.
[00:35:07] Bharat Gidwani: Now the only thing would be now that we have this fluent, fluent catalogs and all, what do we do with this data sources?
[00:35:17] Trevor Anderson: These need to belong in the. And the source that's associated with the.
[00:35:22] Bharat Gidwani: Yeah, but the problem as I was saying, right?
[00:35:24] Bharat Gidwani: If it is, if it is coming from external sources, it will have to be mounted as a.
[00:35:30] Bharat Gidwani: As external volume in your. In your catalog. Because like for example Experian, right?
[00:35:37] Bharat Gidwani: If you build Experian catalog tomorrow we'll need to mount a separate volume for them or remove it from here.
[00:35:48] Bharat Gidwani: This is a subfolder here. We'll have to remove it from here and put it in that other folder.
[00:35:53] Bharat Gidwani: So we have to be careful how this, this thing works because this is an existing SFTP location where the, the vendor is actually uploading data.
[00:36:04] Bharat Gidwani: This is not managed by data databricks at all. This is actually a three model.
[00:36:12] Trevor Anderson: I. Well, we're not posting data here vendors.
[00:36:16] Bharat Gidwani: Yeah, we are not posting to it. But what I was saying is vendor
[00:36:19] Trevor Anderson: has access to this.
[00:36:20] Bharat Gidwani: Yeah. The thing is this, will this allow to move away from central data? Right?
[00:36:24] Bharat Gidwani: Because if you look at the path this is. This is actually mounted in central data prod as A volume.
[00:36:33] Bharat Gidwani: But once we start segregating the catalogs, this will have to come out of central data and move into the respective catalogs.
[00:36:46] Trevor Anderson: Right.
[00:36:47] Bharat Gidwani: Because this. Yeah. This catalog eventually will go away.
[00:36:50] Bharat Gidwani: I mean at some point central data prod will be. Will be gone.
[00:36:54] Bharat Gidwani: You will only have individual vendor or external. The source catalogs or the fluent catalog.
[00:37:04] Trevor Anderson: Yes. And I think for something like this we would have. Yeah.
[00:37:09] Bharat Gidwani: An experience figure out the migration for this because this. So this might get a little more.
[00:37:16] Trevor Anderson: Migration is going to take some time because of all these dependencies. Yeah, yeah.
[00:37:22] Bharat Gidwani: But at least for moving forward, once we build those catalogs, I think we'll have to figure out how the data sources are evolving.
[00:37:29] Bharat Gidwani: Right now I think we're not mounting them as data sources.
[00:37:32] Bharat Gidwani: We just directly hitting the S3 bucket and that is how the access path is. We are not using volume.
[00:37:40] Bharat Gidwani: So maybe we do the same thing.
[00:37:42] Bharat Gidwani: We kind of get rid of volumes and or ingesting data and go to the S3 location directly, which is what I think we are doing for injecting the playful data or lead manager and so on.
[00:37:59] Bharat Gidwani: We just go into the S3 bucket directly.
[00:38:02] Trevor Anderson: So just creating a table on top of that three bucket.
[00:38:05] Bharat Gidwani: Yes. Or even if it's a. As auto loader we are pointing to
[00:38:10] Trevor Anderson: the S3 bucket instead of the volume.
[00:38:13] Bharat Gidwani: Yeah. Instead of. Instead of mounting it as a volume like this.
[00:38:18] Bharat Gidwani: So FIG and xp, FIG and experience are two examples where we actually get data from somewhere else and they are mounted as a volume here.
[00:38:27] Bharat Gidwani: And we use a volumes instead of looking at S3 parts.
[00:38:31] Bharat Gidwani: But we should probably change that and start using S3 mount S3 parts directly in our code. Like this.
[00:38:40] Bharat Gidwani: This should go away for data sources. The data sources thing should go away altogether.
[00:38:47] Trevor Anderson: Okay, that's. I'll just.
[00:38:49] Kapil Sreedharan: Why is that?
[00:38:50] Kapil Sreedharan: Isn't we mount or external locations recommended so we don't have to deal with these S3 directly.
[00:39:00] Bharat Gidwani: Yeah, but we don't. We don't follow that path for playful or any data. We just mount.
[00:39:05] Bharat Gidwani: We just point to the S3 location directly. It is mounted as a external location for us.
[00:39:11] Bharat Gidwani: But we don't mount it as a volume right now. We don't do that for playful data lm or anything.
[00:39:18] Bharat Gidwani: We just point to the S3 location directly. Okay.
[00:39:23] Bharat Gidwani: So because as I was saying, the problem is otherwise you have to mount the volumes in those catalogs which is not being done.
[00:39:32] Bharat Gidwani: That's, that's. That will be a lot more work for DevSecop. I don't think they will.
[00:39:37] Bharat Gidwani: They will agree to that.
[00:39:39] Bharat Gidwani: Like every time we create a catalog, they have to mount a new volume to point to something.
[00:39:44] Bharat Gidwani: And we might run into roadblocks because some of these are just folders, some of these are just folders under, under existing S3 bucket.
[00:40:01] Trevor Anderson: I'll. I put a note here.
[00:40:04] Trevor Anderson: I'll have to set up a separate conversation on like what our pattern is going to be for accessing S3.
[00:40:10] Trevor Anderson: Do we, you know, if we want to create a volume, how we're going to do that?
[00:40:14] Trevor Anderson: If we don't want to at all, then what's our pattern for, you know, accessing S3 directly?
[00:40:21] Trevor Anderson: But it definitely warrants a separate conversation. So I'll make sure it's up time for that.
[00:40:29] Bharat Gidwani: But okay, I think we are saying it at least for volumes we know how, what the path forward is.
[00:40:40] Bharat Gidwani: But for ingesting data from Parquet or CSV files, we just need to figure out where it should reside.
[00:40:48] Bharat Gidwani: Because right now even the files that we manage, like the advertiser file that I think Venki is generating or for campaign data and all the assumption is it'll sit here in the data source as a subfolder and we upload it either via GitHub workflow or manually to this path.
[00:41:13] Bharat Gidwani: Similarly, we have this Melissa data which we get from JC every month.
[00:41:20] Bharat Gidwani: There's a process that he's downloading from somewhere using lambda function and then there's a separate copy path to copy it from there, his location to this folder.
[00:41:35] Bharat Gidwani: So things like that, like we just need to figure out how.
[00:41:39] Trevor Anderson: So we definitely want to get it in the catalog.
[00:41:41] Trevor Anderson: I think we want to move away from this idea of we have this catalog that just has data from all these different sources.
[00:41:50] Trevor Anderson: Right. Like I want Experian to be with Experian, right.
[00:41:56] Trevor Anderson: Melissa to be with Melissa, so on and so forth.
[00:41:58] Trevor Anderson: Like it to actually be in the catalog where it's coming from. Right.
[00:42:02] Trevor Anderson: So, but this is a specific nuance that we'll need to discuss further.
[00:42:07] Trevor Anderson: I know that this is how it's currently being handled.
[00:42:09] Trevor Anderson: So we'll need to find a migration strategy for that. Right.
[00:42:13] Bharat Gidwani: But yeah, like I think you will look at cbur.
[00:42:16] Bharat Gidwani: I think someone, I mean VI Star we have started looking at.
[00:42:20] Bharat Gidwani: Or if you look at the CBR folder there, I think we, we just started the same structure for this.
[00:42:28] Trevor Anderson: Ah, I see. Okay.
[00:42:30] Bharat Gidwani: So yeah, so like this, this would
[00:42:33] Trevor Anderson: want, we would want this in, in the Fluent catalog, right? I think, yeah.
[00:42:37] Trevor Anderson: Okay, so that's how do we handle. Right. We'll need a Place to upload that.
[00:42:44] Bharat Gidwani: Exactly. Yeah. The only caveat I was saying there are two different data sources.
[00:42:48] Bharat Gidwani: One is we manage it so we have it in GitHub or whatever, the CSV file and we can copy it to a managed location and all that.
[00:42:57] Bharat Gidwani: Actually, you may not be able to, but we can talk about that how we do it.
[00:43:01] Bharat Gidwani: Because it's a managed location, the path is not set.
[00:43:04] Bharat Gidwani: So I don't think you can upload directly from GitHub. But the other is a vendor sending us that file.
[00:43:12] Bharat Gidwani: So that has to be definitely a external X3 location where someone else can get to it and it's not managed by databricks and we have to access it and so on.
[00:43:23] Bharat Gidwani: So yeah, they're like those two different access patterns.
[00:43:29] Kapil Sreedharan: Okay.
[00:43:32] Trevor Anderson: Yeah, for.
[00:43:33] Trevor Anderson: For the lack of time, I'm going to move forward, but I'm definitely set up some time to discuss how we're going to handle volumes.
[00:43:41] Bharat Gidwani: Okay, thank you.
[00:43:42] Trevor Anderson: Thanks. Thanks Barat, for the notes on that. Let's just.
[00:43:47] Trevor Anderson: Let's just cover off columns real quick so we can finalize this. But columns.
[00:43:51] Trevor Anderson: I think there's some important notes here to make the lowercase na case appropriate.
[00:43:58] Trevor Anderson: Data type struct. You know, struct. We want to group different columns together if they.
[00:44:06] Trevor Anderson: If there's a logical entity which to group, you know, columns into a struct for.
[00:44:14] Trevor Anderson: This doesn't talk about arrays, but also this would cover like other dig types of arrays.
[00:44:19] Trevor Anderson: I'm not sure if we use anything else besides arrays and structs. Do we use.
[00:44:23] Trevor Anderson: Have we used maps or there's other ones. I know map is like a dictionary.
[00:44:33] Bharat Gidwani: I think we might have used it for intermediate tables and maps and all, but not.
[00:44:36] Bharat Gidwani: It's not like there's any broad table which has. Has that information anyway.
[00:44:46] Bharat Gidwani: I think silver offer events. That table has a map. I think. Yeah. Okay.
[00:44:56] Trevor Anderson: I think, I think advanced data types like that is just going to be on an ad hoc basis.
[00:45:01] Trevor Anderson: Like if there's a need for it, just discuss with the team and then add it to the table.
[00:45:11] Trevor Anderson: Audit columns. So for every silver and gold table we'll want create and update timestamps on.
[00:45:23] Trevor Anderson: On these records. Bronze metadata we want to add to bronze layers. Unless it's.
[00:45:31] Trevor Anderson: Is this a default call created by databricks when you do cdc?
[00:45:35] Bharat Gidwani: No, I think we. For whatever reason we call it underscore bronze metadata.
[00:45:40] Bharat Gidwani: But I think databricks creates a metadata column and we kind of rename it to this.
[00:45:46] Bharat Gidwani: But if you want we can just keep it as Metadata instead of bronze metadata.
[00:45:52] Trevor Anderson: Okay. If it's in the bronze.
[00:45:54] Bharat Gidwani: Yeah,
[00:45:57] Trevor Anderson: we're going to change this to underscore. We yeah, don't want camel case.
[00:46:10] Trevor Anderson: So definitely use the snake case. So no create date camel case keys and IDs.
[00:46:18] Trevor Anderson: So one thing that we want to avoid is just, just a plain ID column.
[00:46:23] Trevor Anderson: Like we'll always want it to be entity qualified. So we want the entity to describe the id.
[00:46:30] Trevor Anderson: Like what type of ID is this? And so for example like customer id, campaign id, placement id.
[00:46:36] Trevor Anderson: So we shouldn't see any just place plane or bare ID or guid.
[00:46:41] Trevor Anderson: I think as a pattern that we want to follow is that primary keys, we want to lean towards having these as an integer type for joins and performance optimization on those joins.
[00:46:56] Trevor Anderson: I know if, if we can't have it as a integer type, I think the preference is integer type.
[00:47:05] Trevor Anderson: But if we need it to be string type then as a GUID then that's fine where it makes sense.
[00:47:14] Trevor Anderson: And I think also this constraint idea as well.
[00:47:19] Trevor Anderson: So constraints and databricks is helpful for, you know, lineage and tracking key types.
[00:47:28] Trevor Anderson: And so you can actually see the, you know, primary key or foreign key within the databricks UI if, if it's set.
[00:47:34] Trevor Anderson: But databricks does not enforce constraints, it will just only suggest that there's a constraint.
[00:47:43] Trevor Anderson: That's it will just provide a warning if a constraint is violated.
[00:47:49] Trevor Anderson: But it does help us track our data assets when we actually set key constraints.
[00:47:56] Trevor Anderson: So if we are defining a primary key, you can just add the primary key type so we can track that it is a primary key in the table.
[00:48:07] Trevor Anderson: It'll make sense that we we've defined a primary key.
[00:48:10] Trevor Anderson: In this case foreign keys are going to be on a per use case basis.
[00:48:14] Trevor Anderson: So if we know that there is an established foreign key, let's add the foreign key constraint in here as well.
[00:48:21] Trevor Anderson: Right.
[00:48:21] Trevor Anderson: We know that there's an existing foreign key, but definitely if we're establishing primary keys, let's make sure that we define it as a primary key that shows up within the Unity catalog.
[00:48:40] Bharat Gidwani: I think don't we need to do even not nulls or defaults for some of these?
[00:48:48] Bharat Gidwani: I know for primary key you have not null.
[00:48:50] Bharat Gidwani: But in this, in that same example, if username or email should always be there, then maybe that should be set as not null.
[00:48:59] Trevor Anderson: If we do know. Yeah, for non key IDs.
[00:49:02] Trevor Anderson: If we also know the certain specifics of this column, if we're fully confident that username should should be not null, we should set it as not null I think so constraints will be on the per use case basis.
[00:49:18] Trevor Anderson: If we can identify specific attributes about a column, we should definitely add, you know, those constraints where we see fit.
[00:49:32] Bharat Gidwani: Makes sense. And then that rely option was there, right?
[00:49:36] Bharat Gidwani: I think it's this example here with when you set a foreign key or something.
[00:49:43] Bharat Gidwani: I think it's in that same article that you just opened. There's a rely. Rely option right somewhere.
[00:49:49] Bharat Gidwani: Oh yeah, the. When you add a constraint, I think there is a. There's something to do with rely.
[00:49:56] Bharat Gidwani: I don't know what it does but train option. Yeah, believe I did read. I think it.
[00:50:05] Bharat Gidwani: Oh actually databricks can use this to optimize your queries. So if you.
[00:50:13] Bharat Gidwani: Yeah, yeah, definitely read up on this, see how it uses it.
[00:50:21] Bharat Gidwani: So if you I think specify that it should rely.
[00:50:25] Bharat Gidwani: Databricks should rely on the primary key and foreign key and unique constraints then it can actually optimize the joins.
[00:50:31] Bharat Gidwani: Just setting those keys may not just help in optimization.
[00:50:39] Trevor Anderson: Yeah, so this is when setting the foreign key, if we know what the foreign key, what primary key it's relying on, then databricks will optimize its query engine around that and so the queries will actually be performance optimized in that case.
[00:50:57] Trevor Anderson: So it does help us in reducing costs when we can actually identify the relationship.
[00:51:05] Trevor Anderson: So this will just be a good pattern moving forward as we build out our unity catalog. Right.
[00:51:13] Trevor Anderson: Like everything will be optimized for query production and everything like that.
[00:51:17] Trevor Anderson: So and this will be especially relevant to finding constraints in our fluent catalog with these, you know, fluent tables that are doing all these joins together right between these different dimensions.
[00:51:31] Trevor Anderson: So try to explicitly define these as much as we can and I think that's pretty much covers everything.
[00:51:42] Trevor Anderson: I'm sorry, we ran over just a note too.
[00:51:45] Trevor Anderson: I did have this extra page here which has the cloud skills that are in GitHub.
[00:51:49] Trevor Anderson: Thanks Barat for updating these. But these can be used to help you follow all these patterns that we.
[00:51:56] Trevor Anderson: That we went over.
[00:52:00] Bharat Gidwani: One quick thing was for these skills I would definitely recommend applying or kind of copying this and tailoring it for audience solutions if needed because I think this exists in that repo in a central data repo.
[00:52:17] Bharat Gidwani: If you guys want to copy this over to audience solutions, it might help there as well.
[00:52:25] Trevor Anderson: These knock out anything. Oh great. I need to update these things. I don't know why.
[00:52:30] Bharat Gidwani: Yeah, I don't know why it's not going there but oh, maybe you're not logged in or you are logged in so.
[00:52:40] Trevor Anderson: Oh I'm on my. I'm on a. I'm on my personal here. So that's.
[00:52:46] Bharat Gidwani: Yeah, maybe that's why.
[00:52:52] Kapil Sreedharan: Okay.
[00:52:53] Bharat Gidwani: But yeah, Grant or Manav, I think. Joseph, take a look at these. See if it makes sense.
[00:52:59] Bharat Gidwani: At least the code review and any of that, that makes sense for you guys, just copy them over, put them in that Claude folder and in fact, even ask Claude to update it for.
[00:53:10] Bharat Gidwani: Based on whatever code you guys have. It will. It'll fix it for you.
[00:53:22] Kapil Sreedharan: Good.
[00:53:23] Trevor Anderson: And as you. As we're developing new stuff, you know, let's follow these naming conventions.
[00:53:28] Trevor Anderson: If something doesn't fit, please bring it up so we can adjust this document to fit with the entire Unity catalog.
[00:53:36] Trevor Anderson: And we have a, you know, followed structure and everything is in sync. Right.
[00:53:41] Trevor Anderson: And so we want to keep this updated as we start developing into Q3.
[00:53:47] Trevor Anderson: So please try to align and bring up anything that doesn't follow this pattern or can't fit in this pattern, and we'll adjust as needed.
[00:53:57] Bharat Gidwani: Thanks, Trevor.
[00:53:58] Trevor Anderson: All right, thank you, everyone.
[00:54:02] Trevor Anderson: Bye Bye.
