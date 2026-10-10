# Microsoft Online Assessment Questions: What Actually Shows Up on HackerRank and Codility, How It's Scored, and a 4-Week Prep Path That Fits Around a Job

The Microsoft OA isn't a mystery test. It's two coding problems, roughly 75–110 minutes, and a scoring model that rewards different things depending on whether your invite says HackerRank or Codility. Most people who get screened out don't fail because they can't code — they fail because they burned 40 minutes on the wrong problem, shipped an O(n²) solution into a performance-graded grader, or submitted a half-finished draft at timeout.

So here's the practical picture: the format you're likely to get, the question types that keep appearing in 2026 reports, and how to spend the weeks before your deadline.

## The format, in one screen

|  | HackerRank invite | Codility invite |
| --- | --- | --- |
| Question count | 2 coding problems | 2 coding problems |
| Time limit | ~75–90 minutes | ~90–110 minutes |
| Difficulty | Mostly Medium, occasional Medium-Hard | Same range, harsher constraints |
| Scoring | % of visible + hidden test cases passed | Correctness and performance scored separately |
| Attempts | One | One (editor auto-submits at timeout) |
| Invite window | Usually about 7 days from the invite | Usually about 7 days |

A few details matter more than the table.

**The pass threshold isn't published.** On HackerRank, each problem is scored on the test cases it clears, visible and hidden, and your employer sets the bar privately. Which means there's no number to aim at, and no reliable "60% gets you through" rule — that figure circulates on prep sites, not from Microsoft.

**Codility grades performance as its own axis.** A correct solution that times out on the big test cases loses points even when the logic is right. If your code is O(n²) and the input goes to 100,000 elements, correctness doesn't save you.

**Submissions are one-shot.** Codility submits whatever is in the editor when the timer ends. Forgetting to hit submit, or leaving a broken refactor in place, costs the whole attempt.

**Retakes need a fresh invite.** Treat your first open as your only real shot, and check whether your invite is HackerRank-branded or Codility-branded before you start practicing, because the two reward slightly different habits.

## The question types that keep showing up

Candidate reports from 2026 cluster tightly. These aren't exotic puzzles — they're standard patterns wearing a Microsoft-flavored story.

**String rewriting with a greedy scan.** The classic is *Min Moves to Obtain String Without 3 Identical Consecutive Letters*: given a string of `a`s and `b`s, swap characters so no three identical letters sit in a row. You scan runs of identical characters and add `length / 3` per run. The whole thing is O(n) and about eight lines long. Related: *String Without 3 Consecutive Letters* variants where you delete instead of swap — same greedy walk, different accumulator.

**Difference arrays and prefix sums.** *Circular Character Roll* asks you to increment the first `roll[i]` characters of a string by one, cyclically, `z → a`. A naive implementation is O(n·m) and dies. The intended solution collects all the increments in a difference array, prefix-sums them once, and applies the total shift mod 26.

**Sliding window with two conditions.** *Subarray Permutation Check*: given a permutation of 1..n, report for each `k` whether some contiguous subarray is exactly a permutation of 1..k. The trick isn't enumerating subarrays — it's recording where each value sits, then expanding `k` while tracking the leftmost and rightmost positions of the values 1..k. The subarray is valid exactly when `R - L + 1 == k`. O(n) total, and brute force will time out.

**Monotonic stack.** A reported pricing problem: for each item, subtract the price of the first item to its right that is less than or equal to its own price, then report the indices that sold at full price. Traverse right to left, keep a monotonically increasing stack, pop anything larger. This is the same "next smaller element" pattern underneath a story about sales.

**Hash maps for small stateful systems.** One 2026 report describes a *session authentication system*: `generate(token_id, current_time)`, `renew(...)`, `count(current_time)`, with tokens expiring at `current_time + TTL`. A single hash map of `token_id → expire_time` covers it. The commonly missed detail is `expire_time <= current_time` counting as expired, not `<`. No heaps or ordered sets needed — overengineering here costs time you don't have.

