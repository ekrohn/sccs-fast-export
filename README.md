# sccs-fast-export
sccs-fast-export.pl - SCCS to git converter using git-fast-import

Legal
=====

This software is licensed under the ISC license[1] and was written by Eric
Krohn <krohn-git at ekrohn.com>. A few ideas were borrowed from
rcs-fast-export.rb, though sccs-fast-export is written in Perl.

Usage
=====

Using sccs-fast-export is quite simple for a SCCS[2] file or repository:

  mkdir repo-git # or whatever
  cd repo-git
  git init
  sccs-fast-export <sccs-file|sccs-directory> | git fast-import

As SCCS files record only a username for each delta, a username to author
mapping file can be given to sccs-fast-export. The file is specified using the
--authors-file (or -A) option. The file should contain lines of the
form "username=Author Info". The example authors.map below will
translate "thatuser" to "Tha T. User <thatuser@example.com>".

-- Start of authors.map --
thatuser=Tha T. User <thatuser@example.com>
-- End of authors.map --

Branch Naming
=============

SCCS does not have branch naming per se, but it does have branch deltas.
For each terminal branch delta x.y.z.w, sccs-fast-export marks the branch
x.y.z.

Design
======

sccs-fast-export reads an SCCS file to parse the delta table, runs _sccs_ _get_
to read the delta contents, and outputs each delta as a separate commit.
It does a little bookkeeping to keep track of which deltas are terminal.

Footnotes
=========

[1] http://opensource.org/licenses/ISC
[2] https://en.wikipedia.org/wiki/Source_Code_Control_System
