# Designing Purgatory Post

*A product development case study by Daniele Minetto*

[Project overview](README.md) · [Development roadmap](ROADMAP.md)

## 1. Project Background

The idea behind Purgatory Post is that even the afterlife has a paperwork problem. A file arrives, someone stamps it, and another takes its place. I wanted to turn that familiar routine into a puzzle: what happens when the player has the tools to do the work, but not quite enough time or space to do everything?

I chose a working week as the unit of play. Getting to Friday gives the player a goal they can understand before learning any of the rules. It also gave me a useful way to evaluate the design. A purchase that looks sensible on Monday has several days to prove its worth. A rule that makes one turn more interesting might still make the rest of the week too easy.

## 2. Product Design

Consider a desk with one empty space and a delivery due shortly. The player can spend a Red Stamp to process the top file in a stack. If the file has a page left afterwards, the stamp has done useful work but has not freed any space. Alternatively, the player can stack two compatible files together. No work has been completed and no pay is due, but there is now somewhere to put the next arrival.

That distinction shaped what the interface needed to show. The player should know which files an action affects, how much time it costs and whether it will actually clear a space. They should not have to predict the order of the deck or remember what a rule said several screens ago. The design priority was to keep the uncertainty of the next hand without making the current decision hard to read.

## 3. Systems and Economic Design

Minutes, desk space and credits solve different problems. Space keeps the office open now. Minutes buy actions before the next deadline. Credits buy options for later days. I kept score separate from spendable income: a valuable scoring play need not be the move that pays for tomorrow's Tool.

The shop makes the trade-off tangible. Buying another copy of a useful Tool changes what the player might draw. Buying a Disposition changes the value of things they already own. For example, being able to process a compatible stack with a Red Stamp gives the player a reason to spend time building that stack first. The purchase changes the appeal of a move, not just the number displayed at the end.

My economics background was useful here in a practical way. When a player cannot afford anything, it matters *why*: too little income, an expensive refresh, and an unhelpful offer are different problems. Raising all rewards might hide the first problem while making a strong late-week build stronger still.

## 4. Development Workflow

My part is product design: I set the goal, write the rules and exceptions, play the build, and decide what to keep or revise. AI helps turn those specifications into code, proposes implementation options and helps write regression tests. I use GitHub branches to keep a playable baseline separate from experiments. I judge the implementation against the behaviour I asked for, rather than accepting it because it runs.

One useful specification came from the lunch break. A player who reads a reward should not receive it; a player who declines should receive nothing; either route should lead to exactly one afternoon delivery. Those sentences can be tested independently. But in a browser playtest, a Tool reward revealed another issue: the panel closed while the clock still showed morning and the planning control remained disabled. Checking the reward alone would have missed it. The acceptance check became: take the reward, see 14:00, then perform the next action.

## 5. Three Design Challenges

### A. Economic balancing: give Monday a second option

A weak Monday could leave too little money to improve the starting deck. The cost of looking for a different shop offer mattered almost as much as the price of the card. If a refresh consumes most of the balance, a better offer is not much of a choice.

I revised the paid refresh costs so an early second look is cheaper while repeated searching still carries a cost. A free refresh does not make the first paid one more expensive. Later, when profitable days produced too much cash, I reduced the flat payment for a completed file from 15 credits to 12. I left combination bonuses and the shop's ordinary prices alone. These were separate decisions: make it easier to look for an option, then tune how much money successful work brings in.

**What the evidence says:** in a controlled five-day comparison, the lower rate took 240 credits out of the final balance both with and without the tested income-focused Dispositions. That checks the accounting for those two builds. It does not tell me how often a player facing random offers will find a worthwhile first purchase. That remains a private playtest question.

### B. User experience: put the choice where the files are

Office Sliding lets the player move a neighbouring stack into a space they have just cleared. The original interaction asked them to choose a stack in a separate window. The desk already contained the answer; the window made the player translate a menu choice back into a location.

I moved the choice onto the desk. The freed cell and eligible stacks are highlighted, the inspector explains the next step, and the player can skip. I used a related principle for Disposition cards. The front describes the effect briefly and shows a counter only when it says something useful. The expanded view holds the full rule and an example. A player looking up a rule no longer needs to read it inside a narrow card.

**What was checked:** regression tests cover valid targets, skipping and cancellation, plus opening and closing the card details in Italian and English. Those checks show that the controls work. Whether a first-time player notices the highlighted destination without prompting is a different question.

### C. System scalability: define what counts once

Expanding from 20 to 54 Dispositions introduced a problem that no single card description could resolve. Suppose a Tool advances a file and a Disposition supplies the last page of progress. Was one file completed or did two effects earn a completion? If the answer depends on which effect runs first, a new card can quietly change the economy of an existing one.

The experimental build uses a shared account of the action: what was on the desk before it began, which pages progressed, which files actually finished and which follow-up effects are allowed. I specified cases where nothing should be paid, too. Joining two files with a Stapler, for instance, changes the workload without completing it.

**What was checked:** the experimental catalog contains 54 implemented effects. Tests exercise interactions such as free progress completing a file once and an earned card entering the afternoon deck once. This establishes specific behaviours, not exhaustive coverage of every possible combination.

## 6. Lessons Learned

I learned to write down the decision I want a player to face before asking for a feature. That made it easier to reject changes: if moving the choice onto the desk fixes Office Sliding, there is no reason to make the ability more powerful as well. I also learned to keep two kinds of evidence separate. A failing test can show that the same file was paid twice. Only playing the week can tell me whether the money led to a genuinely difficult choice.

## 7. Next Steps

The next milestone is a more reliable week, not a larger number on the catalog page. I want to compare different builds across full runs, revisit the first shop after weaker Mondays, and see whether the tutorial and card text explain decisions without my help. Further content and visual work will build on those findings. Wider beta testing and a public release remain ahead.
