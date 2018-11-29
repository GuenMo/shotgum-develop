# [Managing the event backlog](https://youtu.be/YPZ1FWvhf7k?t=29m26s)

At this point you might as well kick up your feet let the daemon do all the work and just wait for your next paycheck.

That'll probably work for a week or so but eventually you'll probably run into a
situation or a specific plugin crashes on a weird event.

You'll get an email when that happens.

Or you want a specific plugin to reprocess old events.

So how do we do that well remember that ID file we pointed to earlier in the config file.

This little file stores.

One a list of plugins.

Two the ID of the last event each plugin processed and.

Three the backlog of events the plugins still has to process.

So by manipulating this file we can effectively fast-forward or rewind specific plugins to an exact point in the event log history.

If you're looking at this file and thinking wTF is this.

Then you're likely not familiar with the Python pickle file format.

No worries I'll show you how to load up the ID file in Python and make some changes.

Start by making sure the daemon is shut down.

Next fire up a Python session important pickle module and use it to load whatever  is in our IT file.

Cool looks like we've got a dictionary of stuff.

Organized by plug-in paths.

In our case I only specified one plug-in path.

So I'll just grab that and toss it in a dictionary called desktop plugins.

Let's see what we've got in here now.

Looks like two plugins if I pop them open we get a tuple with two bits of nformation.

The first is the ID of the last event log entry that plug-in processed .

And the second is a backlog dictionary of events the plug-in still needs to process.

Most of the time you'll be in a situation like this.

With an empty backlog or you'll only need to modify the ID.

So let's do that.

For kicks I'll rewind our log arcs plug in two events into the past and I'll fast forward our entity status update task status plug-in two events into the future.

Now I can just dump our changes out to the ID file and we're done.

Let's see what happens when we fire up the daemon.

Cool exactly what we wanted.

The log Argus not py plug-in reprocessed two old events and the entity status update task status plug-in is telling us it won't process any events until this one is passed.

That's it.

A bit messy but super useful in a pinch.

And if you get good at cracking the ID file you could just edit it manually but make sure to make a backup first.