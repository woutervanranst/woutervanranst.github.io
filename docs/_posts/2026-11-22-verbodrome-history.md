---
layout: post
title: 'Verbodrome: 25 years of programming instead of learning French verbs'
date: 2026-11-22 18:00:00 +0100
permalink: /verbodrome-history/
subtitle: 'From an Excel macro in 2001 to a rule-based conjugation engine in 2026, and every technology I tried out in between.'
---

In September 2001, in the fourth year of secondary school, I had to learn the *Bescherelle* by heart. The Bescherelle is the little French book with a conjugation table for every model verb, and our French teachers wanted us to know it by heart. I thought it was the perfect excuse to *write a program that would quiz me* rather than learn the verbs.

![The green Bescherelle, La Conjugaison pour tous, published by Hatier.](/assets/posts/verbodrome/bescherelle-book.jpg)

*The book we had to learn by heart.*

That program kept me busy, on and off, for 25 years. It started as an Excel macro, became a Visual Basic 6 application, a .NET client/server system for the classroom, a ClickOnce desktop app, a Windows 8 Store app, a game called Angry Verbs, a couple of web apps and, this year, a conjugation engine. Every rewrite was an excuse to try a technology I wanted to learn, so in hindsight it is a decent portfolio of how I learned to program.

I dug up the old backups, my mail archive from 1997 to 2007 and a handful of later repositories, and put every version I could find in one repository: [woutervanranst/verbodrome6](https://github.com/woutervanranst/verbodrome6).

## Before Verbodrome: Blue

I started programming in DOS batch files when I was seven. By 1999 I was writing Visual Basic under the alias *Vengakid* (the Vengaboys were big) and reading the computer magazine *Netwerk*, which I called *mijn lijfblad*, my bible. Four years later that magazine would write about me.

In 2000 my nickname was BlueBoy, and the programs I wrote with a few classmates needed a publisher. So we invented one: *Blue®*. The logo was a blue square with the name in white in the bottom-left corner. It was a play on the then just-launched third mobile operator in Belgium, Orange (after Proximus and Mobistar). It was an ad in the bus stop I took to school. I changed the color and the name and I thought that was very clever.

![The Orange logo: an orange square with the name in white in the bottom-left corner.](/assets/posts/verbodrome/orange-logo.png)

*The original: [Orange](https://nl.wikipedia.org/wiki/Orange_%28bedrijf%29).*

![The Blue website in November 2000, made with the FrontPage Corporate Presence Web Wizard.](/assets/posts/verbodrome/blue-website-2000.png)

*The Blue website, November 2000: "We are two students ( almost three ) who like programming. […] don't expect us to write super-fabulous-microsoft-overwhelming stuff, but we're trying."*

The product list had an encryption program called *Deep Blue* (named after [IBM's chess computer](https://en.wikipedia.org/wiki/Deep_Blue_%28chess_computer%29) that had beaten Kasparov). Next to it were a periodic table quiz, a quiz on the capitals of Africa and *English Irregular Verbs* v1.4. That last one is the real ancestor of everything below: a list of verbs, a question, an answer, a log file.

Today Blue Software is the name of my company as a freelancer, a wink to the teenager who drew that square. When I incorporated in 2021, the logo was the blue square with the name in the bottom-left corner again. In 2025 it got a refresh.

![The Blue logo from 2000 to 2025: Blue® with the slogan Programming The Future, Blue®, Blue, the lowercase blue. wordmark, the 2021 square and the 2025 rounded square.](/assets/posts/verbodrome/blue-logo-evolution.png)

*The Blue logo, 2000 to 2025.*

## 2001: an Excel macro

The Bescherelle has 88 tables. About 150 of us in the fourth year had to learn them, and the first test was coming. Together with my friend Steven I put the verbs in an Excel sheet, one column per verb and one row per form, and wrote a macro that asked for a random form in an `InputBox` and said right or wrong in a `MsgBox`. The sheet was the database. Version 1 had 27 of the 56 verbs we needed; the manual asked for volunteers to type in the rest.

![The about box of Bescherelle oefenprogramma v. 1.0 over the spreadsheet with the conjugations.](/assets/posts/verbodrome/bescherelle-1-about.png)

*Bescherelle v1, September 2001. The worksheet behind the dialog is the database.*

![The question box: Geef de 2e mv van aller.](/assets/posts/verbodrome/bescherelle-1-question.jpg)

![The answer box: Fout! ... = TU SERAS.](/assets/posts/verbodrome/bescherelle-1-wrong.jpg)

*A question and a wrong answer. The log file shows the first session on 22 September 2001.*

Only one thing was computed rather than typed. If a cell said just "avoir" or "être", the macro built the compound tense from the auxiliary and the past participle. Keep that in mind: it took 25 years to get to the next step. The code is in [`v1.1/01-bescherelle-excel`](https://github.com/woutervanranst/verbodrome6/tree/v1.1/01-bescherelle-excel), with the VBA extracted as text.

A presentation text I wrote two years later claims "the results of the tests went up considerably". I have no data to back that up.

## 2002: diskettes, VB6 and a registration server

Version 2 (January 2002) got a settings window to pick the tenses and persons, points saved in the registry, a `/cheat` command ("Cheating NOT enabled... Crack the code, LÖSER") and a release on diskette, with a label.

![The Bescherelle v. 2 settings window with checkboxes for tenses and persons.](/assets/posts/verbodrome/bescherelle-2-settings.jpg)

*Bescherelle v2, January 2002.*

![Two diskette labels: Bescherelle Oefenprogramma 2, insert the diskette, read Leesmij.html.](/assets/posts/verbodrome/bescherelle-2-diskette-label.png)

*The diskette label, with Bescherelle in red like on the book cover.*

The web page for v2 ends with "Het is vrijdagnacht, halftwee, het is koud en het regent. Ik stop ermee." (It's Friday night, half past one, it's cold and it's raining. I'm quitting.) Versions 3 to 4 followed within six weeks: a verb picker, questions in French or Dutch, the subjonctif passé, and something I called the *Blue Moon Engine*, which pre-computed the list of forms it was allowed to ask.

In March 2002 version 5 was the first one in Visual Basic 6. It still asked its questions in an `InputBox`, but now came with an MSI installer, a text database and 127 verbs. The 72 extra verbs came from *Le Conjugateur*, a program by Brewalan Le Drû. I wrote an *Extra Verbes Extractor* that parsed its text exports into my format. Version 5 also contained a time bomb ("Deze versie van het programma is verlopen" after August 2002) and a routine called `BentUEenComputerAnalfabeet` that asked "Are you computer illiterate?" and then tested whether you knew the `/quit` command.

![The setup wizard of Bescherelle v5 Bèta 2.](/assets/posts/verbodrome/bescherelle-5-installer.jpg)

*Bescherelle v5, March 2002: a real installer.*

Version 6 (summer 2002 to January 2003) finally had a main window. It stored its 200 verbs in an Access database and had an interface in Dutch, French and English. I also wrote a custom OCX with Winsock client and server controls so the program could register itself online against a server on my home PC, behind a dynamic DNS name. On 23 August 2002 two of my teachers, my French teacher Eric Van Hool among them, got a beta CD with a letter. A few months later the to-do list says *internetbrol wegwerken* (get rid of the internet junk), and version 6.3 shipped without online registration.

![Bescherelle V6 - Probabiliteitsmanagement: per tense, how many of 100 questions should be about it.](/assets/posts/verbodrome/bescherelle-6-probability.png)

*Bescherelle v6.2, "probability management": you set how often each tense is asked.*

The probability screen became a theme. In version 6.3 it turned into *dynamic* probability: the tenses you get wrong are asked more often. In December 2003 my maths teacher, Pascale, designed a better algorithm for it, and in some form that idea (every tense starts at the same weight, which halves on a right answer and doubles on a wrong one) is in every version after it.

## January 2003: Bescherelle becomes Verbodrome

In January 2003 Eric Van Hool, by then my former French teacher, was so taken with the program that he wanted it registered. That raised an awkward point: *Bescherelle* is a trademark of the publisher Hatier. The manual of version 6.3 already said so, and added that "every request for an immediate change of name will be granted at once". I needed a name of my own, and my friend Christian Wildiers came up with it: Verbodrome. On 24 January 2003 I sent him three icons with a V to choose from, and the program was renamed the same day. In April I deposited it with SABAM, "against all your good advice", as I wrote to the other Eric in this story, who had offered me a lawyer.

That other Eric was a family friend and the CEO of VANAS, a small software company. Its lead software engineer, Danny Depecker, became my mentor. So while I was still in secondary school, I had a CEO for the commercial questions and a senior developer for the technical ones. The about box of Verbodrome 6 thanks "het VANAS team" for "hun antwoord op (al) mijn vragen en onoplosbare bugs" (their answers to (all) my questions and unsolvable bugs). Years later I would work at VANAS myself.

![The main window of Verbodrome V6: a question, an answer field, the exercise number, score and time.](/assets/posts/verbodrome/verbodrome-6-main-window.jpg)

*Verbodrome 6, February 2003.*

On 9 February I mailed my whole class: "GERAAK JE OOK HELEMAAL IN DE KNOOP VAN DIE FRANSE WERKWOORDEN? [...] \*trompetgeschal\* VERBODROME" (are you also completely tangled up in those French verbs? *fanfare*), with "( ja dit programma is volledig van mijn hand )", yes, I wrote this program entirely myself. It was a 15 MB download. Danny got a CD and a printed manual as a New Year's present. He called it "a professional approach", but the CD was stuck in its sleeve with glue and could not be used.

![The Verbodrome 6 package: a binder with the Blue logo and a CD with the round V logo.](/assets/posts/verbodrome/verbodrome-6-package.jpg)

*The deluxe edition: a binder with a printed manual and a CD.*

During 2003 the VB6 code grew from about 4,400 to almost 9,000 lines. Teachers could create exercise series and verb lists and hand them out to their pupils as files, there was a score report with a mark out of 20, an autorun CD, a 30-day trial and licence keys. Version 6.5 (October 2003) was a rewrite around classes, the folder of the build before it is called *Laatste Niet-OOP*, the last non-object-oriented one. The last VB6 release was 6.6, in January 2004. All of it is in [`02-vb6`](https://github.com/woutervanranst/verbodrome6/tree/v6.6/02-vb6), and `git diff v6.4 v6.5 -- 02-vb6` shows the rewrite.

![The session creator: pick a verb, tense and person and build a list of questions.](/assets/posts/verbodrome/verbodrome-6-session-creator.png)

![Scorerapport na 26 oefeningen: per tense the score, total 19/26 or 73.1%.](/assets/posts/verbodrome/verbodrome-6-score-report.png)

*The session creator for teachers and the score report, Verbodrome 6.31 (April 2003).*

## Selling it: publishers and magazines

A standard licence cost €9.99, a deluxe edition with the binder €24.99. I asked in an email whether that income was "100% black money". It was not legal, Eric answered, and he suggested invoicing it through VANAS "for a small percentage". It never came to that, because there was very little to invoice.

The real plan was a publisher, and Eric Van Hool was the driving force behind it. He used Verbodrome in his classes, with verb lists per month that I shipped as database updates. He proofread my letter to the publishers, got me in at De Boeck through someone he knew there, and let me use his name as a reference, which opened more doors than mine did. The quote on the CD insert is his: "Saaie Franse werkwoorden vervoegen werd plots een uitdaging, een spel [...] Het spel werd een competitie waarin iedereen de winnaar wilde zijn en ook werd." (Boring French conjugations suddenly became a challenge, a game. The game became a competition that everyone wanted to win, and did.)

At the end of April 2003 I sent the letter and a trial version to the Belgian educational publishers. Van In first answered "we would like to look at the software" and in September said no: "Ik geef toe dat de werkwoorden een pijnpunt blijven" (I admit verbs remain a sore point), but a verb drill on its own did not fit their courses. De Boeck gave me a 45-minute slot on 8 October 2003 and cancelled it the day before. I did not take that well.

In October 2003 two magazines wrote about it. *Netwerk*, the magazine I read in 1999, printed a short piece without telling me. *Clickx* put the website in its top 5 under the title "Blauwe Wouter".

![Netwerk, October 2003: Franse vervoegingen oefenen.](/assets/posts/verbodrome/netwerk-10-2003.jpg)

*Netwerk, October 2003, page 118: "Verbodrome was received with great enthusiasm by the teachers at his school."*

![Clickx 51, October 2003: Blauwe Wouter, with a screenshot of Verbodrome V6.](/assets/posts/verbodrome/clickx-51-2003.jpg)

*Clickx 51, October 2003. The last line reads: "Er zijn geen kinderen meer..." (Kids these days...)*

The articles brought a few buyers, all parents. The first person to actually pay was An, a French teacher at Xaveriuscollege who had been testing it. Sophie Ann, the head of French at a school in Ghent, asked for a price for her computer lab after reading *Netwerk*. I forwarded her mail to Steven with one line: "JACKPOT ;)". I don't know if she bought it.

Then I wrote to the others: Die Keure, Standaard Uitgeverij, Wolters Plantyn, the e-learning company eXplio and, later, Visiria in the Netherlands. In January 2004 I mailed: "Onverhoopt heb ik afspraken met 4 uitgeverijen [...] Misschien laat ik mij wel afschepen met 500€ (voor tweeëneenhalf jaar werk)" (Against all odds I have meetings with 4 publishers. Maybe I'll let them fob me off with €500, for two and a half years of work). I was in my final year of secondary school and could only go on Wednesday afternoons. That winter I presented at De Boeck, Pelckmans, Die Keure and Wolters Plantyn.

All of them said no:

- Die Keure: "We vinden het op zich een zeer goed programma, maar zien er eerlijk gezegd niet direct een voldoende rendement in." (We think it's a very good program, but honestly don't see enough return in it.)
- Wolters Plantyn: not selected for commercialisation. "Jammer, maar helaas!"
- Visiria asked the best question: "voelt u zich programmeur of bent u auteur Frans?" (Do you see yourself as a programmer or as an author of French?) Teaching verbs in context was work for an author, they wrote, not for a programmer.
- De Boeck thought it was "cool", and would get back to me "within the year".
- Pelckmans would "look into it further". I'm still waiting.

Looking back, Van In and Visiria were right. A drill for verbs alone is a feature, not a product, and the people who decide what schools buy are not the pupils who use it. Pierre, one of the parents who bought it, put it better than I could: "De uitgevers zijn natuurlijk niet de gebruikers." (The publishers are, of course, not the users.)

## Blue Software Corporation

By then Blue had become *Blue Software Corporation*, with a website at `users.pandora.be/bluesoftware` (`/blue` was taken by somebody who put up pictures of Braveheart), the slogan "Software is Architecture", a news page, a support centre and a merchandising page.

![The merchandising page: a T-shirt with the Blue logo, S €9,99, L €12,99, XL €14,99.](/assets/posts/verbodrome/blue-website-merchandising.png)

*Blue Software T-shirts, 2003. Clickx noticed them too.*

There were T-shirts with the blue square on the chest, in three sizes. Under "About Blue" a draft page said: "Onze software is onmenselijk: hij moet niet gereboot worden elke 24 uur." (Our software is inhuman: it doesn't need a reboot every 24 hours.)

## The engine I couldn't have

Every version so far stored the conjugations. I typed them in, extracted them or, later, scraped them. What I really wanted was an engine: give it any verb and it conjugates it, invented verbs included, because it knows the rules. Brewalan Le Drû's site (today [Le Conjugueur](https://leconjugueur.lefigaro.fr/)) could do exactly that, so I knew it was possible.

In August 2003 I mailed him to ask for his program. He had taken it off his site because he was rewriting it and concentrating on the website: "Après tout, il fait la même chose que le logiciel" (after all, it does the same thing as the software). The engine stayed his.

In June 2004 I tried to license an engine for a multilingual version. I offered Verbix "some royalty contract". The makers of WinDi answered: "You will never reach the level of WinDi, 95 people have worked on this project! Why not develop something else?" I replied: "No offense, but I think you have very little to say about what I am going to develop." I had not even finished secondary school.

## 2004: the classroom server

The idea for a network version came from An: a teacher should be able to follow the whole class from one screen. In October 2003 I wrote my first C# code, a console server on port 6000 with one thread per client and a line-based text protocol, and gave Verbodrome 6.6 a network client to talk to it. On 20 October I mailed An the first handshake, "het ontspruiten van uw gedachtegoed" (your idea, sprouting):

```
CS: AUTH_REQUEST Verbodrome Build 6500.20031007
CS: AUTH_USER Wouter
CS: AUTH_PASS hallo
CS: AUTH_PCID 481606069
CS: AUTH_KEY 2EF13A0B
SC[0]: AUTH_AUTHENCITATED USERNBR 1 BUILD BETA
```

"De eerste communicatie tussen Verbodrome-server en Verbodrome-client: de aanmeldprocedure!!!!!" (The first communication between Verbodrome server and client: the login procedure!) The spelling of `AUTHENCITATED` is original.

Danny had told me: "VB6 is over, .NET Framework and C# are the new kids on the block." So naturally I wanted to try that, and in the summer of 2004, after my final exams, I rewrote everything in C# on .NET 1.1: Verbodrome 7 for the pupils, Verboserve 7 for the teacher, and a shared library, *Verbosystem*, code name *Bellerophone*, a reference to *Mission: Impossible 2*. It used .NET Remoting over TCP. The pupil's session lived on the server as a `MarshalByRefObject`, and the client was "only a thin shell" that showed what the server sent. The draft website said: "Twee jaar expertise. Een half jaar ontwerptafel. Een maand ontwikkeling. Verbodrome 7 is een feit." (Two years of expertise. Half a year at the drawing board. One month of development.)

![Verboserve 7: per pupil the list of questions with right and wrong answers, coloured from red to green, and a table per tense.](/assets/posts/verbodrome/verboserve-7-session-report.png)

*Verboserve 7, September 2004: the teacher's live view of a session, here with a stress-test user.*

The teacher started a session with a wizard: individual or shared questions, which verbs and tenses, a time limit, and a competition mode where every pupil saw their rank ("Je staat laatst...", you're last, followed by how many points the next one was ahead). The background of the client changed colour with your score, from green to red. I wrote to Pierre in 2006 that the colour does hurt when you score below 20%, "but that's a bit the point".

![Verbodrome 7 at 80%: a yellow-green window.](/assets/posts/verbodrome/verbodrome-7-score-80.png)

![Verbodrome 7 doing badly: an orange window at 33%.](/assets/posts/verbodrome/verbodrome-7-doing-badly.png)

*Verbodrome 7 at 80% and at 33%.*

![A laptop running Verboserve next to a monitor with four Verbodrome clients, and a Pocket PC.](/assets/posts/verbodrome/verbodrome-7-codegasm.jpg)

*8 September 2004: Verboserve on the laptop, four stress-test clients on the monitor.*

On 13 September 2004 I installed it at my old school, on Windows 98 machines with .NET 1.1: "VERBODROME WERKT!!! (op de krotcomputers van t OLVC :-))" (it works, on the school's crappy computers). A week later, during the session where I explained it to the French teachers, it crashed: "4 weken (à 15 uur per dag) werk in minder dan een seconde in rook te zien opgaan doet pijn." (Watching four weeks of work, at 15 hours a day, go up in smoke in less than a second hurts.) University started the same week.

In April 2005 I opened the code again: "Tot mijn groot spijt/verdriet/woede/onmacht/frustratie versta ik niets meer van mijn code." (To my great regret/sorrow/anger/helplessness/frustration I no longer understand any of my code.) Danny answered: "Programmeren <=> Studeren ... het 2de moet nu." (Programming versus studying, the second one has to happen now.) A file called `Final Notes.txt`, dated 13 August 2005, ends it: "onopgeloste bug, te grootschalig, te weinig tijd" (unsolved bug, too large, too little time). The code is under [`v7-final/04-verbodrome7`](https://github.com/woutervanranst/verbodrome6/tree/v7-final/04-verbodrome7).

In December 2004, in my first months at university, I had written to Nicole that writing Verbodrome 6 and 7 was in my top 5 of best moments, and that seeing my own creation run on a computer was "God spelen", playing God.

## 2006: Verbodrome 8

In September 2006, at the start of my third year at KU Leuven, Pierre, a parent who had bought a licence in 2004, asked for a new key. He thought I had stopped. I sent him the key generator, and two months later, "in een ambitieuze bui" (in an ambitious mood), I had a new version: Verbodrome 8, a standalone C# WinForms app on .NET 2.0 with SQL Server 2005 Express, installed with ClickOnce. Licences were XML files signed with an RSA key: you mailed your licence request, I signed it and mailed it back. "Maar het werkt en dat is zalig." (But it works and that is bliss.) "Programmeren tot 5 uur smorgends is zalig." (Programming until 5 in the morning is bliss.)

![An unhandled SqlException in Verbodrome 8: Incorrect syntax near 'appeler'.](/assets/posts/verbodrome/verbodrome-8-sql-apostrophe-bug.jpg)

*Verbodrome 8, 30 November 2006. The verb was* s'appeler*, and the apostrophe ended up in a SQL string. A textbook SQL injection, by a French verb.*

I added German and, in December 2007, Spanish: I was on Erasmus in San Sebastián and needed to train Spanish verbs myself. Along the way I moved to Visual Studio 2008, inside a Windows VM on a Mac. The testers, Pierre and his daughter, were honest: "eigenlijk komt het er op neer dat de vorige versie beter was" (it comes down to the previous version being better). The installation was too heavy, the colours too loud. And about the planned price of €5 a year: "zwier die 5 euro van de website" (throw that €5 off the website). The last build is from March 2008. There never was a final release.

![The Verbodrome 8 score report on a green background: 16 out of 21.](/assets/posts/verbodrome/verbodrome-8-score-report.png)

*A Verbodrome 8 score report.*

## 2012: Windows 8

From 2011 to 2014 I worked at Microsoft in solution sales, for Windows 8 among others, so in the summer of 2012 Verbodrome became a Windows 8 Store app: Verbodrome 9, C# and XAML on WinRT, with a home-made MVVM layer and the score colour still in the background.

![Verbodrome 9: a green full-screen question, Geef de conditionnel présent van asseoir in de 1e pers. enk.](/assets/posts/verbodrome/verbodrome-9-question.png)

*Verbodrome 9, September 2012.*

It supported five languages: French, German, Spanish, English and Latin. The verbs came from a `Parser` console app that downloaded conjugation pages from five websites and turned them into XML. Still a list, just a longer one.

![Pick your exercises: languages, verbs and tenses, with flags for German, English and Spanish.](/assets/posts/verbodrome/verbodrome-9-pick-exercises.png)

![Doctus eris: a Latin question in Verbodrome 9.](/assets/posts/verbodrome/verbodrome-9-latin.png)

*Picking exercises, and a Latin question.*

The feedback page grew from one case to five: first time right, first time wrong, right after a mistake, wrong again with a new answer, wrong again with the same answer. And for the first time the app looked up what you had typed and told you which form it was.

![Woops, again the same mistake! You gave the gérondif passé of plaire (io. pouvoir).](/assets/posts/verbodrome/verbodrome-9-same-mistake.png)

*"You gave the gérondif passé of plaire (instead of pouvoir)."*

The app cost €2.60 or $3.49 with a free trial. The first sale came on 22 October 2012, four days before Windows 8 went on sale in the shops. The sales export of May 2013 has 25 transactions, one of them a refund. For teachers I built a backend on Azure Cloud Services (WCF on an ExtraSmall web role, two instances) that served teacher profiles: type a code in the app and you get your teacher's selection of verbs and tenses. The source went into the Team Foundation Service preview. It is all under [`v9-post`](https://github.com/woutervanranst/verbodrome6/tree/v9-post/06-verbodrome9-windows8) and [`verboserve-azure`](https://github.com/woutervanranst/verbodrome6/tree/verboserve-azure/07-verboserve-azure-2012).

## 2013-2015: Angry Verbs, and a photo of Berlin

Version 10 got a new name: Angry Verbs. Those were the days of Angry Birds and Flappy Bird, when every app seemed to need a bird or a face, so the logo became a `>_<` face. The file is dated 11 February 2014, two days after Flappy Bird was pulled from the App Store.

![The Flappy Bird logo.](/assets/posts/verbodrome/flappy-bird-logo.png)

*[Flappy Bird](https://en.wikipedia.org/wiki/Flappy_Bird), the game everybody was playing that winter.*

There was an Arcade mode, a Campaign with levels, stars and padlocks per mood, and a Teacher mode. Later it also became a website on Azure, which is how I learned AngularJS. Every language got a background photo: the Eiffel Tower, Big Ben, Amsterdam canals, the Roman Forum, Berlin.

![The Angry Verbs logo: a white >_< on blue.](/assets/posts/verbodrome/angry-verbs-logo.png)

![Angry Verbs: choose your game, Arcade, Campaign or Teacher.](/assets/posts/verbodrome/angry-verbs-choose-game.png)

![Campaign Overview: levels per tense with stars and padlocks.](/assets/posts/verbodrome/angry-verbs-campaign.png)

*Angry Verbs: the logo (2014), the game modes and the campaign.*

The photo for German was a Berlin skyline at dusk. I found it by googling "Berlin", where it was the fourth result, and it appeared on hundreds of other sites without any copyright notice. In 2015 a letter came from a Berlin lawyer at Photoclaim, on behalf of the photographer. By July 2015 the claim was €1,253, with a calculation of what a court case at the District Court of Berlin would cost on top.

I took the site down and negotiated. Angry Verbs was a free, non-commercial student project. "Until this day, I have exactly made EUR 0 of revenues with the site." In May, a representative month, it had 120 page views, "of which a large part are probably from indexing sites, you, me and your client". The photo could not be bought on Flickr, and €350 for one licence was far above what stock sites charged. They came down to €350 plus €551 in legal costs, and in August 2015 we settled on €200.

Looking back, it is a bit ridiculous: €200 for a photo, and the end of a project I had carried since secondary school. I didn't put Angry Verbs back online.

## 2020: VerbodromeX

During the 2020 lockdowns pupils were practising at home, and I picked it up once more. VerbodromeX ran on `verbodrome.eu`: ASP.NET Core 3.1 Razor Pages, Entity Framework Core (the `DbContext` was still called `AngryVerbsContext`), Azure App Service with a pre-production slot to swap, Azure AD B2C with school and class as sign-up attributes, Application Insights, and a Let's Encrypt certificate I renewed by hand. The parsers now scraped seven languages.

Two things were new. Questions became multiple choice, asked from a Dutch prompt. And the score page analysed your mistakes: *Deze tijden verwar je regelmatig*, these tenses you mix up regularly. That was an idea from a mind map I drew in February 2003 and never built.

![VerbodromeX: wij hadden geworpen, with eight French answer buttons.](/assets/posts/verbodrome/verbodromex-question.png)

![VerbodromeX score page: 62 exercises, coloured bars per attempt, and a list of tenses you confuse.](/assets/posts/verbodrome/verbodromex-score.png)

*VerbodromeX, October 2020.*

I mailed it to a few language teachers ("Met Verbodrome geen getreur"). The last code commits are from October 2020. After that my pet project became [Arius](/developing-arius-5-during-the-dawn-of-ai-assisted-code/).

## 2026: an engine, finally

All this time the conjugations were data. Typed by hand in 2001, extracted from Le Conjugateur in 2002, scraped from websites in 2012 and 2020. In 2012 I started a `FrenchConjugator.cs` with real rules; the file in the repository is entirely commented out. The engine I asked Brewalan for in 2003 never came.

This autumn I tried again, in a new repository, [woutervanranst/verbodrome](https://github.com/woutervanranst/verbodrome). This is the whole prompt I gave Claude Fable 5.2 for the engine:

```
in typescript, make a verb conjugation engine. it should start from the grammatical rules, it can hardcode the exceptions but it should not just ’store all conjugations’ - it should also work with invented verbs and correctly apply the rules

It should support multiple languages, lets start with French (‘fr’) and Dutch (‘nl)

For French: use https://leconjugueur.lefigaro.fr/regle as grammar
Use the corpus available at https://github.com/woutervanranst/VerbodromeX/tree/master/code/VerbodromeX.Database.Parsers/SourceFiles/Fran%C3%A7ais to build your test suite

For nederelands, find an authoritative source
Use the corpus available at https://github.com/woutervanranst/VerbodromeX/tree/master/code/VerbodromeX.Database.Parsers/SourceFiles/Nederlands

The future use is for a web app, but for now make it a simple cli wrapper around the main entrypoint function along the lines of conjugate(language (‘fr’/‘nl’, infinitive, mood, tense, person (1/2/3), number (singular/plural)) 

For French, use the mood/tense namings from the bescherelle

Refer https://en.wikipedia.org/wiki/French_conjugation
https://nl.wikipedia.org/wiki/Werkwoordsvervoegingen_in_het_Nederlands
```

An hour later I had a rule-based engine for French and Dutch. The scraped corpus from VerbodromeX, the thing I had always used instead of an engine, became its test suite. It conjugates verbs that don't exist:

```sh
npm run cli -- fr ploucer --mood indicative --tense present --person 1 --number plural
# nous plouçons

npm run cli -- nl zorpen --mood indicative --tense past --person 1 --number singular
# ik zorpte
```

Then I designed the trainer as a mockup in Claude Design and gave Claude Fable 5.2 a second prompt:

```
Consider this Claude Design project. Implement it.

the goal is that the conjugation engine (which now makes up the repository) is a
  standalone module that is referenced / used by other apps. you may modify it to enable the
  aforementioned application if needed (eg. to generate purposefully wrong answers) but if
  you can isolate them in the 'core' verbodrome functionality that would be better. would it
  be possible to host this as a static website?
```

It was possible. The engine stayed a standalone package, the trainer is a separate app on top of it, and the purposely wrong answers live in the trainer, not in the engine. It runs as a static site, and you can try it at [wouteronarchitecture.com/verbodrome](https://wouteronarchitecture.com/verbodrome/). You get a Dutch prompt and pick the French form out of ten cards. The wrong cards are not random: they are built from the engine's grammatical parts, by swapping the auxiliary, regularising the stem, putting an indicative after *que*, dropping an accent, changing the ending. So every wrong answer comes with the kind of mistake you made: *verkeerd hulpwerkwoord* (wrong auxiliary), *verkeerde stam* (wrong stem), *indicatief na que*. Telling people why their answer was wrong was the feature they asked for most, and in 25 years I never got around to it. Missed questions come back after four to eight others, and weak tenses are asked more often, the same idea as Pascale's algorithm from 2003.

![Verbodrome 2026: the Dutch prompt "ik heb" over Paris, with ten French answer cards.](/assets/posts/verbodrome/verbodrome-2026-question.jpg)

*Verbodrome, 2026. The Paris in the background is AI-generated. I had learned my lesson about city photos.*

![Feedback after picking "tu vas aimer" for "jij houdt van": wrong tense, futur proche instead of présent, with the rule for regular -er verbs.](/assets/posts/verbodrome/verbodrome-2026-feedback.jpg)

*The feature everybody asked for: "tu vas aimer" means "jij gaat houden van", futur proche, so* verkeerde tijd *(wrong tense), followed by the rule for regular -er verbs.*

## Looking back

Over 25 years I must have spent well over a thousand hours on this program. The thing I wanted most from the beginning, an engine that knows the rules, now came out of one prompt and an hour of work. I find that amazing, and a little hard to take in.

Look at what is in that prompt, though. In the [Arius 7 post](/arius-7-and-spec-driven-development-at-scale-a-failure-mode/) I found that agents implement the domain concepts you give them and don't invent the ones you leave out. This prompt contains the one domain decision I had been carrying around since 2001: start from the rules, don't store the conjugations. The other requirements (invented verbs as the test, the Bescherelle names, the scraped corpus as test data) all come from earlier versions. Without those versions I would not have known what to ask for.

The other thing I see now is what the project actually was. It barely made any money, the publishers turned it down and a photo ended it. But every version taught me something I used later: VBA, VB6 and Access, sockets and .NET Remoting, WinForms and ClickOnce, WinRT, WCF on Azure, AngularJS, ASP.NET Core with Azure AD B2C, and now working with agents. It was where I tried out new technology, and French verbs were the test case every time.