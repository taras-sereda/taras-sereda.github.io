---
layout: post
title: "general-purpose LLMs are bad at chess and planning"
tags: ML LLM Planning AlphaZero
---

### LLMs and path autonomy
Agentic capabilities of LLMs are typically evaluated on domain-specific benchmarks. [LiveBench](https://livebench.ai) maintains a set of questions with verifiable answers across multiple categories; [ProgramBench](https://arxiv.org/abs/2605.03546) evaluates the capabilities of LLMs in writing production-grade code without access to the internet; [SOL-ExecBench](https://arxiv.org/abs/2502.10517) is designed to evaluate the performance of GPU kernels against hardware limits. These benchmarks are challenging, but even passing them doesn't provide a good measure of the planning and strategic capabilities of the models.

### AlphaZero moment
DeepMind's earlier work on [AlphaZero](https://deepmind.google/research/alphazero-and-muzero/) achieved remarkable results in board games, including chess and Go. As the [AlphaZero Wikipedia](https://en.wikipedia.org/wiki/AlphaZero) article notes, after just 4 hours of training through self-play, AlphaZero was playing at a higher Elo rating than Stockfish 8, a state-of-the-art chess engine. A follow-up work, [MuZero](https://arxiv.org/abs/1911.08265), published at the end of 2019, combines tree-based search with a learned model of the environment and matches AlphaZero's performance without ever being given the rules of the game. Instead, MuZero learns the game dynamics through interaction with the environment, and its learned model predicts the quantities needed for planning during search.

### Frontier LLMs playing chess
Chess, and board games in general, allow us to evaluate planning in isolation. Experienced players can see possible outcomes of the game several moves in advance. So in this experiment I wanted to see which frontier LLM is better at chess, and set up a competition: GPT-5.5 vs Opus-4.8, LLMs of comparable capabilities. Both models played with high reasoning effort, limited to 128,000 tokens per move. I bought $15 worth of tokens for each and set them up to play two games.

I used [pgx](https://github.com/sotetsuk/pgx), a JAX-native game simulator for reinforcement learning (RL). At every step, each LLM received the current state of the board in [FEN](https://en.wikipedia.org/wiki/Forsyth%E2%80%93Edwards_Notation) notation and the full history of previous moves in [UCI](https://en.wikipedia.org/wiki/Universal_Chess_Interface)[^1] notation. The LLM was prompted to return a single move encoded in UCI.

<div style="display: flex; gap: 1rem; justify-content: center; flex-wrap: wrap;">
  <div style="flex: 1; min-width: 280px; max-width: 420px;">
    <video controls style="width: 100%; height: auto;" preload="metadata">
      <source src="{{ '/assets/video/game_017.mp4' | relative_url }}" type="video/mp4">
    </video>
      <div class="caption">Game 1: Black (GPT-5.5) wins in 39 moves</div>
  </div>
  <div style="flex: 1; min-width: 280px; max-width: 420px;">
    <video controls style="width: 100%; height: auto;" preload="metadata">
      <source src="{{ '/assets/video/game_018.mp4' | relative_url }}" type="video/mp4">
    </video>
      <div class="caption">Game 2: White (Opus-4.8) wins on token timeout</div>
  </div>
</div>

GPT-5.5 won the first game in 39 moves, but it was also the more expensive player throughout, burning 1.5–1.7x as many tokens as Opus-4.8 on average and spending $8 to Claude's $5 in the first game. The most expensive moves for both models peaked at around 25,000 output tokens. In the second game that appetite caught up with it: partway through, GPT-5.5 exhausted its allocation and fell back to random moves, which handed the win to Claude. So the final score was 1–1, and Claude came out $6 cheaper overall.

The first 10 moves in each game were identical for both White and Black. It's nice to have reproducible results, but at the same time there was no evidence of exploration from either of the LLMs, and I didn't observe an interesting strategy in their moves.

### What's next?

I'm now mostly interested in evaluating LLMs on chess puzzles: problems where a certain sequence of moves is guaranteed to lead to the globally optimal outcome. Another experiment would be to change the representation of the state in favor of vectorized SVG images of the board. I think this could even be more efficient in terms of reasoning tokens used, since FEN is a compressed representation that the model must mentally unpack, whereas an image shows the board directly.

To advance open-ended research and the discovery of optimal algorithms, new materials, or new drugs, it's crucial to have models that are capable of planning. And today's frontier LLMs are not yet well suited for the general-purpose setting of this task. Something is missing yet...

[^1]: the classic opening `e2e4` is an example of UCI notation