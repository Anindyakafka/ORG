# Org-mode: Emacs/Spacemacs/DOOM emacs and how it helps me with research.

---

#Introduction

This guide introduces an open-source toolkit for academic research and
writing. The main features of this toolkit centered around Emacs and
Org-mode are:

- embedded R code in the document that allows for statistical results
  to be revised and reproduced,
- bibliographic citations from a personal bibliographic database,
- formatting using well defined styles with minimal markup,
- support for production of final output as pdf, odt, docx, html and
  many other formats.

<a id="orgc90b25b"></a>

## Will this guide be useful for you?

This guide will be useful for you if you are writing a research paper,
a dissertation or an academic book. It would be useful if your writing
involves one or more of the following:

- Citing existing literature in your area
- Presenting results of statistical analyses (in tabular form and/or
  graphically)
- Using mathematical equations

Following this guide would need some investment of time but benefits
far outweigh the investment you make.

<a id="orgec97dfe"></a>

## What is our goal?

What are the most interesting features of the writing platform that
you will set up using this guide?

- With easy style specifications that you provide, the document will
  be almost-entirely automatically formatted by the software.
  
  - Complicated LaTeX-style markup is a pain and Openoffice/MS-Word
    documents require too much manual formatting. Basic Org-mode
    mark-up is extremely simple, and can be mastered in very little
    time.
  - Org-mode can produce well formatted output in LaTeX, pdf, odt,
    docx, html and many other formats.
- Instead of including statistical results (tables, graphs, etc), we
  would embed appropriate R programs in the document, so that when the
  formatted output is produced, all programs are run to generate the
  results. Advantages of doing this are:
  
  - Any changes in the data being used can be accommodated just by
    publishing the document again.
  - Any modifications in statistical analysis are easily made by
    modifying the programs that are embedded in the file itself.
  - Anyone who has the org file, can reproduce your results. You can
    also extract all R programs from the org file and distribute those
    for reproduction of your results.
- The document will be integrated with a citation manager, so that
  bibliographic information will be pulled automatically from a
  central database to create a fully formatted bibliography.
  
  - You will maintain a bibliographic database in BibTeX format, that
    you can build over time, adding bibliographic information for
    works that you cite.
  - Many websites (including Google Scholar) provide bibliographic
    information directly in BibTeX format, and we will have integrated
    tools that will allow us to pull this information directly into
    our local database.

<a id="org22ceafa"></a>

## Acknowledgements

