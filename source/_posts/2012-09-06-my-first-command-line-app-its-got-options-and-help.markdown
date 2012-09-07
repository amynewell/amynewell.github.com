---
layout: post
title: "My first command-line app: version 0"
date: 2012-09-06 19:30
comments: true
categories: 
---

This makes me so pathetically proud it is ridiculous:

	 ~/code/toys/paythesitter (master) $ ruby bin/paythesitter --help
	Usage: pay_the_sitter.rb -c CALENDAR -s MM/DD/YYYY -r RATE [OPTIONS]

	Specific options:
	    -t, --extra_time [1,2,3]         Extra minutes to add to total time
	    -m, --extra_money [4,3,4]        Extra money to add to total cost
	    -c, --calendar CALENDAR          calendar name
	    -r, --rate RATE                  pay rate
	    -s, --start_date MM/DD/YYYY      start date is required
	    -e, --end_date [MM/DD/YYYY]      end date, defaults to today
	    -h, --help                       Print this help

So what does this app do? It logs in via Oauth2 to my google calendar account,
checks the specified calendar via the API v3, finds all the events on it between the specified
dates, tallies up their time, adds in the extra time i give it (mostly for when
I arrive home late, though I can use negative numbers here for if the sitter is late),
multiplies by the rate I give, adds the extra money I give it (for sitter expenses),
and spits out a number which is the amount of cash I need to come up with at the end
of the day.

This is unlikely to be useful to any other hackers out there, and frankly the
hand-calculating method was not especially cumbersome. But it's been a terrific
project for a few reasons:

*    I hadn't worked with Oauth before. Google strongly recommends you use their libraries to access their APIS, [their Ruby library](http://code.google.com/p/google-api-ruby-client/source/browse/) is in an alpha state since 2010, and it's using an Oauth implemention called [Signet](http://code.google.com/p/oauth-signet/source/browse/) that has no useful information on the web but is their choice because apparently all the others have 'security issues'. Oh, and Oauth really doesn't work on the commandline very well anyway. It's really a webby kind of tech. So I'm not happy with the hacky solution that exists right now, which is mostly cribbed from a google example, but I wasn't able to figure out yet how to refresh a token without rewriting the API, which I didn't quite want to do, so you have to re-auth every time you use the app. Which is a pain. It's high on my to-do list to make it so you can just keep refreshing and don't have to keep authorizing. But anyway, I obviously have a better understanding of Oauth now, so, WIN.

 *    I have never written a commandline app before in Ruby. I mean, I must have written little scripts, obviously. But not something parsing opts and everything. But see, look at my beautiful opts! And my bin! I have a bin directory! With an executable ruby file in it. I know, it's the little things. In any case, it's been a long time since I've written any ruby outside a rails app, and honestly I had no idea how to organize my code, even. I had to do a lot of basic research, and we all know that basic research is good for the soul. I had a few great resources here: [Gregory Brown's article from Practicing Ruby on reimplenting cat in ruby](http://practicingruby.com/articles/shared/qyxvmrgmhuln), and the PragProgs' [Build Awesome Command-Line Applications in Ruby](http://pragprog.com/book/dccar/build-awesome-command-line-applications-in-ruby), which we happened to have lying around the house ( Thanks, husband!). So I basically copied Gregory's code organization, except adding a test directory, ( by the way, **so excited** about require_relative in ruby 1.9), and after reading about the different options for options parsing online and in the commandline apps book, went with OptionsParser, the ruby standard lib option, which produced the output
 I'm so proud of above. ( Also helpful were [this article](http://blog.rubybestpractices.com/posts/rklemme/015-Completing_the_Animal.html), from the [Ruby Best Practices Blog](http://blog.rubybestpractices.com/) Actually, in general, anything Gregory Brown has produced is worth reading...

 * 	I hadn't used minitest before, and it'd been forever since I'd even written tests outside a rails app. [Best minitest resource on internet is here](http://blog.arvidandersson.se/2012/03/28/minimalicous-testing-in-ruby-1-9) , by the way. So that was another learning opportunity for me.

 So it's just a tiny app, but it's been a fun learning experience, and while I doubt that the details of babysitter payment are of use to many others, the general idea of grabbing calendar events from google and adding them up for various purposes. ( How much time did I spend last year at my personal trainer?,etc. I'd have to implement the query parameter for this to be generally useful though...)

 As mentioned above, I hate the way the Oauth is working, but at least it is working. I could use a different ouath library and not use the google ruby api library at all, or at least the auth part of it. Or I could fork the google auth library to make it work better the way I want it to, but I'm not sure I'm even understanding it that well. I'm mulling it all over...

