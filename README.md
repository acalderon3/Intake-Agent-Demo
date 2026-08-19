# Intake-Agent-Demo

Design Rationale: AI Intake & Prioritization Agent (Prototype)

What this is:

This is a prototype of an AI assisted intake and triage system for People Ops style work requests/tickets, inspired by a "PM, People Ops Intake & Portfolio" role at Stripe, which described a need for building an AI agent for automated scoping, duplicate detection, effort estimation, and prioritization guidance. 
This is a self directed learning project, built to demo how I'd think through designing a solution. This is not a production tool and is not built with real data. I used Claude to write the code; the architecture, framework choice and other judgment calls below are my own. 

Why RICE + alignment tag:

I chose RICE (reach x impact x confidence / effort) because it is a recognized framework; it collapses to a sortable number which helps in creating a ranked backlog of requests. 
The job posting called out a need for evaluating requests against People team OKRs and a multi year plan. Because the RICE framework has no built in mechanism for strategic alignment, I added an extra alignment tag (aligned/partial/not stated) to show next to the score. Someone reviewing these results could see a high RICE score and it not tied to a stated priority, to aid in prioritization conversations. 

Handling ambiguous requests:

An important design decision comes when the AI agent can't extract scope, urgency, or effort from a request, it will explicitly say so rather than guessing or assuming. 
The extraction step returns a "missing_information" field listing exactly what wasn't stated (e.g. "no success criteria"_ plus a "confidence" rating. A low confidence extraction is visually flagged in the human review step, so a reviewer sees the request needs clarification rather than nice looking score next to a fully scoped ticket/request. 
I added two backlog tickets specifically to test this: one has a vague submission ("our onboarding process is a mess") and another ticket that clarifies and expands that original request. The system needs to recognize this as an update to an existing ticket, not a new/unrelated request, something that is easy to get wrong if the duplicate detection only looks for closely related wording rather than reasoning about relationship between requests. 
Not having this would result in the system generating a best guess score even from vague requests; a confidently wrong score is worse than a visible gap because it would hide the judgment call a human reviewer would need to step in and make. 

Human review step:

The extracted field, duplicate flag, and score are editable and reversible before anything is added to the ranked backlog. The system has no visibility into team capacity, insights into past prioritization decisions and no authority to make resourcing calls. The system does what the job posting requested: "an AI agent that helps with scoping, dedup, resourcing, prioritization." There is a person who still owns the judgment call. 

What is needed to make this real:

Jira integration for actual ticket ingestion and handling
DB so approved tickets and their audit trail are saved 
Resource model to feed resourcing recommendation, current team workload, skills, open projects
Evaluation against past decisions: back test this agent against past prioritization calls, to see where the model's judgment diverges from human judgment
