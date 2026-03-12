# Scalability for time-horizon complexity in LLMs

The recent publication by METR regarding the expanding time-horizon complexity
of LLMs is an interesting piece of reserach which illustrates how the
capabilities of models has changed as a function of time. 

Seen below, the figure compares the success rate for various LLMs of a
particular prompt as a function of how long it would take a human expert to do
it (the time horizon) and plots it against their release date. THe result is an
exponential curve, which has been received relatively enthusiastically overall.
Researchers working towards the development of frontier models have embraced it
as a sign that exponential growth in the sector is not only possible but
confirmed. 

It seems like a pretty surefire conclusion to make, but in this post I'll be
making the argument that AI capability progress is largely a function of
investment in AI, and that if we are to make statements about the future of the
field then we need to look at it through a lens of model size, training compute
and investment rather than simply a function of time.

Unfortunately, obtaining actual reliable data about the model size,
architecture, training setup, etc. is understandably difficult. The next best
option is to rely on data leaks or guesstimates, which is precisely what the
folks over at Epoch AI are doing. 

They've compiled a range of best available estimates for most publically
available LLMs, where possible. In open-source models the estimates are precise,
in private models such as GPT-5 they do what they can. One of the things they
track is model size (parameters) and training compute (FLOPs). 

## Method

Put simply, we'll take a look at the list of models compiled by Epoch AI,
cross-reference it with the models tested for time-horizon capability by METR,
and then filter for the models for which Epoch has some estimate of training
compute (reliable or otherwise). We'll then plot the time-horizon data against
the training compute data in a log-log scale, and infer what we can. 


## Results

Before I show the results, a few caveats:
- Knowledge about training compute is tentative: The error bars on the y-axis
  are well established but in most private models we don't know what the true
  training compute was for certain, so you'll have to imagine error bars on the
  x-axis for these data points. 
- Training compute isn't the main driving factor, one would expect. Most models
  are running reasoning loops, meaning that inference compute time factors in
  significantly, inference architecture (i.e. mixture-of-experts architecture,
  routing, good reasoning loops etc.) is massively important, and the effect of
  training compute may be slightly diminished.
- To temper the previous two points, if you're designing a model and you have an
  increased computation budget, you're probably going to allocate at least some
  of the additional budget to more training. We're no longer full-sending all
  the money into collecting more training data or increasing model size, but we
  are still doing those things. The following figures do not represent a hard
  and fast scaling analysis, but rather are indicative of spending habits as a
  whole in the field. In short, training compute is correlated with investment. 

Now let's examine the data. 

<img src="/assets/images/p50_scaling.png" alt="Image not available">

In the above image we're plotting a log-log graph for models which have both
METR time-horizon data and Epoch estimates for training compute. A few key
notables:
- Qwen models seem to underperform relative to their training compute neighbours
  (Deepseeks, Kimi and gpt-oss)
- Grok is another underperformer, using significantly more FLOPs than any other
  model but gaining only marginally in capability 
- The underperformance of grok and Qwen may be due to a lack of high quality
  training data and relatively subpar architecture. 
- Conversely, the success of Kimi and Deepseek could be due to the recent
  allegations of distillation attacks by Anthropic; the claim would be that the
  models did not use as much training compute because they piggybacked off of
  Anthropic (allegedly)

Fitted to the data via linear regression is a power law of the form $T =
C^{0.48}$, which we'll round up to a square root law. In other words, when
plotting time horizon capability against training compute, if we want to get a
10x increase in model capability we need a 100x increase in training compute. 

<img src="/assets/images/compute_time_inference_p50.png" alt="Image not available">

Following on from this, we now use this trendline to estimate the training
compute of models we have no information on. We generate error bars on the
compute by applying the inverse mapping to the 5th and 95th quantile time
horizon error bars. This is just for fun. 

## Takeaways

It's pretty difficult to gauge exactly how much money is going into each model,
understandably so. We have partial information about investment in various
forms, via model architecture and compute time, leaks about inference compute
cost (which appear to be massive) and information about data centers being
built. Through this partial information we may be able to stitch together a
picture of what the capability landscape of upcoming models may cost. 

Trends in frontier model development seem to be in acquiring high quality
training data, enforcing high-quality reasoning loops, using reinforcement
learning and neurosymbolic style systems to increase performance. I believe we
can expect then that the vast majority of the cost of a model no longer lies in
training compute, so then it seems quite alarming that we're on a square-root
pattern in training compute! 

Again I need to stress that it's difficult to estimate exactly how much money is
going on inference, data center construction, paying for high quality data etc.
But let's make some estimates anyway. 

We've seen that frontier models take about 10^25 FLOPs to train. From what I've
seen, it takes about 2N FLOPs per generated token (N is number of active
parameters) and let's assume parameter size is a nice round 1 trillion, with fewer
than a trillion active at any time due to MoE architecture, let's assume 10
billion. This means about $2 \times 10^{10}$ FLOPs per token. 

Standard model outputs may be about 500 tokens, but reasoning outputs,
particularly for complex tasks, may reach 10,000 to 50,000 tokens. At 10,000
tokens, a single query costs $2 \times 10^{15}$ FLOPs. To reach the training
compute threshold, a model needs to serve approximately 5 billion complex
queries. For a model deployed on a global scale, and when considering the many
more frequent simple queries, the constant uses of "read this long document and
answer this simple question", I suspect inference outweighs compute in a matter
of months. Over a life cycle of a year to two years, I would expect a total
inference compute somewhere between one to two orders of magnitude above
training compute. Given that adjustment, we can then estimate that the effective
compute scaling laws will sit somewhere between what we have now (exponent =
0.5) and a cubic scaling law (exponent = 0.33) based on the data we've seen
above. 

So, if we're now expecting that a 10x increase in model capability comes at a
100x to 1000x increase in cost (and to reiterate, this estimate completely
ignores the costs of constructing data centers and paying for good data), maybe
we should ask if this should be the way forward. Perhaps we need to pivot to
models with better token efficiency but lower overall performance? 



