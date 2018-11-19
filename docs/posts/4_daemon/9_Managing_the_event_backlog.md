# [Managing the event backlog](https://youtu.be/YPZ1FWvhf7k?t=29m26s)

at this point you
29:29
might as well kick up your feet
29:31
let the daemon do all the work and just
29:33
wait for your next paycheck that'll
29:35
probably work for a week or so but
29:37
eventually you'll probably run into a
29:39
situation or a specific plugin crashes
29:41
on a weird event you'll get an email
29:43
when that happens or you want a specific
29:46
plugin to reprocess old events so how do
29:49
we do that well remember that ID file we
29:52
pointed to earlier in the config file
29:54
this little file stores one a list of
29:56
plugins to the ID of the last event each
30:00
plugin processed and three the backlog
30:03
of events the plugins still has to
30:04
process so by manipulating this file we
30:08
can effectively fast-forward or rewind
30:10
specific plugins to an exact point in
30:13
the event log history if you're looking
30:16
at this file and thinking wTF is this
30:18
then you're likely not familiar with the
30:21
Python pickle file format no worries
30:24
I'll show you how to load up the ID file
30:26
in Python and make some changes start by
30:29
making sure the daemon is shut down
30:30
next fire up a Python session important
30:34
pickle module and use it to load
30:35
whatever is in our IT file cool looks
30:44
like we've got a dictionary of stuff
30:45
organized by plug-in paths in our case I
30:49
only specified one plug-in path so I'll
30:51
just grab that and toss it in a
30:53
dictionary called desktop plugins let's
31:00
see what we've got in here now looks
31:02
like two plugins if I pop them open we
31:05
get a tuple with two bits of information
31:06
the first is the ID of the last event
31:09
log entry that plug-in processed and the
31:12
second is a backlog dictionary of events
31:15
the plug-in still needs to process most
31:17
of the time you'll be in a situation
31:18
like this with an empty backlog or
31:21
you'll only need to modify the ID so
31:24
let's do that for kicks I'll rewind our
31:27
log arcs plug in two events into the
31:29
past and I'll fast forward our entity
31:32
status update task status plug-in two
31:35
events into the future now I can just
31:37
dump our changes out to the ID file and
31:40
we're done let's see what happens when
31:43
we fire up the daemon cool exactly what
31:46
we wanted the log Argus not py plug-in
31:48
reprocessed two old events and the
31:51
entity status update task status plug-in
31:54
is telling us it won't process any
31:56
events until this one is passed
32:03
that's it a bit messy but super useful
32:06
in a pinch and if you get good at
32:08
cracking the ID file you could just edit
32:10
it manually but make sure to make a
32:12
backup first