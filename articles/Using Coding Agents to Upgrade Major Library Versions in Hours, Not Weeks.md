# Using Coding Agents to Upgrade Major Library Versions in Hours, Not Weeks

### September 3rd, 2024 (4 min read)

Over the past day and I half, I used three different coding agents to upgrade the major version of one of Vellum's
core libraries – something that might have taken me 2-3 weeks just a year ago. Here was my process and learnings 👇

The goal was to upgrade our react flow library from v11 to v12. We sued react flow to power our Workflow Builder UI
– a low-code canvas for constructing AI Agents. Going from v11 to v12 was such a big version change that the creators
actually used the opportunity to rename the library to xyflow. There were many breaking changes across versions,
particularly related to sizing and positioning of nodes and we had done lots of customization over the years. We had
been wanting to do this version bump for about a year and a half now, but every time we started, we realized how
daunting it would be and gave up.

![An example of an AI Agent built using Vellum's Workflow Builder](/articles/assets/vellum-workflow-builder.png)

Lately, we've been using coding agents a lot at Vellum and our engineering output has skyrocketed as a result. In fact,
our team of 15 engineers merged over 1500 PRs in the month of August alone! I decided that with the rise of coding
agents, it was time to give this version bump another try.

First, I used [Devin](https://devin.ai/) to initiate the task. I wanted to start with a background coding agent. I knew
this would be a slow process, and I wanted to do other coding-related tasks in parallel. I gave it a simple set of
instructions and told it
to read the react flow migration guide and focus on fixing any breaking changes.

After kicking off Devin, I kicked off another four sessions for other unrelated coding tasks and then took my dogs for a
long walk. Over the course of my walk, I'd open up Devin on the web browser of my phone and flip between the various
sessions I had kicked off. Each time Devin got stuck or ran into test failures that it struggled to get past on its own,
I'd prompt it with what to do next.

By the time I got back from my one hour walk, Devin had correctly fixed all imports and Typescript errors, as well as
completed first drafts of my other three sessions I had kicked off.

Now that typescript was happy and the frontend could compile successfully, it was time to use a foreground agent to
iterate until we reached functional and aesthetic parity with what was on prod with v11.

Because so much had changed as it relates to the positioning and recalculation of node dimensions across versions, it
required lots of manual testing of various user flows. For example, drag-and-dropping a node onto the canvas positioned
the dropped node far away from my mouse cursor – a regression.

I started with [Cursor](https://cursor.com/) as I had been using it for many months now. Probably the coolest hack I
came up with was grabbing a screenshot of what the UI looked like before and what it looked like afterwards, 
stitching them together into a single
image, and including the words "Before" and "After" above each. I'd provide Cursor with the image and in most cases, it
was able to determine and fix the busted css on its own.

[![A screenshot showing a UI regression before and after upgrading a library version, with the words "Before" and"After" above each side](articles/assets/before-and-after-example.png)](/articles/assets/ui-regression-before-after.png)

As I continued to burn down the low-hanging fruit of the regressions I encountered, I began to notice Cursor struggling
with some of the more gnarly bugs – those that required tracking down obscure references to css class names, manually
defined constants that represented positional offsets, and then needing to use sophisticated reasoning to make sense of
it all. I had been hearing great things about [Claude Code](https://www.anthropic.com/claude-code) and figured this
would be a good excuse to finally give
it a
try.

Claude Code seemed to excel at these gnarlier regressions. It was particularly good at making sense of the annotated
screenshots I provided it. I also found that as sessions got longer, quality got worse, and so I'd start a new session
that focused on a specific class of regressions.

After about 8 hours total of working with both background and foreground coding agents, I was done. Our Workflows
Builder UI looked and worked exactly the same before and after the major version upgrade and I had a PR with +764/-854
changes (sorry reviewers!). What would have taken me 2-3 weeks – an amount of time I wouldn't been willing to dedicate
in the first place – instead took me a day and a half, while multi-tasking.

We've similarly been eying a Material UI major version upgrade and I'm excited to run a similar process:

- Use a background coding agent to initiate the task and get the tedious stuff out of the way (compilation and type
  errors)
- Use foreground agents to quickly iterate on specific regressions, one at a time.
- Use Cursor for (what imo is a better devexp) for the low-hanging fruit
- Switch to Claude Code for the gnarlier bugs
- Make ample use of annotated screenshots of UI regressions along the way

