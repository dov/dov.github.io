---
layout: post
title:  Can't use Vulkan on GTX 1070"
date:   Fri 17 May 2019 04:35:58 PM IDT
categories: NVIDIA vulkan
comments: true
---
I few years back I bought a GTX 1070 to be used for learning about machine learning and computer graphics. But one of the issues that has failed me is running Vulkan on this chip. During the last couple of months I have had a continuous email exchange with a representative for NVIDIA trying to resolve this issue. So far without any success.

My system is as follows:

- OS: Fedora 30
- NVidia driver 430.09

Any program I run ends with the following error:

```
$ ./hello-triangle
validation layer: Added messenger
found 1 physical devices
validation layer: terminator_CreateDevice: Failed in ICD libGLX_nvidia.so.0 vkCreateDevicecall
validation layer: vkCreateDevice:  Failed to create device chain.
failed to create logical device!
```

I'll update this page with a gist to hello-triangle, but meanwhile I wanted to create this page in case anyone else has encountered the same error and has a solution.

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
