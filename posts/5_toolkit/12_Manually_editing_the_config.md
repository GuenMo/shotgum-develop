# [구성 직접 편집](https://youtu.be/5nRZ5GgcOnk?t=35m40s)

Next let's edit the ml files by hand to install a couple new apps

If we visit the integrations apps and engines support page.

We can browse for new apps that might be fun to play with. 

How about the shotgun toolkit demos app.

If I pop open this link it gives me the current version number.

Cool.

I'll open the config in my text editor and follow the breadcrumbs two desktops project environment.

We're going to cover the anatomy of toolkit configurations in other videos so don't worry if you don't follow all of this.

Now I'll copy the format here and add an entry for DK multi demo that points to the apps ml
file in the includes common directory
and then pop up in that file and add an
entry for TK multi demo
that's it now if I navigate to any of
our projects again the demo app is
downloaded to my computer and appears
here it is in another project pretty
cool right
incidentally once the app is downloaded
for one project it doesn't have to be
downloaded again for other projects so
loading stays snappy I'd also love a
toolkit app template so I can start
building my own apps it doesn't look
like there's anything here but if we
visit the shotgun software a github org
there's a repo called TK multi a starter
app I could use a git descriptor to pull
this app directly from github but that's
not much used to me because duh the
point of the starter app is to change it
and we don't have permission to change
shotgun software yet how propose I mean
I do but you don't
instead let's download the latest
released version or clone it up to you
and then point to it with a dub
descriptor edit those same two files in
our config to install it
again check out that talk about
descriptor types and their attributes if
you're curious
great looks like we have a starter app
I'm going to load it up in my text
editor and change the app name what
should we call it it might be fun to try
out one of the demos in TK multi demo
how about the Activity Stream demo cool
so I'll call my app TK multi Activity
Stream and copy over some code from the
example we'll talk about how to write
toolkit apps later so no worries if this
is a bit over your head
now if I reload the project environment
and launch the app I can see my changes
awesome
let's leave a couple notes on our
project for kicks
and let's see what that looks like in
shotgun suite next I can upload my
config to shotgun and artists working on
any project will instantly see the new
demo app in my TK multi activity stream
wait a minute how am I going to
distribute the changes to my Activity
Stream app
houston we have a problem how do we
distribute this sweet new app well we
can do it with descriptors and there are
heaps of options but I'm going to
recommend to the shotgun descriptor type
again first I'll create a toolkit app
non project entity type and add an
uploaded app file link field
remember to create an all toolkit apps
page
then I'll create a new toolkit app
entity named TK a multi activity stream
and create a new github repo with the
same name I'm sure you can see where
this is going
I'll copy and upload the code from TK
multi starter app to TK multi activity
stream
create a tagged release download it
upload it to my TK multi activity stream
toolkit app entity replace references to
TK multi starter app with TK multi
activity stream and swap out the dev
descriptor for shotgun descriptor that
points to it
this part is a little bit tricky there
are two IDs that need to be accounted
for the first is the ID of the current
toolkit app entity easy enough to grab
from the ID field the second is the ID
of the related attachment entity one way
to discover that is by adding a files
tab to the toolkit app entity types
detail page now you've got a list of all
the app payloads you've uploaded through
this toolkit app entity and their IDs so
for example if I upload this app again I
have two IDs to choose from I'll just
grab the first ID and pop it in the
script er let's see if it loads Suite
one super convenient side-effect of this
workflow I'm sure you noticed is that we
can use the fields on the toolkit app
entity to track all kinds of things
about it maybe it's development status a
quick description a link to its github
repo more explicit version info sky's
the limit note that you could always
rename toolkit app to toolkit component
if he started building frameworks or
engines and wanted to host apps engines
and frameworks with a single non project
custom entity when we originally took
over the config we copied code from the
TK config basic repo to RTC a config
learn repo and then started modifying it
and TK config basic is a great place to
start if you're building tools for
artists who don't have access to a
shared file system but if you do have
access to a shared file system there's
another config that shotgun software
manages called TK config default to that
is a much better choice as a starting
point it contains a bunch of high value
apps to help artists work together on
you guessed it a shared file system oh
crap why did I show you all this stuff
about how to modify TK config basic if
we were just going to ditch it and
switch to a config
based on the TK config default to repo
hold on hold on hold your horses we're
not going to ditch our TK config Learn
config it's still super useful to host a
config that contains a set of tools that
will work anywhere so here's what I'm
going to do I'm going to rename primary
to a remote to signal to my artists
that it contains tools that will work
when they're off site or working from
home if you want to limit the number of
artists who have off-site access to the
config you could add them to the user
restrictions field here next I'm going
to make a second global site config
called on-site and lock that down to an
app store version number of TK config
default - but what version do we use
well the toolkit team releases TK config
basic and TK config default - on the
same schedule so if the most recent
version of TK config basic is one point
one point five then TK config default -
is also on version one point one point
five and again you can always visit the
integrations release notes page to
investigate version numbers sometimes
there are special version releases that
only pertain to one of the configs now
that we've taken primary out of the mix
what's going on
well shotgun desktop site environment
fallback logic prefers the pipeline
configuration entity with the lowest ID
number I find it helpful to sort them by
ID so I can see the fallback order and
change it if necessary I don't like to
use a capital P primary config if there
are more than one configures in the mix
because desktops fallback logic will
choose a dead last and I prefer a more
explicit control of the config fallback
order cool so I'll restart desktop to
make sure I didn't break anything
oops I'm getting an error it's telling
me I haven't filled out the tank name
field on my project what's up with that
well TK config default to needs to know
a bit about your file system before the
various file system related tools it
contains can initialize first it needs
to know what to name the projects root
folder you can specify that in a
projects tank name field cool done but
let's make this future-proof let's make
a tank name a required field on project
creation so it's never empty
done and let's go back and fill in tank
name fields for old projects done next
TK config default to needs to know where
to create the folder you specified in
the tank name field to tell it that head
over to your site preferences open up
the file management tab and spend some
time choosing the route path for all
your projects everything related to your
projects will live under here assets
sequences shots I files image sequence
files editorial files etc you can see
I've already got mine set great let's
load up the on-site project environment
for hyperspace madness now and see what
happens
voila I get more options compared to TK
config basic if I launch Maya I've got
this fancy file open option in the
shotgun menu now and I can create files
and folders on disk related to tasks
that are assigned to me and then publish
files to shotgun let's see what that
publish looks like in shotgun
cool it's keeping track of the local
file path there's a ton more
functionality in the file system related
apps that I'd love to show you but
that'll be covered in other videos
finally I want to reiterate that now any
project you create will quote just work
because again the tank name field is
required let's try to make a new project
for kicks to test it out
I'll make a new asset and then run the
make folders commands
nice
let's pretend I've been working on this
asset for a little while as an artist
and my producer says guess what the name
of this asset has changed here's the new
name what do I do
hmm maybe I should run the create
folders command again then I can
manually move files from the old folders
to the new ones sounds like a reasonable
plan right let's try it
oops that didn't go well
shotgun is telling me that I have to run
a tank command to unregister this asset
entity from the original folder set
before I can create a new folder set
what a pain in the asset two artists
really have to contact a shotgun
administrator every time an asset she or
he is working on gets a name change
actually this one comes up all the time
it's one of my toolkit pet peeves let's
transform frustration into opportunity
and create a little toolkit up for our
artists so they can unregister folders
themselves and make it available from
the right-click menu in shotgun what do
we have to do to make that happen
well this menu here is populated by
shotgun desktop when it's running based
on instructions in the active config
each of these options activates a
toolkit app so right to control this
list first we have to take over
TK config default to I'll create a new
github repo for our TK config default to
based config a mentee can fake learn on
site and create the same branch setup we
used for TK config learn I'll spare you
the pain of watching me do that again
then point our dev can figure out it
might as well run my update command to
grab the most recent versions of my apps
engines and frameworks
and install our demo and Activity Stream
apps
cool now we're managing two different
site configs one based on TK config
basic and one based on TK config default
to next I'll download the TK shotgun
folders app since the app we want to
write is probably going to be similar
remember to check our integrations page
for the current approved version and
I'll pop it open and take a look
oh sweet here's the create folders
command and here's the preview folders
command you know now that I'm looking at
this it might make more sense to add our
unregister folders command to this app
since it already contains multiple
commands related to managing folders
let's do that
cool now we can point to it from our
local copy of ta config default to
there's my command
hey it works now I can run the create
folders come in and proceed with my
original plan to manually migrate files
now let's make another entry on our
toolkit ops page for TK shotgun folders
even though in the description and toss
it in a repo that we control with that
same branch setup you know the drill
remember to swap out your dev
descriptors for shotgun descriptors
before publishing the config that's
something your VCS can really help catch
hmm how do you feel about what we just
did you have a little creepy tingly
feeling like something is wrong right
since we just forked the app we won't
get any updates to Tk shotgun folders
from shotgun software
bummer so what should we do we could
break our command out as a new app
that's one option
but we could also create a pull request
in the shotgun software organization
repo an email support to see if our
change is something the folks at shotgun
might want to integrate into the
official app then we don't have to
manage our own version of it we help out
shotgun software and folks at other
studios we'll get our sweet update in
fact many great features in our apps are
added by Deb's and other Studios when
win-win in other videos we'll take a
look at hooks which are a way to modify
and add functionality to apps without
having to fork them but that's a little
out of scope here
let's pause and review the differences
between TK config basic and TK config
default to first as you remember TK
config basic is what shotgun desktop
installs on your computer if you and
your artists do absolutely nothing
it's out-of-the-box toolset works
without a shared file system which means
it can be used anywhere so long as users
can access your shotgun site if you take
over TK config basic it's a good idea to
add tools that will also work anywhere
without a dependency and local
infrastructure cool so what about TK
config default - well it's got lots of
file system based apps that enable
powerful workflows but of course your
artists will need at least some access
to shared local file storage to take
advantage of them and path components
related to that shared storage have to
be defined in shotgun specifically a
global local file storage root folder
and the project level tank name folder
easy enough to manage those settings in
shotgun but potentially harder to decide
where to point the primary local file
storage field it's definitely worth
working with the stakeholders at your
studio to settle on a good location hint
it should be on a fast disk that mounts
the same way for everyone to sum it all
up if you don't want or expects to ever
have shared local file storage then riff
off of TK config basic if you are mature
studio with a robust infrastructure or
if you're headed in that direction then
it's certainly worth giving ta config
default to a shot Explorer it's out of
the box file system related integrations
to see what is and isn't there then fill
out what's missing finally I'd like to
promote the idea of centering your
development efforts around the toolkit
platform in general in light of its
powerful code distribution mechanism
large set of examples with documentation
battle-tested api and pre build engines
and frameworks - an enthusiastic
developer toolkits intrinsic value isn't
in the suite of out-of-the-box apps even
if you removed all the apps from TK
config basic for example and started
cobbling together a custom tools from
scratch with our frameworks and api's
and distributing them with descriptors
you'd still have a huge light up
you made it that was a lot of
information are you exhausted or excited
maybe both
at the beginning of this video I
promised pros/cons discussion comparing
this installation strategy to other
popular options I won't get deep into
the specifics but I will say that as of
March 2018 our most often recommended
and best supported install strategy is
to use what we call zero config until
you outgrow it basically TK config basic
set to auto update and then when you're
ready use the shotgun desktop install
wizard to install a config on a shared
file system basically all of these guys
I don't personally like the wizard based
options because one they depend on a
shared file system so you can't work
from home two if that shared file system
is slow and interacting with shotgun
desktop will be slow and three you have
to install a config for every single
project there's no global fallback
config so if you have lots of projects
you can end up with heaps of config
duplication read filesystem bloat and a
management nightmare that said there are
ways to ease the pain you can for
example distribute a shared core via our
sink that speeds up interaction in the
shotgun desktop GUI reduces filesystem
flow and distributes it to local
machines and you can write VCS based
tools to manage batch updates the
project and fix folks do those things
and lots of other stuff to wrangle and
speed up tool kit and they work but yeah
my feeling is that from a purely
functional point of view the descriptor
based workflows makes the wizard install
strategies obsolete
that said descriptor based workflows
rely on new and somewhat unproven
technology in fact we discovered and
fixed a few bugs related to descriptor
workflows during the making of this
video
wizard-based install schemes in contrast
have been around for nearly a decade and
our battle-tested remember there is
inherent risk when adopting new ash
technology and you should factor that
into your install decision but what
about the get descriptor type
why didn't I recommend that github is so
awesome just use that right well a
workflow based around github would be
similar to what I showed and easier to
manage in some ways since you wouldn't
have to pass around zip files
the problem I see is with privacy and
permissions first all your users would
need github accounts with SSH keys
installed it'd be a lot of work to make
that happen I get a shiver in my ghost
when I imagine the frustrated bicoastal
emails from artists who can't connect
the shotgun desktop and launch their my
related tools and if you use a locally
hosted git server you'd still have to
manage get permissions and it's likely
artists wouldn't be able to access the
server remotely so they couldn't work
from home finally if you use the shotgun
descriptor then you have your choice of
VCs technology you can use perforce or
SVN or whatever to host code and the
shotgun descriptor to distribute code
but there is one huge caveat with a
shotgun descriptor type it comes up if
you're managing multiple shotgun sites
the shotgun descriptor can only access
file attachment payloads from the
current site to create a multi-site
workflow you'd have to duplicate all
your config app engine and framework
payloads across sites and repoint all
the IDs and your configs which means
you'd need the site-specific configs if
I was in charge
I'd call that a solid deal breaker and
would likely opt for a git or path based
descriptor workflow instead or I might
research environment variables that
control descriptor behaviors like
whether or not shotgun desktop attempts
to contact the shotgun App Store or
where it looks for the bundle cache that
one might be useful if you're on Windows
and you're using hood roaming profiles
and I definitely take a look at populate
bundle cache chi-wai that script can be
used in super tricky situations to
distribute configs with something like
arcing links to these and other topics
are available under the video done if
you're still hungry for config related
install info after that giant meatball
sandwich of it check out the docs and of
course if anything is confusing or
something seems missing email us at
support at shotgun software comm and
we'll sort you out