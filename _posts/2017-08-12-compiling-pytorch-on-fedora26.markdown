---
layout: post
title:  "Compiling pytorch in Fedora 26"
date:   2017-08-12 22:24:49 +0300
categories: fedora pytorch
comments: true
---
One way of installing pytorch on Fedora 26 for Cuda 8.0 is to do the following:

1. Compile from source an old gcc version. I used the latest version in the 5 series, 5.4.0 . The compiler source may be downloaded from: https://ftp.gnu.org/gnu/gcc/gcc-5.4.0/ . Note that this is quite a time consuming compilation.

2. Compile pytorch with the following command line:

{% highlight bash %}
env CC=/usr/local/gcc-5.4.0/bin/gcc  CXX=/usr/local/gcc-5.4.0/bin/g++ CUDA_NVCC_FLAGS="-ccbin=/usr/local/gcc-5.4.0/bin/g++" python setup.py build
{% endhighlight %}


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