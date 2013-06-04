Behind the scenes of "Siren"
---

>a vehicle location hack powered by GM, Nexmo, and Cloud9

I had never been to [Gluecon](http://www.gluecon.com/2013/), but I had read good things about it. StrongLoop hooked me up with a free ticket (thanks guys!), and May is the perfect time to soak up the cool dry Colorado mountain air. The presentation agenda looked great, but I'll confess I was also pretty excited for the hackathon.

I'm a sucker for hackathons, at least the true "hack"-athons, which are different than the "I'm really here to pitch my latest startup idea so let me walk you through the deck"-athons that have become too common. Thankfully, the Gluecon event was definitely the former. There's something refershing about the quick and dirty prototyping spirit, and the lack of concern for security, scalability, maintainable code, or commercial viability. I've already confessed that [I don't like writing tests](http://blog.strongloop.com/how-to-test-an-api-with-node-js/), and a hackathon is the perfect excuse to skip all that.

Meeting new people, trying out new technologies, and thriving on a bit of friendly competition are all great reasons to participate. And of courses the prizes don't hurt. :)

For the Gluecon event I teamed up with native Coloradoan [Aaron Nielsen](http://www.linkedin.com/in/aaronmnielsen/), whom I met the first evening.

## Siren

Aaron and I wanted to use the soon-to-be-released [GM Remote API](https://developer.gm.com/apis/remote). It lets you poll for realtime telemetry data and find out where your vehicle is, how fast its traveling, and in what direction. We built a simple map-based interface with two kinds of triggers: speed limits and geo-fences. When either of those triggers is tripped, the app sends you a text alert.

![Screenshot of map with truck](img/siren-screenshot.png)

### Cloud9

Neither of us had done much with [Cloud9 IDE](https://c9.io) before, so we thought this would be a great chance to test it out. On the whole, it's really impressive how nice the coding experience is for something just running in a browser. We had a couple of hiccups where we were editing the same file and had to coordinate saves, but it was no more annoying than working out a merge in git.

The added bonus is that our code was already "deployed." Each project gets a unique URL that is private by default, so you need to be logged in to your account in order to access it. It's perfect for a quick demo.

![Screenshot of Cloud 9 IDE Dashboard](img/cloud-9-dashboard.png)

### GM

Being the lazy developer that I am, I really like APIs that require nothing more than a `key` parameter included in your requests. The OAuth style of GM's API requires more than that, but it wasn't too bad once we got it set up. The tokens expire every 15 minutes, so we just added this check:

`if (res.statusCode == 401)`

and repeated the call after retrieving a fresh token.

### Nexmo

Twilio has sponsored just about every hackathon I've participted in over the past 3 years. I've used them in many small hacks and a couple of production applications. [Nexmo](http://nexmo.com) describes themselves as a "wholesale" SMS API, and that's a pretty accurate summary; their rates are cheaper, they have better international support, and their developer experience - while adequate - lacks the level of polish and friendliness that you get from Twilio. Both of these companies tempted participants with prizes, but who doesn't love an underdog? We decided to give Nexmo a shot.

I found one big advantage and one drawback to using Nexmo. The advantage is that all incoming SMS messages are *free*. That could be huge for certain applications. The downside is that you can't directly respond to an incoming SMS like you can with Twilio (via [TwiML](http://www.twilio.com/docs/api/twiml)). Twilio will read the TwiML and send an SMS back to the phone that sent your app the SMS, whereas Nexmo requires you to initiate a new `send` API request. It's not that I mind writing the one line of code to do that, it's that your app needs to be authorized to send an SMS through the Nexmo number that's interacting with your app. That didn't matter in our particular case, but I have written a few little apps in the past that were puclic open end points which any Twilio number could use.

### Google Maps & Geolib

Google Maps is arguably the reigning champ of mashups in terms of sheer number of inclusions. The only thing that trips me up when I work with the maps API is weeding through documentation and samples that are outdated because they're based on older versions.

I always like starting with the [Styled Maps Wizard](http://gmaps-samples-v3.googlecode.com/svn/trunk/styledmaps/wizard/index.html) to strip out some labels and features for a cleaner simpler map.

I've done a bit of geo work using the handy built-in functions of MongoDB, but Aaron found us a great little Node.js library aptly named [Geolib](https://github.com/manuelbieh/Geolib). Thanks to the `isPointInCircle` method, it was a snap to set up the geo-fence triggers.

### It's not about the money, but...

GM gave out two generous cash (literally a stack of bills) prizes, and we were lucky enough to win one! Our bet on Nexmo also paid off since they kicked in a Nexus 4.

Thanks again to Gluecon and all the hackathon sponsors.

### StrongLoop's Announcements at Gluecon

StrongLoop introduced two new things at the conference that are worth noting. First, their core product has reached the [GA 1.0 release](http://blog.strongloop.com/announcing-strongloop-node-1-0-ga-an-enterprise-ready-node-js-distribution) (and has since had a [1.0.1 maintenance update](http://blog.strongloop.com/announcing-strongloop-node-1-0-1-ga-maintenance-release)). Even if you're comfortable running stock Node.js installations and maintaining libraries through the primary NPM repository, you should check out StrongLoop's distro. It provides some pretty cool tools for managing your applications without getting in the way of all the core things you love about Node.js.

Second, Matt Pardee showed off the [Meeup in a Box](http://blog.strongloop.com/introducing-meetup-in-a-box-org/) project he created. Whether you want to start a Node meetup in their own town, or share your Node presentations with the world, check it out!
