# how does closebot work: personas, job flows and sources explained, plus what each plan actually costs

Most explanations of CloseBot open with the pitch: agentic conversational AI that qualifies leads and books appointments. That tells you what the product claims to do, not how it works. If you're the person who has to build and then babysit the thing, the useful questions are narrower. What do I connect first? What am I actually configuring? Where does a lead's message enter, and where does the appointment land?

CloseBot's own help docs compress the answer into one line: it connects **sources** to **agents**, and an agent is a **job flow** (the job to be done) paired with a **persona** (the personality doing the job). A source is where conversations arrive. The persona controls how replies sound and which model writes them. The job flow controls what the agent is trying to accomplish and when it stops.

The rest of this article is that sentence, unpacked, with the limits attached.

## The short version, before the detail

If you want the mental model in one paragraph: you connect a CRM as a source, you attach a persona so the agent doesn't sound like a press release, you build a job flow that collects the details you care about and pushes a qualified lead onto a calendar, you feed it a knowledge library so it stops guessing, and you set filters so it only answers where and when you want it to. Then you test it and publish.

CloseBot's own description of its onboarding says registering, adding a source, and connecting that source to a starter agent takes under a minute. The starter agent is created for the industry you pick at signup and behaves as a simple Q&A bot. It won't qualify or book anything until you build the logic yourself.

## Sources are CRMs, not channels

This is the part people get wrong most often, and it changes the whole cost picture.

CloseBot does not connect to Instagram, WhatsApp, or SMS directly. Its documentation is blunt about it: CloseBot "piggybacks off of any channel that your CRM supports." The source you add is a CRM connection. The supported options are HighLevel sub-account, LeadConnector, HubSpot, and a webhook for anything custom.

What that means in practice:

- If your GoHighLevel inbox is already receiving Instagram DMs, Messenger messages, SMS, live chat, and email, an agent working that source can reply across all of them.
- If your leads arrive as Instagram DMs and you don't run a CRM, there is no channel to connect. You would adopt a CRM first, route the channels into it, then add CloseBot on top.
- Channel mechanics like comment-to-DM triggers or story-reply funnels live in the CRM, not in CloseBot. The agent answers conversations; it doesn't create them.

Connecting a HighLevel sub-account is a standard OAuth flow. You open Sources, click to add a new source, choose HighLevel Sub-Account, approve the app permissions in the window that pops up, select the sub-account, come back to CloseBot and tick the box allowing CloseBot to create and update fields, then click Add Source. That checkbox is doing real work. It's what lets an agent write qualification data into contact records rather than just chatting pleasantly and leaving you to copy it across by hand.