**DP layered on arrays.** Stock trading variants, House Robber, Jump Game. The heaviest reported one is *Max Sum of Three Non-Adjacent* on Codility with arrays up to 100,000 integers, where an O(n³) brute force is a guaranteed timeout and you need an O(n) or O(n log n) formulation before you type anything.

**Graphs, usually small.** Connected components via union-find or DFS, shortest-path BFS, network-rank-style problems. Reports suggest these are lighter than generic prep guides imply — small BFS/DFS structures rather than heavy graph theory.

**Miscellaneous array and list work.** Product of array except self under a large modulus (no division), delete odd nodes from a linked list, maximum XOR, longest palindromic subsequence. Standard LeetCode-medium material, sometimes with a twist that punishes a memorized template.

If you want to see how these get decomposed into reusable templates rather than one-off solutions, that's the specific thing pattern-based prep courses are built for — 👉 [browse AlgoMonster's pattern library and Microsoft OA problems](https://algo.monster?fpr=li69).

## Two worked examples of the reasoning, not the code

Take the permutation problem. The instinct is to think about subarrays. The winning move is to think about *positions*: `pos[1]`, `pos[2]`, … Then for `k = 1, 2, 3, …`, maintain a running min and max of those positions. If the window from `L` to `R` holds exactly `k` indices, it must contain exactly the values 1 through k — no duplicates possible, because every value appears once in a permutation. That reframing turns an O(n²) scan into a single pass, and it's the kind of step interviewers at Microsoft explicitly reward in later rounds too.

Now the monotonic stack one. You're told to look rightward for the first price that's less than or equal to the current one. Reading left to right, you don't know the answer yet. Reading right to left, you do — you keep a stack of candidates and discard any that can't be the "first smaller or equal" for anything further left. Two lines of reasoning, one loop, O(n).

Neither problem is hard once you recognize the shape. The difficulty is recognizing the shape in roughly 35 minutes while a timer runs. That's the actual thing you're training.

## Why people fail with correct code

A short list, all of it documented in candidate reports:

- **Performance limits.** Codility's second scoring axis is unforgiving. Deriving the intended complexity before writing code is worth more than typing fast.
- **Half-submitted work.** The auto-submit at timeout takes your editor contents. Bracket the last few minutes for a clean submit.
- **Tab switching.** One candidate had an unrelated portal auto-refresh in the background and trigger a window-focus warning. Close everything except the test tab.
- **AI tool detection.** HackerRank publicizes its plagiarism detection as running at 93% accuracy, and code replay can flag a flawless fifteen-minute solve with no mistakes. Microsoft's proctoring signals are configurable per test, and the exact set isn't public — but they're live even on lightly proctored sessions. Screenshot-and-recording detection aside, a flagged session can invalidate a score after the fact.
- **Pipeline, not score.** There are public candidate reports of passing every visible case and still getting a rejection days later, usually attributed to headcount or application pipeline rather than the assessment itself. Nothing you can optimize except the things you control.

One more, less dramatic and more common: a solution that passes the sample cases and fails on the empty input, the single-element input, or a long run of identical characters. Keep a short edge-case list next to you and run it every time: empty, length 1, all-same, already-valid, maximum-size.

## A 4-week plan mapped to what gets tested

Four weeks is enough if you already code regularly. Eight to twelve is more realistic if you're rusty. This schedule front-loads the categories with the highest reported frequency.

**Week 1 — arrays, strings, hash maps, two pointers.** Do them as timed pairs, not as a leisurely problem-of-the-day. Two mediums in 60 minutes, then review. The goal isn't coverage, it's getting comfortable writing under a clock.

**Week 2 — sliding window, prefix sums, difference arrays.** This is where Microsoft's reported question set concentrates. The subarray-permutation and character-roll families both live here, and both punish the naive version.

**Week 3 — trees, graphs, heaps, monotonic stack.** BFS/DFS, connected components, union-find, plus "next greater/smaller element" patterns. Graphs are usually small in these assessments, so don't over-invest in advanced graph theory.

**Week 4 — DP plus two full simulations.** Give yourself a real 90-minute block, two unseen problems, no pausing. Then spend more time on the post-mortem than on the test.

Across all four weeks, two rules: bank the easier problem first to build a clock buffer, and leave ten minutes at the end for edge-case validation instead of chasing the last hidden test.

## Where a pattern course fits, and where it doesn't

The gap most people hit isn't "I don't know what a sliding window is." It's "I can't reliably tell, in the first five minutes, which of the twelve things I know applies here." That's a recognition problem, and it's what AlgoMonster is structured around — its pattern templates and decision flowcharts are the actual product, with practice problems as the way you internalize them.

Officially listed Pro content, straight from the subscribe page: all patterns, lessons and problems unlocked, unlimited AI assistant usage, unlimited Speedrun mode, real questions from top tech companies updated monthly, 500+ lessons across 6 courses with 1,000+ illustrations and 3,000+ editorials, and a member discount on live bootcamps. Two bonus courses come bundled — a Foundation Course covering CS fundamentals, and a Build an LLM Course that goes from tokenization through attention and training. External partner courses, like Quant Interview Prep, are sold separately.

On the Microsoft side specifically, AlgoMonster hosts a company-specific discussion area with threads on reported Microsoft OA problems — Min Moves to Obtain String Without 3 Identical Consecutive Letters, Longest Semi-Alternating Substring, Concatenated String Length with Unique Characters, Max Network Rank, and Largest K Such That Both K and −K Exist in Array are all there. The platform also launched a free AI-assisted coding interview hub with a Microsoft guide, which matters if your loop includes an AI-pairing round.

What it isn't: a video course (the content is text and diagrams), a 3,000-problem bank, or a source of live coaching and mock interviews with working engineers. Third-party reviews also note that company-specific guides and system-design coverage are introductory rather than deep. If you're prepping a senior loop with a full system design round, budget for that separately.

## Plans and current prices

AlgoMonster has no monthly option. It's free tier, annual, or lifetime — and the paid prices are promotional and move around, so check the checkout page rather than trusting a coupon blog from three months ago.

| Plan | What you get | Price | Billing | Get it |
| --- | --- | --- | --- | --- |
| Free | Limited sample lessons and problems | $0 | — | [Start with the free tier](https://algo.monster?fpr=li69) |
| Pro — Yearly | All patterns, lessons and problems; unlimited AI assistant; unlimited Speedrun; monthly-updated company questions; 500+ lessons across 6 courses; bonus Foundation and Build an LLM courses | $99/year (listed at 50% off the $198 rate, works out to $8.25/month billed annually) | Annual, cancel anytime | [Get Pro Yearly](https://algo.monster/subscribe?fpr=li69) |
| Pro — Lifetime | Everything in Yearly plus lifetime updates to all current and future content | $189 one-time (listed at 50% off the $378 rate) | One payment | [Get Pro Lifetime](https://algo.monster/subscribe?fpr=li69) |

Whether the annual or lifetime plan makes sense comes down to one question: will you interview again in the next few years? If you're doing a single focused push over the next six weeks, the annual plan is the cheaper entry. If you expect to change jobs or interview again before the subscription would lapse, lifetime pays for itself quickly — the same content, no renewal, and updates included. 👉 [Compare both plans at checkout](https://algo.monster/subscribe?fpr=li69)

> The free tier is enough to judge whether AlgoMonster's text-and-diagram teaching style works for you. It isn't enough to prep for the assessment.

## The short version

Your Microsoft OA will probably be two medium problems in about 90 minutes, on HackerRank or Codility, graded on test cases with a Codility-style performance axis on top. The recurring categories are strings and greedy scans, sliding window, prefix sums and difference arrays, monotonic stacks, hash-map state design, and a light dose of DP and small graphs. The failure modes are almost never "I didn't know the algorithm" — they're time misallocation, unoptimized solutions on large inputs, and edge cases you never ran.

Pick your week-by-week plan, train in timed pairs, and rehearse the two-problem format at least twice before the real thing. The recognition speed is what you're buying, whichever way you build it.
