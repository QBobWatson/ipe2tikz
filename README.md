ipe2tikz
========

TikZ exporter ipelet: a plugin for [ipe](http://ipe.otfried.org/) that exports **readable** [TikZ](https://sourceforge.net/projects/pgf/) pictures for use in LaTeX documents.

TikZ is an amazing piece of software.  To sketch a shape, though, you really want a GUI, like ipe.  This ipelet is meant to allow you to sketch something in ipe, then export it into readable TikZ code that you can tweak and perhaps use in a larger picture.  It is also quite good at exporting large and complex ipe files (subject to the [limitations](#limitations) below) into standalone LaTeX documents.  Thankfully, ipe is well-suited to generating readable TikZ code, since they both rely on flexible symbolic style mechanisms to specify most drawing parameters.

## Installation

1. Copy `tikz.isy` into `~/.ipe/styles` on Linux and Macs.  On Windows, I believe you have to use the directory containing all of the built-in stylesheets.
2. Copy `tikz.lua` into `~/.ipe/ipelets` on Linux and Macs, and into `$USERPROFILE\Ipelets` on Windows.
3. Copy `tikzlibraryipe.code.tex` somewhere  LaTeX can find it, e.g. the same directory as the LaTeX file you're trying to compile.

## Usage

After installing the ipelet, select *TikZ Export* form the *Ipelets* menu, or use the shortcut `Ctrl-Shift-T`.  A dialog appears, with the following options:
 * *Export complete document:* if this is checked, the exporter makes a document that can be compiled standalone.  That is, it generates a preamble, `\begin{document}...\end{document}` tags, etc.  If you want to `\include` the output into another LaTeX file, un-check this option.  Be sure to set `\usetikzlibrary{arrows.meta,patterns,ipe}` somewhere.
 * *Export stylesheet:* if this is checked, the exporter makes a TikZ version of your included ipe stylesheets (see below).
 * *Export colors:* if this is checked, the exporter generates `\definecolor` commands for all colors defined in your ipe stylesheets.
 * *Output file:* this is where the output goes.

The first thing you have to do, however, is to choose whether you want to primarily use TikZ's styles or ipe's styles.

### Using TikZ's styles

Use TikZ's styles if you want to make a sketch in ipe, but do most of the work in TikZ.  In this case, you want ipe's styles to reflect TikZ's styles as closely as possible: for instance, the pen widths should be `very thin`, `thin`, `thick`, etc., arrows should include `Computer Modern Rightarrow` and `Stealth`, and so on.  To do so, use `tikz.isy` in place of the `basic.isy` stylesheet, and un-check *Export stylesheet* in the ipelet dialog.  Be sure to include the TikZ libraries `arrows.meta` and `patterns`, and the provided library `ipe`.  The latter defines a style `ipe import`, which should be executed in any `tikzpicture` (or a `scope`) using exported ipe code.

### Using ipe's styles

Use ipe's styles if (a) you are more comfortable with ipe than TikZ, and (b) you want to do most of the drawing in ipe, then tweak it in TikZ afterwards.  To do this, check *Export stylesheet* in the ipelet dialog.  This has the additional effect of adding an `ipe ...` prefix to many style names: for instance, the `heavier` pen becomes `ipe pen heavier` in TikZ.  This means that, if you have an ipe pen width named `thick`, it will be exported to `ipe pen thick`, and all `thick` paths will reference `ipe pen thick`.  In particular, you can't make a path that uses TikZ's native `thick` style when *Export Stylesheets* is checked.
