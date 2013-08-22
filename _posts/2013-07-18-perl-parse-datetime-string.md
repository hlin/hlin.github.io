---
layout: post
title: parse datetime string via Time::Piece
date: 2012-09-14 15:00:12
---

[Time::Piece][1] becomes core module since perl v5.9.5

This module replaces the standard localtime and gmtime functions with
implementations that return objects. It does so in a backwards compatible
manner, so that using localtime/gmtime in the way documented in perlfunc
will still return what you expect.

Convert STRING to Time::Piece object using the specified FORMAT:

    Time::Piece->strptime(STRING, FORMAT);

```perl
use warnings;
use strict;
use 5.010;

use Time::Piece;

my $string = '3/20/2013/3:48:26';

my $t = Time::Piece->strptime($string, "%m/%d/%Y/%H:%M:%S");

# seconds since the epoch
say $t->epoch;

# seconds
say $t->sec;

# minutes
say $t->min;

```

[1] http://search.cpan.org/~rjbs/Time-Piece-1.22/Piece.pm
