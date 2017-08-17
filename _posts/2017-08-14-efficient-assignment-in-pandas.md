---
layout: post
title:  "Beware of unefficient assignments in pandas"
date:   2017-08-14 23:06:10 +0300
categories: python pandas
comments: true
---
One of the double edged swords in pandas is that it is possible to update a DataFrame in lots of different ways. At work I often find myself generating part of the data in the begininng of a process, and as the algorithm progresses, I fill it in with more data. As I did this today, I was hit with a mysterious time consuming bottleneck. After investigating this further, did I find that the reason for the long run time was my non efficient updating of the dataframe.

Consider the following code, which creates a dataframe and then creates three placeholder columns A, B, C:

{% highlight python %}
import pandas as pd
import numpy as numpy
pd.options.display.float_format = '{:,.0f}'.format
import time

df = pd.DataFrame(numpy.random.rand(20,20)*100)
df.loc[:,'A'] = None
df.loc[:,'B'] = None
df.loc[:,'C'] = None
{% endhighlight %}

I will now loop over the dataframe rows and update these columns in three different ways.

First attempt, update the row, and assign it back to the dataframe:

{% highlight python %}
t0 = time.time()
for idx,row in df.iterrows():
  row.loc[('A','B','C')] = (100+idx,200+idx,300+idx)
  df.loc[idx] = row
print 'First:  ', time.time()-t0
{% endhighlight %}

On my machine at work this takes 0.156s.

Second attempt, make use of the fact that `row` is actually a back reference in to `df`. The problem is that during my experiments the back reference broke. I'm not even sure what I did.

{% highlight python %}
t0 = time.time()
for idx,row in df.iterrows():
  row.loc[('A','B','C')] = (100+idx,200+idx,300+idx)
print 'Second: ', time.time()-t0
{% endhighlight %}

On my machine this took 0.010s . That is more than a fifteen time speedup.

The third attempt is in its simplicity is the absolute winner.
{% highlight python %}
t0 = time.time()
for idx,row in df.iterrows():
  df.set_value(idx,'A', 100+idx)
  df.set_value(idx,'B', 200+idx)
  df.set_value(idx,'C', 300+idx)
print 'Third:  ', time.time()-t0
{% endhighlight %}

This took 0.00344s . 

I.e. just by rewriting the assignments, I got a 45 times speedup!

# Continuation 2017-08-17 Thu

After writing the above lines did I learn that `set_value()` will be deprecated. But there is another accessor `at` that is almost as fast:

{% highlight python %}
t0 = time.time()
for idx,row in df.iterrows():
  df.at[idx,'A'] = 100+idx
  df.at[idx,'B'] = 200+idx
  df.at[idx,'C'] = 300+idx
print 'Forth:  ', time.time()-t0
{% endhighlight %}

This took 0.0044s , about 25% slower, but apparently this constract is "more" correct.

# References

- [https://github.com/pandas-dev/pandas/issues/15269#issuecomment-322571210](https://github.com/pandas-dev/pandas/issues/15269#issuecomment-322571210)

{% if page.comments %}
<div id="disqus_thread"></div>
<script>

/**
*  RECOMMENDED CONFIGURATION VARIABLES: EDIT AND UNCOMMENT THE SECTION BELOW TO INSERT DYNAMIC VALUES FROM YOUR PLATFORM OR CMS.
*  LEARN WHY DEFINING THESE VARIABLES IS IMPORTANT: https://disqus.com/admin/universalcode/#configuration-variables*/
/*
var disqus_config = function () {
this.page.url = PAGE_URL;  // Replace PAGE_URL with your page's canonical URL variable
this.page.identifier = PAGE_IDENTIFIER; // Replace PAGE_IDENTIFIER with your page's unique identifier variable
};
*/
(function() { // DON'T EDIT BELOW THIS LINE
var d = document, s = d.createElement('script');
s.src = 'https://dovg.disqus.com/embed.js';
s.setAttribute('data-timestamp', +new Date());
(d.head || d.body).appendChild(s);
})();
</script>
<noscript>Please enable JavaScript to view the <a href="https://disqus.com/?ref_noscript">comments powered by Disqus.</a></noscript>
                            
{% endif %}
