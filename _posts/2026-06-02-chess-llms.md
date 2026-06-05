---
layout: post
title: "LLMs playing chess"
tags: LLM
---

Agentic capabilities of LLMs are typically evaluated on the domain specific benchmarks, e.g. code generation, KernleBench - gpu kernels, or new exciting benchmarks as ProgramBench.
At the same time, promiss of the open ended reasoning, that could significantly accelerate life science research is exiting and requires strategical planing.
So in this experiment I wanted to evaluate which frontier LLMs is better in chess, and set for the competition GPT-5.5 vs Opus-4.8.
So by playing ches it's possible to evaluate LLMs planning and strategy skills in isolation from the more specific task they are aiming to solve.
LLMs are helpful at the tasks they're explicitly trained on. There's no evidence that frontier labs are training models to play chess, so I decided to set up a tournament where two frontier models play against each other: gpt-5.5 vs claude-4.8, both with high reasoning effort.
I bought $15 worth tokens for each of the models and eventually they played 2 games.

TLDR: gpt-5.5 beats claude-4.8 in a short game of 39 moves. gpt-5.5 was also the more expensive player, spending $8 while claude spent only $5. After that I started another game, gpt-5.5 is a more expensive player and in the middle of the game it used all the allocated tokens and backed up with a random guess game - that led to claude's victory. So the final score is 1 1, claude still saved $6 in the end.

Every game starts with the classic opening `e2e4` — I didn't see the LLMs get any more experimental than that. Moreover first 10 moves are both for white and black are identical. It's good to have reproducible results for the other more deterministic domains, but at the same time it's quite borring looking at the same pattern of the debute, in chess it's interesting to postulate a hypothesis with a move and see how the opponent responds to it, to some extent this is a big part what makes this game fun to play with humans.


<div style="display: flex; gap: 1rem; justify-content: center; flex-wrap: wrap;">
  <div style="flex: 1; min-width: 280px; max-width: 420px;">
    <video controls style="width: 100%; height: auto;" preload="metadata">
      <source src="{{ '/assets/video/game_017.mp4' | relative_url }}" type="video/mp4">
    </video>
      <div class="caption">Game 1: Black (gpt-5.5) wins in 20 moves game</div>
  </div>
  <div style="flex: 1; min-width: 280px; max-width: 420px;">
    <video controls style="width: 100%; height: auto;" preload="metadata">
      <source src="{{ '/assets/video/game_018.mp4' | relative_url }}" type="video/mp4">
    </video>
      <div class="caption">Game 2: White (opus-4.8) wins on token timeout</div>
  </div>
</div>
