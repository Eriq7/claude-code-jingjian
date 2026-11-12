---
name: jingjian
description: Cut the last answer that was too long down to about 1/4 to 1/3. Keep only the points that help with what the user asked this time, keep every source tag as it was, and list what got cut at the end so the user can ask for it. Use when the answer is easy to understand but too long. If it's hard to understand, use /wait-what instead. Only use when the user manually types /jingjian.
argument-hint: "[optional: which answer, or which part, e.g. \"just the conclusion\"]"
disable-model-invocation: true
---

# Jingjian: keep only the points that help this time

"Jingjian" (精简) is Chinese for "cut it down".

The user can understand the last answer, but it's too long: out of 100 points, maybe only 15 actually help solve their problem. Your job is to **cut it down**: keep those 15, drop the rest, and expand only if the user asks. The original is still above, so nothing is really lost.

There are two ways to fail. Cut too little, and you've barely changed anything. Cut too much, and a needed detail is gone, so the user can't follow it or can't decide. Find the balance with one question: **does this point help with what the user is trying to do this time?**

## What to cut down

- No argument: the last assistant answer before this call. If there isn't one, say so in one sentence.
- With an argument: use it to find the target ("the one about X") or narrow the scope ("just the conclusion"). Drop anything outside that scope.

## Steps

1. **Find the goal**: look at what the user asked right before that answer. What they want to know, or the decision they need to make, is your filter.
2. **Pick the points** (do this in your head, don't write it out).

   Keep:
   - The conclusion, or the answer to their question
   - The points that directly support the conclusion, usually 3 to 5, the strongest ones
   - What the user needs to do, with deadlines, amounts and places
   - Risks and limits that would change their decision
   - The setup needed to understand all of the above: if a term is used later, keep the one sentence that explains it the first time it shows up

   Cut: supporting details, background, edge-case warnings, step-by-step math, restating their question, openers, narration of what you did, closing summaries, saying the same thing twice. Don't re-paste content that's already in a file. Give the path.

   If you're not sure about a point, ask: "If I cut this, would the user make the wrong call or fail to follow?" If yes, keep it. If no, cut it.
3. **Rewrite**:
   - The first sentence gives the conclusion.
   - Every fact you keep keeps its source tag exactly as it was ("(source: ...)", "(my guess)"). Cutting is no reason to drop tags. Without them, you can't tell what was looked up from what was guessed.
   - Let the content pick the format: a list for things that are parallel, a table when the same structure repeats 3 or more times, short sentences for cause and effect. Don't squeeze it into a string of fragments. Fragments are harder to read than the original.
   - Keep logic words (because, so, but, unless). Drop filler transitions ("next, let's look at...").
   - Use the same terms as the original. Leave code, commands, errors and file paths exactly as they are.
   - Don't add anything new, don't add background, don't re-explain. That's the job of `/wait-what`.
4. **The last line lists what got cut**: "Cut: A, B, C. Ask if you want them." Topic names only, at most 5. Pick the ones the user is most likely to ask about, so they know what they can still ask for.
5. **Check**:
   - First estimate how long the original is and set the target: 1/4 to 1/3 of it. Estimate again when you're done. If it's over half the original, the lower-level details aren't cut yet. Go back to step 2 for another round instead of sending it.
   - Every fact you kept still has its original source tag.
   - If the original is already short and there's nothing to cut, give it back as it is and add "Already short enough."

## Output

Give the cut-down version directly, with no "Here's the shorter version". The user's CLAUDE.md rules still apply.

## Example

The brand and numbers in this example are made up. It only shows how it works. The user asks: "Is the A-Brand protein bar good for building muscle?"

Original:

> I looked into A-Brand protein bars. First, each bar is 60 g with 20 g of protein (source: a-brand.com). It's also worth noting that it has great reviews on Reddit, and many people say it tastes like a chocolate brownie (source: reddit.com). The brand was founded in Vancouver in 2018 (source: a-brand.com). As for price, a box of 12 is $29.99, which works out to $2.50 per bar (source: amazon.ca). Also, it uses erythritol as a sweetener, and some people get bloated if they eat more than two at once (source: reddit.com). It comes in peanut butter and vanilla flavors too (source: a-brand.com). Overall, this protein bar is high in protein and reasonably priced, making it a great snack for building muscle.

Cut down:

> Yes. 20 g of protein per bar (source: a-brand.com), $2.50 per bar (source: amazon.ca). Heads up: it uses erythritol, and some people get bloated if they eat more than two at once (source: reddit.com).
>
> Cut: taste reviews, brand history, other flavors. Ask if you want them.

What's kept: the answer, the protein and price that back it up, and the bloating risk that changes how the user eats them, each with its original source. Taste, brand history and flavors have nothing to do with "good for building muscle", so they're cut and listed in the last line.
