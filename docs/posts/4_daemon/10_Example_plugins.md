# [Example plugins](https://youtu.be/YPZ1FWvhf7k?t=32m14s)

Over the years employees at shotgun have built a variety of general use plug-ins for the shotgun event daemon.

Many were donated to clients and have enjoyed a long life doing battle and productions at some of the largest studios in the world.

That is to say many of the plugins in the example plugins directory here have been code reviewed battle tested and proven in production.

And may be of use to you out of the box.

I want to note however that they are not part of any shotgun product and should indeed be considered examples.

We may not offer support related to them even if there are bugs.

So if you use one or more of these plugins it's up to you to maintain and modify them as needed.

Let's take a quick look at how to install one of them and change its settings.

First I'll browse to the plugin Docs and github.

Most have a little animated gif that will give you a quick idea of what the plug-in does.

This one is pretty useful.

It sets a field to a value when another field actually it could be the same field it's set to a value.

For this demo though I'm going to grab the date stamp plug-in.

I've got a little description of its input parameters here that I can use for reference.

Cool.

So now all I have to do is copy it from the examples folder to the plugins folder that I specified in the config file.

Now let's pop it open and take a look at the arguments.

Okay it looks like by default plugging the axe on shot entities.

When the status field is set to Finn the SG final on field is stamped with the current date and time converted Pacific time.

And it looks like once the date is set it won't be overwritten.

Let's change this up a bit.

I'll expand the trigger to work on assets since they also have an SG status field change the status we're looking forward to omit and change the field to USG amended on.

I'm on the East Coast.

So how do I get correct time zone syntax for that.


I'll head back to the dock to see if there's a clue there.

Iit wants AP ytz time zone.

So if I google a bit I can quickly track down what I'm after.

All set n

ow let's try it out.

Hmm. I'm getting a key error related to our authentication.

Right I forgot to add a script key for date stamped up py to my shotgun site.

All set let's try again.

Oops looks like our validator is saying omen isn't a valid status.

I just need to change it to om T now what.

It looks like I need to add an S geo mettaton field to the shot entity.

That's probably also true of the asset entity so I'll add those fields now.

Lucky those validators were there to help fix my arguments.

Great the plugin loaded let's test it out on shots.

And now assets.

Very cool.

You can see how flexible this.

Plugin is how the plug-in file could be duplicated given a different name and different arguments.

Of course the same is true of most of the plugins in the examples folder although  there are only 25 or so plugins here their arguments could be configured to handle hundreds or thousands of use cases.

Of course there are also a great reference for building your own unique daemon  plugins from scratch.