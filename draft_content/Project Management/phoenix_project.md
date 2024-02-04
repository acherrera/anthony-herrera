---
tags:
  - productivity_moc/management
creation_date: 2023-11-05 16:38
source: https://www.e4developer.com/2018/03/24/the-phoenix-project-a-key-to-understanding-devops/
---
# Phoenix Project

This is a summary of the phoenix project, a book which I have read 2 or three times now. Instead of reading it again, I'll hopefully be able to just reference this note. Hopefully.


## The Three Ways

This is the main principals on which DevOps systems are built.

##### 1 - Creating flow

This means making sure that items are continually moving through the system and improving. Individual metrics are less important than ensuring the system as a whole is working.

There are the items that help create flow:

 * Building systems and organizations that are safe to change;
 * Limiting work in process;
 * Continuous integration and deployment;
 * Creating environments on demand.

Limiting work in process: Everything must be tracked via the almighty Kanban board. This is not because management is some kind of control freak, but rather, it shows what work is actually being done and ensure that there is not a significant amount of WIP.

Second off, take out work that does not meaningfully contribute. This can be more important than adding more work.

Systems safe to change - allow for trial and error without fear of major production stopping issues. IE: dev / qa / prod systems to ensure the prod sysem is working well before changes are make. 

Continuous integration and deployment: Ensure system change me integrated, deployed and reverted at the press of a button. No manual set up and configuration.

Create environments on demand: This is usually just basic virtualization. Should be able to create environments to test and play with and not have to go through significant amount of set up.

##### 2 - Fix quality early and avoid rework

* Stopping “the production line” when builds and tests fail in the deployment pipeline;
* Constantly improve daily work
* Create a better cooperation between Development and IT Operations – “shared goals and shared pain”;
* Create production telemetry to make sure code and environments work properly and that customers requirements are satisfied.

Stopping the system when builds and tests fail deployment: This is key to ensure that tech debt is not built up or the future. This is only works short term and then requires significant rework to fix later.
Think about putting a machine together, finding and issue and then deciding to fix it later, after the machine is fully together.

Constantly improve daily work. This one is obvious to do, but difficult to implement. Make the things you do ever day as efficient as possible.

Better cooperation between development and IT operations. If IT and development are on the same page, thing go better.

Production telemetry to ensure systems are running as expected. If a tree falls in the forest and no on hears it, is the production system really broken? Make failures LOUD. 
Put it on a screen behind you that shows everyone if you have to.

#####  3 - Create a good culture 

"Good" here being defined as a culture that “fosters experimentation, learning from failure, and understanding that repetition and practice are the prerequisites to mastery.”

* Be open to innovation and risk taking instead of blindly taking orders;
* High trust;
* Allocating at least 20 percent of Development and IT operations cycles toward non-functional requirements;
* Encouraging and celebrating improvements.

Open to Innovation: Allow workers to try new things instead of blindly taking orders. Encourage this behavior. Require it if you must.

High Trust: In order to take risks and fail, the team must have a high degree of trust. Trust they won't be judged harshly and that their failures will lead to team improvements

20% of time to non-functional requirements: I don't know, make things fun?

Encouraging and celebrating improvements: Along with being allow to fail, need to recognize when the team does a good job to make sure the project is not just a series of failures and quiet success. Make sure successes are LOUD as well as failures.
If the work is imaginary, the rewards need to be felt for real.


## Four Types of Work

Four types of work are:

* Business projects
* Internal IT projects
* Updates and Changes
* Unplanned work

Business Projects: These are initiatives - most of the development work that goes on is in this category

Internal IT projects:  Infrastructure and IT Operations. Creating infrastructure and automating tasks. These are high value, but very "quiet" changes

Updates and Changes: Generated from the previous two types of work. Tech debt and updates. 

Unplanned / Recovery work: Incidents and problems caused by other work. This can start eating away at teams if not identified and the root cause discovered.


## Theory of Constraints

This is an item that seems obvious in writing but is really hard to keep in mind when working on a team. In a flow system, there will be constraints. That is, the step / process / person who is slowing the rest of the system down.
Until that constraint is removed (meaning, not longer slowing the system down), all other processes have to wait while that step completes.

Example: If a process requires steps A, B, C and D and B is slow. All other steps can only move as fast as B. In this case, effort should be put on improving B.
Eventually, another step will be a constraint and it all start over again. 

