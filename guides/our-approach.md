# Assessing local AI

We assess whether a setup finishes a task correctly and how much checking and
correction remains. Loading models, running tools and repairing mistakes all
contribute to the cost of the work.

Claims about models, speed and token savings give us questions to test. The
research covers local assistants, workflows with several models and local
helpers for cloud assistants, including setups with limited memory and compute.

## Start from the job

Extracting dates, correcting a paragraph and writing a report from several
sources place different demands on a model. Assessments need a specific task
and a way to decide whether it is finished.

## Give each part of the work a suitable tool

Search can find relevant files, an extractor can turn a document into text,
and a parser can check a structured answer. A model may handle one step or
act as the main assistant. Assessments identify any tools or special prompts
that contributed to the result.

One possible workflow uses a model to extract facts, another to draft from
them, and a program to check required fields. Keeping source references with
the facts makes errors easier to trace. Loading models in sequence can reduce
the number held in memory, but adds loading and coordination costs.

Compare the whole process, including helper errors, checking and fallback
work. Several models are a design to test; their combined value cannot be
inferred from the separate models' results.

## Improve the harness as well as the model

An assistant's **harness** is the software around the model: the context it
receives, the tools it can call, how a session continues and how its work is
checked. Harness engineering means improving those parts.

We investigate task decomposition, selecting relevant source material,
retaining decisions across a long session and verifying saved changes.
A shorter prompt or faster tool needs to be judged alongside any facts lost
or extra repair work introduced.

In the [hashline editing comparison](../investigations/hashline-editing.md),
changing the edit format reduced median output tokens by 20.8%, but the
model produced fewer correct files. Addressing lines directly reduced quoting;
regenerating whole lines introduced indentation and blank-line errors.

## Use local and cloud models together

A cloud model can remain the main assistant while a local helper extracts
fields, classifies records or selects relevant passages. The main assistant
needs enough information to check that output and handle errors.

The [guide to combining models](choose-a-local-model.md#use-a-local-model-beside-another-assistant)
describes possible roles and comparisons. We look at both the work removed
from the main model and the cost of running, coordinating and checking the
helper.

## What a test establishes

The suite covers a limited set of cases. We use it to record behavior and
investigate configuration choices; it does not by itself establish a general
model ranking or reliability across a kind of work. Preset explanations state
why the components were selected and distinguish that rationale from measured
outcomes.

| Observation | What it establishes | What remains unknown |
|---|---|---|
| A model loads and answers | The tested files and runtime handled that request. | Whether the setup can complete useful work. |
| A focused task passes | A particular behaviour worked under the stated conditions. | Reliability on other tasks or longer workflows. |
| A complete job passes its checks | The setup delivered an acceptable result for that job. | How widely the result applies. |
| Human effort and elapsed time are measured | Whether the workflow saved time in that comparison. | Savings for other people, tasks or machines. |

Many model cards report focused cases checked against supplied sources,
including simulated tool operations. The cards identify AI-assisted reviews;
these do not establish that a person independently accepted the finished work.

Untested capabilities remain unknown. Failed cases show what went wrong
under the tested conditions. Assessments include useful partial work,
consequential mistakes and the limits of the review.

## Keep the conditions with the result

Model files, compression, chat templates, engines and settings can change
behaviour. Hardware and conversation length affect memory and speed. Each
measurement therefore keeps its configuration and date.

Server memory measured on a 32 GiB Mac does not establish fit on a smaller
machine. The operating system, working buffers, context and other applications
also need room. Unified memory and dedicated GPU memory require separate
observations.

Assessments are revised as evidence changes. Older measurements retain their
original identities and dates when they still answer a useful question.

Compatibility also needs testing. Changes to templates, dependencies or
settings can break a working combination. Fixes go into the
[Temper installer](https://github.com/temper-sh/temper) and setup notes,
alongside the tested versions and unresolved problems.

## Compare the tradeoffs

A smaller model may handle routine work with fewer resources. A larger model
may require fewer corrections. A workflow with specialist tools may reduce
what either model needs to do.

Use the [model-choice guide](choose-a-local-model.md) for a starting point,
then compare a representative task, including your own review and repairs.

[All guides](README.md) · [Back to Temper](../README.md).
