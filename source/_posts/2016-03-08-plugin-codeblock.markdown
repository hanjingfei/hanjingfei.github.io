---
layout: post
title: "plugins of code highlighting"
date: 2016-03-08 17:42:52 +0800
comments: true
categories: 
---
## Code Highlight by using "Code Block"

### display effect
{% codeblock lang:verilog intital example %}
initial begin
    clkn_rstn   = 0;
    repeat(10)@(posedge clkn);
    #1;
    clkn_rstn   = 1;
end
{% endcodeblock %}

### source
{% codeblock Code Block Syntax %}
{% raw %}
{% codeblock lang:verilog intital example %}
initial begin
    clkn_rstn   = 0;
    repeat(10)@(posedge clkn);
    #1;
    clkn_rstn   = 1;
end
{% endcodeblock %}
{% endraw %}
{% endcodeblock %}

## Code Highlight by using "Backtick Code Blocks"
### display effect
``` verilog
initial begin
    clkn_rstn   = 0;
    repeat(10)@(posedge clkn);
    #1;
    clkn_rstn   = 1;
end
```
### source
{% codeblock Backtick Code Blocks Syntax %}  
{% raw %}  
	``` verilog
	initial begin
		clkn_rstn   = 0;
		repeat(10)@(posedge clkn);
		#1;
		clkn_rstn   = 1;
	end  
```
{% endraw %}
{% endcodeblock %}

The source parts are displayed by `{{ "{% raw " }}%}` and
 `{{ "{% endraw " }}%}` which escape liquid tags.

{% codeblock Liquid Escape Syntax in codeblock %}{% raw %}  
&#123;% codeblock %&#125;
&#123;% raw %&#125;
some code here
&#123;% endraw %&#125;
&#123;% endcodeblock %&#125;
{% endraw %}{% endcodeblock %}  

{% codeblock Liquid Escape Syntax inline %}{% raw %}  
{{ "{% this is your liquid syntax and not bein processed by octopress. It will show in your post as a source code." }}%} 
or `{{ "{% raw " }}%}` 
or `{{ "{% endraw " }}%}`
{% endraw %}{% endcodeblock %}  