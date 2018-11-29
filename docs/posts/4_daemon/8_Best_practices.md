# [Best practices](https://youtu.be/YPZ1FWvhf7k?t=27m42s)

Earlier I removed all the comments from a log argh stop py file so we could focus on the code.

Of course it's imperative that you comment your code and I'll review some best practices for doing that now.

First it's a good idea to leave a bit of information on the top of the file.

So other devs can open it up and quickly croc what the plug-in does and how to use it.

Next et's leave standards Fink style doc strings for each of our functions along with info about parameters and return types.

Cool done that.

Might seem like a lot of work but it'll really pay off in the long run.

You'll get auto-generated documentation for free if you get a Sphynx instances spun up.

Other developers will be able to read and review your plugins much more easily and the comments will help you clarify for yourself what the plug-in should do.

Next it's time to give our callback function and plug in a more appropriate name.

I'll call them both asset status update task status.

They don't have to share a name but many times do.

Cool okay let's head back to the daemon config file.

Once you've got a plug-in you believe is useful to your production and you plan to just leave the daemon running it's a good idea to enter at least one email address down here at the bottom.

That way if something goes wrong and your plugin starts throwing errors folks will  get notified by email and can address the issue.

I'll use my gmail account.

Finally shut the daemon down and launch it again in start mode instead of foreground mode.

That way you can close the terminal and know the daemon is still chugging away  behind the scenes.

If you need to stop the daemon just pop open the shell and run the stop command.