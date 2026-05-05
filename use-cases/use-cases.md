---
status: review
last-reviewed: 2026-05-04
owners: [adam]
version: 0.2
---

# Civic.Social — Use Cases & User Stories

*How real people use Civic.Social: grounded narratives across the six roles that shape the ecosystem, plus multi-protagonist vignettes showing how their actions compound.*

## Contents

- [How to Read This Document](#how-to-read-this-document)
- [1. Citizens](#1-citizens)
  - [1.1 Sarah finds the budgeting process before it closes](#11-sarah-finds-the-budgeting-process-before-it-closes)
  - [1.2 Marcus, a busy parent, surfaces school board input in fifteen minutes a week](#12-marcus-a-busy-parent-surfaces-school-board-input-in-fifteen-minutes-a-week)
  - [1.3 Lena, twenty-two, activates around housing and finds an entire ecosystem](#13-lena-twenty-two-activates-around-housing-and-finds-an-entire-ecosystem)
  - [1.4 Robert, retired, builds a verifiable record of civic life across years](#14-robert-retired-builds-a-verifiable-record-of-civic-life-across-years)
  - [1.5 Yusuf, who had stopped voting, comes back through one specific issue](#15-yusuf-who-had-stopped-voting-comes-back-through-one-specific-issue)
  - [1.6 Brenda raises a Flock-camera question and votes on it without going to a meeting](#16-brenda-raises-a-flock-camera-question-and-votes-on-it-without-going-to-a-meeting)
  - [1.7 Tom proposes the green box site instead of waiting for someone else to](#17-tom-proposes-the-green-box-site-instead-of-waiting-for-someone-else-to)
  - [1.8 Diane gets the Floyd County board meeting she missed in three minutes the next morning](#18-diane-gets-the-floyd-county-board-meeting-she-missed-in-three-minutes-the-next-morning)
  - [1.9 Aisha checks her state rep's profile before the primary](#19-aisha-checks-her-state-reps-profile-before-the-primary)
- [2. Hub Admins / Civic Organization Leaders](#2-hub-admins--civic-organization-leaders)
  - [2.1 Janelle stands up the Agora neighborhood hub in a weekend](#21-janelle-stands-up-the-agora-neighborhood-hub-in-a-weekend)
  - [2.2 David runs a regional environmental coalition across six counties](#22-david-runs-a-regional-environmental-coalition-across-six-counties)
  - [2.3 Reyna, a community college civic engagement coordinator, runs a campus hub](#23-reyna-a-community-college-civic-engagement-coordinator-runs-a-campus-hub)
  - [2.4 Pastor Williams runs a church-led civic education hub](#24-pastor-williams-runs-a-church-led-civic-education-hub)
  - [2.5 A union local runs contract deliberation through a hub](#25-a-union-local-runs-contract-deliberation-through-a-hub)
  - [2.6 The Open Districts coalition issues a verifiable pledge candidates can sign](#26-the-open-districts-coalition-issues-a-verifiable-pledge-candidates-can-sign)
  - [Why not just use Facebook groups?](#why-not-just-use-facebook-groups)
- [3. Elected Officials](#3-elected-officials)
  - [3.1 Councilmember Patricia Diaz reads a representative cross-section](#31-councilmember-patricia-diaz-reads-a-representative-cross-section)
  - [3.2 State Representative Allen Park structures testimony intake](#32-state-representative-allen-park-structures-testimony-intake)
  - [3.3 School Board Member Maya Thompson hears from parents who can't attend](#33-school-board-member-maya-thompson-hears-from-parents-who-cant-attend)
  - [3.4 A mayor running for re-election surfaces a verifiable record of responsiveness](#34-a-mayor-running-for-re-election-surfaces-a-verifiable-record-of-responsiveness)
- [4. Candidates](#4-candidates)
  - [4.1 Tara Mitchell builds a record of listening before she's in office](#41-tara-mitchell-builds-a-record-of-listening-before-shes-in-office)
  - [4.2 Jordan Kim, a first-time state house candidate, runs a digital town hall series](#42-jordan-kim-a-first-time-state-house-candidate-runs-a-digital-town-hall-series)
  - [4.3 Hank Reeves runs cross-partisan listening sessions in a rural district](#43-hank-reeves-runs-cross-partisan-listening-sessions-in-a-rural-district)
- [5. Government Staff](#5-government-staff)
  - [5.1 Kim Pham, a city clerk, finally runs participatory budgeting](#51-kim-pham-a-city-clerk-finally-runs-participatory-budgeting)
  - [5.2 A county public engagement officer runs a formal comment period](#52-a-county-public-engagement-officer-runs-a-formal-comment-period)
  - [5.3 A small town gets engagement infrastructure it could never have afforded](#53-a-small-town-gets-engagement-infrastructure-it-could-never-have-afforded)
- [6. Civic Tool Builders](#6-civic-tool-builders)
  - [6.1 Sam Okafor, an indie developer, ships a citizen assembly plugin](#61-sam-okafor-an-indie-developer-ships-a-citizen-assembly-plugin)
  - [6.2 An established civic-tech nonprofit ports its tool to the protocol](#62-an-established-civic-tech-nonprofit-ports-its-tool-to-the-protocol)
  - [6.3 An academic research lab builds deliberation analytics](#63-an-academic-research-lab-builds-deliberation-analytics)
- [7. Network-Effect Vignettes](#7-network-effect-vignettes)
  - [7.1 The broadband thread: from one resident to a state floor vote](#71-the-broadband-thread-from-one-resident-to-a-state-floor-vote)
  - [7.2 A generic civic plugin proves itself in Floyd County and travels across hub boundaries](#72-a-generic-civic-plugin-proves-itself-in-floyd-county-and-travels-across-hub-boundaries)
  - [7.3 The youth assembly that convenes across four states](#73-the-youth-assembly-that-convenes-across-four-states)
- [Closing Note](#closing-note)

---

## How to Read This Document

User stories in Civic.Social are organized by **role** — the human protagonist whose problem is being solved — because every meaningful interaction crosses multiple software components and process types. Organizing by role keeps the narrative coherent; software components and process types are captured as **tags** at the end of each story so this document can be re-cut by component, process, or jurisdiction later.

The six roles are: **Citizen**, **Hub Admin / Civic Organization Leader**, **Elected Official**, **Candidate**, **Government Staff**, and **Civic Tool Builder**. The first four describe how Civic.Social is used; the last two describe how it is operated and extended. After the role sections, a short set of **Network-Effect Vignettes** follows actions across multiple personas to show how individual participation compounds into ecosystem-level outcomes.

These narratives are illustrative. Several characters and locations carry forward from the [Before & After](before-and-after.md) document so readers can build on a familiar cast. Floyd County, Virginia is referenced as the actual rural pilot context; Athens, Virginia and Agora are the running fictional stand-ins for a small town and a city neighborhood.

**Tag legend.** *Components:* Civic Hub, Civic Identity, Civic Profile, Civic Credentials (Badges), Citizen Dashboard, Civic Activity Feed, Civic Process Plugin. *Processes:* Advisory Vote, Announcement, Citizen Assembly, Credential Issuance, Credential Verification, Deliberation, Endorsement, Meeting Summary, Participatory Budgeting, Petition, Proposal Submission, Public Comment, Town Hall. *Journey Stage:* Discovery, Onboarding, Participation, Sustained Engagement, Influence Over Time. *Adopting organization* (where applicable): the institutional actor whose adoption decision the story implies, plus a one-line rationale. Stories with individual-only protagonists (Citizens, Elected Officials, Candidates, indie builders) do not carry this tag. *Hub operator* (where applicable): the entity that actually runs the hub the story takes place on — sometimes the same as the adopting organization, sometimes a partnering nonprofit, sometimes Civic.Social as a hub-as-a-service provider. Used in the Government Staff section to make the operator-versus-engager distinction explicit.

---

## 1. Citizens

The end users of Civic.Social. The product earns its keep when civic life feels continuous, navigable, and consequential to people whose lives are not organized around politics.

### 1.1 Sarah finds the budgeting process before it closes

Sarah lives in Athens, Virginia. In the old model she found out about a participatory budgeting process in her county three weeks after it closed — exactly the kind of friction failure that pushed her toward disengagement. After her town's hub joins the network, she logs into her Civic.Social dashboard once and sees a unified feed: an advisory vote on the school board hub closing this week, a county participatory budgeting process accepting proposals through next month, a state-level rural broadband comment period from a coalition organization she follows. She files a budgeting proposal for a sidewalk repair on her block, casts the school board vote, and forwards the broadband comment period to a neighbor. None of this required a new account or a re-verification. What changed was not Sarah's motivation — it was the friction between her motivation and where the civic action was actually happening.

> *Components:* Citizen Dashboard, Civic Activity Feed, Civic Hub, Civic Identity. *Processes:* Advisory Vote, Participatory Budgeting, Public Comment. *Journey Stage:* Discovery, Participation.

### 1.2 Marcus, a busy parent, surfaces school board input in fifteen minutes a week

Marcus has two kids in the Bridgewater County public schools. He cares about district decisions but the existing channels — board meetings on Tuesday nights, a PDF agenda dropped two days before, a single email address for written comment — do not fit a working parent's life. The district adopts the Civic.Social school board hub. Now the agenda for each board meeting surfaces in his dashboard with a structured comment window opening five days before the meeting and closing the night before. Each agenda item links to a short staff briefing and an advisory poll. Marcus files written input on a curriculum item, votes on two items, and skips the others. He spends roughly fifteen minutes a week. The board now sees something that looks more like the parent population — not just the same dozen people who can attend in person on Tuesday nights — and Marcus has a verifiable record that he weighed in.

> *Components:* Citizen Dashboard, Civic Hub, Civic Process Plugin. *Processes:* Advisory Vote, Public Comment. *Journey Stage:* Onboarding, Sustained Engagement.

### 1.3 Lena, twenty-two, activates around housing and finds an entire ecosystem

Lena has never participated in a civic process before. She lives in a state college town and is angry about a local rent ordinance. A friend sends her a link to an advisory vote on the town hub. She creates a Civic.Social account in under two minutes, casts her vote, and writes a short comment. Within a week her dashboard begins surfacing related opportunities: a tenants' coalition hub running a deliberation on rent stabilization, a state-level public comment period on landlord licensing, a citizen assembly forming next county over on housing density. She joins the tenants' coalition hub and follows the state process. Two months in she is contributing comments, organizing a neighborhood discussion, and tracking three active processes. The platform did not radicalize her or recruit her — she came in already caring. What it did was take her one act of engagement and make every adjacent opportunity legible to her, instead of hidden behind dozens of disconnected sign-up forms.

> *Components:* Civic Identity, Citizen Dashboard, Civic Activity Feed, Civic Hub. *Processes:* Advisory Vote, Public Comment, Deliberation, Citizen Assembly. *Journey Stage:* Onboarding, Discovery, Participation.

### 1.4 Robert, retired, builds a verifiable record of civic life across years

Robert is sixty-eight and has been showing up to Board of Supervisors meetings in Floyd County for decades. He was an early adopter when the Floyd hub launched. Three years in, he can open his civic profile and see his record laid out as a set of badges — a participation badge for the green box advisory vote, one for the Flock camera vote, deliberation badges from two issue coalitions, and a longstanding membership badge from a regional environmental hub he joined after seeing it in his feed. Each badge links back to the process or organization that issued it, and any of them can be verified by anyone who wants to confirm one. The badges travel with him; they don't live on any one platform. When a regional planning commission is taking applications for a citizen advisory seat, Robert chooses which badges to share with the application — not as gatekeeping, but as evidence that he has been doing the work. Civic participation, for Robert, has accumulated into something that resembles a civic résumé without ever pretending to be one.

> *Components:* Civic Identity, Civic Profile, Civic Credentials (Badges), Citizen Dashboard, Civic Hub. *Processes:* Advisory Vote, Public Comment, Citizen Assembly, Credential Verification. *Journey Stage:* Sustained Engagement, Influence Over Time.

### 1.5 Yusuf, who had stopped voting, comes back through one specific issue

Yusuf is thirty-four. He stopped voting after the 2020 cycle — not from any single grievance, but from a slow accumulation of the sense that nothing he did mattered. A coworker shares a link to a county participatory budgeting process; Yusuf's neighborhood is on the list of areas where new investment will be allocated based on resident input. He participates because it is concrete: this much money, these specific blocks, vote on the priorities. It works. His proposal for a bus shelter at his stop reaches the final round. The process publishes a clear record of what citizens voted for and what the county funded. The next time Yusuf opens his dashboard he is more inclined to engage — not because he has been "re-engaged" by an algorithm, but because for the first time in years he saw a direct line between his input and an outcome. Re-engagement, when it happens, will not look like more notifications. It will look like more proof.

> *Components:* Citizen Dashboard, Civic Hub, Civic Process Plugin. *Processes:* Participatory Budgeting. *Journey Stage:* Discovery, Participation.

### 1.6 Brenda raises a Flock-camera question and votes on it without going to a meeting

Brenda Hatcher lives in rural Floyd County, Virginia. Word reaches her that the Sheriff's Office has been using Flock Safety license plate reader cameras. She is uneasy about it — not in a partisan way, but in a civil-liberties-plus-rural-distrust-of-surveillance way that is hard to articulate at a microphone. She would never speak at a Board of Supervisors meeting; the meetings are held on Tuesday nights in town, twenty-five minutes from her home, and the public comment format is intimidating. So a few weeks ago she did something different — she filed a short proposal on the Floyd Civic Hub asking the county to put the question to residents. Other Floyd residents endorsed the proposal until it crossed the hub admin's threshold for promotion to a structured advisory vote. Now the vote is open. Brenda opens it on her phone after dinner. The hub presents structured background — what the cameras are, how they work, who decides whether they continue, the concerns raised, the local-context note about the Sheriff's Office and the Board of Supervisors. She votes No and writes a short comment explaining what she is worried about. By the close of the vote, several hundred Floyd residents have weighed in, with a documented split and a documented set of substantive concerns. The Board now has something they did not have before: a resident-level read of where the county actually stands on a question whose answer will shape what surveillance their county looks like for the next decade. Brenda did not have to drive twenty-five minutes, speak at a microphone, or organize a coalition. She filed a proposal. She cast a vote. The infrastructure made both count.

> *Components:* Civic Hub, Civic Process Plugin, Citizen Dashboard. *Processes:* Proposal Submission, Advisory Vote, Public Comment. *Journey Stage:* Participation.

### 1.7 Tom proposes the green box site instead of waiting for someone else to

Tom Whitaker lives on the eastern side of Floyd County. The nearest fenced-in green box site, on Christiansburg Pike, is a long drive from his home. The unfenced bin closer to him keeps getting raided by bears — trash strewn across the road, animals close to homes, and the same conversation among neighbors every couple of weeks about how somebody ought to do something. Tom does not know how to start a petition, does not have a template for talking to the Board, and would not describe himself as the kind of person who organizes anything. On the Floyd Civic Hub he files a proposal: an additional fenced-in dumpster site at the old Green Box location on North Route 8. He writes three paragraphs and submits. Within ten days, residents from across the eastern half of the county are commenting on the proposal — same problem, different roads, same bears. The hub admin promotes the proposal into a structured advisory vote on expanding secure dumpster infrastructure across the county. By the time the question reaches the Board of Supervisors, Tom's three paragraphs have become a documented count of supporting residents and a rough map of where demand concentrates. The Board does not have to take Tom's word for the problem. The hub took his word and turned it into evidence.

> *Components:* Civic Hub, Civic Process Plugin, Citizen Dashboard. *Processes:* Proposal Submission, Advisory Vote, Public Comment. *Journey Stage:* Discovery, Participation, Sustained Engagement.

### 1.8 Diane gets the Floyd County board meeting she missed in three minutes the next morning

Diane Mason has cared about Floyd County government for years. The county website posts Board of Supervisors meeting minutes a week or two after each biweekly meeting, in PDFs nested behind a Wix calendar that few residents can navigate efficiently. For most of the last decade, staying current has required Diane to remember to check, find the right page, download the PDF, and skim it for anything that might affect her. She stopped doing it. She subscribes to the Floyd Civic Hub's daily digest. Now, the morning after each Board meeting, an AI-generated summary lands in her inbox: agenda items, decisions made, votes taken, the substance of public comment, links back to the source recordings and the official PDF minutes once those post. The summary is reviewed and approved by the hub admin before it is sent. Diane reads it in three minutes over coffee. When something matters to her, she clicks through to the full record. Her relationship with her county government, after years of friction-induced disengagement, is something she can sustain again — not because she has more time, but because the infrastructure finally respects how little time she has.

> *Components:* Citizen Dashboard, Civic Activity Feed, Civic Hub. *Processes:* Meeting Summary, Announcement. *Journey Stage:* Discovery, Sustained Engagement.

### 1.9 Aisha checks her state rep's profile before the primary

Aisha Martin lives in a Virginia state house district where the primary is two weeks away. She has half an hour over lunch to figure out who to vote for. The candidates' campaign websites all sound the same — broad commitments, no specifics. The local newspaper endorsement is paywalled. Mailers are mailers. She opens the incumbent state representative's public profile on her phone. The profile lists the badges the representative has accepted from issuing organizations — an "Open Primaries" pledge from a national reform coalition, a "Constituent Communication Standard" badge from a good-government nonprofit, a participation badge for a regional rural broadband working group, and a declined-with-explanation note for a "Term Limits Pledge" she chose not to sign. Each badge links back to the organization that issued it, the criteria the representative agreed to, and the date she accepted it. Anyone who wants to verify a badge can do so. Aisha clicks through three of them. She doesn't agree with every position the representative has staked out, but she can now see them — not in campaign-website prose, but as a list of public commitments other organizations have testified to. She votes accordingly. The infrastructure took thirty minutes that would have produced a coin-flip-quality decision and turned it into a decision she could actually defend.

> *Components:* Civic Profile, Civic Credentials (Badges), Civic Identity, Citizen Dashboard. *Processes:* Credential Verification, Endorsement. *Journey Stage:* Discovery, Participation.

---

## 2. Hub Admins / Civic Organization Leaders

The operators of Civic Hubs. They run the spaces where civic life actually happens at the community level — and Civic.Social succeeds only if running a hub is genuinely tractable for the kinds of people and organizations that already do this work.

### 2.1 Janelle stands up the Agora neighborhood hub in a weekend

Janelle is the volunteer board secretary for the Agora neighborhood association. Their existing setup is a Facebook group plus an aging mailing list. The Facebook group has worked, in the loose sense that members are nominally present in it, but year after year the same problems recur: civic posts disappear into an algorithmic feed beside clickbait and unrelated content; the group has no way to verify that someone voting in a poll actually lives in the neighborhood; ads run against discussions about a proposed traffic-calming measure; and any process more structured than a basic poll — a ranked choice across multiple projects, a deliberation with separate proposal and vote phases — is simply unavailable. After the association votes to try Civic.Social, Janelle stands up the hub from a hosted operator template in a weekend. She spends most of the time on community-specific work: configuring membership rules so only verified residents of the four-block area can vote on neighborhood matters, drafting welcome content, and importing the list of upcoming items the board wants member input on. By the second meeting she runs a structured ranked-choice poll on which three projects to prioritize for the year — a process the Facebook group simply could not host. Two hundred and twelve of three hundred and forty households participate. The board, for the first time, has a defensible record of community priorities to bring to the city council.

> *Components:* Civic Hub, Civic Identity, Civic Process Plugin. *Processes:* Advisory Vote, Proposal Submission. *Journey Stage:* Onboarding, Participation.  
> *Adopting organization:* Neighborhood Association — gains structured polling and verified-resident eligibility that consumer social platforms cannot host, without ongoing platform maintenance.

### 2.2 David runs a regional environmental coalition across six counties

David coordinates a regional watershed coalition spanning six counties in southwest Virginia. The coalition has always been hobbled by the same problem: it convenes engaged residents from each county, but it has no infrastructure for cross-county participation that respects each county's autonomy. With Civic.Social, the coalition operates an Issue Network Hub federated with the six county hubs. Members in each county authenticate once and see coalition-wide processes — a regional comment campaign on a proposed industrial permit, a deliberation on watershed monitoring priorities, a coordinated petition to the state environmental agency — alongside the local processes from their own county. The coalition no longer has to rebuild a participation pipeline for every campaign; the pipeline is the federation.

> *Components:* Civic Hub, Civic Activity Feed, Civic Identity. *Processes:* Public Comment, Deliberation, Petition. *Journey Stage:* Sustained Engagement, Influence Over Time.  
> *Adopting organization:* Regional Issue Coalition — federates participation across member jurisdictions without rebuilding a campaign pipeline for each one.

### 2.3 Reyna, a community college civic engagement coordinator, runs a campus hub

Reyna directs civic engagement programming at a community college. Each year she fights the same battle: the students who participate in her programming are real, but the proof of their participation evaporates the moment they leave campus. With a campus Civic Hub, students who enroll in her civics seminar participate in structured deliberations and advisory processes through the hub. Their participation is recorded against their portable Civic Identity. When they transfer to a four-year school, register to vote in a new county, or apply for a civic fellowship, that record travels with them. Reyna's programming stops being a one-semester experience and starts being the front door to a lifetime of civic participation that is legible across institutions.

> *Components:* Civic Hub, Civic Identity, Civic Process Plugin. *Processes:* Deliberation, Advisory Vote. *Journey Stage:* Onboarding, Influence Over Time.  
> *Adopting organization:* Educational Institution — civic programming produces a participation record that travels with students after they leave campus.

### 2.4 Pastor Williams runs a church-led civic education hub

Pastor Williams leads a congregation in a small city. For years the church has run informal civic education sessions — voter registration drives, candidate forums, policy explainers — that lived in disconnected fliers and folding-chair conversations. The church operates a Civic Hub as an extension of that work. Members and community attendees authenticate, follow walkthroughs of upcoming ballot measures, participate in structured discussions, and see relevant updates from county and state hubs surface alongside the church's own civic content in the church hub feed. The church does not become a political actor; it becomes what it has always tried to be — a space where civic life is taken seriously — with infrastructure that finally matches the seriousness of the work.

> *Components:* Civic Hub, Citizen Dashboard, Civic Identity. *Processes:* Deliberation, Town Hall. *Journey Stage:* Onboarding, Sustained Engagement.  
> *Adopting organization:* Faith Community — civic education programming gains infrastructure proportional to its institutional commitment.

### 2.5 A union local runs contract deliberation through a hub

A regional service workers' union local runs contract deliberation through a Labor Union Hub. Members authenticate using credentials that verify membership in the local chapter. They review the proposed contract through structured documents, comment, and ultimately vote on ratification. The union retains full sovereignty over the hub — Civic.Social is infrastructure, not a stakeholder — and the deliberation produces a record of member engagement that strengthens the legitimacy of the ratification result. When a sister chapter in another state runs a parallel deliberation a year later, they do not start from scratch; they fork the process plugin the previous chapter refined.

> *Components:* Civic Hub, Civic Identity, Civic Process Plugin. *Processes:* Deliberation, Advisory Vote. *Journey Stage:* Participation, Sustained Engagement.  
> *Adopting organization:* Labor Union Local — member-verified ratification produces an auditable record, and refined deliberation patterns are forkable to sister locals.

### 2.6 The Open Districts coalition issues a verifiable pledge candidates can sign

The Open Districts coalition is a reform organization advocating for ranked-choice voting in state and local elections. For years their work has involved candidate questionnaires, op-ed campaigns, and a manually maintained list of candidates who had publicly committed to support the reform — a list built from press clippings and personal correspondence, with no way for a voter to verify the underlying commitment three years later, and no way for the coalition to update the list at scale when candidates changed positions. The coalition becomes an issuing organization on Civic.Social. They define a badge — "Supports Ranked Choice Voting at State Level" — with public criteria documenting what the commitment means, what evidence supports it, and what would constitute a violation. Candidates and elected officials who commit publicly are offered the badge; recipients accept it, or decline with an explanation, and accepted badges appear on their public profiles. Three years later, a voter looking at a state senator's profile can see whether that senator accepted the Open Districts badge, when, and what the specific commitment was — and the senator can be held to it because the criteria are public. The coalition gains something it has not had before: a durable mechanism for turning reform commitments into a legible, verifiable record. Voters can audit alignment on their own; the coalition itself can hold candidates and officials to the criteria they signed onto; and the artifact survives news cycles, election cycles, and staff turnover. The advocacy work has always existed. What's new is the leverage.

> *Components:* Civic Profile, Civic Credentials (Badges), Civic Identity, Civic Activity Feed. *Processes:* Credential Issuance, Endorsement. *Journey Stage:* Onboarding, Sustained Engagement, Influence Over Time.  
> *Adopting organization:* Reform Coalition / Issuing Organization — long-standing advocacy commitments become verifiable, portable artifacts that outlast news cycles and let voters audit candidate alignment without trusting the coalition's word.

### Why not just use Facebook groups?

The honest version of this question is the one funders and skeptics will ask: a Facebook group is free, ubiquitous, and most members are already there. Why build new infrastructure?

Four reasons, each structural rather than cosmetic.

**The platform's incentives run against the work.** Facebook is built to maximize advertising revenue, which means optimizing for engagement time and emotional response. Civic conversations about parking, dumpster placement, or rent stabilization are exactly the kind of low-engagement content the algorithm suppresses. The work organizers most want to surface is the work the platform structurally hides.

**There is no way to verify participation.** A Facebook group cannot answer the question "is this person actually a resident of the four-block area we're voting on?" Anyone in the group can vote in any poll, and there is no audit trail to refer to afterward. Civic decisions need eligibility verification; consumer social platforms have no concept of it because their commercial model does not require it.

**Structured processes do not exist.** A Facebook poll is one option per question. A neighborhood association running a ranked-choice prioritization, a participatory budgeting cycle, a deliberation with separate proposal and vote phases, or a structured comment period has no Facebook equivalent. The platform's expressive primitives are post, comment, like, react. The civic primitives are vote, propose, deliberate, decide. The two languages do not overlap.

**The data does not belong to the community.** When a neighborhood association runs a year of decisions through a Facebook group, the record of those decisions is hostage to a platform whose policies, algorithms, and very existence are outside the community's control. Civic Hubs are operated by the community, and hub data is portable by design — a community that outgrows one hub engine can move to another without losing its history.

A Civic Hub is not a Facebook replacement; the two products solve different problems. But Facebook groups have always been a compromise — never a good fit for civic life.

---

## 3. Elected Officials

People currently holding office at any level — school board members, city councilmembers, state legislators, mayors, members of Congress. Civic.Social changes their job by giving them a higher-fidelity, more representative read of constituent input than email lists, town halls, or social media — and by making their responsiveness visible.

### 3.1 Councilmember Patricia Diaz reads a representative cross-section

Patricia Diaz represents the third district of Athens on city council. The proposed downtown rezoning has been lighting up her inbox for weeks, mostly from a familiar set of about thirty constituents. She posts a structured advisory question to the Athens municipal hub: three rezoning options, with summaries of trade-offs, open for two weeks. Four hundred and eleven residents weigh in. The result surfaces something her inbox could never tell her: opposition is concentrated in two blocks adjacent to the rezoning area and is driven by specific concerns about parking and a specific intersection — not blanket opposition. She votes for a modified option that addresses the specific concerns and posts publicly on the hub explaining what input shaped her decision. The vote opponents who showed up loudest are still upset, but the broader district sees a councilmember who listened to them, not just to the loudest voices.

> *Components:* Civic Hub, Civic Activity Feed. *Processes:* Advisory Vote, Public Comment. *Journey Stage:* Participation, Influence Over Time.

### 3.2 State Representative Allen Park structures testimony intake

Allen Park represents a suburban district in the state house. Bills move fast, and the existing public comment process — a clerk's email, a one-day in-person hearing, a flood of form letters — does not give her a representative read of where her constituents as a whole actually stand. What she gets is the views of those who could take a Tuesday afternoon off to come to the capitol, weighted by whichever advocacy organization mobilized hardest — useful information, but not a high-fidelity picture of district sentiment. Her office runs structured comment intake on the state legislature's hub for bills she sponsors or is the floor manager for. Constituents see the bill in plain language with a structured comment form, and the hub aggregates and tags responses. By the time the bill comes to floor debate she has a usable distribution of constituent positions and a representative sample of testimony — including from constituents who would never have shown up to the capitol on a Tuesday afternoon.

> *Components:* Civic Hub, Civic Process Plugin. *Processes:* Public Comment, Town Hall. *Journey Stage:* Sustained Engagement.

### 3.3 School Board Member Maya Thompson hears from parents who can't attend

Maya is a first-term school board member. She ran on the premise that the existing public-comment-at-meeting model was structurally biased toward the parents who could afford the time. With the district hub she opens a structured comment window for every agenda item, opening five days before each meeting. Comments are tagged by item and visible in advance to all board members. By her second year, the board has shifted its discussion patterns: less time spent rehashing what the same dozen attendees said at public comment, more time engaging the broader pattern of input from across the district. Maya is not more popular as a result — some of the most active in-person commenters resent that their voices are now one input among many — but her board has more representative input to work with, and that is what she ran for.

> *Components:* Civic Hub, Citizen Dashboard. *Processes:* Public Comment, Advisory Vote. *Journey Stage:* Onboarding, Sustained Engagement.

### 3.4 A mayor running for re-election surfaces a verifiable record of responsiveness

Three years into office, the mayor of a small city is running for re-election. The campaign has historically meant glossy mailers and a few endorsements. With Civic.Social, the mayor's office has been running advisory processes and publishing decisions-with-rationale on the city hub for the entire term. Her public profile carries a year-by-year record: every advisory question she ran, every comment period, every decision she made and what input shaped it. Alongside that record, her profile displays the badges she has accepted from issuing organizations during her term, including the Open Districts coalition's ranked-choice voting pledge and a climate-action commitment from a national mayors' alliance focused on municipal carbon reduction. Voters who want to confirm a badge's issuer or the criteria behind it can do so in a click. The record is not curated by her campaign; it is emitted by the hub as standard civic events. Her opponent has nothing comparable to point to, because his predecessor did not operate this way. Responsiveness, for the first time, becomes a thing voters can verify rather than just hear claimed.

> *Components:* Civic Hub, Civic Activity Feed, Civic Identity, Civic Profile, Civic Credentials (Badges). *Processes:* Advisory Vote, Public Comment, Town Hall, Endorsement, Credential Verification. *Journey Stage:* Influence Over Time.

---

## 4. Candidates

People running for office but not yet holding it. Their needs differ from elected officials' in one key way: they cannot point to votes they have cast, so they need a way to demonstrate listening and responsiveness that the existing system makes nearly impossible.

### 4.1 Tara Mitchell builds a record of listening before she's in office

Tara is challenging Patricia Diaz in the next council cycle. Historically, a challenger has had no scalable way to demonstrate she is listening — town halls reach dozens, speeches are one-directional. Tara stands up a candidate hub on Civic.Social. She posts five issue questions to the third district and invites structured input. Over six months she collects thousands of constituent positions and questions, and she publishes responses to each one — what she heard, where she agrees, where she sees it differently and why. By election day, voters who care about responsiveness can compare Patricia's three years of decisions on the city hub to Tara's six months of structured listening on her candidate hub. The race is about substance, not yard signs. Tara loses, narrowly, but the model exists. Two cycles later, candidates in three counties are using it.

> *Components:* Civic Hub, Civic Activity Feed. *Processes:* Public Comment, Town Hall, Advisory Vote. *Journey Stage:* Onboarding, Influence Over Time.

### 4.2 Jordan Kim, a first-time state house candidate, runs a digital town hall series

Jordan is twenty-nine, a first-time candidate for state house in a swing district. The traditional candidate forum format reaches the same political insiders every cycle. Jordan runs a digital town hall series through a candidate hub: ten sessions over three months, each on a specific issue, with structured pre-submitted questions and a recorded response. The hub publishes a transcript and tags each question against the issues. By the end of the series, Jordan has the most concrete and verifiable issue platform of anyone in the race — not a list of bullet-point positions, but a record of the specific concerns raised by specific constituents and his specific responses. Voters who do their own research find something they have rarely been able to find before: a candidate whose positions are visibly downstream of having actually listened.

> *Components:* Civic Hub, Citizen Dashboard. *Processes:* Town Hall, Public Comment. *Journey Stage:* Participation, Sustained Engagement.

### 4.3 Hank Reeves runs cross-partisan listening sessions in a rural district

Hank is running for county supervisor in Floyd County. The district is rural, politically mixed, and exhausted by partisan posturing. He uses a candidate hub to run a series of structured listening sessions on local issues — the school consolidation question, the broadband expansion, the property tax base. The sessions are explicitly cross-partisan: participants self-identify their leanings, and the resulting reports show areas of consensus alongside areas of genuine disagreement. The reports surface that the actual disagreement on most local issues is much narrower than the political affiliation of participants would suggest. Hank wins. The record from his listening sessions becomes the foundation for how he governs — and his successor, when she runs four years later, knows the bar.

> *Components:* Civic Hub, Civic Process Plugin, Civic Activity Feed. *Processes:* Deliberation, Town Hall, Public Comment. *Journey Stage:* Onboarding, Influence Over Time.

---

## 5. Government Staff

The public servants who actually operate jurisdiction-level civic processes — clerks, public engagement officers, town managers. Their needs are about legitimacy, auditability, and reduced operational burden, not narrative. Government processes do not require government-operated hubs to run on; the stories below show three patterns — a city running its own hub, a county partnering with a regional civic-engagement nonprofit, and a small town contracting hub-as-a-service from Civic.Social. The stories illustrate the value that materializes once a hub is in place; the procurement, security review, accessibility, and public-records compliance work that gets a jurisdiction to that point is real and is outside the scope of these narratives.

### 5.1 Kim Pham, a city clerk, finally runs participatory budgeting

Kim is the city clerk in a mid-sized municipality. The city already operates a Civic Hub that it stood up two years ago for announcements, advisory votes, and the public comment periods around its capital improvement plan. The mayor has wanted to run participatory budgeting for as long as Kim has been clerk — most peer cities Kim talks to have wanted it too — but the procurement math has never worked. Commercial SaaS platforms exist but their pricing puts them squarely in procurement-decision territory for a city this size — annual subscriptions, plus implementation, plus the ongoing work of selecting and managing a separate vendor relationship. Open-source platforms like Decidim and Stanford's PB tool are free as software but require internal technical capacity to deploy and maintain that a small clerk's office does not have. A custom build is more expensive than either. None of these paths is impossible. Each is a discrete decision — a new vendor, a new contract, a new system to learn, a new line item in a budget already pulled thin by inflation and post-pandemic staffing gaps. None has cleared the council's priority list in five years. With Civic.Social, the question changes shape. The city's IT department deploys the participatory budgeting plugin from the Civic Process Plugin marketplace onto the existing hub, and Kim configures and runs the process itself. The plugin handles proposal intake, eligibility verification through Civic Identity, deliberation, voting, and result publication — and emits standardized civic events her communications team can syndicate without custom integration work. The marginal cost of adding the capability is hosting overhead and a few weeks of staff configuration time, small enough that the council approves it inside the year's existing technology budget rather than as a new line item. The city is running participatory budgeting this fall for the first time in its history. The clerks who run the process have a plugin documented for clerks across dozens of jurisdictions, not a one-off build they have to maintain alone.

> *Components:* Civic Hub, Civic Process Plugin, Civic Identity. *Processes:* Participatory Budgeting. *Journey Stage:* Onboarding, Sustained Engagement.  
> *Adopting organization:* Municipal Government — adds participatory budgeting as a marginal-cost capability on an existing hub, instead of as a separate vendor selection, technical build, or new procurement decision the council had never prioritized.  
> *Hub operator:* Municipal Government (operates its own hub in-house).

### 5.2 A county public engagement officer runs a formal comment period

A public engagement officer in a county planning department needs to run a thirty-day comment period on a draft comprehensive plan. State statute requires specific procedural elements: notice, written comment intake, response to substantive comments. Historically this was a PDF posted to the county website and an email address. The county does not operate its own hub — but a regional civic-engagement nonprofit operates a community hub serving the county and three neighboring jurisdictions, and the planning department runs the comment period through that hub via partnership. Residents authenticate with verified address-of-record, file comments tagged by chapter and section, and receive a structured response from county staff. The hub emits civic events that constitute the public record, satisfying the statutory requirements. The audit trail is automatic. The county engaged a structured public process without procuring, operating, or funding new infrastructure of its own. Compliance staff, who used to dread this process, now treat it as routine.

> *Components:* Civic Hub, Civic Identity, Civic Process Plugin. *Processes:* Public Comment. *Journey Stage:* Onboarding, Sustained Engagement.  
> *Adopting organization:* County Government — statutory comment-period requirements satisfied with automatic audit trail and structured response capacity, without the county taking on hub-operation responsibilities.  
> *Hub operator:* Regional civic-engagement nonprofit serving the county and three neighbors.

### 5.3 A small town gets engagement infrastructure it could never have afforded

Maria Reyes is the town manager of a 9,000-resident community in rural Virginia. State law requires public hearings on land-use changes, the annual budget, and certain capital projects. The historical pattern has been the standard small-town one: bulletin-board notices, a Tuesday-evening hearing in the town hall meeting room, fifteen or twenty residents who always show up, and a public record that consists of the meeting minutes plus whatever written comments were physically delivered to the town clerk. The same handful of voices dominate; everyone else finds out about decisions after they're made. The commercial civic-engagement platforms — CitizenLab, Polco, Granicus EngagementHQ — start at price points well above the town's entire annual technology budget. A custom build is unthinkable. There has been no path. Civic.Social, operating a tiered hub-as-a-service offering specifically priced for small jurisdictions, stands up the town's hub at a price the town can absorb without procurement-level deliberation — orders of magnitude below what commercial vendors charge for similar functionality, sized to fit inside a small-town technology budget. Residents get a structured comment window that opens before each town council meeting, an advisory-vote interface for the questions the council wants public input on, and a digest that lands the morning after each meeting summarizing what was decided. Participation rates triple within the first year. Maria, who has been doing this job for fourteen years, has never had a public record that looked anything like this — and the town is now running structured engagement on infrastructure that, until this option existed, was simply not available at any price the town could pay.

> *Components:* Civic Hub, Civic Process Plugin, Civic Activity Feed. *Processes:* Public Comment, Advisory Vote, Meeting Summary, Announcement. *Journey Stage:* Onboarding, Sustained Engagement.  
> *Adopting organization:* Small Town / Local Municipality — gains structured public engagement at a price point sized for small jurisdictions, where commercial platforms have historically been priced out of reach.  
> *Hub operator:* Civic.Social, as a hub-as-a-service provider on a small-jurisdiction tier.

---

## 6. Civic Tool Builders

Developers and organizations building new process plugins or interfaces on top of the protocol. They are what keeps the ecosystem alive and growing past the initial set of process types Civic.Social ships. Civic.Social is enabling an open ecosystem — not delivering a closed product — and what emerges from that openness, over time, is a vibrant civic operating system that no single team could have built alone.

### 6.1 Sam Okafor, an indie developer, ships a citizen assembly plugin

Sam is an independent developer with a background in deliberative democracy research. The Civic Process Plugin Framework specifies how a process must be built to run inside a Civic Hub. Sam builds a citizen assembly plugin — a three-phase process with stratified random selection, structured deliberation, and final report — against the plugin spec. He tests it against the reference Civic.Social hub engine and against Bonfire and Decidim adaptations. He publishes the plugin under an open license. Within eight months it is running in eleven hubs across four states. Sam does not have to build a platform, find users, or stand up infrastructure. He builds a process. The hubs do the rest.

> *Components:* Civic Process Plugin, Civic Hub. *Processes:* Citizen Assembly. *Journey Stage:* Onboarding, Influence Over Time.

### 6.2 An established civic-tech nonprofit ports its tool to the protocol

A civic-tech nonprofit has run a respected deliberation tool for a decade. It is widely used in academic and government settings, but every deployment is a custom integration — separate authentication, separate data export, separate operational burden. The nonprofit ports the tool to expose a Civic.Social-compliant interface. Existing customers see no change to the tool itself, but new deployments inherit the entire ecosystem: identity, event emission, dashboard discoverability, federation. The nonprofit's deployment cost drops; their integration support burden drops more; and their reach expands to hubs they would never have sold into directly. Other long-standing civic tools begin doing the same.

> *Components:* Civic Process Plugin, Civic Hub, Civic Identity. *Processes:* Deliberation. *Journey Stage:* Sustained Engagement, Influence Over Time.  
> *Adopting organization:* Civic-Tech Nonprofit — existing customers see no disruption, new deployments inherit ecosystem reach, and integration support burden drops.

### 6.3 An academic research lab builds deliberation analytics

A university research lab studies the quality of deliberative processes. Historically, gathering data has required individual research agreements with each platform or municipality. Through the standardized Civic Event Specification, the lab can — with appropriate consent and governance — analyze events from any hub that publishes them, tracking the structure and quality of deliberation across jurisdictions, demographics, and process types. Their findings feed back into the plugin marketplace: refined process types, improved facilitation patterns, evidence-based defaults. Civic.Social becomes the substrate that allows civic science and civic practice to inform each other at the speed they should always have moved.

> *Components:* Civic Activity Feed, Civic Hub. *Processes:* Deliberation, Citizen Assembly, Public Comment. *Journey Stage:* Influence Over Time.  
> *Adopting organization:* University Research Lab — cross-jurisdictional deliberation analysis becomes possible without per-platform research agreements.

---

## 7. Network-Effect Vignettes

User stories work for individual personas. Network effects, by definition, do not — they require multiple protagonists. The vignettes below follow a single thread across roles to show how individual actions compound into something the ecosystem could not produce in any other way.

### 7.1 The broadband thread: from one resident to a state floor vote

Sarah, in Athens, casts a vote on her county hub on a proposed rural broadband expansion plan. The county hub publishes the result as a standardized civic event. A regional rural broadband coalition — operating an Issue Network Hub spanning four counties — ingests the event and surfaces it to its members alongside parallel results from three other counties. Allen Park, the state representative, is running structured public comment intake on a state-level rural broadband bill she is sponsoring; her intake hub ingests the coalition's aggregate brief and tags it as constituent-relevant input. Her public profile already shows the participation badge she accepted from the rural broadband coalition the previous year, making the alignment between her sponsored bill and the citizen input she is collecting visible at a glance. A challenger candidate in Sarah's district cites the aggregate result, by name, in a debate, and points to the gaps in his opponent's profile where badges from rural-issue coalitions are conspicuously absent. None of this required Sarah to do anything beyond cast her vote on her county hub. The compounding happened because the layer beneath all of them was the same.

> *Components touched:* Civic Hub, Civic Activity Feed, Civic Identity, Civic Profile, Civic Credentials (Badges), Civic Process Plugin. *Processes touched:* Advisory Vote, Public Comment, Credential Verification. *Personas touched:* Citizen, Hub Admin, Elected Official, Candidate.

### 7.2 A generic civic plugin proves itself in Floyd County and travels across hub boundaries

A generic civic-process plugin — proposal intake, structured deliberation, advisory vote, and civic brief generation — built by Sam (§6.1) against the Civic Process Plugin Framework, sits in the plugin marketplace. Floyd County's hub admin has enabled the plugin in the county hub, making the proposal-and-vote capability available to residents. Brenda Hatcher (§1.6) uses it to propose an advisory process on whether the Sheriff's Office should continue using Flock Safety license plate reader cameras. Other Floyd residents endorse the proposal until it crosses the threshold the hub admin set for promoting community-proposed issues to a county-wide advisory vote, and the vote opens. Brenda's own vote, alongside several hundred others, becomes part of the result. The process unfolds in public, and because hubs federate, residents in neighboring counties and other hubs can follow it through their own dashboards — some out of curiosity, some because their own jurisdictions are about to face the same Flock contract decision in the same eighteen-month window. When Floyd's brief is delivered to the Board of Supervisors and the Sheriff's Office and published openly, hub admins in other jurisdictions reach out to ask Floyd's admin how it worked. The plugin spreads from there, and it spreads on its own merits — not as a Flock-camera plugin, but as a generic proposal-and-vote process that produces a defensible civic brief and that any hub can enable. Within two years, more than two hundred jurisdictions are running structured civic processes on the same plugin — some on surveillance-technology questions like Floyd's, many on completely different local issues. Floyd's process became the demonstration that the plugin could carry real weight on a contested issue; what spread was the plugin itself. Civic.Social did not build the plugin. It built the conditions under which one well-designed plugin, demonstrated on one rural county's question proposed by one resident, could be picked up and used by jurisdictions and citizens far beyond it.

> *Components touched:* Civic Hub, Civic Process Plugin, Civic Activity Feed. *Processes touched:* Proposal Submission, Advisory Vote, Public Comment, Deliberation. *Personas touched:* Citizen, Hub Admin, Civic Tool Builder, Government Staff, Elected Official.

### 7.3 The youth assembly that convenes across four states

A youth-led organization runs a citizen assembly on climate adaptation through Sam's plugin (§6.1) on its Issue Network Hub. The assembly draws stratified random participants from the youth populations of forty-one hubs across four states. Lena (§1.3), now twenty-four and active across a half-dozen hubs, is selected. Maya Thompson (§3.3), as a school board member, watches the assembly's published deliberation and references its findings in a board discussion three months later on climate education. The assembly's final report is ingested by two state-level public comment processes as registered input. None of this required convening physical bodies. None of it required the youth organization to build infrastructure. Network effects, here, look like a generation finding it easier than the previous one to act collectively at the scale their problems actually exist at.

> *Components touched:* Civic Hub, Civic Process Plugin, Civic Identity, Civic Activity Feed. *Processes touched:* Citizen Assembly, Public Comment, Deliberation. *Personas touched:* Citizen, Hub Admin, Elected Official, Civic Tool Builder.

---

## Closing Note

The use cases above are illustrative, not exhaustive. The point of building shared infrastructure rather than another civic application is that the use cases that matter most — the ones we will build the next decade of civic life around — have not been imagined yet. Sam's plugin will be followed by plugins no one has thought to build. The Floyd County brief will be followed by formats no one has conceived. The youth assembly will be followed by deliberative configurations no one has tried.

What this document captures is the floor: the participation patterns that already exist in the world, made tractable, navigable, and compounding for the people who do this work. The ceiling is open.

---

**civic.social** | contact@civic.social
