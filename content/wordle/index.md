---
title: Can LLMs solve Wordle?
share: false
date: 2024-05-01
---

[Github](https://github.com/jbaik1/CS-263-Wordle)

LLMs have demonstrated incredible capabilities for both [zero-shot](https://arxiv.org/abs/2205.11916) and [few-shot]() learning.

For our final project in UCLA's CS 263 (Natural Language Processing) course, we tested an LLM's ability to play [Wordle](https://www.nytimes.com/games/wordle/index.html)

Here is our final report: Report

We chose Meta's [LLama3 8B](https://huggingface.co/meta-llama/Meta-Llama-3-8B) and [Zephyr-7B β](https://huggingface.co/HuggingFaceH4/zephyr-7b-beta) from HuggingFace

In essence, this project tests the effectiveness of prompting to guide the LLMs to solve a Wordle problem.

The core focus here is on prompting. We want to teach the model the rules of the game, and give feedback on how it performs.

Feedback is done in the style of [chain-of-thought](https://arxiv.org/pdf/2201.11903)

Our setup is such that the Wordle game consists of multiple "turns", where each turn is a pair of guesses and feedback. The feedback returns information about the validity of each of the 5 letters in the word. For example, it might say "Letter "A" at Position 1 is in the wrong position but is in the word", and similarly for the other remaining 4 letters. This guess and feedback exchange will happen until the model guesses the word correctly or until it exhausts the maximum number of guesses.

For few-shot learning, we supply the model with some example games. After each guess, there is an automated feedback that looks something like this:

``` 
Feedback:
Your guess was: 'CARRO'. The feedback for each letter is:
{
   1: Letter C is not anywhere present in the word.
   2: Letter A at Position 2 is in the word and at the correct position.
   3: Letter R at Position 3 is in the wrong position but is in the word.
   4: Letter R at Position 4 is in the wrong position but is in the word.
   5: Letter O is not anywhere present in the word.
}

Please enter your next guess:
```

After this feedback, the goal is to have the LLM output a guess that includes its reasoning. For example: 

``` 
"guess: "APPLE",
"reasoning" :
"The list of previously guessed words are: ['BANANA', 'ORANGE', 'CARROT']
The list of valid letters are: ['A', 'L', 'P', 'E']
The list of invalid letters are: ['C', 'B', 'R', 'O', 'G']

The previous guess was: CARROT
Given the previous feedback, we know that
1. The letter at position 1 (A) was in the string and in the correct position.
2. The letter at position 1 (P) was in the string and in the correct position.
3. The letter at position 1 (P) was in the string and in the correct position.
4. The letter at position 1 (L) was in the string and in the correct position.
5. The letter at position 1 (E) was in the string and in the correct position.

Improving on this feedback,
the new guess is APPLE
```

Note that the LLM did not guess 5 letter words before, and so the previous feedback just considered 'CARRO' instead of 'CARROT'.

The results weren't great, and here are some possible issues:
1. Model size was too small. We only had 1 GPU with 16GB memory to use, and we barely crammed in the Llama model using [4-bit quantization](https://huggingface.co/blog/4bit-transformers-bitsandbytes).

2. LLMs aren't great at reasoning with positions and letters. However, this might be an issue with our prompting method. Maybe LLMs have gotten better at these tasks since then.

Still a fun project!