In my adaptation of org, I have benefited immensely from the great
community of org-mode developers and users. The [Org-mode manual](http://orgmode.org/manual/), [Worg](http://orgmode.org/worg/),
and archives of the [Org-mode mailing list](http://orgmode.org/community.html) have been the most important
resources. In addition, I have greatly benefited from solutions
provided by various people to my specific queries on the Org-mode
mailing list. What I present in this document is essentially a
synthesis of solutions provided by various people. The community has
been extremely generous in providing these.

I would particularly like to thank

- Carsten Dominik, the author of Org-mode.
- Bastien Guerry, who has been a great maintainer of Org-mode, after
  Carsten passed on the baton to him.
- Nicolas Goaziou, who wrote the brilliant new exporter framework. The
  amount of code Nicolas has contributed to Org over the last two
  years or so is incredible. Nicolas very kindly responded to several
  of my queries.
- Eric Schulte, the main author of Babel, which gave Org mode the
  ability to execute code. I used to use Org-mode as a task manager
  and for taking notes. I discovered org-babel in the summer of 2010,
  when I was doing fieldwork in villages in eastern India. This
  discovery completely changed my work flow, and Org-mode became
  central to all my academic work.
- John Kitchin, the author of scimax, org-ref and org-ref-cite, with
  whose use of org-mode I have the greatest overlap. His documentation
  and videos are a great resource.
- In addition to the above, Suvayu Ali, for responses to several of my
  queries on the mailing list.

<a id="org0886f9b"></a>

# Installing necessary software

This set up will work with any operating system. I have tested it on
GNU/Linux and Mac OS-X, but it should work on Windows as well. For
this setup, you need to install Emacs (Version 24 along with a few
additional Emacs packages), Texlive, R (along with whatever
additional R packages you want to use) and Pandoc.

<a id="org4980734"></a>

## Emacs

- GNU/Linux
  
  Emacs can be installed using package managers of all GNU/Linux
  distributions. Latest versions of most common distributions provide
  version 24. I strongly recommend using the latest version of Emacs.
  
- Mac OS-X
  
  The built-in Emacs on OS-X is an older version, and it would be a
  good idea to install the latest version instead.
  
  The best option is to install it via homebrew. I like the version
  available from railwaycat/emacsmacport tap
  (<https://github.com/railwaycat/emacs-mac-port>).
  
  After installing homebrew, or if you already have it installed, just
  do the following from the terminal
  
  `$ brew tap railwaycat/emacsmacport`
  
  `$ brew install emacs-mac`
  
- Microsoft Windows
  
  Download the latest version of Emacs from
  <http://ftp.gnu.org/gnu/emacs/windows/>, and install.
  

<a id="org5f9d875"></a>

## Texlive

- GNU/Linux
  
  Texlive can also be installed from package managers in most
  GNU/Linux distribution.
  
- Mac OS-X
  
  For OS-X, install MacTeX from <http://www.tug.org/mactex/>
  
- Microsoft Windows
  
  For Windows, download Texlive and follow instructions from
  <https://www.tug.org/texlive/doc.html>
  

<a id="org10c6f77"></a>

## R

In this guide, I assume that you are familiar with R
(<http://www.r-project.org>). I will not cover R programming in this
guide.

For GNU/Linux, R can be installed from native package managers (look
for r-base in debian and debian-based distributions). For Mac OS-X and
Windows, download and see installation instructions at
<http://www.r-project.org>

<a id="org23cf48d"></a>

## Pandoc

Pandoc (<http://johnmacfarlane.net/pandoc/>) is an extremely powerful
converter, which can translate one markup into another. It supports
conversion between many file formats, and supports &ldquo;syntax for
footnotes, tables, flexible ordered lists, definition lists, fenced
code blocks, superscript, subscript, strikeout, title blocks,
automatic tables of contents, embedded LaTeX math, citations, and
markdown inside HTML block elements.&rdquo; That is pretty much everything I
use.

We shall use pandoc to convert our file from LaTeX to odt/docx/html
formats.

<a id="org661f435"></a>

# Emacs basics

GNU Emacs is an extensible platform. Although its primary function is
as an editor, it can be extended to do almost anything that you would
want your computer to do. Now, that really is not an overstatement. It
is a worthwhile aim to slowly shift an increasing number of tasks you
do on your computer to emacs-based solutions. For each major task you
do on your computer, ask if it can be done using emacs. For almost
everything, the answer is yes, and in most cases, emacs does it better
than other software you are used to. Many emacs users have learnt
emacs by shifting, one-by-one, all major tasks that they
do on the computer to emacs.

I am not going to give a detailed guide to use of emacs. A few tasks
for which I use Emacs include

- File management (copying files, moving files, creating directories)
- Reading and writing e-mails
- Reading RSS feeds
- Calender, scheduler, planner
- Calculator
- Statistical work (by hooking Emacs to R)
- And, of course, as an editor (including for writing research papers)

In this guide, I will just provide a minimal set of basic commands in
emacs to get you started. This is a minimal but sufficient set to be
able to work. I expect that you would learn more commands as you start
using emacs.

<a id="org40fa72e"></a>

## Notations

In emacs, a buffer is equivalent to a tab in a web browser. It is
normal to have several buffers open at the same time. Each file opens
in emacs as a buffer. Buffers could also have processes like R running
in them. Emacs displays any messages for you in a separate buffer.

Most commands in emacs are given using the Control (ctrl) or the Meta
(often mapped to alt) keys.<sup><a id="fnr.1" class="footref" href="#fn.1" role="doc-backlink">1</a></sup> Control key is usually referred to as `C-`
and the Meta key as `M-`. So a command `C-c` means pressing Control
and c together. Command `M-x` means pressing Meta and x
together. Everything is case-sensitive. So `M-X` would mean, pressing
Meta, Shift and x together. `C-c M-x l` would mean pressing C-c,
release, then M-x, release, and then l.

<a id="org3d14ac4"></a>

## Basic commands

Table [1](#orgd7bf1e8) gives the commands that are the most
important. This is a minimal set, commands that you should aim to
learn as soon as possible. There are many more, which you will learn
as you start using emacs.

All commands have a verbose version that can be used by pressing `M-x`
and writing the command. For example, `M-x find-file` to open a file.
All major commands are also mapped to a shortcut. For example, instead
of typing `M-x find-file` to open a file, you can say `C-x C-f`. I
remember shortcuts for commands that I use most frequently. For
others, I use the verbose versions. Over time, one learns more
shortcuts and starts using them instead of the verbose versions.

<table id="orgd7bf1e8" border="2" cellspacing="0" cellpadding="6" rules="groups" frame="hsides">
<caption class="t-above"><span class="table-number">Table 1:</span> Essential emacs commands</caption>

<colgroup>
<col  class="org-left" />

<col  class="org-left" />

<col  class="org-left" />
</colgroup>
<thead>
<tr>
<th scope="col" class="org-left">Description</th>
<th scope="col" class="org-left">Verbose command</th>
<th scope="col" class="org-left">Shortcut</th>
</tr>

<tr>
<th scope="col" class="org-left">&#xa0;</th>
<th scope="col" class="org-left"><code>M-x</code> followed by</th>
<th scope="col" class="org-left">&#xa0;</th>
</tr>
</thead>
<tbody>
<tr>
<td class="org-left"><i><b>Opening files, saving and closing</b></i></td>
<td class="org-left">&#xa0;</td>
<td class="org-left">&#xa0;</td>
</tr>

<tr>
<td class="org-left"><i>Open a file</i></td>
<td class="org-left"><code>find-file</code></td>
<td class="org-left"><code>C-x C-f</code></td>
</tr>

<tr>
<td class="org-left"><i>Save the buffer/file</i></td>
<td class="org-left"><code>save-buffer</code></td>
<td class="org-left"><code>C-x C-s</code></td>
</tr>

<tr>
<td class="org-left"><i>Save as: prompts for a new filename and saves the buffer into it</i></td>
<td class="org-left"><code>write-named-file</code></td>
<td class="org-left"><code>C-x C-w</code></td>
</tr>

<tr>
<td class="org-left"><i>Save all buffers and quit emacs</i></td>
<td class="org-left"><code>save-buffers-kill-emacs</code></td>
<td class="org-left"><code>C-x C-c</code></td>
</tr>

<tr>
<td class="org-left"><i><b>Copy, Cut and Delete Commands</b></i></td>
<td class="org-left">&#xa0;</td>
<td class="org-left">&#xa0;</td>
</tr>

<tr>
<td class="org-left"><i>Delete the rest of the current line</i></td>
<td class="org-left"><code>kill-line</code></td>
<td class="org-left"><code>C-k</code></td>
</tr>

<tr>
<td class="org-left"><i>To select text, press this at the beginning of the region and then take the cursor to the end</i></td>
<td class="org-left"><code>set-mark-command</code></td>
<td class="org-left"><code>C-spacebar</code></td>
</tr>

<tr>
<td class="org-left"><i>Cut the selected region</i></td>
<td class="org-left"><code>kill-region</code></td>
<td class="org-left"><code>C-w</code></td>
</tr>

<tr>
<td class="org-left"><i>Copy the selected region</i></td>
<td class="org-left"><code>copy-region-as-kill</code></td>
<td class="org-left"><code>M-w</code></td>
</tr>

<tr>
<td class="org-left"><i>Paste or insert at current cursor location</i></td>
<td class="org-left"><code>yank</code></td>
<td class="org-left"><code>C-y</code></td>
</tr>

<tr>
<td class="org-left"><i><b>Search Commands</b></i></td>
<td class="org-left">&#xa0;</td>
<td class="org-left">&#xa0;</td>
</tr>

<tr>
<td class="org-left"><i>prompts for text string and then searches from the current cursor position forwards in the buffer</i></td>
<td class="org-left"><code>isearch-forward</code></td>
<td class="org-left"><code>C-s</code></td>
</tr>

<tr>
<td class="org-left"><i>Find-and-replace: replaces one string with another, one by one, asking for each occurrence of search string</i></td>
<td class="org-left"><code>query-replace</code></td>
<td class="org-left"><code>M-%</code></td>
</tr>

<tr>
<td class="org-left"><i>Find-and-replace: replaces all occurrences of one string with another</i></td>
<td class="org-left"><code>replace-string</code></td>
<td class="org-left">&#xa0;</td>
</tr>

<tr>
<td class="org-left"><i><b>Other commands</b></i></td>
<td class="org-left">&#xa0;</td>
<td class="org-left">&#xa0;</td>
</tr>

<tr>
<td class="org-left">Divide a long sentence into multiple lines, each smaller than the maximum width specified</td>
<td class="org-left"><code>fill-paragraph</code></td>
<td class="org-left"><code>M-q</code></td>
</tr>

<tr>
<td class="org-left"><i><b>Window and Buffer Commands</b></i></td>
<td class="org-left">&#xa0;</td>
<td class="org-left">&#xa0;</td>
</tr>

<tr>
<td class="org-left"><i>Switch to another buffer</i></td>
<td class="org-left"><code>switch-to-buffer</code></td>
<td class="org-left"><code>C-x b</code></td>
</tr>

<tr>
<td class="org-left"><i>List all buffers</i></td>
<td class="org-left"><code>list-buffers</code></td>
<td class="org-left"><code>C-x C-b</code></td>
</tr>

<tr>
<td class="org-left"><i>Split current window into two windows; each window can show same or different buffers</i></td>
<td class="org-left"><code>double-window</code></td>
<td class="org-left"><code>C-x 2</code></td>
</tr>

<tr>
<td class="org-left"><i>Remove the split</i></td>
<td class="org-left"><code>zero-window</code></td>
<td class="org-left"><code>C-x 0</code></td>
</tr>

<tr>
<td class="org-left"><i>When you have two or more windows, move the cursor to the next window</i></td>
<td class="org-left"><code>other-window</code></td>
<td class="org-left"><code>C-x o</code></td>
</tr>

<tr>
<td class="org-left"><i><b>Canceling and undoing</b></i></td>
<td class="org-left">&#xa0;</td>
<td class="org-left">&#xa0;</td>
</tr>

<tr>
<td class="org-left"><i>Abort the command in progress</i></td>
<td class="org-left"><code>keyboard-quit</code></td>
<td class="org-left"><code>C-g</code></td>
</tr>

<tr>
<td class="org-left"><i>Undo</i></td>
<td class="org-left"><code>undo</code></td>
<td class="org-left"><code>C-_</code></td>
</tr>
</tbody>
</table>

<a id="customemacs"></a>

# Customising Emacs

Emacs is highly customisable. We will need a well customised emacs
installation with a number of additional emacs packages. There are
many configuration frameworks available for emacs (including
[spacemacs](https://www.spacemacs.org/), [doom](https://github.com/hlissner/doom-emacs), [prelude](https://github.com/bbatsov/prelude), and [scimax](https://github.com/jkitchin/scimax)). You may try these and choose
whichever you like. In most cases, you would need to do some
additional customisation to suit your needs.

My current emacs configuration does not use any of these
frameworks. Instead, I have a rather modular setup primarily based on
use-package for loading the additional packages I need. Packages are
installed mainly using package.el, the most popular package manager
for emacs. A few packages, including org-mode, are directly installed
from their git repositories to take advantage of the latest features
available.

### Clone the git repository

My configuration directory can be cloned from github using the following commands:

`$ mv .emacs.d .emacs.d.old`

`$ git clone --recurse-submodules -j8 https://github.com/ep624/emacs.d.2021.git ~/.emacs.d`

`$ cd ~/.emacs.d/org-mode && make`

The first command will move any existing emacs configuration to
.emacs.d.old, and the second command will install my configuration
instead. The third command compiles org-mode.

### Install additional packages

Starting version 24, Emacs includes a package-manager. You can
install/update add-on packages using the package manager. To use the
package manager, press M-x in emacs, and then type
package-list-packages and press return. This would bring up a list of
packages.

To mark a package for installation, take the cursor to the item and
press i. Once you have marked the packages you want to install, press
x to execute the installation.

The following emacs commands will install all the packages that I
currently have in my emacs installation. You many not need some of
them. You may also want some others depending upon your use. You can
always delete the ones you do not need, and add any other packages
that you need.

```
  (setq package-selected-packages '(ag anzu async auto-complete avy
                                    bibretrieve auctex cdlatex
                                    citeproc coffee-mode consult
                                    counsel-projectile
                                    counsel-tramp counsel
                                    dired-subtree dired-hacks-utils
                                    dirtree ebib edit-server
                                    elfeed-goodies ace-jump-mode
                                    elfeed-score elfeed emacsql
                                    embark erc-hl-nicks erc-image
                                    eshell-git-prompt
                                    ess-R-data-view ctable
                                    ess-r-insert-obj
                                    ess-smart-equals
                                    ess-smart-underscore ess-view
                                    ess-view-data csv-mode ess
                                    flx-ido flx flycheck
                                    git-gutter+ git-gutter
                                    highlight-indentation htmlize
                                    ido-completing-read+
                                    ido-vertical-mode iedit
                                    ivy-bibtex bibtex-completion
                                    biblio biblio-core
                                    ivy-prescient key-chord keycast
                                    kurecolor magit git-commit
                                    magit-section memoize move-text
                                    multi-web-mode multiple-cursors
                                    nameless noflet notmuch-labeler
                                    notmuch org-gcal alert log4e
                                    gntp org-sidebar org-ql f
                                    dash-functional
                                    org-sticky-header
                                    org-super-agenda ht ov parsebib
                                    pdf-tools peg persist
                                    persistent-scratch pinentry
                                    popup popwin powerline
                                    prescient pretty-hydra hydra lv
                                    projectile pkg-info epl
                                    quelpa-use-package quelpa queue
                                    rainbow-delimiters rainbow-mode
                                    remember-last-theme
                                    request-deferred request
                                    deferred simple-httpd
                                    smartparens string-inflection
                                    super-save swiper ivy tblui
                                    tablist magit-popup transient
                                    tree-mode ts s dash use-package
                                    bind-key visual-fill-column
                                    which-key windata with-editor
                                    yasnippet-snippets yasnippet
                                    zenburn-theme))
(package-install-selected-packages)
```

You can copy the above lines, paste them in an empty buffer (e.g.,
\\\*scratch\\\*), select them, and do M-x eval-region.

<a id="org1fbf4de"></a>

# Speed up with Yasnippet templates

You can considerably speed up your work in emacs by using
yasnippets. Yasnippets are chunks of text &#x2013; forms, templates, lines
of code &#x2013; that can be inserted in a buffer using a keyword. It allows
you to insert text that needs to be used repeatedly by using a short
keyword. If you have cloned my emacs configuration, a whole bunch of
snippets would be in $~/.emacs.d/snippets$ directory. I will
illustrate their use in the sections on org-mode.

<a id="orgmodebasics"></a>

# Org-mode basics

<a id="org6bdaede"></a>

## Preamble

An Org file has a few special lines at the top that set up the
environment. Following lines are an example of the minimal set of
lines that we shall use.

```
#+title: Using Emacs, Org-mode and R for Research Writing in Social Sciences
#+subtitle: A Toolkit for Writing Reproducible Research Papers and Monographs
#+author: Vikas Rawal
#+date: May 4, 2014
#+options: toc:2 H:3 num:2
```

As you can see, each line starts with a keyword, and the values for
this keyword are specified after the colon.

Table [2](#org1d59128) gives details of a few major special lines that we shall use. The table also gives snippet keywords that can used to create the keyword if you have got the yasnippets from my emacs configuration.

<table id="org1d59128" border="2" cellspacing="0" cellpadding="6" rules="groups" frame="hsides">
<caption class="t-above"><span class="table-number">Table 2:</span> Main special lines to be used at the top of an Org buffer</caption>

<colgroup>
<col  class="org-left" />

<col  class="org-left" />

<col  class="org-left" />
</colgroup>
<thead>
<tr>
<th scope="col" class="org-left">Keyword</th>
<th scope="col" class="org-left">Snippet</th>
<th scope="col" class="org-left">Purpose</th>
</tr>
</thead>
<tbody>
<tr>
<td class="org-left"><code>#+title</code></td>
<td class="org-left"><code><ti</code></td>
<td class="org-left">To declare title of the paper</td>
</tr>

<tr>
<td class="org-left"><code>#+author</code></td>
<td class="org-left"><code><au</code></td>
<td class="org-left">To declare author/s of the paper</td>
</tr>

<tr>
<td class="org-left"><code>#+date</code></td>
<td class="org-left"><code><da</code></td>
<td class="org-left">Sets the date. If blank, no date is used. If this keyword is omitted, current date is used.</td>
</tr>

<tr>
<td class="org-left"><code>#+options</code></td>
<td class="org-left"><code><op</code></td>
<td class="org-left">There are many options you can give. These are what I find the most important Multiple options can be separated by a space and specified on the same line.</td>
</tr>

<tr>
<td class="org-left">&#xa0;</td>
<td class="org-left">&#xa0;</td>
<td class="org-left">toc:nil (Do not include a Table of contents), toc:n (Include n levels of sections and sub-sections in Table of contents)</td>
</tr>

<tr>
<td class="org-left">&#xa0;</td>
<td class="org-left">&#xa0;</td>
<td class="org-left">H:2  (Treat top two levels of headlines as section levels, and anything below that as item list. Modify the number as appropriate)</td>
</tr>

<tr>
<td class="org-left">&#xa0;</td>
<td class="org-left">&#xa0;</td>
<td class="org-left">num:2 (Number top two levels of headlines. Modify the number as appropriate.)</td>
</tr>
</tbody>
</table>

In addition to these, we can use LaTeX specific options for formatting
the pdf output, odt specific options for formatting the odt/docx
output, and R specific options for setting up the R environment. These
would also be specified using special lines at the top of the file. I
shall provide details of some of these in the sections where these
topics are discussed.

In research monographs prepared using org, you may need quite a few
lines in the preamble to set everything. These would tend to make the
top of the org file look daunting. It may be helpful to store these
lines in a set of separate org files, and include them in your main
document using line such as this:

`#+INCLUDE: path-to-file/papersetup.org`

<a id="org3ece024"></a>

## Sections and headlines

After the special lines at the top comes the main body of the Org
file.

The content in any Org file is organised in a hierarchy of headlines.
Think of these headlines as sections of your paper.

A headline in Org starts with one or more stars (\*) followed by a
space. A single star denotes the main sections, double star denote the
subsections, three stars denote sub-subsections, and so on. We shall
use this to create the section structure of our document. You can
create as many levels of sections as you need.

See the following example. Note that headlines are not numbered. We
leave section numbering for org-mode to handle automatically.

<div class="footnotesize" id="org1142cea">
<div class="org-src-container">
<pre class="src src-org"><span style="color: #5B6268;">  #+TITLE:</span> <span style="color: #c678dd; font-weight: bold;">Reproducible Research Papers using Org-mode and R: A Guide</span>
<span style="color: #5B6268;">  #+AUTHOR:</span> <span style="color: #c678dd;">Vikas Rawal</span>
<span style="color: #5B6268;">  #+DATE:</span> <span style="color: #c678dd;">May 4, 2014</span>
  * Introduction

This is the first section. Add your content here.

Aliquam erat volutpat. Nunc eleifend leo vitae magna. In id erat non
 orci commodo lobortis. Proin neque massa, cursus ut, gravida ut,
 lobortis eget, lacus. Sed diam.

- Literature review
  
  ** Is this an important issue
  
  This is a sub-section under top-level section "Literature review".
  
  Now indulgence dissimilar for his thoroughly has terminated. Agreement
  offending commanded my an. Change wholly say why eldest period. Are
  projection put celebrated particular unreserved joy unsatiable its. In
  then dare good am rose bred or. On am in nearer square wanted.
  
  ** What are the major disputes in the literature
  
  <span style="font-weight: bold;">***</span> adulterated text
  
  Instrument cultivated alteration any favourable expression law far
  nor. Both new like tore but year. An from mean on with when sing pain.
  Oh to as principles devonshire companions unsatiable an delightful.
  The ourselves suffering the sincerity. Inhabit her manners adapted age
  certain. Debating offended at branched striking be subjects.
  
  <span style="font-weight: bold;">***</span> Unadulterated prose
  
  Announcing of invitation principles in. Cold in late or deal.
  Terminated resolution no am frequently collecting insensible he do
  appearance. Projection invitation affronting admiration if no on or.
  It as instrument boisterous frequently apartments an in. Mr excellence
  inquietude conviction is in unreserved particular.
  
  You fully seems stand nay own point walls. Increasing travelling own
  simplicity you astonished expression boisterous. Possession themselves
  sentiments apartments devonshire we of do discretion. Enjoyment
  discourse ye continued pronounce we necessary abilities.
  
- Methodology
  
  This is the next top-level section. There are no sub-sections under
  this.
  
- Results
  
  This is the third top-level section. There are sub-sections under
  this.
  
  ** Result 1
  
  This is a sub-section under section Results.
  
  ** Result 2
  
  This is another sub-section under section Results
  
- Conclusions
  
  This is the next and final top-level section. There are no
  sub-sections under it.
  </pre>
  
  </div>
  

</div>

Org handles these headlines beautifully. With your cursor on the
headline, pressing tab folds-in the contents of a headline. If you
press tab on a folded headline, it opens to display the contents. If
there are multiple levels of headlines, these open in stages as you
repeat pressing the tab key.

When you are on a headline, pressing M-return creates a new headline
at the same level (that is, with the same number of stars). Once you
are on the new headline, a tab moves it to a lower level (that is, a
star is added), and shift-tab moves it to a higher level (that is, a
star is removed).

When I start writing a paper, I start with a tentative
headline/section structure, and then start filling in the content
under each headline, and modify the section structure, if needed, as
the paper develops.

(For further reading, see [Headlines](http://orgmode.org/manual/Headlines.html#Headlines) in the Org-mode manual)

<a id="orgf4992d2"></a>

## Itemised lists

### Unordered (bulleted) lists

Following syntax produces unordered (bulleted) lists:

```
+ bullet
+ bullet
  - bullet2 1
  - bullet2 2
+ bullet
+ bullet
```

This is how this list shows up in the final document

- bullet
- bullet
  - bullet2 1
  - bullet2 2
- bullet
- bullet

Note that, in unordered lists, `+` and `-` signs are interchangeable.

### Ordered (numbered) lists

Following syntax produces ordered/numbered lists:

```
1. Item 1
2. Item 2
  1) Item 2.1
  2) Item 2.2
     1) Item 2.2.1
3) Item 3
```

This is how the ordered list shows up in the final document.

1. Item 1
2. Item 2
  1. Item 2.1
  2. Item 2.2
    1. Item 2.2.1
3. Item 3

Note that, in ordered lists 1. and 1) are interchangeable.

### Description lists

Description lists are used for providing definitions/explanations for
words/phrases like in a dictionary. Following syntax produces a
description list:

```
+ item1 :: description of item1
+ item2 :: description of item2
  - bullet1 under item2
  - bullet2 under item2
```

This is how this list shows up in the final document

- **item1:** description of item1
- **item2:** description of item2
  - bullet1 under item2
  - bullet2 under item2

Note that:

- In lists, levels of bullets and numbering are determined by indentation.
- Different types of lists can be mixed using numbers and bullets for different levels.
- If the cursor is on a line that is part of an itemised list, M-return inserts a new line with a bullet/number below the present
  line with the same level of indentation.

<a id="org0603797"></a>

## Inserting footnotes

- To insert a footnote at any point, use `C-c C-x f`
  
- To reorder and renumber footnotes after inserting a footnote in a
  text that already has some footnotes after the point where a new
  footnote is being inserted, use `C-u C-c C-x f S`
  

<a id="orga0d9ecc"></a>

## Tables

### Sample code

The following sample code produces a reasonably formatted table, with
a numbered title above the table and a name for cross-referencing the
table from the text anywhere in the document.

See Table [3](#org956200f) for an illustration of how this table shows
up in the final document.

```
#+name: table-yield
#+caption: Simple table created using LaTeX tabular environment
#+attr_latex: :environment tabular :width \textwidth :align lrr
 | State          | Average yield | Average income |
 |----------------+---------------+----------------|
 | Madhya Pradesh |           672 |          13000 |
 | Haryana        |           300 |          25000 |
 | Punjab         |           260 |          35000 |
```

<table id="org956200f" border="2" cellspacing="0" cellpadding="6" rules="groups" frame="hsides">
<caption class="t-above"><span class="table-number">Table 3:</span> Simple table created using LaTeX tabular environment</caption>

<colgroup>
<col  class="org-left" />

<col  class="org-right" />

<col  class="org-right" />
</colgroup>
<thead>
<tr>
<th scope="col" class="org-left">State</th>
<th scope="col" class="org-right">Average yield</th>
<th scope="col" class="org-right">Average income</th>
</tr>
</thead>
<tbody>
<tr>
<td class="org-left">Madhya Pradesh</td>
<td class="org-right">672</td>
<td class="org-right">13000</td>
</tr>

<tr>
<td class="org-left">Haryana</td>
<td class="org-right">300</td>
<td class="org-right">25000</td>
</tr>

<tr>
<td class="org-left">Punjab</td>
<td class="org-right">260</td>
<td class="org-right">35000</td>
</tr>
</tbody>
</table>

### Table editor

Org-mode has an in-built table editor, which is very simple to use.

- Tables in Org have columns separated using |.
- Once you create the first row by separating columns using |,
  pressing tabs takes you from the first column to the next. Org
  automatically aligns the columns.
- At the end of the row, pressing tab again, creates a new blank row.
  You can also create a new blank row by pressing return anywhere in
  the last row.
- For creating a horizontal line anywhere, type |- at the starting of
  the line, and press tab.
- Contents of each cell are aligned automatically by Org.
- To delete a row, use `C-k`.

Org provides various commands for manipulating the design of
tables. Table [4](#org76044f9) provides the most important
ones. Note that Table [4](#org76044f9) is created using Org mode. It
also gives you an idea of how the table would look eventually.

<table id="org76044f9" border="2" cellspacing="0" cellpadding="6" rules="groups" frame="hsides">
<caption class="t-above"><span class="table-number">Table 4:</span> Commands to manipulate tables in Org</caption>

<colgroup>
<col  class="org-left" />

<col  class="org-left" />
</colgroup>
<thead>
<tr>
<th scope="col" class="org-left">Command</th>
<th scope="col" class="org-left">Description</th>
</tr>
</thead>
<tbody>
<tr>
<td class="org-left"><code>M-<left></code></td>
<td class="org-left">Move the column left</td>
</tr>

<tr>
<td class="org-left"><code>M-<right></code></td>
<td class="org-left">Move the column right</td>
</tr>

<tr>
<td class="org-left"><code>M-S-<left></code></td>
<td class="org-left">Delete the current column</td>
</tr>

<tr>
<td class="org-left"><code>M-S-<right></code></td>
<td class="org-left">Insert a new column to the left of the cursor position</td>
</tr>

<tr>
<td class="org-left"><code>M-<up></code></td>
<td class="org-left">Move row up</td>
</tr>

<tr>
<td class="org-left"><code>M-<down></code></td>
<td class="org-left">Move row down</td>
</tr>

<tr>
<td class="org-left"><code>M-S-<up></code></td>
<td class="org-left">Delete the current row or horizontal line</td>
</tr>

<tr>
<td class="org-left"><code>M-S-<down></code></td>
<td class="org-left">Insert a new row above the current row</td>
</tr>
</tbody>
</table>

For more commands for manipulating tables, see [this section of the Org
manual](http://www.orgmode.org/manual/Tables.html). In particular, you may want to look at spreadsheet-like
functions of the table editor.

If you have snippets included with my emacs configuration, you can
type `nct1`, `nct2`, `nct3` or `nct4` and press the tab key to create
blank tables with 1-4 columns.

Note that we will directly create only those tables in org that are
not produced as a result of some statistical analysis. For tables that
are a result of some statistical analysis, we will embed R programs
rather than the tables themselves. This is discussed in Section
[7](#orgmodeandr) of this guide.

<a id="orgfacd9bb"></a>

## Images

You can insert images in documents as follows

```
[[file:path-to-file/a.jpg]]
```

You should do this for images that you already have, and you just want
to insert them in the document. If you have snippets included in my
emacs configuration, you can use yasnippet `ncf` to help in inserting
a named figure. For graphs produced by R, we will embed the code
instead, so that the graph is generated and inserted automatically.

<a id="org1125184"></a>

## Captions and cross-references

We would like to give a title to our tables and images. And we would
like to be able to refer to them from the text. These are achieved by
adding two lines above every table and image.

- A line starting with `#+caption:` placed just above a table or a
  figure adds a title to it. All Tables and Figures titles are
  automatically numbered.
  
- For referring to these Tables and Figures in the text, we shall name
  each table and figure in a line starting with `#+name:` as below.
  

To illustrate, for inserting an image, with a caption and a name, this
is what we shall do.

```
#+name: literacy-rate
#+caption: Percentage of literate men and women, by country (per cent)
[[file:a.jpg]]
```

Similarly, a table will be inserted as follows.

```
#+name: literacy-rate-table
#+caption: Percentage of literate men and women, by country (per cent)
| Country    | Men | Women |
|------------+-----+-------|
| India      |  75 |    43 |
| Bangladesh |  83 |    63 |
| Rwanda     |  77 |    60 |
```

To refer to the Table above in the text, write Table
`[[literacy-rate-table]]`. As an illustration, see the following sentence.

```
Tables [[literacy-rate-table]] and [[health-table]], and Figure
[[literacy-figure]], show the level of underdevelopment.
```

By default, all objects with captions are numbered, and names are used
to anchor cross-references. When the formatted output is produced, all
the references would be automatically converted to appropriate
numbers. If new objects are inserted in the paper, numbering will be
adjusted automatically when you create the formatted output.

<a id="orgmodeandr"></a>

# Org-mode and R

<a id="org85c25a8"></a>

## Configuration

Following code in .emacs.d/<sub>configs</sub>/use-org.el enables Org to run different types
of code. If you are using my emacs configuration, these are already enabled.

I have included here the languages that I commonly use. See Org
manual, if you would like to add any more.

```
(org-babel-do-load-languages
   'org-babel-load-languages
   '((R . t)
     (org . t)
     (ditaa . t)
     (latex . t)
     (dot . t)
     (emacs-lisp . t)
     (gnuplot . t)
     (screen . nil)
     (shell . t)
     (sql . nil)
     (sqlite . t)))
```

<a id="org00f0bcf"></a>

## Embedding R code in an Org document

Org uses ESS (emacs-speaks-statistics) to provide a fully functional,
syntax-aware, development environment to write R code. R code is
embedded into Org as a source block. The basic syntax is

```
#+name: name_of_code_block
#+BEGIN_SRC R <switches> <header-arguments>

  <Your R code goes here.>

#+END_SRC
```

This is how source blocks are created.

- First write the lines starting with `#+NAME`, `#+BEGIN_SRC` and
  `#+END_SRC`. We will use different snippets to quickly insert these
  lines for different types of code blocks.
  
- Then with your cursor in between the `BEGIN_SRC` and the `END_SRC`
  lines, give the command C-c &rsquo; (that is, press Ctrl-C, release, and
  press &rsquo;).
  
  - This would open a new buffer using ESS mode. If you type your code
    in this buffer, you will see that ESS is syntax-aware and nicely
    highlights R code.
    
  - ESS also allows you to run (evaluate) the code that you write, to
    test what your code is doing. `C-return` or `C-c C-n` can be used
    to evaluate a line/region of code and move to the next line. Or
    you can use `C-c C-j` for evaluating a single line of code, `C-c
    
    ```
    C-b` for evaluating the entire ess buffer, or `C-c C-r` for a
    ```
    
    marked region within the ess buffer.
    
- Once you have finished writing a code block and tested it, press
  `C-c '` again to come back to your Org buffer.
  
- In your Org buffer, with your cursor in a source-block, press `C-c
   C-c` to evaluate the whole code block and have the results included
  in your document.
  
- You can always edit your source code by opening a temporary ESS
  buffer using C-c&rsquo;
  

<a id="orgdba9223"></a>

## Code blocks that read data and load functions for later use in the document without any immediate output

I normally have one or two code blocks that read the data I am going
to use, call the libraries that I use, and define a few functions of
my own that I plan to use. I want this code block to be evaluated, so
that these data, libraries and functions become available in my R
environment. But no output from such code blocks is expected to be
included into the document.<sup><a id="fnr.2" class="footref" href="#fn.2" role="doc-backlink">2</a></sup>

Code block [13](#orgea4ca51) is an example of such a code block. Note
`:results value silent` switch used in the `#+begin_src` line.

```
#+name: readdata-code
#+BEGIN_SRC R :results silent :exports none
   read.data("datafile1.csv",sep=",",
             header=T)->mydata1
#+END_SRC
```

<a id="orgc50f3b3"></a>

## Code blocks that produce results in the form of an Org-mode table

The output of a lot of R code I write is presented in some kind of table.

There are two approaches for producing formatted, publication-ready tables.

1. We get R to produce bare tables, add Org-mode markup to those, and
  get Org-mode mode to export these into nice looking LaTeX
  tables.
  
2. Alternatively, we can get R to produce nicely formatted
  LaTeX tables, and let Org just export the LaTeX code as it is.
  

I will discuss the first approach in this section.

The code block may use data and functions made available by previous
code blocks, read some new data and may load some new functions. The
code block does some statistical processing. The last command of the
code block produces an object (for example, a data frame) that is
included in the document as a Table.

For example, the code block [14](#org135001e) below uses mydata1 read in
the previous code block, reads a new dataset, and processes them to
create a table that shows average BMI by country.

```
#+name: bmi-table-code
#+BEGIN_SRC R :results value :exports results :colnames yes :hline
  aggregate(height~Country,data=mydata1,mean)->a1
  read.data("datafile2.csv",sep=",",header=T)->mydata2
  aggregate(weight~Country,data=mydata2,mean)->a2
  merge(a1,a2,by="Country")->a1
  a1$weight/a1$height->a1$BMI
  subset(a1,select=c("Country","BMI"))
#+END_SRC
```

If you are using my emacs configuration, you can use the snippet
`srct` to insert a blank code block of this kind. You can then use
`C-c '` to go into a temporary ess buffer and write the code.

You can evaluate the code block using `C-c C-c`. When you do that, it
produces the output, and places it immediately below the code block.
The results display the output of the code under a line that looks
like below

```
#+RESULTS: bmi-table-code
```

Note that the results are tied to the code block using the name of the
code block. Every time you go to the source code block and press `C-c
C-c`, the code is evaluated again and the results are updated.

On top of the line starting with `#+RESULTS:`, we shall add two more
lines, to give the table a caption and a name. Note that the code
block and the result of the code block have separate names.

```
#+name: bmi-table-output
#+caption: Average BMI, by country
#+RESULTS: bmi-table-code
```

Like any Org table, you can cross-refer to this table using
`[[bmi-table-output]]`.

Section [9.1](#tableformatting) discusses how to format tables for LaTeX
export. All that can be done on tables created by source code blocks.

<a id="org642a5c1"></a>

## Code blocks that produce a formatted LaTeX table

An alternative approach would be to use various R packages designed
for producing formatted tables. R has excellent libraries for
producing tables formatted for LaTeX, html, RTF and docx
exports. Particularly noteworthy are [xtable](https://cran.r-project.org/web/packages/xtable/index.html), [gt](https://gt.rstudio.com/articles/gt.html)/[gtExtras](https://jthomasmock.github.io/gtExtras/), [kableExtra](https://haozhu233.github.io/kableExtra/),
and [tabularray](https://github.com/turbanisch/tabularray) packages. It is beyond the scope of the present
document to describe each of these. Of these, [xtable](https://cran.r-project.org/web/packages/xtable/index.html) is most versatile
but complex. [xtable](https://cran.r-project.org/web/packages/xtable/index.html) does not work out of the box with threeparttable
for producing table notes. [gt](https://gt.rstudio.com/articles/gt.html)/[gtExtras](https://jthomasmock.github.io/gtExtras/) are excellent but are mainly
aim at producing html tables. [kableExtra](https://haozhu233.github.io/kableExtra/) produces tables that work
with tabular, longtable and tabu LaTeX environments to produce tables,
while [tabularray](https://github.com/turbanisch/tabularray) produces output for tabularray LaTeX environment.
[kableExtra](https://haozhu233.github.io/kableExtra/) works well with threeparttable.

The code block [16](#orgfde0595) shows code that uses kableExtra
to produce a formatted LaTeX table. Similarly, code block
[17](#org07ee373) shows code that produces a table formatted
for tabularray LaTeX package.

A disadvantage of this approach would be that the results of the code
block will not contain an Org-mode table, which is a visually
appealing and functionally useful representation while working in
Org-mode.

One could, of course, use a combination of two approaches by creating
two separate code blocks: the first producing an Org-mode table that
is not meant for export (`:exports none`), and another that reads the
Org-mode table, and produces LaTeX code meant for export.

```
#+NAME: code1
#+BEGIN_SRC R :results value verbatim latex :exports results
 library(kableExtra)
 dt <- mtcars[1:5, 1:4]

 kbl(dt, format="latex", booktabs = T, caption = "Demo Table") |>
   kable_styling(latex_options = c("striped", "hold_position"),
                 full_width = F) |>
   add_header_above(c(" ", "Group 1" = 2, "Group 2[note]" = 2)) |>
   add_header_above(c(" ", "Group 3" = 4)) |>
     footnote(c("1. table footnote","2. another footnote"))->t
#+end_src

#+NAME: code2
#+BEGIN_SRC R :results value verbatim latex :exports results
  library(tabularray)
  library(dplyr)
  df <- starwars |>
  filter(homeworld == "Tatooine") |>
  select(name, height, mass, sex, birth_year) |>
  arrange(desc(birth_year))
  df |>
  mutate(sex = stringr::str_to_title(sex)) |>
  group_by(sex) |>
  tblr(type = "float", caption = "Starwars Creatures from Tatooine") |>
  set_source_notes(
    Note = "Entry C3PO altered to test characters that have a special meaning in LaTeX.",
    Source = "R package \\texttt{dplyr}"
  ) |>
  set_alignment(height:birth_year ~ "X[r]") |>
  set_column_labels(
    name = "",
    height = "Height",
    mass = "Mass",
    birth_year = "Birth Year"
  ) |>
  set_theme(row_group_style = "panel") |>
  set_interface(width = "0.7\\linewidth") |>
  set_column_spanner(
    c(height, mass) ~ "Group 1",
    birth_year ~ "Group 2"
  ) |>
  set_column_spanner(!name ~ "All my vars")->t
  t
#+end_src
```

<a id="org6be6936"></a>

## Code blocks that produce a graph to be included in the document

These code blocks can have a series of commands. The last command
produces a graph that we would like to be included in the document.

The following code shows an example of a code block that produces a
graph.

<div class="footnotesize" id="org11143d7">
<div class="org-src-container">
<pre class="src src-org"><span style="color: #83898d;">  #+name: mygraph-code</span>
<span style="color: #5B6268; background-color: #23272e;">  #+BEGIN_SRC R :results file graphics :exports results :file bmi2.png :width 825 :height 1050 :fonts serif :res 300</span>
<span style="background-color: #23272e;">    library(ggplot2)</span>
<span style="background-color: #23272e;">    data.frame(var1=c(1:10),var2=c(1:10)^2)->data</span>
<span style="background-color: #23272e;">    ggplot(data,aes(x=var1,y=var2))->p</span>
<span style="background-color: #23272e;">    p+geom_line()</span>
<span style="color: #5B6268; background-color: #23272e;">  #+END_SRC</span>
</pre>
</div>

</div>

You can use snippet `<srcg` and press tab to insert an empty code
block, and then go into a temporary ESS buffer by using C-c &rsquo;.

- Once in this temporary ESS buffer, you can write the R commands for
  making your graph.
- As you write, you can evaluate the commands using `C-c C-n`, `C-c
   C-r` and `C-c C-b` and see what your output looks like.
- The output is displayed on your screen using the default graphic
  device used by R (X11, quartz or windows graphic device depending
  upon your operating system).
- Once you have finalised your graph, you press C-c &rsquo; and come back
  to the Org buffer.

Note that creation of the image file is left to appropriate switches
in the `#+BEGIN_SRC` line. Org automatically chooses appropriate
graphic device to produce the file. When you evaluate this code using
`C-c C-c`, the results are displayed below the code block as follows.

```
#+RESULTS: mygraph-code
[[bmi2.png]]
```

Note that, taking the file name from our `#+BEGIN_SRC` line, a file
called `bmi2.png` was automatically created and linked, so that the
graph would be inserted in the document when you produce the formatted
output.<sup><a id="fnr.3" class="footref" href="#fn.3" role="doc-backlink">3</a></sup> Every time you evaluate the code using `C-c C-c`, the
underlying image file containing the graph is overwritten by a new
file.

As with the tables, we shall add a caption and a name to it. We can
also use `:attr_latex` to adjust width (andcorrespondingly the
height, maintaining the aspect ratio).

```
#+name: my-bmi-graph
#+caption: Average BMI, by Country
#+attr_latex: :width 5in
#+RESULTS: mygraph-code
[[gini.png]]
```

You can now refer to this graph in the text using `[[my-bmi-graph]]`.

<a id="org54242bd"></a>

## Named R Sessions

You can run R code blocks in two ways.

One possibility is to send the block as a batch of commands to R and
receive the results in the org-mode file. In this case, R runs each
block independent of the others. The code is sent to R, evaluated
independently, and results inserted in the org file. This is the
default behaviour.

Another possibility is to start a named R session within emacs, which
is persistent, and execute multiple code blocks in the same R
session. In this case, output of one code block (say, some data
objects that have been created or some libraries that have been
called) is available to the next code block that is executed in the
same session.

In fact, org allows you to run multiple R sessions simultaneously. If
you are working on two documents side by side, and would like to keep
statistical work for the two separately, you can run them in two
separate sessions.

If you want to have one R session for a particular org file, you can
specify a named session (in this case, with the name \`my-r-session\`)
for the whole file in the preamble as follows:

```
#+property: :session my-r-session
```

You could also use \`:session my-r-session\` as a header argument for a
particular code block to evaluate that code block in
\`my-r-session\`. This means that you could also evaluate different code
blocks from the the same document in separate but persistent R
sessions. Say, if your paper draws upon two different sets of data and
analyses, you could process them in two separate R sessions (with
different working directories, etc).

<a id="orgb5ef6a0"></a>

# Citations and Bibliographies using Org-mode

Starting Version 9.5, Org-mode has introduced a new system for
embedding citations and creating bibliographies in org-mode
documents. You must upgrade your org-mode to Version >9.5 to be able
to use this new citation syntax.

Users are advised to read the discussion on citation handling in the
[org-mode manual](https://orgmode.org/manual/Citation-handling.html#Citation-handling) and a very useful [summary article](https://blog.tecosaur.com/tmio/2021-07-31-citations.html#fn.3) by Timothy.

If you are using my emacs configuration, you should have the latest
version of org-mode. My configuration of functions related to
citations and bibliography resides in `_configs/use-oc.el`. It is
loaded if sym-linked to `_activated/use-uc.el`.

The tasks related to citations and bibliography can be divided into
two parts: (i) maintaining a bibliographic database and (ii) inserting
citations in org-mode documents and processing them to create
formatted citations and bibliography at the time of export.

The new syntax of citations divides the process of inserting and
processing citations in org-mode into four components: insert, follow,
activate and export. The core functionality is designed to allow
development and use of different tools for each of these. John Kitchin
and Bruce D&rsquo;Arcus have built useful extensions on top of the core
functionality.

<a id="org16f333a"></a>

## Building and maintaining your bibliographic database

The citation processors available for org-mode allow the use of
bibliographic databases maintained in bibtex, biblatex or
citeproc/json formats.

I use a biblatex database because biblatex provide tools that are
highly customisable, to format bibliographies in any
style. Citeproc/json has become increasingly popular in recent years
because this is the native format used by pandoc and other
applications in the markdown ecosystem.

We shall use a master bibliographic database to contain bibliographic
records for the literature that we cite. The database, in Biblatex or
BibTex format, is stored in a text file with .bib extension.

In a BibTeX/BibLaTeX database, each bibliographic record is given a
unique key, which is used to cite it. Each record is classified as one
among various categories of publications (journal article, book,
chapter, etc.), and for the given publication type, the record
specifies values for various fields (author, title, volume, publisher,
etc). BibLaTeX extends the BibTeX specification to cover a wider
variety of publication types and fields. Given that, it is more
versatile than BibTeX.

Bibliographic information in BibTeX/BibLaTeX format is available from
many online sources, including journal/publisher websites, and Google
Scholar. Many applications/tools allow you to search download
bibliographic records from these sources directly. Please note that
you would often need to clean/correct the downloaded entries. And, when
the bibliographic information in BibTeX/BibLaTeX format is not available
from any existing database, you may have to enter the information
yourself.

To start with, you may wish to use a GUI application like JabRef
(cross-platform, <http://jabref.sourceforge.net/>) or BibDesk (OS-X
only, <http://bibdesk.sourceforge.net/>) to build and maintain your
database. In my experience, most of these tend to add additional junk
to stamp the entries that a particular application has been used to
create the database.

It is much cleaner, and efficient, to use excellent tools available
for maintaining the database in emacs itself. Ebib gives a nice
interface to manually enter the records. Bibretrieve and org-ref give
you commands that can download BibTeX records directly from online
databases.

Eventually, you should use bibretrieve from within Emacs to
add entries to your database. org-ref.el provided by John Kitchin
(<https://github.com/jkitchin/jmax>) also has some useful functions.

As a sample, my own bibliographic database is available from
<https://github.com/indianstatistics/bibliobase/blob/master/bibliobase.bib>.

<a id="orgf49c39b"></a>

## Using biblatex with Org (under revision)

### Setup

Using biblatex with Org requires some customisation of variables. These
are done in `_config/use-oc.el` and `_config/use-org-contrib.el` files. Both the files are sym-linked to files with the same names in `_activated` directory).

Please note the configuration of ivy-bibtex in `_config/use-oc.el`. You may want to modify the values of variables bibtex-completion-bibliography (default bibliographic database), bibtex-completion-notes-path (directory where you may optionally keep your notes for each source), bibtex-completion-library-path (the directory where you optionally keep pdf files for each source) and bibtex-completion-pdf-open-function (the application that should be used to open the pdfs).

```
(use-package ivy-bibtex
  :ensure t
  :init
  (setq bibtex-completion-bibliography '("~/bibliobase/bibliobase.bib")
        bibtex-completion-notes-path "~/bibliobase/notes/"
        bibtex-completion-notes-template-multiple-files "#+TITLE: Notes on: ${author-or-editor} (${year}): ${title}\n\nSee [cite/t:@${=key=}]\n"
        bibtex-completion-library-path '("~/pdfbibliobase/")
        bibtex-completion-additional-search-fields '(keywords)
        bibtex-completion-display-formats
        '((article       . "${=has-pdf=:1}${=has-note=:1} ${author:36} ${year:4} ${title:*} ${journal:40}")
          (inbook        . "${=has-pdf=:1}${=has-note=:1} ${author:36} ${year:4} ${title:*} Chapter ${chapter:32}")
          (book          . "${=has-pdf=:1}${=has-note=:1} ${author:36} ${year:4} ${title:*}")
          (incollection  . "${=has-pdf=:1}${=has-note=:1} ${author:36} ${year:4} ${title:*} ${booktitle:40}")
          (inproceedings . "${=has-pdf=:1}${=has-note=:1} ${author:36} ${year:4} ${title:*} ${booktitle:40}")
          (t             . "${=has-pdf=:1}${=has-note=:1} ${author:36} ${year:4} ${title:*}")))
  (setq bibtex-completion-pdf-open-function
        (lambda (fpath)
          (call-process "evince" nil 0 nil fpath)))
  )
```

The operative part in the `_config/use-org-contrib.el` file is the following:

```
(setq org-latex-pdf-process
      '("xelatex -interaction nonstopmode -output-directory %o %f"
        "biber %b"
        "xelatex -interaction nonstopmode -output-directory %o %f"
        "xelatex -interaction nonstopmode -output-directory %o %f"))
```

Every time you export the document to pdf via latex, it runs xelatex,
then runs Biber and then runs xelatex twice again. This is necessary
to get the citations in the pdf file.

### Document configuration

Three sets of things have to be specified for each document.

First, the following line in the header of the org file calls biblatex. You may want to modify the options used here.

```
#+LATEX_HEADER: \usepackage[hyperref=true,maxcitenames=3,doi=false,url=true,
backend=biber,natbib=true, maxbibnames=99,uniquename=false,uniquelist=false,
indexing=cite,sorting=nyt,mergedate=compact,innamebeforetitle=true,articlein=false]{biblatex}
```

Second, the following line in the header of the org file specifies the biblatex citation and bibliographic styles to be used.

```
#+cite_export: biblatex authoryear/authoryear-comp
```

Third, the following line specifies the bibliographic database to be used for this document.

```
#+bibliography: bibliobase/bibliobase.bib
```

Where I have the choice, I also customise the bibliography in the way
I like. These customisations are in the file [vikas-bibstyle.org](https://github.com/indianstatistics/bibliobase/blob/master/vikas-bibstyle.org) in
[indianstatistics/bibliobase](https://github.com/indianstatistics/bibliobase.git) repository. This file can be included in
the document as follows:

```
#+INCLUDE: PATH-TO-FILE/vikas-bibstyle.org
```

### Inserting citations

Let us first understand the general syntax of citations in org. To
insert a citation at any point in the document, you need to use the
following syntax in square brackets.

```
[cite/style/variant: common_prefix; prefix@key1 pageno suffix; prefix@key2 pageno suffix; common_suffix]
```

Let us understand this syntax. There are four parts in the syntax.

1. cite/style/variant: :: First, you specify the citation style. Table [5](#orgd67c5f5) lists citations styles and variants available with the biblatex citation processor in org. You can use abbreviations provided in the parentheses rather than the full notation for the citation styles and variants. This part ends in a colon.
  
2. common<sub>prefix</sub>: :: You may optionally use some arbitrary text as prefix. This would be `see` in a citation that you want to export as `(see Seema, 2010 and Nancy, 2011 as examples of this)`. This part ends in a semicolon.
  
3. citations: :: Then you specify the sources that you want to cite. Each source that you want to cite at this point is separated by a semicolon. The source is cited using the citation key from your bibliographic database. So, if the citation key for `Seema, 2010` in the database is `seema2010`, you would cite it as `@seema2010`. The citation key may be preceded by a source-specific prefix, and followed by a location identifier such as page number and citation-specific suffix. Prefix, pageno and suffix are optional.
  
4. Finally, you may optionally use some arbitrary text as a common suffix. This would be `as examples of this` in the citation to be exported `(see Seema, 2010 and Nancy, 2011 as examples of this)`.
  

<table id="orgd67c5f5" border="2" cellspacing="0" cellpadding="6" rules="groups" frame="hsides">
<caption class="t-above"><span class="table-number">Table 5:</span> Important citation styles and variants provided by the biblatex processor in org</caption>

<colgroup>
<col  class="org-left" />

<col  class="org-left" />

<col  class="org-left" />

<col  class="org-left" />
</colgroup>
<thead>
<tr>
<th scope="col" class="org-left">Style (abbreviation)</th>
<th scope="col" class="org-left">Variant (abbreviation)</th>
<th scope="col" class="org-left">Biblatex command</th>
<th scope="col" class="org-left">Example output</th>
</tr>
</thead>
<tbody>
<tr>
<td class="org-left">author(a)</td>
<td class="org-left">caps(c)</td>
<td class="org-left">Citeauthor*</td>
<td class="org-left">Seema et al.</td>
</tr>

<tr>
<td class="org-left">author(a)</td>
<td class="org-left">full(f)</td>
<td class="org-left">citeauthor</td>
<td class="org-left">Seema, K and David, P</td>
</tr>

<tr>
<td class="org-left">author(a)</td>
<td class="org-left">caps-full(cf)</td>
<td class="org-left">Citeauthor</td>
<td class="org-left">Seema, K and David, P</td>
</tr>

<tr>
<td class="org-left">author(a)</td>
<td class="org-left">nil</td>
<td class="org-left">citeauthor*</td>
<td class="org-left">Seema et al.</td>
</tr>

<tr>
<td class="org-left">locators(l)</td>
<td class="org-left">bare(b)</td>
<td class="org-left">notecite</td>
<td class="org-left">p. 56</td>
</tr>

<tr>
<td class="org-left">locators(l)</td>
<td class="org-left">caps(c)</td>
<td class="org-left">Pnotecite</td>
<td class="org-left">(P. 56)</td>
</tr>

<tr>
<td class="org-left">locators(l)</td>
<td class="org-left">bare-caps(bc)</td>
<td class="org-left">Notecite</td>
<td class="org-left">P. 56</td>
</tr>

<tr>
<td class="org-left">locators(l)</td>
<td class="org-left">nil</td>
<td class="org-left">pnotecite</td>
<td class="org-left">(p. 56)</td>
</tr>

<tr>
<td class="org-left">noauthor(na)</td>
<td class="org-left">bare(b)</td>
<td class="org-left">cite*</td>
<td class="org-left">2010</td>
</tr>

<tr>
<td class="org-left">noauthor(na)</td>
<td class="org-left">nil</td>
<td class="org-left">autocite*</td>
<td class="org-left">(2010)</td>
</tr>

<tr>
<td class="org-left">nocite(n)</td>
<td class="org-left">nil</td>
<td class="org-left">nocite</td>
<td class="org-left">No citation but included in bibliography</td>
</tr>

<tr>
<td class="org-left">text(t)</td>
<td class="org-left">caps(c)</td>
<td class="org-left">Textcite</td>
<td class="org-left">Seema and David (2010)</td>
</tr>

<tr>
<td class="org-left">text(t)</td>
<td class="org-left">nil</td>
<td class="org-left">textcite</td>
<td class="org-left">Seema and David (2010)</td>
</tr>

<tr>
<td class="org-left">nil</td>
<td class="org-left">bare(b)</td>
<td class="org-left">cite</td>
<td class="org-left">(Seema and David, 2010)</td>
</tr>

<tr>
<td class="org-left">nil</td>
<td class="org-left">caps(c)</td>
<td class="org-left">Autocite</td>
<td class="org-left">(Seema and David, 2010)</td>
</tr>

<tr>
<td class="org-left">nil</td>
<td class="org-left">bare-caps(bc)</td>
<td class="org-left">Cite</td>
<td class="org-left">Seema and David, 2010</td>
</tr>

<tr>
<td class="org-left">nil</td>
<td class="org-left">nil</td>
<td class="org-left">autocite</td>
<td class="org-left">(Seema and David, 2010)</td>
</tr>
</tbody>
</table>

Insert processors provide convenient shortcuts for inserting the
citations and this does not need to be done manually. I use the insert
processor from the [org-ref-cite](https://github.com/jkitchin/org-ref-cite) repository created by John
Kitchin. Although this insert processor is created for use with
bibtex/natbib, it works reasonably well for biblatex as well. `C-c \`
is bound to org-cite-insert. It would open your database in a
convenient form using Ivy completion framework, where you can type
keywords to select the source you want to cite, and press enter to
insert it. Please look at [org-ref-cite](https://github.com/jkitchin/org-ref-cite) for additional details on how
to modify citation styles, insert multiple citations, and insert
prefixes and suffixes.

[BROKEN LINK: org-ref-cite] also provides other useful facilities. If you click on
the citation, a menu opens which gives you several options including
to go to the entry in the biblatex database (in case you want to edit
it), to look at your notes about the source or to open the pdf file of
the source document. org-ref-cite also colours the citations
differently including colouring them differently (red) if the citation
key does not match with any valid entry in the database. Please note
that since the fontification is designed for bibtex/natbib, it may
give some false alerts for a biblatex database.

### Creating a bibliography

To insert the bibliography, add the following line where you want to
insert the bibliography (usually, at the end of your paper, but before
the Footnotes)

```
#+print_bibliography:
```

<a id="orgb999052"></a>

# Producing a formatted LaTeX, pdf, odt, docx or html file

From Org, we can get a well-formatted document as a LaTeX, PDF, odt, docx
or html file. To produce a formatted output, we shall use the built-in
exporters provided with Org, and for some file types, use Pandoc for
further conversion.

Built-in exporters can be called in Org using `C-c C-e` or `M-x
org-export-dispatch`.

<a id="tableformatting"></a>

## Formatting tables for LaTeX/PDF export

There are many LaTeX packages that can be used to format tables. They
provide different options for formatting tables, and using them
involves different degrees of complexity. We consider four different
libraries for creating tables.

The choice of LaTeX environment and other associated options are
specified in orgmode using lines starting with [`#+attr_latex`](http://orgmode.org/manual/LaTeX-specific-attributes.html). These
lines generate required LaTeX commands on export. We will also use org
[special blocks](http://orgmode.org/manual/Special-blocks.html) for advanced table layouts. `#+attr_latex` lines affect
only the object (table, image or a special block like begin<sub>table</sub> or
begin<sub>tablenotes</sub>) that follows immediately after. These must,
therefore, be placed immediately before the object they are supposed
to be applied on. Note that there should not be even a blank line
between `#+attr_latex` lines and the object.

Some key attributes that need to be specified using `#+attr_latex`
lines are as follows:

- **`:environment:`:** `:environment` specifies the LaTeX package to be
  used for a particular table. This is the simplest of all but allows
  limited possibilities for formatting columns. We will use tabulary,
  tabularx and tabularray packages for most of our tables. Other
  packages that you might want to look at are tabu, longtable and
  sidewaystable. However, the packages used in this guide will suffice
  for most needs.
  
- **`:align`:** `:align` specifies how to render each columns by using
  one letter (l,L,r,R,c,C or J) for each column. Each column type is
  represented by a character. For example, in most packages, c stands
  for center aligned columns, l for left aligned columns and r for
  right aligned columns. The number of letters should exactly match
  the number of columns in your table. For example, for a four column
  table, you would specify something like `:align lccr`, which means
  that the first column is left aligned, next two columns are center
  aligned and the last column is right aligned. A `|` anywhere in this
  string implies a vertical line. So in a table formatted with `:align
   l|ccr`, there would be a vertical line after the first column. When
  you have many columns, you can use expressions like `*{3}{c}` in
  place of `ccc`. This says, use column `c` three times. Normally,
  tables are slightly indented on the left and the right. To remove
  this indentation, you can add `@{}` before and after the alignment
  expression (for example, `:align @{}l*{3}{c}r@{}`).
  
- **`:width`:** `:width` is used to specify the width of the table that
  the table can take [it may be specified as `\textwidth`, implying
  full text width, as `0.8\textwidth`, implying a fraction (0.8 here)
  of the text width, in centimeters (like, 10cm) or in inches (like,
  5in)]. Note that, different environments use the information on
  width differently. In particular, if the contents of your table
  columns do not require the width specified by you, some packages
  would only use the minimum width required while others would widen
  the table to the total width specified by you. The total width is
  also divided between different columns differently by various
  packages.
  
- **`:center`:** This option followed by `t` or `nil` would center align
  or left align the table horizontally.
  
- **`:float`:** This option specifies whether this object (table, figure
  or any other special block) is a floating object which can be
  optimally placed by LaTeX. In most cases, you would want these
  objects to float so that LaTeX can optimally place them.
  
- **`:booktabs`:** This option followed by `t` gives us nice horizontal
  lines. In most cases, when using `booktabs`, one should not use
  vertical lines in the table (that is, `:align` should not have any
  `|`). Booktabs automatically inserts a top line (`\toprule` in LaTeX),
  a middle line just below the header row (`\midrule`) and a line at the
  bottom of the table (`\bottomrule`).
  

### Multicolumn cells and horizontal rules

One limitation of Org is lack of support for merging of cells in a
Table. However, Eric Schulte has created useful functions that can be
used to overcome this limitation while exporting to LaTeX or HTML. I
have extended those functions to also provide facilities for creating
horizontal lines (`\midrule` and `\cmidrule`). These functions
(org-export-midrule-filter-latex and org-export-cmidrule-filter-latex)
are already included in my emacs configuration in
(`_configs/use-org-contrib.el`).

The method of creating multicolumn (and multirow) cells in `tabularay`
is different from other tabular environments and will be discussed
separately when I discuss `tabularray` environment. For all other
`tabular` environments discussed here, the content of any cell can be
preceded by a string such as <3colc> to make those contents span and
centred across 3 columns to the right.

In addition, <mid> placed in a cell draws a `\midrule` at that
position across the entire width of the table. Other cells in that row
will be ignored.

A string such as <2cid4> can be used to say that a horizontal line be
drawn to span second and fourth columns.

The table below illustrates the use of <2colc> to write text that
spans two columns, <mid> to draw a horizontal line across the width of
the table, and <2cid3> to draw a line spanning second and third columns.

A particular advantage of these functions is that they can be inserted
in a table by the code block that is generating the tables.

```
#+name: tabulary-yield-out
#+caption: Table formatted using tabulary package
| State          | <2colc>Two Column Text |          | Variable4 |
| <2cid3>        |                        |          |           |
|                |                  Yield |   Income |           |
| <mid>          |                        |          |           |
| Madhya Pradesh |                    669 | 13121.25 |       123 |
| Haryana        |                    300 |  2532.30 |        22 |
| Punjab         |                    260 | 35232.45 |       324 |
```

### Formatting tables using tabulary package

Package `tabulary` extends the `tabular` environment by providing
three additional column types. It is relatively easy to integrate this
package into orgmode.

Table [6](#orgf42c5e8) shows different types of columns available
in `tabulary` package.

<table id="orgf42c5e8" border="2" cellspacing="0" cellpadding="6" rules="groups" frame="hsides">
<caption class="t-above"><span class="table-number">Table 6:</span> Types of columns in LaTeX tabulary package</caption>

<colgroup>
<col  class="org-left" />

<col  class="org-left" />
</colgroup>
<thead>
<tr>
<th scope="col" class="org-left">Type</th>
<th scope="col" class="org-left">Description</th>
</tr>
</thead>
<tbody>
<tr>
<td class="org-left">l</td>
<td class="org-left">Left aligned, no wrapping</td>
</tr>

<tr>
<td class="org-left">L</td>
<td class="org-left">Left aligned with wrapping</td>
</tr>

<tr>
<td class="org-left">r</td>
<td class="org-left">Right aligned, no wrapping</td>
</tr>

<tr>
<td class="org-left">R</td>
<td class="org-left">Right aligned with wrapping</td>
</tr>

<tr>
<td class="org-left">c</td>
<td class="org-left">Centre aligned, no wrapping</td>
</tr>

<tr>
<td class="org-left">C</td>
<td class="org-left">Centre aligned with wrapping</td>
</tr>

<tr>
<td class="org-left">J</td>
<td class="org-left">Justified and wrapped</td>
</tr>
</tbody>
</table>

Load tabulary and booktabs using the following line in the preamble.

`#+LATEX_HEADER: \usepackage{tabulary,booktabs}`

Following `#+attr_latex` lines illustrate the way of declaring that
the table should be constructed using `tabulary` and passing various
options to it:

```
#+attr_latex: :environment tabulary :width 0.8\textwidth :align @{}L|llR@{}
#+attr_latex: :center :booktabs t
```

The following code uses tabulary to create a table. The output of the code is shown in Table [BROKEN LINK: tabulary-yield-out].

```
#+name: tabulary-yield-out
#+caption: Table formatted using tabulary package
#+ATTR_LATEX: :environment tabulary :width \textwidth :align @{}lRR@{}
#+ATTR_LATEX: :center t :booktabs t :float t
   | State          | Average yield | Average income |
   |----------------+---------------+----------------|
   | Madhya Pradesh |           669 |       13121.25 |
   | Haryana        |           300 |        2532.30 |
   | Punjab         |           260 |       35232.45 |
```

### Using tabularx and siunitx packages

Package tabularx provides a flexible tabular environment. In addition,
siunitx provides an additional column type S which can be used to
align numbers currently on the decimal. Packages tabularx, siunitx and
booktabs are included in the Memoir class, and do not need to be
called if you use vmemoir or varticle LaTeX classes defined in my
emacs configuration. If you are using article (may be the default) or
any other class, you may need to add the following line in the
preamble of your org file.

`#+LATEX_HEADER: \usepackage{tabularx,booktabs}`

Following lines need to be added to the header of the org file to set
the alignment of numeric columns correctly (using siunitx LaTeX
package) and to properly align headings of table columns.

```
#+LATEX_HEADER: \usepackage[add-decimal-zero = true,add-integer-zero = true,
                            round-integer-to-decimal,round-mode = places,
                            round-precision=1]{siunitx}
#+LATEX_HEADER: \newcolumntype{C}{>{\centering\arraybackslash}X}
#+MACRO: M @@latex:\multicolumn{1}{C}{$1}@@
```

With this, you are set to create nicely formatted tables in LaTeX/PDF files.

The following code uses tabularx and siunitx to create a table. The
output of the code is shown in Table [7](#org97f6a70).

In the `:align` specification for `tabularx` tables (with `siunitx`),
you can use following types of columns:

- `l` specifies a left aligned column with no wrapping.
- `r` specifies a right aligned column with no wrapping.
- `X` specifies a left aligned column with wrapping.
- `S` specifies a numeric column aligned to the decimal point.
  - Use option table-format=n.m to specify the maximum n digits and m
    decimals allowed in the column. Numbers will be rounded off to m
    decimals.
  - Use option round-mode=off to turn the rounding-mode off (since we
    turned it on while loading siunitx). This is needed in columns
    that consist only integers.

You are advised to look at the tabularx and siunitx manuals for the
many options that they provide.<sup><a id="fnr.4" class="footref" href="#fn.4" role="doc-backlink">4</a></sup>

For the `S` columns, the column headings should be wrapped in macro M
as shown in the example below to ensure that these are properly
aligned in the center of the columns.

```
#+name: tabularx-yield-out
#+caption: Table formatted using tabularx and siunitx packages
#+ATTR_LATEX: :environment tabularx :width 0.8\textwidth
#+ATTR_LATEX: :align @{}l{S[table-format=3.0, round-mode=off]}{S[table-format=5.2]}@{}
#+ATTR_LATEX: :center t :booktabs t :float t
   | State          | {{{M(Average yield)}}} | {{{M(Average income)}}} |
   |----------------+------------------------+-------------------------+
   | Madhya Pradesh |                    669 |                13121.25 |
   | Haryana        |                    300 |                 2532.30 |
   | Punjab         |                    260 |                35232.45 |
```

<table id="org97f6a70" border="2" cellspacing="0" cellpadding="6" rules="groups" frame="hsides">
<caption class="t-above"><span class="table-number">Table 7:</span> Table formatted using tabularx and siunitx packages</caption>

<colgroup>
<col  class="org-left" />

<col  class="org-right" />

<col  class="org-right" />
</colgroup>
<thead>
<tr>
<th scope="col" class="org-left">State</th>
<th scope="col" class="org-right">&#xa0;</th>
<th scope="col" class="org-right">&#xa0;</th>
</tr>
</thead>
<tbody>
<tr>
<td class="org-left">Madhya Pradesh</td>
<td class="org-right">669</td>
<td class="org-right">13121.25</td>
</tr>

<tr>
<td class="org-left">Haryana</td>
<td class="org-right">300</td>
<td class="org-right">2532.30</td>
</tr>

<tr>
<td class="org-left">Punjab</td>
<td class="org-right">260</td>
<td class="org-right">35232.45</td>
</tr>
</tbody>
</table>

### Using threeparttable to write notes below tables

LaTeX package `threeparttable` is used for including notes below the
table. For using `threeparttable` you need to call the package. In
addition, it is a good idea to include the following special line for
better formatting of notes below the table

```
#+LATEX_HEADER: \renewcommand{\TPTminimum}{\linewidth}
```

The following code produces a table (Table [BROKEN LINK: threeparttable-table-yield])
with notes below.

```
#+name: threeparttable-table-yield
#+caption: Table created using tabularx, siunitx and threeparttable
#+begin_table
#+ATTR_LATEX: :float t :options [hb]
#+begin_threeparttable
#+ATTR_LATEX: :environment tabularx :width 0.8\textwidth
#+ATTR_LATEX: :align @{}l{S[table-format=3.0]}{S[table-format=5.2]}@{}
#+ATTR_LATEX: :booktabs t
| State          | {{{M(Average yield)}}} | {{{M(Average income)}}} |
|----------------+------------------------+-------------------------|
| Madhya Pradesh |                    669 |                13121.25 |
| Haryana^{a}    |                    300 |                 2532.30 |
| Punjab         |                    260 |                35232.45 |
#+attr_latex: :options [flushleft]
#+begin_tablenotes
#+begin_footnotesize
  + Notes: :: \mbox{}
    1. This table is very nice. This note is very long. But the long
       note wraps nicely under the table.
    2. This is the second note. But this is not very wide.
    +  We can use bullets.
    + a :: Or use description lists to refer to footnote markets in
      the table
  + Source: :: https://www.indianstatistics.org
#+end_footnotesize
#+end_tablenotes
#+end_threeparttable
#+end_table
```

Org lists can be used to format the notes properly. `\mbox{}` is used
to leave white space in the given example since the description list
items `+Notes:` and `Source:` do not have any description in the same
line.

I have used footnotesize to render the notes in a slightly smaller
font. Option [flushleft] is used to align the notes to the left.

### Using tabularray package for making tables

`tabularray` is a new package that is very versatile and provides
flexible ways of creating multi-row and multi-column cells, adding
table notes and using colours in the tables. You can also create
multi-page tables with tabularray.

You are strongly advised to go through the [manual](http://mirrors.ctan.org/macros/latex/contrib/tabularray/tabularray.pdf) of tabularray
package to understand various options. The focus here is to illustrate
how to use the package in org documents.

There are three environments provided by `tabularray`:

- **tblr::** Basic tabularray environment with excellent support for merging
  cells across columns and rows.
  
- **talltblr::** In addition to features of `tblr`, this also allows for
  notes below the tables.
  
- **longtblr::** For tables running across multiple pages. All the
  features of `tblr` and `talltblr` are available here.
  

To use `siunitx`, `booktabs` and `diagbox` along with `tabularray`,
you need to add these lines to the preamble of the orgmode file.

```
#+LATEX_HEADER: \usepackage{comment,multirow,tabularray}
#+latex_header: \UseTblrLibrary{booktabs}
#+latex_header: \UseTblrLibrary{siunitx}
#+latex_header: \UseTblrLibrary{diagbox}
```

A few additional hacks are required for using these environments in
orgmode.

1. :align
  
  Both the table width and column specifications will be provided as
  value to the :align keyword. Note that a separate :width keyword
  does not work with `tabularray`.
  
2. Macro to center align column headings for siunitx columns
  
  For siunitx(S) columns, we would like to align column headings
  differently from alignment of cells containing numbers. For this,
  column headings must be wrapped in triple curly braces. However,
  org uses triple curly braces to trigger macros. To deal with this,
  we define a macro (mc below) which just replaces itself with the
  triple curly braces. So, column headings will be wrapped in the mc
  macro as: `{{{mc(colheading)}}}`, and on export, this would create
  `{{{colheading}}}`.
  
  We will add this macro specification to the header.
  
  ```
  #+MACRO: mc @@latex:{{{$1}}}@@
  ```
  
  You can also optionally create macros such as this to conveniently
  add colour/highlights to specific cells:
  
  ```
  #+MACRO: HG @@latex:\SetCell{bg=gray9} $1@@
  ```
  
3. Some of the options &#x2013; such as specification of footnotes &#x2013; are
  provided in `tabularray` as an optional argument in square braces
  when invoking the `tabularray` environments. Juan Manuel Macias
  recently provided an elisp function that allows for the use of an
  `:options` keyword to specify such arguments. This is included in
  `_configs/use-org-contrib.el` in my emacs configuration.
  
4. `\cmidrules`
  
  The LaTeX syntax for `\cmidrules` in `tabularray` is slightly
  different from other tabular environments. Just as we use <2cid3>
  to declare that we want a line going from second to third column,
  we can use <2cd3> with `tabularray` environment. This feature is
  provided by the function
  org-export-tabularray-cmidrule-filter-latex defined in
  `_configs/use-org-contrib.el`.
  
  #+name: tabularrray-yield-out
  #+caption: Table formatted using tabularray and siunitx packages
  #+ATTR_LATEX: :environment talltblr
  #+ATTR_LATEX: :align width=0.8\textwidth,colspec={lS[table-format=3.0,round-precision=0]S[table-format=5.2,round-mode=places,round-precision=true,alignment-mode=marker]S[table-format=3.0,round-precision=0]}
  #+ATTR_LaTeX: :options remark{Notes} = {This note goes below the table},
  #+ATTR_LaTeX: :options remark{Source} = {Second note goes below the first note}
  #+ATTR_LATEX: :center :booktabs t :font \small
  | State | <2colc>Two Column Text | | {{{mc(Variable4)}}} |
  | <2cd3> | | | |
  | | {{{mc(Average yield)}}} | {{{mc(Average income)}}} | |
  | <mid> | | | |
  | Madhya Pradesh | 669 | 13121.25 | 123 |
  | Haryana | 300 | 2532.30 | 22 |
  | Punjab | 260 | 35232.45 | 324 |
  

<table id="orgdf65fe2" border="2" cellspacing="0" cellpadding="6" rules="groups" frame="hsides">
<caption class="t-above"><span class="table-number">Table 8:</span> Table formatted using tabularray and siunitx packages</caption>

<colgroup>
<col  class="org-left" />

<col  class="org-left" />

<col  class="org-left" />

<col  class="org-left" />
</colgroup>
<tbody>
<tr>
<td class="org-left">State</td>
<td class="org-left"><2colc>Two Column Text</td>
<td class="org-left">&#xa0;</td>
<td class="org-left">&#xa0;</td>
</tr>

<tr>
<td class="org-left"><2cd3></td>
<td class="org-left">&#xa0;</td>
<td class="org-left">&#xa0;</td>
<td class="org-left">&#xa0;</td>
</tr>

<tr>
<td class="org-left">&#xa0;</td>
<td class="org-left">&#xa0;</td>
<td class="org-left">&#xa0;</td>
<td class="org-left">&#xa0;</td>
</tr>

<tr>
<td class="org-left"><mid></td>
<td class="org-left">&#xa0;</td>
<td class="org-left">&#xa0;</td>
<td class="org-left">&#xa0;</td>
</tr>

<tr>
<td class="org-left">Madhya Pradesh</td>
<td class="org-left">669</td>
<td class="org-left">13121.25</td>
<td class="org-left">123</td>
</tr>

<tr>
<td class="org-left">Haryana</td>
<td class="org-left">300</td>
<td class="org-left">2532.30</td>
<td class="org-left">22</td>
</tr>

<tr>
<td class="org-left">Punjab</td>
<td class="org-left">260</td>
<td class="org-left">35232.45</td>
<td class="org-left">324</td>
</tr>
</tbody>
</table>

<a id="org13f44e9"></a>

## Creating LaTeX and/or PDF files

The file <default_packages.md> lists a set of LaTeX packages that
I normally use. Please modify this as you please, save it in a
convenient location, and call it using a line of the following kind.

`#+INCLUDE: "path/to/default_packages.org"`

For producing LaTeX and/or PDF files, use `C-c C-e` to call the Org export dispatcher.

- Press l to select LaTeX, and then chose one of the following options.
  - Press l again, if you just want to create a LaTeX file
  - Press p, if you want to create a pdf file. This will first create
    a latex file, then use pdflatex and Biber to create a pdf file.
  - Press o, if you want to create pdf and have it opened in the
    default pdf viewing application.

<a id="org7bba664"></a>

## Creating odt or docx files

There is a built-in odt exporter in Org. While it works well for most
situations, there are two components of the setup proposed here that
it does not support. It does not support biblatex and it does not
support LaTeX-specific solution we have for Notes under Tables and
Images.<sup><a id="fnr.5" class="footref" href="#fn.5" role="doc-backlink">5</a></sup>

Fortunately, [Pandoc](http://johnmacfarlane.net/pandoc/) provides an excellent solution for converting
LaTeX output to odt or docx documents. Pandoc supports all the LaTeX
syntax that Org produces from our files, and you can get a very well
formatted output.

Use `C-c C-e l l` to create a LaTeX file. Then, from the terminal, use
Pandoc as follows to create an odt or a docx file.

```
pandoc --bibliography=biblidatabase.bib --filter pandoc-citeproc \
latexfile.tex -o outputfile.odt

pandoc --bibliography=biblidatabase.bib --filter pandoc-citeproc \
latexfile.tex -o outputfile.docx
```

If you want, you can use &#x2013;template to specify an ott or a .dotx
template file, so that the fonts and other formatting attributes are
to your liking.

<a id="orgebfa910"></a>

## HTML

For html as well, there is a built-in exporter in Org. The built-in
exporter is very good, and the way to go if you are planning to
maintain a website using Org (as I do for
<http://www.indianstatistics.org>).

The built-in exporter can support BibTex citations using ox-BibTex.el,
which is including in Org, and will be loaded if you have installed
[research-toolkit.org](https://github.com/vikasrawal/orgpaper/blob/master/research-toolkit.org). You may need to install BibTex2html separately to make it
work.

However, ox-BibTex.el uses BibTex2html for converting citations and
bibliography to html. BibTex2html provides limited support for
citation and bibliography styles.

If you want full support for bibliography and citation styles, as well
as for other LaTeX components like Table notes explained in this
document, you can use Pandoc for converting LaTeX to html.

<a id="org79ac174"></a>

# Additional tips and tricks

This section points some additional solutions that you may like to
use. Some of these may come handy when you start using Org for
documenting your research.

<a id="orgcfe17b6"></a>

## Defining LaTeX classes

If are using my emacs configuration, you will have access to three
custom classes: vreport, vmemoir and varticle. Of these, vmemoir and
varticle are based on the [Memoir](https://ctan.org/pkg/memoir?lang=en) class, a very versatile LaTeX package
that allows for many customisations.

While vmemoir and varticle classes should work for books and articles,
you will benefit by looking at [~/.emacs.d/<sub>configs</sub>/use-org-contrib.el](https://github.com/ep624/emacs.d.2021/blob/main/_configs/use-org-contrib.el)
and customising them for your need.

LaTeX class is specified in the org header using a line as follows

`#+LATEX_CLASS: vmemoir#`

Additional options can be provided by using the keyword `latex_class_options`:

`#+latex_class_options: [11pt,twoside,openany,strict,extrafontsizes,article]`

Those using my emacs configuration can call the snippet latex-class,
taken from Scimax, using keyword `lc` to insert this line. The snippet
shows LaTeX classes that are defined in your installation of
org. Additional options for any latex class can be specified by
calling another snippet using `lco`.

<a id="orge2187b3"></a>

## Evaluating code during export

By default, Org evaluates source code at the time of exporting. If
your code involves a lot of computation, this can slow down exporting.

You can block evaluation of a source code block at the time of export
by using `:eval never-export` in the header arguments of the
block. Such code blocks will have to be evaluated manually using `C-c
C-c`. To prevent all blocks from being evaluated, set it buffer-wide
using:

```
#+PROPERTY: header-args :eval never-export~
```

If your buffer has this line, the source code is not evaluated at the
time of export, and whatever already exists in `#+RESULTS` block is
exported.

<a id="org647f156"></a>

## Fonts

I like to use the Garamond font. If you do too, add this special line at
the top:

```
#+LaTeX_CLASS_OPTIONS: [garamond]
```

<a id="orge6180b7"></a>

## TODO Page size and margins

In LaTeX, package `geometry` allows you to modify page margins. The
following line in [research-toolkit.org](https://github.com/vikasrawal/orgpaper/blob/master/research-toolkit.org) sets the margins. You can tweak
this to define the margins as you like.

```
("innermargin=1.5in,outermargin=1.25in,vmargin=1.25in" "geometry" t)
```

If you would like to do it for each document separately, remove the
above line, and add the following special line at the top in your
documents.

```
#+LaTeX_HEADER: \usepackage[innermargin=1.5in,outermargin=1.25in,vmargin=3cm]{geometry}
```

### TODO Add instructions for memoir

<a id="org9ff9052"></a>

## Line spacing

Use the following line at the top. Modify the number to whatever suits
you.

```
#+LATEX_HEADER: \linespread{1.3}
```

<a id="org48ec43b"></a>

## Acknowledgements in footnote

When writing a research paper, it is common to put acknowledgements in
a special footnote to names of authors. It is conventional to use \* as
the symbol for this footnote, and to keep this footnote out of the
list of numbered footnotes that the paper may have.

This is achieved as follows.

- As illustrated in the example below, add acknowledgements in the
  special line that specifies authors of the paper.
  
  #+AUTHOR: Vikas Rawal\footnote{Write your acknowledgements here...}
  
- Then, before your first headline, add the following text.
  
  #+BEGIN_LaTeX
  {% begin group
  \renewcommand{\thefootnote}{\fnsymbol{footnote}}% set smybols
  \setcounter{footnote}{0}% set footnote counter back to 0
  }% end group
  #+END_LaTeX
  

<a id="org0c83abb"></a>

## Restricting location of tables and images in LaTeX export

LaTeX has a very sophisticated algorithm for determining the location
of Tables and Images in a document. If, however, you want to add a
restriction that the Tables and Images should not cross section
boundaries, or a particular boundary, this can be done using command
`\FloatBarier` provided by [placeins](http://www.ualberta.ca/afs/ualberta.ca/sunsite/ftp/pub/Mirror/CTAN/help/Catalogue/entries/placeins.html) package in LaTeX.

You can put any number of `\FloatBarrier` commands, each in a line by
itself, in the document. Tables and Images before such a barrier will
be placed before the barrier.

You can use the following special line at the top to restrict all
Tables and Images within their own sections.

```
#+LATEX_HEADER: \usepackage[section]{placeins}
```

An extension to placeins package, [extraplaceins](http://lexfridman.com/blogs/research/2011/03/06/prevent-figures-from-floating-outside-sections-in-latex/) can be used if you
want to restrict the Tables and Images within subsections.<sup><a id="fnr.6" class="footref" href="#fn.6" role="doc-backlink">6</a></sup>

<a id="org78958dc"></a>

## Customising Biblatex style

I like to use authoryear bibliography style. However, I need some
customisations. The file [vikas-bibstyle.org](https://raw.githubusercontent.com/vikasrawal/orgpaper/master/vikas-bibstyle.org) contains all my
customisations.

Download the file, adjust the path to the file in the line below and
add it to your org file.

`#+INCLUDE: /path-to-the-file/vikas-bibstyle.org`

<a id="org861f72d"></a>

## Index

<a id="orgd6090c5"></a>

# Important Resources

- [Org-mode manual](http://orgmode.org/manual/)
- [Worg](http://orgmode.org/worg/)
- [Org-mode mailing list](http://orgmode.org/community.html)
- [Emacs manual](http://www.gnu.org/software/emacs/manual/emacs.html)
- [R website](http://www.r-project.org)
- [Pandoc](http://johnmacfarlane.net/pandoc/)
- E. Schulte, D. Davison, T. Dye, and C. Dominik. A multi-language
  computing environment for literate programming and reproducible
  research. Journal of Statistical Software, 46(3):1–24, 1 2012.[http://www.jstatsoft.org/v46/i03](http://www.jstatsoft.org/v46/i03)
- [Tutorial: Writing scientific papers for ACPD using emacs org-mode, http://draketo.de/english/emacs/writing-papers-in-org-mode-acpd](http://draketo.de/english/emacs/writing-papers-in-org-mode-acpd)
- [Writing papers Using org-mode, http://nakkaya.com/2010/09/07/writing-papers-using-org-mode](http://nakkaya.com/2010/09/07/writing-papers-using-org-mode/)

# Footnotes

<sup><a id="fn.1" href="#fnr.1">1</a></sup> Depending on the keyboard and the default configuration of the
flavour of emacs you have installed, Meta may instead be mapped to a
different key (for example, Windows key, or Option or Command key in
Apple computers.

<sup><a id="fn.2" href="#fnr.2">2</a></sup> For libraries and functions that you need to call, it is even
better to include them in a .Rprofile file in your working
directory. These libraries and functions would then be called when R
is started, and not each time you evaluate code blocks in your
document.

<sup><a id="fn.3" href="#fnr.3">3</a></sup> Of various image formats, I find that png files are most
versatile. png files support transparency, and are rendered well both
on the web and in print. You can also specify jpeg or pdf files. pdf
files for images work very well if you are only going to produce a pdf
document.

<sup><a id="fn.4" href="#fnr.4">4</a></sup> These are available at <http://mirrors.ctan.org/macros/latex/required/tools/tabularx.pdf> and <http://mirrors.ctan.org/macros/latex/contrib/siunitx/siunitx.pdf>.

<sup><a id="fn.5" href="#fnr.5">5</a></sup> Author of the odt exporter has chosen to develop the exporter
outside Org-mode. He has developed a JabRef exporter to integrate
citations into odt exports, but that is not a part of Org-mode and
needs to be installed separately. In any case, since our toolkit
primarily uses LaTeX, using Pandoc to create odt or docx files from
LaTeX export works better.

<sup><a id="fn.6" href="#fnr.6">6</a></sup> See <http://lexfridman.com/blogs/research/2011/03/06/prevent-figures-from-floating-outside-sections-in-latex/>
