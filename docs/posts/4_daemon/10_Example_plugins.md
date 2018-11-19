# [Example plugins](https://youtu.be/YPZ1FWvhf7k?t=32m14s)

over the years employees at
32:18
shotgun have built a variety of general
32:20
use plug-ins for the shotgun event
32:22
daemon many were donated to clients and
32:24
have enjoyed a long life doing battle
32:26
and productions at some of the largest
32:28
studios in the world that is to say many
32:31
of the plugins in the example plugins
32:33
directory here have been code reviewed
32:35
battle tested and proven in production
32:37
and may be of use to you out of the box
32:39
I want to note however that they are not
32:42
part of any shotgun product and should
32:45
indeed be considered examples we may not
32:47
offer support related to them even if
32:49
there are bugs so if you use one or more
32:52
of these plugins it's up to you to
32:53
maintain and modify them as needed let's
32:55
take a quick look at how to install one
32:57
of them and change its settings first
32:59
I'll browse to the plugin Docs and
33:00
github most have a little animated gif
33:03
that will give you a quick idea of what
33:04
the plug-in does this one is pretty
33:06
useful
33:06
it sets a field to a value when another
33:09
field actually it could be the same
33:10
field it's set to a value for this demo
33:17
though I'm going to grab the date stamp
33:19
plug-in I've got a little description of
33:23
its input parameters here that I can use
33:25
for reference cool so now all I have to
33:28
do is copy it from the examples folder
33:29
to the plugins folder that I specified
33:32
in the config file
33:43
now let's pop it open and take a look at
33:45
the arguments
33:53
okay it looks like by default plugging
33:56
the axe on shot entities when the status
33:58
field is set to Finn the SG final on
34:01
field is stamped with the current date
34:02
and time
34:03
converted Pacific time and it looks like
34:06
once the date is set it won't be
34:08
overwritten let's change this up a bit
34:10
I'll expand the trigger to work on
34:12
assets since they also have an SG status
34:14
field change the status we're looking
34:16
forward to omit and change the field to
34:19
USG amended on I'm on the East Coast so
34:21
how do I get correct time zone syntax
34:23
for that I'll head back to the dock to
34:26
see if there's a clue there ah
34:27
it wants AP ytz time zone so if I google
34:31
a bit I can quickly track down what I'm
34:33
after all set now let's try it out
34:51
hmm I'm getting a key error related to
34:53
our authentication right I forgot to add
34:56
a script key for date stamped up py to
34:58
my shotgun site
35:15
all set let's try again oops looks like
35:18
our validator is saying omen isn't a
35:19
valid status I just need to change it to
35:22
om T now what it looks like I need to
35:26
add an S geo mettaton field to the shot
35:28
entity that's probably also true of the
35:30
asset entity so I'll add those fields
35:32
now
35:44
lucky those validators were there to
35:46
help fix my arguments great the plugin
35:52
loaded let's test it out on shots
36:00
and now assets
36:04
very cool you can see how flexible this
36:07
plugin is how the plug-in file could be
36:09
duplicated given a different name and
36:11
different arguments of course the same
36:13
is true of most of the plugins in the
36:15
examples folder although there are only
36:17
25 or so plugins here their arguments
36:20
could be configured to handle hundreds
36:21
or thousands of use cases of course
36:24
there are also a great reference for
36:25
building your own unique daemon plugins
36:27
from scratch