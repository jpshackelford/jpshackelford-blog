title = "Solo Coding with QuickNote"
date = "2011-09-13T11:29:00Z"
template = "main"
tags = []
---
While I like to code paired, I still regularly work solo when my chums are working on something else, especially since I have been working an iteration ahead of the team doing prep work for the iteration to come. I miss the focus, feedback and the speed with which one uncovers design flaws or edge cases that a solo coder tends to miss without the extra mind at work. This week I am trying something new that appears to be working well for me. 

I have been using [QuickNote](https://chrome.google.com/webstore/detail/mijlebbfndhelmdpmllgcfadlkankhok), which has become one of my favorite tools over the last year, to talk through the code as I write it. I've found that typing my thoughts rather than leaving notes in a scratch pad or lab book has improved my productivity significantly. I'll start a test, realize that I have some open problem I need to make a decision about, and then type through a conversation until I have enough of an answer to write my test and perhaps enough direction to write several more. Its so easy to Alt-Tab between my code and QuickNote that I found myself switching back and forth frequently and having to do less back-tracking.  Also having the conversation with myself in print seems to bring an extra objectivity to it that actually feels similar to pairing. I spend less time thinking in circles and more time learning and moving forward. Plus, I am left with a record of my decisions, to-do items, etc. which frees up brain cells that would otherwise be burned on anxiety, trying to remember an important thought, etc.

I love the simplicity and obviousness of this.

The following is a snippet from a coding session yesterday. I am building plugin for [Resque](https://github.com/defunkt/resque) which allows one to enqueue a directed acyclical graph of jobs while workers dequeue jobs so as to work as many as possible in parallel while respecting the dependencies expressed in the graph. (Fun!) 

<pre class="prettyprint"><code class="language-xml">Now do we really want to be marking job completion in the child process or do we want that in the parent process? What will happen if we kill a stuck job? 

Hmm. It won't be marked complete. That isn't good. So maybe this has go back to the worker... 
    
No, keep it in the child but add an on_exit block. 
    
What about platforms without fork? 
    
In that case we wouldn't be killing child processes, now would we? 
    
Hmm. Worker already uses exit! in the child to avoid at_exit handlers. 
    
Maybe we shouldn't be trying to solve this problem right now. Lets focus on getting the job marked complete. Plus, I suspect that our rescue Object in the Task is going to handle most of what we need.

Sure, it catches SystemExit so the only case we have to worry about is when child process killed with an untrappable signal. That would be easy to detect by checking $?.signaled after Process.wait in worker#perform.
</code></pre>

And then back to coding. I like that in addition to the answers I also get the impatience with being off-task or solving too much at once I get in a good pair session. Okay, back to the code...