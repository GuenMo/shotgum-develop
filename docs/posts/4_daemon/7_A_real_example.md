# [A real example (실제 예시)](https://youtu.be/YPZ1FWvhf7k?t=23m6s)

23:09
okay now let's create something that
23:11
might actually be useful we've already
23:13
got a plug-in that fills in an assets
23:15
description field when that assets
23:17
status field is set to you on hold
23:19
let's extend our plug-in let's get all
23:22
the tasks linked to that asset and set
23:24
their statuses to on hold - unless
23:27
they're already set to final or n/a I'll
23:29
need to run multiple update commands to
23:31
accomplish this so I'm going to
23:33
initialize a batch data command list and
23:35
change this update call to be part of it
24:17
next if we're going to update tasks
24:19
status fields we should make sure the SG
24:21
status list field is in the schema again
24:24
this is super paranoid especially since
24:26
it's impossible to delete this field
24:27
from the GUI but I'll do it here anyway
24:29
as an exercise if it was a custom field
24:32
that didn't exist on all entity types
24:34
this check would make more sense after
24:36
that we can pull down all the tasks
24:38
linked to the entity and log and update
24:40
requests with our batch data list
25:02
remember we don't want to change tasks
25:04
at the final or n/a so I'll add another
25:07
setting for that maybe I'll call it skip
25:09
statuses and then skip tasks set to
25:12
those statuses cool almost done
25:32
finally we can pass our batch data list
25:34
to an SG batch command and tell the
25:37
logger about it
26:11
let's see if it works
26:16
great just what we expected let's check
26:19
the output in the terminal coal so now
26:22
we've got a plugin that filters for
26:23
specific event types validates relevant
26:26
schema fields and incoming events checks
26:28
field data for expected values based on
26:30
the event and finally if everything
26:32
looks right it performs a little
26:34
automation with the shotgun Python API
26:37
I'll leave it up to you to judge whether
26:39
what we've created here would be helpful
26:41
in your production personally I would
26:43
only use a plugin like this if I had a
26:45
lot of small assets in flux whose many
26:48
tasks were distributed across remote
26:50
artists and my producers were spread
26:52
thin if instead assets at my studio are
26:55
infrequently put on hold
26:56
I'd prefer to trust my producer to
26:59
manage the statuses and message out
27:01
changes personally but that's just me
27:03
and of course you're in complete control
27:05
and free to decide what your custom
27:07
automation should or shouldn't do
27:08
writing daemon plugins is a bit of an
27:10
art and the best way to learn is to
27:12
write a few finally I'd like to say that
27:14
I believe I'm overly paranoid for a good
27:16
reason after writing a large number of
27:18
event daemon plugins I've seen about
27:20
everything go wrong that can go wrong I
27:22
wouldn't necessarily run so many checks
27:24
in say toolkit app or standalone script
27:27
but the daemon related coding habits I'm
27:30
passing on here are hard one of course
27:33
maybe you think this plug-in is overly
27:35
complex and every developer is free to
27:37
walk his or her own path of many lessons
27:39
far be it for me to keep those lessons
27:41
from you