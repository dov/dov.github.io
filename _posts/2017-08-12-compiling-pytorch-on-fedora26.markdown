---
layout: post
title:  "Compiling pytorch in Fedora 26"
date:   2017-08-12 22:24:49 +0300
categories: fedora pytorch
comments: true
---
It is not trivial to compile pytorch on Fedora 26 as the gcc compiler 7.1.1 is not compatible with Cuda 8.0. This note describes a way around this problem.

1. Compile from source an old gcc version that is compatible with cuda. I used the latest version in the 5 series, 5.4.0 . The compiler source may be downloaded from: [https://ftp.gnu.org/gnu/gcc/gcc-5.4.0/](https://ftp.gnu.org/gnu/gcc/gcc-5.4.0/) . Note that this is quite a time consuming compilation.

2. Compile pytorch with the following command line:

{% highlight bash %}
env CC=/usr/local/gcc-5.4.0/bin/gcc  \
    CXX=/usr/local/gcc-5.4.0/bin/g++ \
    CUDA_NVCC_FLAGS="-ccbin=/usr/local/gcc-5.4.0/bin/g++" \
    python2 setup.py install --prefix=/usr/local
{% endhighlight %}

3. Make sure that your `PYTHONPATH` includes the following two directories:
{% highlight bash %}
export PYTHONPATH=/usr/local/lib/python2.7/site-packages:\
                  /usr/local/lib64/python2.7/site-packages:
{% endhighlight %}

4. Test that everything works:
{% highlight bash %}
ipython2
... import torch
... torch.version.__version__
{% endhighlight %}

5. To install `pytorch-vision` apply the following path, whereupon `pytorch-vision` may be installed with a simple `python setup.py install`.
{% highlight patch %}
diff --git a/setup.py b/setup.py
index a83d889..d2a16bc 100644
--- a/setup.py
+++ b/setup.py
@@ -13,7 +13,6 @@ requirements = [
     'numpy',
     'pillow',
     'six',
-    'torch',
 ]
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