👉 [Connect a source on the free plan and watch the builder populate itself](https://app.closebot.com/a?fpr=li87)

## The persona decides how it sounds

A persona is the voice layer. CloseBot ships a default persona you can use immediately, and there's some thought put into the response mechanics rather than just "be friendly."

Three settings shape how the texting feels:

- **Extra delay** adds a pause before replies, which matters on SMS. The docs also note it lets the agent answer a double-text with a single message instead of two.
- **Typos** occasionally inserts a misspelling, then sends the correction. It's a live-chat realism trick, not a bug.
- **Message splitting** breaks long answers into separate bubbles.

Typos and splitting are automatically set to 0% when the reply channel is email, because nobody wants a three-bubble email. You also get a free-form "how to respond" field and up to three voice-style words. The docs include a useful warning here: the model is already friendly and kind, so stacking more of those words makes it cloying.

The persona is also where you choose your AI provider and the order of fallbacks. Different providers respond differently, and CloseBot automatically picks the model per provider. Fallbacks matter more than they sound. If your primary model has a bad afternoon, you don't want every live conversation stalling with it.

One practical detail: a job flow can't be published or tested without a persona attached.

## Job flows are objectives, not scripts

A job flow is the agent's job description, built on a drag-and-drop canvas. Every flow starts at a START node and follows the arrows.

The nodes you'll actually use:

- **Objectives** collect specific data. The docs make a small but important point about phrasing: if you want a name and an email, put both in one objective so the agent asks for them in a single message instead of a two-message interrogation.
- **Conversation** nodes let the agent sit and chat, answer questions, and hold the thread.
- **Custom scenarios** are jump-outs. If the contact signals they want to book, the agent jumps from the main flow into a sub-flow, does the specific thing, then returns to where it left off on the main flow. You can also tell it where to re-enter.
- **Booking nodes** connect the calendar. For GoHighLevel you either pick a calendar from a dropdown or paste the calendar ID.

One thing to get right on day one: you choose the workflow type when you create it, and the docs state that it cannot be changed later. Renaming is easy; retyping is not.

## Tools are what make it agentic

Without tools, an agent just talks. Tools let it act outside CloseBot, and they come in three flavors.

**Source-specific tools** are prebuilt for HubSpot and HighLevel, so the agent can read and write to the CRM it's already connected to.

**General tools** are CloseBot's own. The example worth knowing is the check appointment availability tool, which can look across every calendar it can see. You can narrow that inside the node instruction, so one agent can be told to check only certain calendars depending on the conversation.

**Custom tools** let you tie in third-party software, and they're available on paid plans.

Tool access can be restricted per node, which is the sensible way to stop an agent doing something clever halfway through a qualification sequence. You can also name a specific tool inside a node's instruction to steer when it gets used. The docs are clear that limiting tools isn't just hygiene, it also helps control cost. CloseBot says anything you can do in the UI can also be done through the API, so custom tools and re-engagement jobs can be scripted if you'd rather not click.

## Knowledge base and Smart FAQ: how it avoids making things up

This is the part that decides whether you keep a client.

You upload knowledge documents and attach them to a source, so the agent has something to answer from. Storage is billed on the text content size of the upload, not the file size. The docs put that in perspective: 1 MB of text is roughly a thousand pages.

Smart FAQ is the safety valve. When an agent hits a question it can't answer from the knowledge base or its prompting, it tells the contact it doesn't know, and files the question as a Smart FAQ item. The account owner gets notified, answers it once, and that answer becomes a knowledge document attached to the right source. The docs note Smart FAQ is checked in every conversational action except booking, and that switching it on visibly reduces hallucination because of the prompting it inserts.

There's also an API endpoint for re-engaging every lead who asked the question once you've answered it, which is a decent trick for dead leads: someone asked about a service you didn't offer yet, you add it, and the agent goes back to everyone who asked.

Smart FAQ items are source-specific, which matters if you run multiple client sub-accounts. Each one gets its own queue.

## Turning it on for real leads

Two controls decide where the agent is allowed to work.

**Source filters** set which channels, tags, and list rules the agent should be ON or OFF for, per source. **Tags** are the runtime switches: a ready-to-book tag can send the scheduling link, a dnd tag stops the agent replying, and a lead-qualified tag hands the conversation to sales.

After that it's test, publish, watch. You can prove conversations out in the testing portal before anything goes live, roll back what you don't like, and pause the AI on an individual conversation for a human takeover. CloseBot's site also describes a Smart FAQ flag when the agent can't answer, which is the same loop described above.

## Where the mechanism stops working

The same architecture that makes CloseBot predictable also defines its edges. Worth knowing before you promise anything to a client:

- **Text only.** No voice agent. If your offer needs phone calls, this isn't the product.
- **No direct channel connections.** No standalone Instagram or WhatsApp integration, no comment-to-DM triggers.
- **A message is a segment, usually.** One message equals one segment, except when you switch on the Agent Node's unlimited potential with lots of tools and unlimited instruction size, where billing moves to token costs and a single message can consume several segments. Heavy agents cost more than the headline rate suggests.
- **No refunds.** CloseBot states this plainly. You get a free-forever plan under 100 messages a month and a 7-day trial of any paid plan before you're billed, and that trial is where your testing needs to happen.
- **Your CRM bill is separate.** CloseBot runs on top of a CRM you pay for, so the total cost of ownership is CloseBot plus whatever your CRM charges.
- **The message rate for agencies has two published figures.** The current plans page FAQ lists $0.012 per rebillable message; an older help-center article still cites $0.006. If you're building margin math, confirm the rate with their team before you quote a client.

## Pricing: what it costs once it's running

CloseBot splits its pricing into a business track and an agency track under the same "Core" heading, plus a free tier and a custom tier. Here's the full picture from the plans page.

| Plan | Best for | What you get | Price | Billing | Get started |
| --- | --- | --- | --- | --- | --- |
| **Free** | Testing the builder, or genuinely low volume | 100 monthly messages, 1 agent, 1 user seat, 1 MB upload storage, unlimited account connections | $0 | Free forever while you stay under 100 messages; overage billed at $0.08/message | [Start on the free plan](https://app.closebot.com/a?fpr=li87) |
| **Core (Business)** | Businesses running their own pipeline, from solo owners to teams | Message costs included in the base price, 15+ templates, 50+ extra templates on annual billing, human support, additional users at $5 per seat, add-on storage, add-on agents | $64/mo monthly, or $53/mo billed as $640/yr on annual | Month to month or annual; 7-day trial before billing | [See Business plan pricing](https://app.closebot.com/settings?tab=subscription&plan=business&toggle=monthly&fpr=li87) |
| **Core (Agency)** | Agencies building and reselling agents for clients | Re-bill all costs, white-label client portal, 15+ templates, added users and storage, unlimited agents across unlimited sources; messages listed at $0.012 each and rebillable at your own markup | $397/mo monthly; around $331/mo equivalent on annual billing | Month to month or annual; 7-day trial before billing | [See Agency plan pricing](https://app.closebot.com/settings?tab=subscription&plan=agency&toggle=monthly&fpr=li87) |
| **Growth** | Teams that need SLAs, compliance commitments, or very high volume | HIPAA compliance, quarterly audits, 99.99% priority uptime, priority support, 50+ templates | Custom quote | Negotiated directly with their team | [Ask about a custom Growth plan](https://app.closebot.com/a?fpr=li87) |

Two numbers underneath that table are worth internalizing.

First, paid business plans start with a **500-message ceiling** included in the base price. Go over it and you pay per message at a 2x overage rate drawn from a wallet. You can also raise the monthly ceiling in the plan selector, and the page notes that bulk pricing improves as you do.

Second, annual billing works out to two months free across the paid tiers. The $640/yr figure on the Core business plan against a $64 monthly rate is the arithmetic behind that.

The site's own testimonials and milestones (over a million booked appointments, 150,000 daily messages, more than a thousand agencies) are vendor-published numbers, not audited ones. On G2 the product sits at 4.8 stars across more than 190 reviews, which is the closest thing to independent signal in this category, and reviewers there tend to praise setup speed and lead handling rather than anything exotic.

## Which setup actually matches your situation

If you're a business running your own pipeline and you want qualification and booking handled: Core Business is the straightforward answer, and the free plan is enough to decide whether the builder makes sense to you.

If you're an agency selling AI setting to clients: the agency version of Core is the point of the product. Rebilling, the white-label client portal, and unlimited agents across sources are what let you turn a software line item into a revenue line. The margin control is the feature; everything else is table stakes.

If your leads live in Instagram DMs and you don't run a CRM: the honest answer is that CloseBot adds a layer you'd have to build a foundation for first. Same job, one more moving part, one more bill.

If you need a voice agent or phone-based qualification: this isn't it. CloseBot is deliberately text-only, and the docs' channel piggybacking means voice sits outside the architecture entirely.

## Common questions, answered plainly

**Does CloseBot work without a CRM?** The site lists "standalone compatible" as a capability, and there's a webhook source option. But the documented sources are HighLevel, LeadConnector, HubSpot, and webhook, so if you have no CRM and no way to send conversations in, there's nothing for the agent to respond to.

**Can one agent book to different calendars?** Yes. Booking nodes let you select a calendar, and the docs cover one agent booking to different calendars, including conversational rescheduling and cancellations across appointment types.

**What happens when the agent can't answer something?** It says it doesn't know, files a Smart FAQ item, and notifies the account owner. Answer it once and it becomes knowledge for the next person who asks.

**How long until it's live?** CloseBot markets a 48-second setup that gets you an account, a source, and a starter Q&A agent. A production agent with qualification, booking, and tested flows is a longer session, but it's a builder exercise rather than a development one. The docs are explicit that most flows only need the agent action node.

**Which languages does it handle?** The site claims 40+ languages on the basis that it uses the same underlying models as Claude and ChatGPT. That's their claim, not a documented list.

## The bottom line

CloseBot works the way a good hire works: you tell it what the job is, give it the information it needs, decide where it's allowed to operate, and check the output before it goes live. The mechanism is a source, a persona, and a job flow, plus tools and a knowledge library holding it together. The constraints are equally concrete: text only, CRM-dependent, and priced so that message volume is the number you should watch rather than the subscription.

Judge it on the architecture. If a CRM is already the centre of your operation, the free plan will tell you more in an afternoon than any review will.

👉 [Build your first agent free and see the flow builder for yourself](https://app.closebot.com/a?fpr=li87)
