---
layout: post
title: HTML::Mason syntax
date: 2013-07-18 14:55:09
---

### <%init> </%init>

This section contains initialization code that executes as soon as the component is called.
Technically an <%init> block is equivalent to a <%perl> block at the beginning of the component.

Example:

    <%init>
    # Fetch article from database
    my $dbh = DBI::connect ...;
    my $sth = $dbh->prepare("select * from articles where id = ?");
    $sth->execute($article_id);
    my ($headline, $date, $author, $body) = $sth->fetchrow_array;
    # Massage the fields
    $headline = uc($headline);
    my ($year, $month, $day) = split('-', $date);
    $date = "$month/$day";
    </%init>

### <%cleanup> </%cleanup>

This section contains cleanup code that executes just before the component exits.
A <%cleanup> block is equivalent to a <%perl> block at the end of the component.

### <%args> </%args>

Declared named arguments.

Example:

    <%args>
    $a
    @b       # a comment
    %c

    # another comment
    $d => 5
    $e => $d*2
    @f => ('foo', 'baz')
    %g => (joe => 1, bob => 2)
    </%args>

### %

Most useful for conditional and loop structures - if, while, foreach, , etc. - as well as side-effect commands like assignments. To improve readability, always put a space after the '%'. Examples:

    # Conditional code
    % my $ua = $r->header_in('User-Agent');
    % if ($ua =~ /msie/i) {
    Welcome, Internet Explorer users
    ...
    % } elsif ($ua =~ /mozilla/i) {
    Welcome, Netscape users
    ...
    % }

    # Loop code
    <ul>
    % foreach $item (@list) {
    <li><% $item %></li>
    % }
    </ul>

### <% xxx %>

Most useful for printing out variables, as well as more complex expressions. To improve readability, always separate the tag and expression with spaces. Examples:

    Dear <% $name %>: We will come to your house at <% $address %> in the
    fair city of <% $city %> to deliver your $<% $amount %> dollar prize!

    The answer is <% ($y+8) % 2 %>.

    You are <% $age < 18 ? 'not' : '' %> permitted to enter this site.

### <%perl> xxx </%perl>

Useful for Perl blocks of more than a few lines.

    <%perl>
    my @array = qw/1..10/;
    foreach my $v (@array){
        say $v;
    }
    </%perl>

### <& comp_path, [name=>value, ...] &>

To call one component from another.

comp_path:
The component path. With a leading '/', the path is relative to the component root (comp_root). Otherwise, it is relative to the location of the calling component.
comp_path could be a literal string (quotes optional) or a Perl expression that evaluates to a string.

name => value pairs:
Parameters are passed as one or more name => value pairs, e.g. player => 'M. Jordan'.

Examples:

    # relative component paths
    <& topimage &>
    <& tools/searchbox &>

    # absolute component path
    <& /shared/masthead, color=>'salmon' &>

    # this component path MUST have quotes because it contains a comma
    <& "sugar,eggs", mix=>1 &>

    # variable component path
    <& $comp &>

    # variable component and arguments
    <& $comp, %args &>

    # you can use arbitrary expression for component path, but it cannot
    # begin with a letter or number; delimit with () to remedy this
    <& (int(rand(2)) ? 'thiscomp' : 'thatcomp'), id=>123 &>

### <&| comp_path, [name=>value, ...] &> xxx </&>

Components can be used to filter part of the page's content using an extended component syntax.

The filtering component can be called in all the same ways a normal component is called, with arguments and so forth. The only difference between a filtering component and a normal component is that a filtering component is expected to fetch the content by calling $m->content and do something with it.

Here is an example of a component used for localization. Its content is a series of strings in different languages, and it selects the correct one based on a global $lang variable, which could be setup in a site-level autohandler.

    <&| /i18n/itext &>
       <en>Hello, <% $name %> This is a string in English</en>
       <de>Schoene Gruesse, <% $name %>, diese Worte sind auf Deutsch</de>
       <pig>ellohay <% substr($name,2).substr($name,1,1).'ay' %>,
       isthay isay igpay atinlay</pig>
    </&>

Here is the /i18n/itext component:

    <% $text %>

    <%init>
    # this assumes $lang is a global variable which has been set up earlier.
    local $_ = $m->content;
    my ($text) = m{<$lang>(.*?)</$lang>};
    </%init>

