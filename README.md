# Reinforcement Learning Specialization (Coursera / University of Alberta)

Hi everyone! 👋

This is my repository for the [Reinforcement Learning Specialization](https://www.coursera.org/specializations/reinforcement-learning) by the University of Alberta and the Alberta Machine Intelligence Institute (Amii) on Coursera. I will be uploading my notes, experiments and anything else interesting I come across while doing the course.

So please, feel free to take this information, use it and review it however you like.

## What's inside

The specialization is made up of four courses, and this repository follows the same structure. The notes live right next to the code: each folder has its own `README.md`, so you can read everything straight from GitHub.

| # | Course | Topics |
|---|--------|--------|
| 1 | [Fundamentals of Reinforcement Learning](01-fundamentals-of-rl/README.md) | Bandits, exploration vs. exploitation, Markov decision processes, value functions, Bellman equations, dynamic programming |
| 2 | [Sample-based Learning Methods](02-sample-based-learning/README.md) | Monte Carlo methods, temporal-difference learning, Q-learning, planning with Dyna |
| 3 | [Prediction and Control with Function Approximation](03-function-approximation/README.md) | Feature construction, on-policy prediction and control with approximation, policy gradient methods |
| 4 | [A Complete Reinforcement Learning System (Capstone)](04-complete-rl-system/README.md) | Building and tuning a complete RL agent from scratch |

The experiments I write on my own, outside the graded assignments, live in [`experiments/`](experiments/README.md). Every example is a standalone script, so you can run any of them on its own:

```bash
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
python experiments/<script_name>.py
```

## Honor Code

This repo only contains my own notes and side experiments. Graded assignment solutions are **not** published here, in line with the Coursera Honor Code. If you are taking the course, please try the assignments yourself first. It's the best way to learn this material.

## Found a mistake?

Very likely there are some! I'm learning as I go, so if you spot anything wrong, please open an [issue](../../issues) or drop me an email and I'll fix it as soon as possible.

## Official course resources

The course follows Sutton & Barto's *Reinforcement Learning: An Introduction*, which the authors make freely available online: [incompleteideas.net/book/the-book-2nd.html](http://incompleteideas.net/book/the-book-2nd.html). It's the best companion to the lectures.

---

**I hope you find this repository helpful.** 😊

---

<div align="center">
<br>

[![Email](https://img.shields.io/badge/Contact%20me-imadariaga__albizuri%40outlook.com-0078D4?style=for-the-badge&logo=maildotru&logoColor=white)](mailto:imadariaga_albizuri@outlook.com)

</div>