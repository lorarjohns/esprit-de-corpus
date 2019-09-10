---
title: Dying Tongues and the Curse of Dimensionality
date: "2019-08-05T23:24:37.121Z"
layout: post
draft: false
path: "/posts/dying_tongues_dimensionality/"
category: "Data Science"
tags:
  - "probability"
  - "statistics"
  - "pandas"
  - "law"
  - "data science"
description: "Can Natural Language Processing force endangered and non-Western languages out of science?"
---

# Dying Tongues and The Curse of Dimensionality

### Can Natural Language Processing force endangered and non-Western languages out of science?

![Photo by [Kobu Agency](https://unsplash.com/@kobuagency?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)](https://cdn-images-1.medium.com/max/11138/0*-SixFFtsEfoPPXK8)*Photo by [Kobu Agency](https://unsplash.com/@kobuagency?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)*

Natural language processing has exploded in the last decade. With the rise in both computing power and availability of data, the market for mining human speech for insight is strong.

As NLP proliferates, however, we have to pay close attention to the space into which it’s expanding. Are we creating an Anglophone feedback loop with the data we collect and the tools we use to analyze it?

### Non-English linguistic features complicate NLP performed with tools designed by Anglophones.

NLP techniques, by and large, were invented by native English speakers. English tends toward what linguists call an *analytic *language. That means it has a low morpheme-per-word count and lacks the kind of grammatical markers that other languages use for things like tense, person, mood, and aspect, which it expresses instead through word order and associations with other words.

For example, in English, the following sentences are not equivalent:
> Klaus eats the salad.
> The salad eats Klaus.

But in German (a fusional language), they would both mean “Klaus is eating a salad,” because the fusional morphology indicating case marking makes the word order less strictly necessary:
> Klaus isst den Salat.
> Den Salat isst Klaus.

Even more meanings per word can occur in agglutinative and polysynthetic languages:

![](https://cdn-images-1.medium.com/max/2164/0*C93js_oC5g0l16jY.png)

But most NLP libraries are optimized for tokenizing, lemmatizing, and tagging parts of speech in English and English-like European languages.

### Low-volume, difficult-to-handle data perpetuate the bias.

Although resources have been expanding, there are still fewer options for languages other than English. In part, this conundrum stems from the bias towards applying NLP to the languages with the most data. English is still the language of the internet, and it — along with the tongues of other affluent, connected countries — is, therefore, overrepresented in the datasets. Without enough data, using traditional high-dimensional NLP techniques on smaller languages won’t be effective.

Since NLP is often used to provide high-value market insights and spur investment, this lack of attention can lead to a self-perpetuating cycle in which low-resource languages continue to receive fewer services, and thus generate less data, than better-attended ones. Ultimately, they may lose speakers, and [the world will lose linguistic diversity](http://labs.theguardian.com/digital-language-divide/).

Yet as we know on the project level, “[g]reat care should be taken not to hastily remove or change values, especially if the sample size is small.” Max Kuhn and Kjell Johnson, *Applied Predictive Modeling *33 (5th ed. 2016). “Also, the outlying data may be an indication of a special part of the population under study that is just starting to be sampled.” *Ibid. *34. This advice is no less true from the macro-level perspective of cross-linguistic data science. For historically understudied languages, this incipient exploration can make an enormous difference — for the intellectuals who study it, the businesses who can profit from that insight, and the communities who benefit from having their words acknowledged and examined.

### Remedying the gap requires careful attention.

The gap must first be narrowed at the data-collection stage. Some NLP practitioners have [begun implementing machine learning](https://dl.acm.org/citation.cfm?id=1699549) much earlier in the linguistic research process to improve the chances that the corpus will be usable — and used. Ensuring that field linguists and data scientists communicate to produce mutually beneficial, machine-readable corpora is an important consideration.

Other techniques may seek to make the best of a sparse dataset by implementing methods that do not require such high-dimensional feature creation. Semi- and unsupervised techniques can be used to skirt [Hughes phenomenon](http://37steps.com/2322/hughes-phenomenon/). To adapt existing models to entirely new structures requires not only linguistic knowledge but creative thinking. [Neural techniques](https://arxiv.org/pdf/1411.2738.pdf) are beneficial in this scenario. One group has already [used deep learning to revive Seneca](https://sciencenode.org/feature/Seneca%20deep%20learning.php). Others are [following suit, with Google’s help](https://www.forbes.com/sites/cognitiveworld/2018/11/23/turning-to-ai-to-save-endangered-languages/#52a7c0306f45).

Machine learning gives us an unprecedented opportunity to preserve and explore understudied languages that no generation before has ever had. Those who rise to the challenge will enrich not only themselves, but the collective understanding of our shared linguistic heritage.
