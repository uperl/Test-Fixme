# Test::Fixme [![Build Status](https://secure.travis-ci.org/plicease/Test-Fixme.png)](http://travis-ci.org/plicease/Test-Fixme)

Check code for FIXMEs.

# SYNOPSIS

    # In a test script like 't/test-fixme.t'
    use Test::Fixme;
    run_tests();
    
    # You can also tailor the behaviour.
    use Test::Fixme;
    run_tests( where    => 'lib',      # where to find files to check
               match    => 'TODO',     # what to check for
               skip_all => $ENV{SKIP}  # should all tests be skipped
    );

# DESCRIPTION

When coding it is common to come up against problems that need to be
addressed but that are not a big deal at the moment. What generally
happens is that the coder adds comments like:

    # FIXME - what about windows that are bigger than the screen?

    # FIXME - add checking of user privileges here.

[Test::Fixme](https://metacpan.org/pod/Test::Fixme) allows you to add a test file that ensures that none of
these get forgotten in the module.

# METHODS

## run\_tests

By default run\_tests will search for 'FIXME' in all the files it can
find in the project. You can change these defaults by using 'where' or
'match' as follows:

    run_tests( where => 'lib', # just check the modules.
               match => 'TODO' # look for things that are not done yet.
    );

- where

    Specifies where to search for files.  This can be a scalar containing a
    single directory name, or it can be a list reference containing multiple
    directory names.

- match

    Expression to search for within the files.  This may be a simple
    string, or a qr//-quoted regular expression.  For example:

        match => qr/[T]ODO|[F]IXME|[B]UG/,

- filename\_match

    Expression to filter file names.  This should be a qr//-quoted regular
    expression.  For example:

        match => qr/\.(:pm|pl)$/,

    would only match .pm and .pl files under your specified directory.

- manifest

    Specifies the name of your MANIFEST file which will be used as the list
    of files to test instead of _where_ or _filename\_match_.

        manifest => 'MANIFEST',

- warn

    Do not fail when a FIXME or other pattern is matched.  Tests that would
    have been failures will still issue a diagnostic that will be viewed
    when you run `prove` without `-v`, `make test` or `./Build test`.

- format

    Specifies format to be used for display of pattern matches.

    - original

        The original and currently default format looks something like this:

            # File: './lib/Test/Fixme.pm'
            #     16      # ABSTRACT: Check code for FIXMEs.
            #     25          $args{match} = 'FIXME' unless defined $args{match} && length $args{match};
            #     28          $args{format} ||= $ENV{TEST_FIXME_FORMAT};
            #     228      # FIXME - what about windows that are bigger than the screen?
            #     230      # FIXME - add checking of user privileges here.
            #     239     By default run_tests will search for 'FIXME' in all the files it can
            #     280     Do not fail when a FIXME or other pattern is matched.  Tests that would
            #     288     If you want to match something other than 'FIXME' then you may find
            #     296      run_tests( skip_all => $ENV{SKIP_TEST_FIXME} );
            #     303     L<Devel::FIXME>

        With the line numbers on the left and the offending text on the right.

    - perl

        The "perl" format is that used by Perl itself to report warnings and errors.

            # Pattern found at ./lib/Test/Fixme.pm line 16:
            #  # ABSTRACT: Check code for FIXMEs.
            # Pattern found at ./lib/Test/Fixme.pm line 25:
            #      $args{match} = 'FIXME' unless defined $args{match} && length $args{match};
            # Pattern found at ./lib/Test/Fixme.pm line 28:
            #      $args{format} ||= $ENV{TEST_FIXME_FORMAT};
            # Pattern found at ./lib/Test/Fixme.pm line 228:
            #   # FIXME - what about windows that are bigger than the screen?
            # Pattern found at ./lib/Test/Fixme.pm line 230:
            #   # FIXME - add checking of user privileges here.
            # Pattern found at ./lib/Test/Fixme.pm line 239:
            #  By default run_tests will search for 'FIXME' in all the files it can
            # Pattern found at ./lib/Test/Fixme.pm line 280:
            #  Do not fail when a FIXME or other pattern is matched.  Tests that would
            # Pattern found at ./lib/Test/Fixme.pm line 288:
            #  If you want to match something other than 'FIXME' then you may find
            # Pattern found at ./lib/Test/Fixme.pm line 296:
            #   run_tests( skip_all => $ENV{SKIP_TEST_FIXME} );
            # Pattern found at ./lib/Test/Fixme.pm line 303:
            #  L<Devel::FIXME>

        For files that contain many offending patterns it may be a bit harder to read for
        humans, but easier to parse for IDEs.

    You may also use the `TEST_FIXME_FORMAT` environment variable to override either
    the default or the value specified in the test file.

# HINTS

If you want to match something other than 'FIXME' then you may find
that the test file itself is being caught. Try doing this:

    run_tests( match => 'TO'.'DO' );

You may also wish to suppress the tests - try this:

    use Test::Fixme;
    run_tests( skip_all => $ENV{SKIP_TEST_FIXME} );

You can only run run\_tests once per file. Please use several test
files if you want to run several different tests.

# CAVEATS

This module is fully supported back to Perl 5.8.1.  It may work on 5.8.0.  
It should work on Perl 5.6.x and I may even test on 5.6.2.  I will accept
patches to maintain compatibility for such older Perls, but you may
need to fix it on 5.6.x / 5.8.0 and send me a patch.

# SEE ALSO

[Devel::FIXME](https://metacpan.org/pod/Devel::FIXME)

# ACKNOWLEDGMENTS

Dave O'Neill added support for 'filename\_match' and also being able to pass a
list of several directories in the 'where' argument. Many thanks.

# AUTHOR

Original author: Edmund von der Burg

Current maintainer: Graham Ollis <plicease@cpan.org>

Contributors:

Dave O'Neill

gregor herrmann <gregoa@debian.org>

# COPYRIGHT AND LICENSE

This software is copyright (c) 2005-2019 by Edmund von der Burg <evdb@ecclestoad.co.uk>, Graham Ollis <plicease@cpan.org>.

This is free software; you can redistribute it and/or modify it under
the same terms as the Perl 5 programming language system itself.
