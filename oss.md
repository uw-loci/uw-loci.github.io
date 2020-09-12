---
title: Open Source in Science
---
# Open Source in Science

We believe in open software standards, open source licensing and open
development processes.

We do science to *discover knowledge* and to *improve the human
condition*. Computers help by enabling *quantitative research*:

- **Data provenance** - recording what we did and how we did it
- **Knowledge transfer** - explaining the research to someone else
- **Reproducibility** - verifying or invalidating the research of others

To fully achieve these ideals, [open
science](https://en.wikipedia.org/wiki/Open_science) practices are
necessary. Given the complexity of 21st century science, full details of
the research methods must be open to public scrutiny for there to be any
hope of achieving scientific
[reproducibility](https://en.wikipedia.org/wiki/Reproducibility).

## Lessons from software

> "When in doubt, make it public."  
> —[Jeff Atwood](https://blog.codinghorror.com/when-in-doubt-make-it-public/)
> (co-creator of Stack Overflow)

Science can learn many lessons from the recent evolution of computing
toward the Internet:

- [Wikipedia](https://wikipedia.org/): a public encyclopedia
- [Stack Exchange](https://stackexchange.com/): a public knowledge base
- [GitHub](https://github.com/): a public source code repository
- [Facebook](https://facebook.com/): a (quasi-)public social network
- [Google](https://google.com/): a searchable database of public online content

The utter success of such tools serves to illustrate the power of
public-facing resources and information.

## Requirements for open scientific software

> "An article about computational science in a scientific publication is
> *not* the scholarship itself, it is merely *advertising* of the
> scholarship."  
> —[David Donoho](https://www-stat.stanford.edu/~wavelab/), "Wavelab and Reproducible Research," 1995

Analogously in science, a full research publication includes:

1.  Protocols and methodology
2.  Raw data
3.  Computer code

In particular, reproducibility demands the source code of any software
used to process the data. It turns out it is good for your career
anyway:

> "Papers describing software published as open source are amongst the
> most widely cited publications (e.g., BLAST, and Clustal-W),
> suggesting many scientific studies may not have been possible without
> some kind of open software to collect observations, analyze data, or
> present results."  
> —Andreas Prlić & James Procter  
> [Ten Simple Rules for the Open Development of Scientific Software](https://doi.org/10.1371/journal.pcbi.1002802)

The [Science Code Manifesto](https://sciencecodemanifesto.org/) concisely
summarizes the requirements for scientific software source code:

* __Code__ - All source code written specifically to process data for a published paper must be available to the reviewers and readers of the paper.
* __Copyright__ - The copyright ownership and license of any released source code must be clearly stated.
* __Citation__ - Researchers who use or adapt science source code in their research must credit the code's creators in resulting publications.
* __Credit__ - Software contributions must be included in systems of scientific assessment, credit, and recognition.
* __Curation__ - Source code must remain available, linked to related materials, for the useful lifetime of the publication.

> "Nobody is entitled to demand technical support for freely provided
> code: if the feedback is unhelpful, ignore it."  
> —N. Barnes, [Publish your computer code: it is good enough](https://www.nature.com/news/2010/101013/full/467753a.html)

Some top reasons people cite for not sharing their scientific source
code include:

- Time to document and clean up
- Dealing with questions from users
- Not receiving attribution

*\[Source: [Victoria Stodden on Open Science, July 2009](https://www.youtube.com/watch?v=lGNv0EtqC64)\]*

Fortunately, thanks to software licenses like the [Community Research
and Academic Programming License
(CRAPL)](http://matt.might.net/articles/crapl/), releasing source code
for the purposes of verification and reproducibility need not require
any cleanup or support. As for attribution, releasing software code
publicly is the best way to establish "prior art" in case infringement
occurs, which can happen regardless of whether the source is published.

The CRAPL's stipulations include:

1.  Permission to validate results
2.  Disregard any evidence of quality
3.  No mocking the author
4.  No support

The CRAPL offers a pragmatic way of saying: "Here is what I did—with no
promises whether it will work for you."

## Beyond open results: towards an open process

![](/scientific-method.jpg)  
*\[[Source](https://everydayscienceforall.blogspot.com/2010/12/scientific-method.html)\]*

In actuality, both science and software are not results, but continuous
processes. The best and most enduring scientific software is built to
grow a community using an open [software development
process](https://en.wikipedia.org/wiki/Software_development_process):

-   Improve software as a worldwide community
-   Open access resources, including:
    -   Open revision history (e.g., Git)
    -   Open mailing lists and/or forums
    -   Open issue tracker
    -   Open project roadmap
    -   Open contribution mechanism (e.g., [GitHub pull requests](https://help.github.com/articles/using-pull-requests))
-   Responsive, reliable maintainers
-   Powerful collaboration tools like GitHub
-   A focus on interoperability and extensibility

All of [LOCI's software efforts](https://uw-loci.github.io/) are
driven with the above goals in mind.

Other aspects of an open scientific process may include:

- **Open access journals**, to make research results truly publicly accessible
  \[[further reading](https://doi.org/10.1371/journal.pone.0011273)\]
- **Open peer review**, to increase editorial transparency
  \[[further reading](http://blogs.nature.com/peer-to-peer/2006/06/perspective_the_pros_and_cons.html)\]

Of course, there are new challenges when taking an open process so far,
but we believe the benefits of doing so are well worth it.

## The benefits of standardization

One key way of achieving software interoperability is the adoption,
reuse and (where necessary) definition of *software standards*. Such
adoption is of massive benefit to both individual scientists and
commercial and academic organizations. A brief article on the benefits
of standardization from `thinkstandards.net` (now offline) provides an
excellent summary:

> An extensive study initiated by DIN (German Standards Institute) and
> the German Federal Ministry of Economic Affairs and Technology in 1997
> was completed in May 2000. The study provides detailed insight into
> the economic benefits for standards—to businesses and to the economy.
> Highlights of the study include:
>
> - Standards contribute more to economic growth than patents and
>   licenses
> - Standards play a strategic significance to companies
> - Companies that participate actively in standards work have a head
>   start on their competitors in adapting to market demands
> - Research risks and development costs are reduced for companies
>   contributing to the standardization process
> - Business that are actively involved in standards work more
>   frequently reap short and long term benefits with regard to costs
>   and competitive status than those who do not participate
> - Participating in standards development enables one to anticipate
>   technology standardization thereby facilitating one's products
>   progress simultaneously with technology
> - Leaders in technology should become more involved in standards
> - Standards are a positive stimulus for innovation
> - Standards are internationally respected

You can read DIN's full publication, [Economic Benefits of
Standardization](https://www.din.de/blob/89552/68849fab0eeeaafb56c5a3ffee9959c5/economic-benefits-of-standardization-en-data.pdf),
in PDF format.

Many other articles have been published by various organizations
documenting the advantages of standardization:

- IBM: [Standards and reuse](https://www.ibm.com/developerworks/rational/library/2277.html)
- Dell: [Standardization: The Secret to ITT Leverage](https://www.dell.com/downloads/global/solutions/idc_standard.pdf) (PDF)
- BSI Group: [Economic benefits of standards – research reports](https://www.bsigroup.com/en-GB/standards/benefits-of-using-standards/research-reports/)
- Nah Soo Hoe's FOSS book on Open Standards: [Importance and Benefits of Open Standards](https://en.wikibooks.org/wiki/FOSS_Open_Standards/Importance_and_Benefits_of_Open_Standards)
