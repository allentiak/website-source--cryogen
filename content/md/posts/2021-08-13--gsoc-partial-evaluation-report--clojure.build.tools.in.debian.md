{:title "Clojure CLI Tools in Debian - GSoC 2021 Partial Evaluation Report"
 :layout :post
 :tags  ["debian", "gsoc", "clojure"]}

**NOTE**: this blog post is based on [my "Clojure CLI Tools in Debian" GSoC 2021 project Partial Evaluation Report](https://lists.debian.org/debian-clojure/2021/07/msg00000.html).

<!--more-->

Hi, everybody!

For those who don't know me, my name is Leandro Doctors (allentiak on IRC), and I'm the Debian Clojure Team's GSoC 2021 intern. My mentor is Louis-Philippe Véronneau[*] (pollo on IRC). My co-mentor is Utkarsh Gupta (utkarsh2102 on IRC). My 'no-mentor' :) is Elana Hashman (ehashman on IRC).

In this message, I will summarize what I've been up to during the Coding I period (June 7th - July 16th).

***************************************************************************************************
TL;DR: my updated `data-xml-clojure` package was accepted[1] in experimental on Wednesday night :-)
***************************************************************************************************

[1] [https://tracker.debian.org/news/1244465/accepted-data-xml-clojure-020alpha6-1-source-into-experimental/](https://tracker.debian.org/news/1244465/accepted-data-xml-clojure-020alpha6-1-source-into-experimental/)


Now, let's tell the full story.


During the first days of the "Coding I" phase, I did some research on the `clj` dependencies. And after the precious feedback from Louis-Philippe, Elana, Utkarsh, and Alex Miller (the upstream developer of `clj`, among many other libraries), I came up with the following packaging strategy.

 1) update `org.clojure:data.xml` (`data-xml-clojure`) to 0.2.0-beta6.
 2) package `org.clojure:tools.gitlibs` (`tools-gitlibs-clojure`).
 3) consider updating `libjsch-agent-proxy-java`, `jgit`, `libtools-cli-clojure` (all of them already in Debian).
 4) consider packaging `com.cognitec.aws:*` packages.

According to my original proposal, I should have completed all four tasks during Coding I.
Looking back, the main lesson from these past weeks is a known classic: my timeline was too optimistic: I definitely underestimated the difficulty of the packaging process. Out of the four tasks, I only finished the first one.

There were many challenges I had to overcome in order to update the library from version 0.0.8 to 0.2.0-alpha6:


This what I did:

First, I patched the source code so:
- - we avoid needlessly packaging `data-codec-clojure` (not currently in Debian).
- - we can ignore Clojure-Script-related dependencies.

Then, I completely overhauled the packaging code (this is, what goes inside `debian/`).
- - added support for automatically tracking newer releases.
- - the package now builds with Leiningen, instead of plain Java.
- - it now actually supports autopkgtest (the existing test was trivial).

All this improved the quality of the package.

I also improved the Clojure Packaging Tutorial[2] to make the process easier to follow.

[2] [https://wiki.debian.org/Clojure/PackagingTutorial](https://wiki.debian.org/Clojure/PackagingTutorial)


Looking back, it is almost as if I had started packaging the library from scratch...

But, more that what I produced, I think the most important part of all this is what I learned during these weeks.

As it was the first time I ever packaged anything in Debian, I had to learn the basics, bump by bump. (And, oh my, I surely did bump quite a few times!)
- - Basic packaging workflow.
- - Setup `sbuild`[3] to make it work with `gbp`[4] (when I had rebuilt bidi-clojure from scratch, I had used plain `sbuild`.)
- - Learn how to patch upstream files with `quilt`[5] (actually, via the `dquilt` frontend). This was the first really difficult task to do, since it took me quite some time to grasp it. Looking back, I think I was simply scared of breaking something :)
- - Understanding `debian/` files.
- - Understanding `debian/tests` (autopkgtest) files. This was particularly difficult, since the doc wasn't clear about it. So I then improved the Clojure Packaging Tutorial[2] to make the process easier to follow.

[2] [https://wiki.debian.org/Clojure/PackagingTutorial](https://wiki.debian.org/Clojure/PackagingTutorial)

[3] [https://wiki.debian.org/sbuild](https://wiki.debian.org/sbuild)

[4] [https://manpages.debian.org/unstable/git-buildpackage/gbp.1.en.html](https://manpages.debian.org/unstable/git-buildpackage/gbp.1.en.html)

[5] [https://wiki.debian.org/UsingQuilt](https://wiki.debian.org/UsingQuilt)


Definitely, all this was worth it. After all, my updated `data-xml-clojure` package was accepted[1] in experimental on Wednesday :-)

[1] https://tracker.debian.org/news/1244465/accepted-data-xml-clojure-020alpha6-1-source-into-experimental/


Now that I've ~~learned the basics of packaging~~ bumped enough to get my package accepted, I'm hopeful I can ramp up and catch up with (at least most of) my original schedule during Coding II.

`tools-gitlibs-clojure`, you're next :-)


Thank you very much Louis-Philippe, Elana, and Utkarsh (plus a special mention to Alex) for your precious support during the last few weeks!
