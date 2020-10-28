---
attachments: [Clipboard_2020-10-27-08-28-06.png, Clipboard_2020-10-27-08-36-28.png, Clipboard_2020-10-27-13-05-21.png]
title: GPT3
created: '2020-10-26T17:09:39.778Z'
modified: '2020-10-27T19:32:57.846Z'
---

# GPT3

## overview
- GPT3 175B param transformer decoder language model 
- Zero-shot SOTA 
- Very strong few-shot performance

Large language models are few-shot learners
![](@attachment/Clipboard_2020-10-27-08-28-06.png)

## approach

GPT-2 model scaled up
- alternate dense and sparse attention (SparseTransformer)
- 175B params

Common Crawl filtered 
- used on similar to high quality reference
- fuzzy deduplication at doc-level
- and WebText, Books1, Books2, En-Wiki 

![](@attachment/Clipboard_2020-10-27-08-36-28.png)

Few-shot evaluation
- select $K$ random examples from training set as conditioning
- use natural language prompt for $K=0$ and others sometimes

NL evaluation
- for multiple choice, compare likelihood of each choice
- for binary, give semantic names (True, False)
- for free-form, use beam search 

## results

[Power-law scaling](@note/Neural Scaling Laws) generally continues to 175B params 

![](@attachment/Clipboard_2020-10-27-13-05-21.png)

language modelling
- zero-shot SOTA on PTB (previous was GPT-2)
- zero-shot SOTA on LAMBADA
- few-shot LAMBADA helps frame as a fill in the blank
- good but worse than SOTA on HellaSwag
- k=70 good but worse than SOTA finetune BERT on StoryCloze

closed-book QA 
- TriviaQA SOTA
  - zero-shot outperforms T5
  - one-shot matches open-domain QA with retrieval
  - few-shot gets SOTA
- WebQuestions not as good finetune SOTA
  - few-shot beats T5 but not QA-specific T5
- Natural Questions 
  - few-shot not as good as QA-specific T5

translation 
- 7% of train non-English
- one / few shot use paired examples (*not really* unsupervised)
- One-shot is competitive on unsupervised FR/DE/RO $\to $ EN
- Few-shot (64) SOTA on *supervised* FR/DE $\to $ EN, close on RO
- RO issues might be because of BPE tokenizer

winograd-style
- decent results
- zero/one/few all similar, close to SOTA on Winograd
- few-shot close, but worse than finetune RoBERTA, T5 on Winogrande

common-sense
- mixed results
- PhysicalQA SOTA
  - even zero-shot beats finetune RoBERTA
- ARC (middle school science) worse than SOTA
  - "Challenge" apporaches finetune RoBERTA
  - "Easy" edges out RoBERTA 
  - Both much worse than UnifiedQA
- OpenBookQA much worse than SOTA
  - few-shot similar to BERT L baseline, ouch

reading comprehension
- generally bad, usually baseline level
- CoQA, few-shot close to human
- DROP, discrete reasoning, better than BERT baselines, much worse than SOTA
- SQaAD v2, few-shot is good, beats baseline
- RACE, multiple-choice english test, same as first baseline
- QuAC, worse than ELMo baseline, ouch

SuperGLUE
- mixed performance across tasks but one, few-shot (32) definitely help
- outperforms fine-tuned BERT with only K=8
- still very short of fine-tuned SOTA

NLI
- Bad performance on RTE (SuperGLUE) but BERT-level with few-shot
- Essentially random on ANLI (adversarial)
  - All models are generally at random-level except GPT-3 175B on ANLIR3

Synthetic Tasks
- Arithmetic
  - GPT-3 few-shot learns arithmetic for 2,3-digit addition/subtraction
  - huge boost in performance at 13B params, even bigger 175B params
  - better than random on multiplication but still not good
  - very few exact matches for 3-digit addition in train, likely learning
- Word-scrambling
  - GPT-3 few-shot removes letters and unscrambles sometimes
  - reversing letters is always 0% accuracy, for some reason
  - kind of impressive since BPE means model has learned even smaller sub-token understanding
- SAT analogies
  - few-shot is better than average college applicant!
- news article generation
  - few-shot (3) + title,subtitle context
  - human fake news detection of 215 word article from newser
  - scale of (1) “very likely written by a human” to (5)“very likely written by a machine”
  - control model 160M parameter model with no context 
  - deception increases with model size, but not conclusive 
  - longer news article continuation has same trend (500 words)
- learning and using novel words
  - given new word (gigamaru), definition, GPT-3 can generate a plausible example sentence
  - really impressive results that show it can generally understand descriptions!

## memorization aka train/test overlap

heuristic deduplication didn't fully work and not all overlap was removed
- retraining was infeasible

comparing regular benchmark to "cleaned" benchmark, no significant difference!
- either deduplication heuristic is overly conservative or contamination doesn't make a big difference

## limitations

NLP performance has issues
- isn't SOTA on benchmarks
- text synthesis can be kinda shit
- common sense / physics is not fully learned
- in-context learning doesn't really work on some comparison tasks (WIC, ANLI)

model issues
- unidirectional means worse performance on reading comprehension
- poor pretrain sample efficiency 
- way too big! inference is expensive

pretraining objective is lacking
- weighs all tokens equally (instead of what is important)
- just predicts what is likely (not how to accomplish a task)

learning vs memorization
- is it learning from scratch at inference? or just identifies learned task?
- how does few-shot learning work?




## citation

```
@article{gpt3,
    author = {Tom B. Brown and Benjamin Mann and Nick Ryder and Melanie Subbiah and Jared Kaplan and Prafulla Dhariwal and Arvind Neelakantan and Pranav Shyam and Girish Sastry and Amanda Askell and Sandhini Agarwal and Ariel Herbert-Voss and Gretchen Krueger and Tom Henighan and Rewon Child and Aditya Ramesh and Daniel M. Ziegler and Jeffrey Wu and Clemens Winter and Christopher Hesse and Mark Chen and Eric Sigler and Mateusz Litwin and Scott Gray and Benjamin Chess and Jack Clark and Christopher Berner and Sam McCandlish and Alec Radford and Ilya Sutskever and Dario Amodei},
    title = {Language Models are Few-Shot Learners},
    year = {2020},
    booktitle = {arXiv preprint arXiv:2005.14165}
}
```
