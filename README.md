*Hey, maybe you are just looking for a side-by-side diff viewer?  Try [cdiff](https://github.com/ymattw/cdiff) instead.*

# Coderev

A toolkit generates side-by-side html pages for code review

## About

Coderev is a toolkit generates static side-by-side html pages for code review.
Typical use case is to generate diff pages for local modification in svn/cvs
workspace and send page link to teammates for code review.

See [ymattw.github.com/coderev/demo/](http://ymattw.github.io/coderev/demo/index.html) for a demo.

This toolkit contains two scripts that can be used separately.

- coderev.sh - generate diff page in svn/cvs workspace
- codediff.py - generate diff page for any two given files/directories

This project was originally hosted at [google code](http://code.google.com/p/coderev/)
and recent moved to github.

## Usage of coderev.sh

Just type `./coderev.sh -h` to see the usage.

```
    Usage:
        coderev.sh [-r revision] [-w width] [-o outdir] [-y] [-d name] \
                [-F comment-file | -m 'comment...'] [file...]

        coderev.sh [-r revision] [-w width] [-o outdir] [-y] [-d name] \
                [-F comment-file | -m 'comment...'] [-p num] < patch-file

        All options are optional.

        -r revision     - Specify a revision number, or symbol (PREV, BASE, HEAD)
                        in svn, see svn books for details.  Default revision is
                        revision of your working copy

        -w width        - Let code review pages wrap in specific column

        -o outdir       - The output dir to save code review pages

        -y              - Force overwrite if outdir alredy exists

        -d name         - Use this name instead of a dynamically timestamp string
                        as coderev directory basename

        -F comment-file - A file to read comments from

        -m 'comment...' - To set inline comments, note '-m' precedes '-F', if
                        neither `-F' nor `-m' is specified, $EDITOR (default
                        is vi) will be invoked to write comments

        file...         - File/dir list you want to diff, default is `.'

        patch-file      - A patch file (usually generated by `diff(1)' or `svn
                        diff') to use to generate coderev

        -p num          - When use a patch file, this option is passed to utility
                        `patch(1)' to strip the smallest prefix containing num
                        leading slashes from each file name found in the patch

    Example 1:

        You are working on the most up-to-date revision and made some local
        modification, now you want to invite others to review, just run

            cd workspace
            coderev.sh -w80

        This generates coderev pages (wrap in column 80) in a temp directory.  Then
        copy the coderev dir to somewhere on a web host and send out the link for
        review.  Read coderevrc.sample for how to make this automated.

    Example 2:

        You are making local modification when someone else committed some changes
        on foo.c and bar directory, you want to see what's different between your
        copy and the up-to-date revision in repository, just run

            cd workspace
            coderev.sh -r HEAD -o ~/public_html/coderev foo.c bar/

        This generate coderev based on diffs between HEAD revision (up-to-date
        version in repository) and locally modified revision, this will retrieve
        diffs from SVN server, output pages saved under your web home, i.e., if
        you correctly configured a web server on your work station you can visit
        http://server/~you/coderev to see the coderev.  (Replace HEAD with a
        revision number this example also works for CVS).

    Example 3:

        Someone invite you to review his code change, unfortunately he sent you raw
        diff generated by `cvs diff' named `foo.patch', you can run

            cd workspace
            cvs up
            coderev.sh -m 'applying patch foo' -o ~/public_html/foo < foo.patch

        Again, you can visit http://server/~you/foo to see his change.  Note you
        may need to use option `-p num' depends on how he generated the patch.

    Example 4:

        You want to see what's different between previous revision and your
        current working copy (modified or not) for foo.c and dir bar/, just run

            cd workspace
            svn diff -r PREV foo.c bar/ | coderev.sh -w80 -F comments

        This read comments from file `comments' and generate coderev in a temp
        directory.  (Replace PREV with a revision number this example also works
        for CVS).
```

## Usage of codediff.py

Just type `./codediff.py -h` to see the usage.

```
    Usage:
        codediff.py [options] OLD NEW
        codediff.py OLD NEW [options]

        Diff two files/directories and produce HTML pages.

    Options:
    -h, --help            show this help message and exit
    -c, --context         generate context diff (default is full diff), only
                            take effect when diffing two files
    -F FILE, --commentfile=FILE
                            specify a file to read comments
    -f FILE, --filelist=FILE
                            specify a file list to read from, filelist can be
                            generated by find -type f, specify - to read from
                            stdin
    -m COMMENTS, --comments=COMMENTS
                            specify inline comments (precedes -F)
    -n NUM, --lines=NUM   specify context line count when generating context
                            diff or unified diff, default is 3
    -o OUTPUT, --output=OUTPUT
                            specify output file or directory name
    -p NUM, --striplevel=NUM
                            for all pathnames in the filelist, delete NUM path
                            name components from the beginning of each path name,
                            it is similar to patch(1) -p
    -t TITLE, --title=TITLE
                            specify title of output index page
    -w WIDTH, --wrap=WIDTH
                            specify column number where lines are broken and
                            wrapped for sdiff, default is no line wrapping
    -y, --yes             do not prompt for overwriting
```
