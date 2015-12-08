# How to set up a Training

1. [General guideline](#general-guidelines)

2. [Lessons learned](#lessons-learned)

This document should help you to work out the training materials, work out a proper training concept and execute it on spot. 

## Goals and Strategy

Before you start to type down your concept and create your training materials, think in depth about the following questions and document them for yourself:
* Who is the target audience?
* What should be the outcome for the participants, the hosts and ContentMine from the training?
* What is needed to achieve the outcomes?

## Narrative

Questions to answer:
* Who is the audience?
* What do the visitors expect (reagarding the communicated knowledge about the session in advance)?
* What do the hosts expect?
* What is the problem?
* Why does the workshop help to solve the problem?
* What are the crucial transformation events from one session to the next? 
* How do we manage them? How do we transfer the activity?

prepare text snippets:
* what is a fact?
* what is content mining?
* what is content mine?
* what is content mine good for?
* what is content mining good for?
* what does it have to do with the TDM exception?

## Work out the Training
* Don't pressure yourself and the participants: trainings must be easy going
* Have a good balance between hands on sessions, social interactive sessions and theoretical talks.
* Ask if people are hold back by others things out of their control to implement things learned
* Is there anywhere pressure in the timeline? Keep spaces free to relax for everyone. People should never think “I need a break!”.
* Make everyone happy: Hosts, participants and you, the facilitator!
* create an inclusive and diverse space: no racism, no sexism, etc. 
* be clear about problems, limitations and advantages, but always offer a solution
* identify transfers of different training blocks and formats and think about what is needed to change from one to another (e.g. to get from an World Cafe to a presentation).
* prepare offline alternatives, WIFI always can fail.
* Let participants group up (2-3-4), target: 4-6 groups
* are you allowed to share your data with others?
* are people excluded from some acitivies? e.g. non-devs
* Do we assume anywhere pre-knowledge? Is the content to difficult?

### Workflow
* plan with time to change the room setup
* think about participants coming later in: how to include them most easily, how often will this happen?
* seperate questions and tasks in different time phases: 
  * conceptualizing
  * planning
  * executing
  * evaluating
* create a checklist for things which needs to be done (and when)
* check previous trainings: documents, PR work, lessons learned
* check the cumulative lessons learned document

### Session Formats

#### Presentation
* Presentations: offer basic advice how to present well
* ask questions to the audience: have they heard of it? etc.
* tell a strong story
* ask yourself: what is the purpose of the talk? 

#### Hack Session
This means a open space hack session, where everyone does something different and the faciliators are among the participants

* think about different system requirements: OS, CPU power, inet bandwitdh, storage, RAM, pre-installed packages conflicts, 

#### On Screen tutorials
This means a teaching method, where everyone watches a facilitor doing something on the screen. Often the participants are then replicating the things done on their own machine. (e.g. explaining iPython notebook)

* take it easy and make intentional breaks to let users follow up.
* take yourself time to type and execute commands. be aware, that after executing the command, the command itself may disappear from the screen, so it is not visible anymore for the participants. When you want to use short-cuts like tab-completion, explain and show them.
* Execute the tutorials in a sequential way and plan breaks when blocks are done
* have in mind, that the beamer may has lower resolution

#### Discussion
* Have a backup plan, if no one wants to start.
* Try to distribute talk-time equally.
* order people in a circle to get a sense of a common feeling

#### World Cafe
TEXT FROM MOZFEST GOOGLE DOC

### Examples 
offer some examples of slots in a workshop in general and for CM in specific. Which formats are available?
how do facilitators organize their activites during the training? notes, laptop docs??


narrative: offer information how to create a strong and compelling narrative in your training. this is a very crucial point

link to older training repos

## Training Materials
* think about color-blindness 

### Slides
offer some advice, how and when to create slides

* try to not use lists, except it is obviously a list
* use big enough font size
* use colors which create high contrast to make it easier visible
* have in mind, that the beamer may has lower resolution
* use strong visual approach

### Screencasts
Create them on 
* Ubuntu/Linus: RecordMyDesktop
* MacOS: 
* Windows: 

### Hand Outs
* use qr link or bit.ly for it

### GitHub repository
* offer participants ways to get to the necessary level in advance: install sw, learn stuff, read things

## Dealing with problems

* offer advice for offline alternatives: AoH, prepare always something!
* offer hints for testing the software, the data and the slides for readiness
Social interaction: offer some basic methods and concrete examples for social interactions. Give hints for common problems (people not interested, people start to fight, people blame you for failing/weak training, etc.)
* Which informations need to be available all the time, especially for new arriving people?
* think through if way more or less people arrive: will the sessions still work? what could help, what do you have to change?
* how to treat racism, sexism, etc. Think about this when inviting people in advance and put focus on this 
* work out alternative strategies for 3 and for 50 persons


## Lessons learned

This list describes common problems that may occur and how they can be solved.

| PROBLEMS | LESSONS LEARNED |
| The central message (vertical integration, ease of access and scalability, sectioning of papers, use of supplementary information) does not come across. | Put focus on unique points of interaction (sHTML, ctree, results) and not the technical details. |
| The use case demonstrated is not really appropriate for audience. | Start with powerful demo first, then go into technical details |
| Too much content in too less time, not enough time for teaching/delivering key message | Start low but have more in-depth stuff available. |
| Missing narrative, difficult transitions between sessions | Change preparation time allocation from 80% tech/ 20% storyline to 50/50. |
| Early technological problems (keyboard locales, VM on windows) lose large parts of the group, and it is difficult to recover from there. | Have backup plans and material: reserve more time for error handling and if something does not work. Define alternative tasks while solving problems. |
| Getting started with the VM could be problematic especially for newcomers. | People should not have to worry about CLI other than copying the relevant commands. Everything should have been within one folder which would be the starting folder of the command line. |
| Missing central documentation to direct people to. | Prepare a github repo and an etherpad. |
| Long URLs are difficult to type. | When uris are used, use bit.ly |
| People start wandering off on their own e.g. when some are still stuck at installing, and others already completed the task. | Define clearer “group stages” (Now we’re installing for 5 min; then we’ll begin with notebook; play around with facts for X min; then talk about results) |
| When a session loses focus, it is difficult to catch people's attention again and pull them back together. | Try to provide "a social solution for a technical problem" - clear statements what to do if something fails; have people group up in teams of two with different OS and experience level, so that they move more together. |
