---
layout: post
title: 'From Two Hours to 0.3 Seconds: Laying Out a Newspaper in Prolog'
date: 2026-11-22 08:00:00 +0100
permalink: /newspaper-layout-in-prolog/
subtitle: 'A 2008 internship: modelling newspaper layout as a search problem, and why the fast solution never went to production.'
---

In the summer of 2008, between the first and second year of my Master in Computer Science (Artificial Intelligence) at KU Leuven, I did an internship at XT-i, a Java consultancy in the Cronos group. Their client, a Flemish newspaper, wanted to lay out its pages automatically.

That was a long time ago, but I still think about this from time to time. Part of it is about modelling: the same problem took 100 days or 6 seconds depending on how I represented it. The other part I only understood years later, and it has nothing to do with algorithms.

The code, the data and the [internship report](https://github.com/woutervanranst/prolog/blob/main/Stageverslag/Stageverslag.pdf) are on GitHub: [woutervanranst/prolog](https://github.com/woutervanranst/prolog).

## The problem: the deadline is set by the layout

The newspaper wanted more regional news, so it planned to split its five regional editions into smaller ones. That meant more editions to print and more editions to lay out every night.

Printing was already solved. New digital presses no longer needed printing plates to be swapped by hand. Layout was not. Someone had to decide where each article went on each page, by hand, and that took about 5 hours per edition.

Those 5 hours set the deadline for the journalists. The first papers had to be delivered at 05:00 and printing took 4 hours, so printing started at 01:00 and layout at 20:00. If a story broke at nine in the evening, it was in the paper a day late.

The newspaper's IT department had already built a layout algorithm in Java. It produced usable layouts in about 2 hours. My assignment was to design a new algorithm "with an open mind", and I got no information about how theirs worked.

## The input: rectangles

Every page starts from a master page. The yellow area in the figure below is the part that can be filled with articles, ads and reserved space. In the XML I received, that area is described by four numbers per page: `margex` and `margey` (where it starts) and `usewidth` and `useheight` (how big it is).

![The master page: page size, type area, folio at the top, the area for articles and ads, and a strip for registration marks at the bottom.](/assets/posts/newspaper-layout-in-prolog/master-page.png)

*The master page, as drawn by the newspaper.*

Journalists wrote their articles in QuarkXPress, which could render each article in several shapes: wider or narrower, a different headline, and so on. Every shape is a rectangle with a width and a height. The XML combined the pages, the ads and all the article shapes.

The [first sample](https://github.com/woutervanranst/prolog/blob/main/XML/data.xml) had 4 pages, 1 ad and 66 articles, with 181 shapes in total. Each article also had a type and a preferred position. The type is a lead article (*kopstuk*, only allowed at the top of a page), a normal article, or a short article (*kortje*). The preferred position is the front, middle or back of the regional section.

So the problem is this: pick a subset of the articles, pick one shape for each, and place the rectangles on the pages without overlap and with as little white space as possible, while following the placement rules.

## Choosing the tool

My [report](https://github.com/woutervanranst/prolog/blob/main/Stageverslag/Stageverslag.pdf) lists four candidates. The first was the factory layout algorithms CRAFT, ALDEP and CORELAP, which I knew from a logistics course (*Fysieke Logistiek*, Prof. Liliane Pintelon). They plan floor area with the distance between departments as the main constraint. For newspaper articles that distance does not matter, and the algorithms do not guarantee rectangles, so I dropped them.

The second was linear programming. The rules can be written as linear constraints, and the objective could be the total priority of the placed articles. The downside is that an LP solver is a black box: you cannot steer how it searches. Truth be told - I also didn't understand LPs that much.

The third was Prolog. Prolog has [backtracking](https://en.wikipedia.org/wiki/Backtracking) built in (when you model a problem like a search tree with options to choose from, when a choice leads to a dead end, Prolog undoes it, goes back to the most recent choice and tries the next option), so once you have one layout, asking for the next one costs almost nothing.

The fourth was Java. The 2008 version of me rejected it for having "too much overhead" and no backtracking. I no longer agree with that. As the rest of this post shows, the speedup came from the representation and the search strategy, and those would have worked in Java too.

I had just taken *Advanced Programming Languages for Artificial Intelligence* (H02A8A) with Prof. Gerda Janssens. That course covered constraint solvers built on top of Prolog, among them CHR (Constraint Handling Rules) and [ECLiPSe](https://eclipseclp.org/), a constraint logic programming system. Prof. Janssens had also taught us Prolog itself in *Declarative Languages* (H04H5A). The search strategies (greedy search, A\*, branch and bound) came from Prof. Danny De Schreye's *Artificial Intelligence* course (H01O4A).

In the first days I kept thinking about how to describe a layout, and at some point I realized it is a search tree. Each level is a decision: which article, which page, which shape, which position. A finished layout is a path from the root to a leaf, and walking such a tree is what Prolog does by default. CHR had been slower than plain Prolog in our course project, so I picked ECLiPSe.

## Algorithm 1: a grid (does not scale)

I started with the obvious approach ([`algoritme1.pl`](https://github.com/woutervanranst/prolog/blob/main/ECLiPSe/algoritme1.pl)). Divide the page into a grid of cells and store it as a matrix of unbound Prolog variables. Placing an article binds the cells it covers to the article id. A nice side effect: printing the matrix shows the layout.

```
 31073 31073 31073 31073 31073 31073 31073 31052 31052 .....
 31073 31073 31073 31073 31073 31073 31073 31052 31052 .....
 31073 31073 31073 31073 31073 31073 31073 30902 30902 .....
 ...
 31042 31042 31042 31042 31042 31042 31042 ..... ..... .....
 30945 30945 30945 30945 ..... ..... ..... ..... ..... .....
```

For each article, the algorithm tries the free positions from left to right and top to bottom. When you ask for another solution, Prolog backtracks, moves the last article one cell and prints the next layout.

On a 10x15 grid the first layout appeared after 0.01 seconds. After that it went downhill. The algorithm produces every possible layout, ugly or not, and the good ones come late. Article sizes are rounded up to whole cells, so a coarse grid wastes space and you need a fine one. Even with only 5 articles on a 10x15 grid there are about 2·10⁹ placements, which at the speed I measured would take around 100 days to go through.

## Algorithm 2: anchor points and a fit heuristic

The second version started from a simple observation about newspaper pages: articles never start at a random spot. They start in a corner of the page or at a corner of another article.

I called those corners *anchor points*. An empty page has four. An article can hang off an anchor point in four directions (down-right, down-left, up-right, up-left), and once it is placed, its own four corners become new anchor points. The grid is gone, so nothing gets rounded, and there are far fewer positions to try.

![A page with one article and six anchor points, each with its possible directions.](/assets/posts/newspaper-layout-in-prolog/anchor-points.jpg)

*A page with one article. Each anchor point shows the directions in which a new article can be attached.*

The other idea was a [heuristic](https://github.com/woutervanranst/prolog/blob/main/ECLiPSe/versions/hln5.pl#L105-L116) that scores a placement by how well it fits. Every side of the article that touches another article or the page border adds to the score. A touching side scores its length to the power 1.2, so one long touching side beats two short ones with the same total length (4^1.2 > 2^1.2 + 2^1.2). A side that has exactly the same length as its neighbour looks tidier, so it counts three times.

![Two possible placements of the light green article. The left one fits badly, the right one fits well.](/assets/posts/newspaper-layout-in-prolog/fit-heuristic.jpg)

*Two placements of the light green article: a bad fit on the left, a good fit on the right.*

The algorithm:

1. Start with empty pages, each with four anchor points, and place the ads. Ads do not get anchor points. Otherwise the first article could end up against an ad instead of in the top-left corner.
2. Try every combination of unplaced article, page, shape, anchor point and direction. Keep the ones that fit on the page, do not overlap anything and follow the business rules (lead articles at the top, front articles not on the last page, back articles not on the first page). Score each one.
3. Place the best one and add its corners to the anchor points.
4. Repeat until nothing fits anymore.

In Prolog, step 2 is a chain of choice points, and backtracking walks the tree below.

![The search tree of algorithm 2: article, page, layout, anchor point, direction, business rules.](/assets/posts/newspaper-layout-in-prolog/search-tree.jpg)

*The search tree of algorithm 2. The numbers give the order in which nodes are visited. The struck-through nodes fail.*

With [greedy search](https://github.com/woutervanranst/prolog/blob/main/ECLiPSe/versions/hln5.pl#L38-L73) (ECLiPSe's `bb_min` picks the best candidate at every step), the 4 sample pages took 6 seconds, with 41 of the 66 articles placed. This version is [`versions/hln5.pl`](https://github.com/woutervanranst/prolog/blob/main/ECLiPSe/versions/hln5.pl); [`algoritme2.pl`](https://github.com/woutervanranst/prolog/blob/main/ECLiPSe/algoritme2.pl) is the same algorithm, adapted later to the second dataset.

![Result of algorithm 2 with greedy search: four pages with lead articles at the top and an ad in the bottom-left corner of page 2.](/assets/posts/newspaper-layout-in-prolog/algorithm-2-greedy.png)

*Algorithm 2, greedy search. Dark green: lead articles, green: normal articles, light green: short articles. The first number is the order of placement on the page, the second the article id. The ad sits in the bottom-left corner of page 2. The layouts in this post were plotted in MATLAB with [`plotarticles.m`](https://github.com/woutervanranst/prolog/blob/main/MATLAB/plotarticles.m).*

I wrote up this approach, with a first generated layout, in a short [proposal for the newsroom](https://github.com/woutervanranst/prolog/blob/main/Paper%20Redactie/Voorstel.pdf) (in Dutch).

I also ran a [complete search](https://github.com/woutervanranst/prolog/blob/main/ECLiPSe/versions/hln5.pl#L74-L103) that ignores the heuristic. Its first layout came after 0.7 seconds, and it was a mess, with articles scattered and a lot of white space.

![Result of algorithm 2 with complete search: many small articles, chaotic, with gaps.](/assets/posts/newspaper-layout-in-prolog/algorithm-2-complete.jpg)

*Algorithm 2, first solution of the complete search.*

I let the complete search run for 5 days. The best layout it found scored about 98,000, while the greedy search had reached 105,293 in 6 seconds. My rough estimate of the number of possible layouts was 2·10¹⁵³. Going through all of them at the measured speed would take about 10¹⁵² seconds, and the universe is about 4·10¹⁷ seconds old. Making the language faster would not have helped at all.

### Embedding in Java

XT-i only did Java projects, so I wrapped the Prolog code in a [small Java program](https://github.com/woutervanranst/prolog/blob/main/Java/src/base/Main.java). It reads the XML, [turns it into ECLiPSe terms](https://github.com/woutervanranst/prolog/tree/main/Java/src/xmltoeclipseparser), calls the layout predicate in an embedded ECLiPSe engine, and [writes the page id, shape id and start coordinates](https://github.com/woutervanranst/prolog/tree/main/Java/src/eclipsetoxmlparser) of every article back into the XML. The page layout is then built from that XML.

## Algorithm 2.1: the newsroom rules

Then I visited the newsroom, and the requirements got a lot more specific:

1. A page can have a reserved lead article that must go on that page.
2. Articles with a photo are staggered (top-left, middle-right, bottom-left) so the page is easier on the eye.
3. Articles that come earlier in the XML are more important and should be placed first.
4. The first shape of an article in the XML is the preferred one.
5. A page has exactly 6 columns.
6. No more than two baselines of white space between articles.
7. No two boxed articles next to or above each other, no two photos side by side, and no two headlines with the same font and size next to each other.

I made the page 6 columns wide and rounded article heights to the baseline (9.6 points). The algorithm ([`algoritme21.pl`](https://github.com/woutervanranst/prolog/blob/main/ECLiPSe/algoritme21.pl#L16-L33)) now places the ads first, then the reserved lead articles, the other lead articles, the photo articles in a staggered pattern, and finally everything else in XML order. Rules 3 and 4 fix the order of the articles and their shapes, so there is nothing left to search over there. The algorithm [takes the next article](https://github.com/woutervanranst/prolog/blob/main/ECLiPSe/algoritme21.pl#L93-L135) and puts it on the first anchor point where it fits.

I ran out of time before rule 7. With the rest in place, the first layout for 6 pages took 0.3 seconds.

![Result of algorithm 2.1: six pages with ads, staggered photo articles and short articles in the right column.](/assets/posts/newspaper-layout-in-prolog/algorithm-2-1.jpg)

*Algorithm 2.1 on the [second dataset](https://github.com/woutervanranst/prolog/blob/main/XML/data_algoritme21.xml). Ads are dark green, short articles yellow. Pages 1 and 3 show the staggered photo articles.*

I did not expect that: more rules, and the algorithm got faster. Looking back, it makes sense. Every rule removes part of the search space, and a rule that fails early sends the search to the next candidate sooner.

## Algorithm 3: the place looks for an article

In all the versions so far, an article looks for a place. I also sketched the opposite, where a place looks for an article. You start with one empty rectangle per page. For each empty rectangle you look for an article that fits it almost exactly. If there is none, you put a *placeholder* in it: a fixed pattern of smaller rectangles, each of which looks for an article or another placeholder in turn.

![Placeholders: (a) a page with staggered photo articles, (b) columns for short articles, (c) a block for normal articles, (d) the composition of (b) and (c) in (a).](/assets/posts/newspaper-layout-in-prolog/placeholders.png)

*Placeholders. From left to right: a page with staggered photo articles, two columns for short articles, a block for normal articles, and the three combined.*

Since every rectangle fits exactly inside a bigger one, the pages come out tidy almost by construction. It also fits constraint programming well, because each placeholder is just a set of equations on the coordinates of its rectangles. I did not get to implement it. There is a partial prototype in [`algoritme3.pl`](https://github.com/woutervanranst/prolog/blob/main/ECLiPSe/algoritme3.pl).

## The result

My [report](https://github.com/woutervanranst/prolog/blob/main/Stageverslag/Stageverslag.pdf) ends with "2 hours versus 0.3 seconds". That comparison is not fair. I had not implemented all the rules of the existing solution, and my layouts were probably not as good. Still, the gap was four orders of magnitude, and algorithm 2.1 had just shown that extra rules made my version faster, not slower.

The last sentence of the report recommends turning the ECLiPSe search core into a proper component of the production system.

## Lesson 1: the model is the solution

Algorithm 1 and algorithm 2 ran in the same language on the same laptop. Going from 100 days to 6 seconds came from two changes: anchor points instead of a grid, and following a heuristic instead of trying everything. Writing the Prolog was the easy part. What took effort was to stop thinking about articles on pages and start thinking about rectangles inside rectangles and a tree of decisions. I wrote about this kind of reframing in [Classic Problems Have Classic Solutions](/classic-problems-have-classic-solutions/).

## Lesson 2: fast is not the same as usable

Together with my supervisors from Cronos, I presented the results to the newspaper's IT team. Everyone agreed the numbers were impressive. Then the team, all Java developers, said: "We don't know Prolog. Nobody here can maintain this. We can't use it."

I was confused about that at the time. It took me years to see that they were right. If the layout fails, the paper does not come out the next morning. You do not run that on a language nobody in the team understands.

[Choose Boring Technology](/choose-boring-technology/) describes this well. Every organization can afford only a few "innovation tokens", and a niche language in a critical process uses up a lot of them. Most of that cost shows up in the years of maintenance after the first version.

With the power of hindsight, there was a middle ground. None of the ideas depend on Prolog: anchor points, the fit heuristic, greedy instead of complete search, a fixed article order to cut down the search space. Prolog made it quick for me to find and try them, but the Java team could have built them in Java. Backtracking would have taken more code, but they would have owned it.