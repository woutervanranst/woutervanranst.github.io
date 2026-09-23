---
layout: post
title: 'When You Outsource the Work, You Outsource the Knowledge'
date: 2026-09-23 09:00:00
permalink: /when-you-outsource-the-work-you-outsource-the-knowledge/
subtitle: 'How local cost savings, change requests, and lost expertise can shift the economics and bargaining power of outsourcing.'
---

I recently read [*Flying Blind: The 737 MAX Tragedy and the Fall of Boeing*](https://www.goodreads.com/book/show/55994102-flying-blind) that details how corporate dysfunction and outsourcing over a long period eventually produced the 737 MAX disaster. It was written in the stars that this was going to happen: in 2001, Boeing engineer L. J. Hart-Smith wrote a paper with a brilliant tongue-in-cheek title: [*Out-Sourced Profits: The Cornerstone of Successful Subcontracting*](https://techrights.org/wp-content/uploads/2022/06/2014130646.pdf). His question was simple: when outsourcing reduces costs, *whose* costs are being reduced, and what happens to the cost of the whole undertaking?

He was writing about aircraft. The pattern applies wherever separate teams and organizations have to deliver one outcome. A cheaper component can make the finished product more expensive. A cheaper contract can leave the customer dependent on the supplier. And the people who still understand the work tend to end up on the side doing it.

I worked at one of the bigger IT connsulting firms, so I've seen this from the supplier side. There are quite a few parallels to be drawn.

**TL;DR:** Local cost savings can raise the total cost of a complex project. Scope starts changing as soon as the work begins, 'change requests' are the name of the game. Meanwhile, the customer gradually loses the expertise needed to specify, judge, or take back the work. That is when bargaining power moves to the supplier.

## The cheapest part can make the whole more expensive

Hart-Smith's first graph distinguishes a local minimum from a global one. **You can minimize one cost,** or find the best answer within a chosen constraint, **and still be well above the lowest cost for the entire system.**

![Schematic redraw of Hart-Smith's Figure 1. Minimizing one variable gives a higher total cost than a constrained minimum, which in turn is higher than the global minimum.](/assets/posts/when-you-outsource-the-work-you-outsource-the-knowledge/local-vs-global-optima.png)

*Local (outcourced) optima vs global optima [source](https://techrights.org/wp-content/uploads/2022/06/2014130646.pdf#page=5).*

Think about a typical outsourcing decision. Procurement tries to lower the contract price. A manager tries to reduce headcount. The supplier tries to protect its margin. Each party can hit its target while the organization pays more for specification, coordination, rework, change control, integration, and the next generation of the product or service.

Some constraints are real. Others are artifacts of how we measure people: a headcount cap, a departmental budget, a target day rate. When one of those becomes the definition of success, it is easy to celebrate a saving at point A while the total cost of the work sits far above C.

Hart-Smith describes an accounting spiral that makes this worse. After work moves to a supplier, the overhead (specifying, managing, and checking it) still takes effort. Those costs can be charged to the work left in-house, making internal capability appear even more expensive. That becomes the case for outsourcing the next piece.

## The sport of change requests

The initial price usually assumes that we are clairvoyant and that we know everything upfront.

In a complex project, scope starts shifting on day one. People see a prototype and understand what they actually need. An interface exposes a dependency nobody had documented. A business rule has an exception. A decision that looked settled turns out to depend on a different team. Discovery is part of the work.

A contract needs a baseline, though, and every new discovery prompts a question: is this included or is it a change? During my career I saw what I can only call the *sport of change requests*. The supplier has an incentive to classify work as outside the agreed scope; the customer has an incentive to argue it was always implied. Each discussion consumes time. Each accepted request adds cost. The low original bid gradually stops resembling the total bill.

These incentives are ordinary. Suppliers price the risk they take. Customers learn and revise priorities, and complex systems reveal surprises. The mistake is treating the initial specification as a complete map of reality, then treating every encounter with reality as a separately priced exception.

Hart-Smith [noticed the same mechanism in manufacturing](https://techrights.org/wp-content/uploads/2022/06/2014130646.pdf#page=4): a supplier needs a much more exact specification, and omissions or refinements can turn into costly contract discussions. Changes have a cost inside an organization too. Across a contract boundary, they also require negotiation, classification, approval, and often another handover.

The local metric says the original contract was cheap. The global cost includes all the change requests and the management overhead.

## The loss happens gradually

The more serious cost is harder to put in a business case. It is the loss of the ability to understand and direct the work. What typically happens:

1. Internal experts explain the system, the customer, and the undocumented exceptions to the supplier.
2. The supplier does the work and learns which assumptions were wrong, which problems recur, and which apparently simple changes have awkward consequences.
3. Experienced internal people leave or move into roles that manage the contract. Their replacements learn budgets, status reports, and supplier relationships instead of the work itself. See also my post on [Monkeys, Bananas and Why](https://wouteronarchitecture.com/monkeys-bananas-and-why/).
4. The next specification has less of the context that matters. The supplier fills in the gaps, while the customer becomes less able to challenge the proposed solution or the estimate.

Each step can look reasonable on its own. Together they form **a feedback loop: the less work you do, the less you know; the less you know, the harder it becomes to take work back**.

I saw client-side organizations whose remaining staff had become, for want of a better description, Excel-and-PowerPoint managers. They knew the rates, milestones, and status colours. They had lost enough practical knowledge of the work that they struggled to judge whether a design or an estimate made sense. Those were the jobs the outsourcing model had left them: managing a contract instead of learning from the work.

A few architects and managers do not automatically restore that knowledge. A diagram can be accurate and still leave out how a system behaves on a bad Tuesday, why a customer exception exists, or what breaks when a process changes. To make good judgments, some people inside the company have to stay close to delivery and operations.

## Bargaining power follows the expertise

Then the contract comes up for renewal.

On paper, the customer can invite competing bids. In practice, **the incumbent knows where the bodies are buried**: the implcit decisions, the awkward interfaces, the operational shortcuts, and the risks of a transition. A new bidder needs to price the unknowns. The customer no longer knows enough to tell whose estimate is credible.

That is bargaining power. It doesn't require the supplier to own the intellectual property or behave badly. It is enough for the supplier to be the only party that can confidently say what a change will take and what might go wrong. Replacing them means paying someone else to relearn the system, while the business continues to depend on it.

Hart-Smith makes a related point about sole-source suppliers: the organization assembling the aircraft **cannot simply let a critical supplier fail**, because the whole programme is at stake. The party that owns the end result carries the integration risk; the supplier with scarce capability gains leverage.

Having the documentation, the code, or the contract is useful, but none of those instantly recreates the judgment built by years of doing the work.

## Keep enough capability to steer

Hart-Smith did see a case for outsourcing to specialists with better facilities and enough customers to make those facilities worthwhile. The question is whether the outside party brings a genuine capability, and whether the customer can still understand and steer the result.

To avoid all of this, companies doing outsourcing still need to allow their own people to own and perform a meaningful slice of the workL investigate a failure, challenge an estimate, and explain the consequences of a design choice. Consulting companies on the other hand should budget for discovery and change instead of pretending every requirement is knowable at the start. Fixed price projects are doomed from the start imo. And at the end, I would judge the sourcing arrangement by the full cost of delivery, change, operations, and transition, including the change requests.

**A useful test is to ask what happens if the incumbent supplier cannot help next month. Can the organization still make a decision, diagnose a problem, and explain the work to someone new? If the answer is no, a competitive tender at renewal time offers less choice than it appears to.**

The loss of control rarely arrives as one dramatic decision. It happens one handover, one change request, one departed expert, and one renewal at a time. Eventually the supplier holds the expertise and the customer holds the spreadsheet. The bargaining power follows the expertise.
