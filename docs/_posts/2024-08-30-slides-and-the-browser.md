---
layout: post
title:  "Slides and the browser"
excerpt: History and future of slide shows
image: assets/images/projector.webp
---

![Opera Show Projector](/assets/images/projector.webp)


# Slides and the browser

0. Microsoft PowerPoint or Google Slides experience is abysmal
1. Opera 4.0, released in 2000, had Opera Show for displaying HTML documents as slides
2. Since then, Beamer (2003), S5 (2004), W3C-backed Slidy (2005),  Slideous (2005), DZSlides (2011), and the React-based Reveal.js (2011) were created
3. S5, Slidy, Slideous were inspired by Opera Show
4. Beamer comes from the academic and LaTeX world
5. One of the reasons the Opera browser and Chrome browser were created was to finally add proper support for printing
6. One of the reasons CSS was created was to support various media types other than computer screen
7. As of 2024, there's no indication that HTML + CSS will get proper printing or non-screen media support

# Markdown
1. But there was a Markdown revolution.  It was inspired by webpage templating and wiki page editing
2. It's easy to create a human-readable document that can also be styled by automated tools
3. As of 2024, GitHub Flavored Markdown (GFM) is one of most popular variant.  This text is created using GFM syntax
4. Markdown is simple by design, thus it needs to contain extensions
5. I need a graph drawing extension and mathematics formula extensions

# Graph drawing
1. Mermaid has strong development momentum and is being adopted by various tools.  
2. Mermaid syntax is as simple as Markdown syntax
3. Mermaid uses two layout engines: Dagre and ELK
4. Dagre implements some graph algorithms in JavaScript
5. Eclipse Layout Kernel (ELK) is written in Java
6. Mermaid has started using Cytoscape.js as a third layout engine
7. Cytoscape.js is written in JavaScript and implements more graph algorithms

# Mathematics formulas
1. There's one gold standard - Knuth's TeX (1978), and on top of it, Lamport's LaTeX (1984)
2. Every mathematics and computer science student should use it for their Master of Philosophy thesis
3. There's MathML, but it's as cumbersome as XML, so it should be treated as a binary format - not for human eyes

# Editor support
1. Live preview of a document during editing can either help or hinder writing
2. It's possible to embed a web browser for Markdown preview, including previews from numerous extensions

# Binding it together
1. A lot of work has been put into interoperability between these technologies
2. For example the Visual Studio Code Markdown Preview Enhanced extension integrates GFM, React, Viz, TailwindCSS, ImageMagick, KaTeX, Markdown-it, Mermaid, PlantUML and even Puppeteer to launch a web browser in the background.  It's an endless mess of dependencies, however surprisingly well-behaved out of the box
3. There's also Pandoc (2006), written in Haskell
4. Pandoc is the tool designed to bring all of the mentioned elements together
5. Pandoc includes the Beamer LaTeX extension for producing slideshows
6. Pandoc and Beamer support Markdown and LaTeX mathematics formulas but do not have native support for Mermaid graphs in Markdown
7. But Pandoc supports user-provided extensions.  Such as mermaid-filter
8. All together: GFM → Pandoc → [mermaid-filter → mermaid → SVG → rsvg-convert → png] → Beamer → LaTeX → TeX → MacTeX → GhostScript → PDF
9. Or: GFM → Pandoc → [mermaid-filter → mermaid → SVG → rsvg-convert → png] → KaTeX (for maths) → HTML+CSS+JavaScript
10. The output sizes:

| Format    |    Size  |
|-----------|---------:|
| Slidy     |    72 KB |
| DZSlides  |    84 KB |
| Slideous  |   160 KB |
| S5        |   200 KB |
| PDF       | 4 100 KB |
| Reveal.js | 6 900 KB |

# References
- [https://web.archive.org/web/20021021190945/http://www.opera.com/support/tutorials/operashow/](https://web.archive.org/web/20021021190945/http://www.opera.com/support/tutorials/operashow/) -  Opera Show
- [https://meyerweb.com/eric/tools/s5/primer.html](https://meyerweb.com/eric/tools/s5/primer.html) - S5, Simple Standards-based Slide Show System
- [https://meyerweb.com/eric/tools/s5/s5-intro.html](https://meyerweb.com/eric/tools/s5/s5-intro.html) - S5 history
- [https://www.w3.org/Talks/Tools/Slidy2/Overview.html](https://www.w3.org/Talks/Tools/Slidy2/Overview.html) - Slidy
- [https://paulrouget.com/dzslides/](https://paulrouget.com/dzslides/) - DZSlides
- [https://goessner.net/articles/slideous/](https://goessner.net/articles/slideous/) - Slideous
- [https://revealjs.com](https://revealjs.com) - Reveal.js, The HTML Presentation Framework
- [https://github.github.com/gfm/](https://github.github.com/gfm/) - GitHub Flavored Markdown
- [https://mermaid.js.org](https://mermaid.js.org) - Mermaid, Diagramming and charting tool
- [https://github.com/dagrejs/dagre/wiki](https://github.com/dagrejs/dagre/wiki) - Dagre, Directed Graphs' Layout Engine
- [https://eclipse.dev/elk/](https://eclipse.dev/elk/) - Eclipse Layout Kernel, Automatic Layout for Diagrams
- [https://cytoscape.org](https://cytoscape.org) - Network Data Integration, Analysis, and Visualization in a Box
- [https://tug.org](https://tug.org) - TeX, Typesetting program
- [https://www.latex-project.org](https://www.latex-project.org) - LaTeX, A document preparation system
- [https://github.com/shd101wyy/vscode-markdown-preview-enhanced](https://github.com/shd101wyy/vscode-markdown-preview-enhanced) - Markdown Preview Enhanced, Microsoft Visual Studio Code extension
- [https://pandoc.org](https://pandoc.org) - Pandoc, a universal document converter
- [https://github.com/raghur/mermaid-filter](https://github.com/raghur/mermaid-filter) - Pandoc  Mermaid filter
