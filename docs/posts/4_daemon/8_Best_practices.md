# [Best practices](https://youtu.be/YPZ1FWvhf7k?t=27m42s)

earlier I removed all the
27:47
comments from a log argh stop py file so
27:50
we could focus on the code of course
27:52
it's imperative that you comment your
27:53
code and I'll review some best practices
27:55
for doing that now first it's a good
27:58
idea to leave a bit of information on
27:59
the top of the file so other devs can
28:02
open it up and quickly croc what the
28:03
plug-in does and how to use it next
28:06
let's leave standards Fink style doc
28:07
strings for each of our functions along
28:10
with info about parameters and return
28:12
types
28:22
cool done that might seem like a lot of
28:25
work but it'll really pay off in the
28:27
long run you'll get auto-generated
28:29
documentation for free if you get a
28:30
Sphynx instances spun up other
28:32
developers will be able to read and
28:34
review your plugins much more easily and
28:36
the comments will help you clarify for
28:38
yourself what the plug-in should do next
28:41
it's time to give our callback function
28:43
and plug in a more appropriate name I'll
28:45
call them both asset status update task
28:48
status they don't have to share a name
28:50
but many times do cool okay let's head
28:53
back to the daemon config file once
28:55
you've got a plug-in you believe is
28:56
useful to your production and you plan
28:58
to just leave the daemon running it's a
29:00
good idea to enter at least one email
29:02
address down here at the bottom that way
29:04
if something goes wrong and your plugin
29:05
starts throwing errors folks will get
29:07
notified by email and can address the
29:09
issue
29:10
I'll use my gmail account finally shut
29:13
the daemon down and launch it again in
29:15
start mode instead of foreground mode
29:17
that way you can close the terminal and
29:19
know the daemon is still chugging away
29:20
behind the scenes if you need to stop
29:22
the daemon just pop open the shell and
29:24
run the stop command