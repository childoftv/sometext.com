---
layout: post
category : coding
tags : [mobile, coding, phonegap]
---

 I made a simple [Speech Timer](http://www.childoftv.com/debate-speech-timer/www-repo/) app for use by competitive [British parliamentary](http://en.wikipedia.org/wiki/British_Parliamentary_debating) debaters with my brother [Sam](https://bitbucket.org/smorrow) in the last few weeks. 

 The app provides specialized timing assistance to debaters, judges and timekeepers on their android or iOS device via a debate-speech tailored interface.

## Background

[Competetive debating](http://idebate.org/) sees thousands of young people practice structured spoken argumentation around the world each week in tournaments like the [Belgrade European Championship](http://www.eudc2012.org/) shown below.

<iframe width="640" height="360" src="https://www.youtube.com/embed/MJ-dZHk1AYc?#t=0m40&amp;feature=player_detailpage" frameborder="0" allowfullscreen="true"> </iframe>

 With emerging charities like [DebateMate](http://www.debatemate.com/) the activity has even attracted the interest of people like [Stephen Fry](http://www.looktothestars.org/charity/debate-mate). There's even an emerging community of people analyzing the game theory of debates (e.g. [http://trolleyproblem.blogspot.com/](http://trolleyproblem.blogspot.com/)).

(For more details see [this guide](http://www.mdu.manchester.ac.uk/training/index.php))

# Details

Debating involves each participant giving 5 or 7 minute speeches throughout the tournament which need to be timed in line with [the structure of a speech (pdf)](http://www.mdu.manchester.ac.uk/training/resources/Debating%20Intro.pdf).

Roughly:
* **1 minute of protected time** (no-one else can speak)
* **3 or 5 minutes of unprotected time** (other competitors can request to interrupt)
* **1 minute of protected time** (to summarize)
* **~30s of overtime** (if needed)

There are three types of person who time debate speeches:
* **Speakers** -competing to win the debate and are likely to use the timer whilst they are standing and speaking
* **Judges** -making notes about speeches and ensuring that the debaters only interact with each other during the allotted times.
* **Timekeeper** -judge who normally bangs his hand or a [gavel](http://en.wikipedia.org/wiki/Gavel) to signify when each section of a speech begins. The app should make this rather manual process less involving, freeing this person to act as a more effective "wing judge".

## Timer Concept

[James Dixon](http://www.linkedin.com/pub/james-dixon/57/922/856), a competitive debater from my old society at the [MDU](http://www.mdu.manchester.ac.uk/) mentioned that it would be good to create a mobile app which optionally could output "debate bangs" sounds at the appropriate moments while timing a competitive debate.

# It Begins...

We had never coded anything together before but decided we'd collaborate on James' idea using a [bitbucket](https://bitbucket.org/) private repository until we decided what to do with the code. Sam took on the android project and I took on the iOS one, using phoneGap for shared code.

I chatted to sam and suggested we use [MVC](http://en.wikipedia.org/wiki/Model%E2%80%93view%E2%80%93controller) and the [factory pattern](http://en.wikipedia.org/wiki/Factory_method_pattern). We decided to use setInterval for the update/refresh callbacks and use new Date() as the source of time.

Sam immediately began learning js and phoneGap so he could start building a the timer controller.


## Design v1.0
I started with a loose list of functions required
I used [google drawings](http://support.google.com/drive/bin/answer.py?hl=en&answer=177123&from=185180&rd=2) so we could collaborate and share the original designs:

<img src="https://docs.google.com/drawings/pub?id=1K_UAfgPO0OP16-ajqIFD3uQHCqD_ptBiGUQC-CBFJ20&amp;w=509&amp;h=696">

## Design 2.0

I showed the v1.0 designs to my wife [Jennifer Morrow](http://jboriss.wordpress.com/) who is a professional UX designer. She gave some feedback and I refined the flow into v2.0:

<img src="https://docs.google.com/drawings/pub?id=1MCoCPtqq07_2Bjhc7qZFv5fPjCjJIvl1JmPD7XUuNHk&amp;w=512&amp;h=769">

## Project Structure

We used [git submodules](http://www.bithai.me/2012/03/16/phonegap-project-structure-using-git-submodules/) and four separate repos to modularize the code. I quickly realized we needed a shell script to synchronize the submodules with each major change:

	# SyncAll.sh
	git pull
	pushd DebateTimer/iOS-repo/project2/www/src
	git pull
	popd
	pushd DebateTimer/iOS-repo/
	git pull
	git add .
	git commit -m "sync iOS"
	git push origin master
	popd
	pushd DebateTimer/android-repo/assets/www/src
	git pull origin master
	popd
	pushd DebateTimer/android-repo/
	git pull origin master
	git add .
	git commit -m "sync android"
	git push origin master
	popd
	pushd DebateTimer/www-repo
	git pull origin master
	popd $FOLDER
	git add .
	git commit -m "sync master repo"
	git push origin master

This complexity is annoying and I'll probably use a [different modular strategy](https://www.acquia.com/blog/using-git-subtree-make-distro-your-docroot) in the future.

## Implementation

We quickly realized we'd want a templating technique and I did some research. It was clear that [Mustache](http://mustache.github.com/) has become the de facto lightweight templating standard so I choose [IcanHaz.js](http://icanhazjs.com/). In a fit of architectural obsession I then flirted with [Chevron](https://github.com/postal2600/jquery-chevron) for a morning before finding that offline apps do not play nice with ajax requests. I moved back to ICanHaz.js and sucked up the issue of having our 'partials' in the main code.

## Working Prototype

Launch [browser based prototypee](http://www.childoftv.com/debate-speech-timer/www-repo/) (the timer has been sped up to help test and demo).

## Next steps

I'm going to try and get this on a few people's phones to test at tournaments in the next week. I'll update with progress in a new post soon.

