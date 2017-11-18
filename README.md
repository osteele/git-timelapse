# Git Time-Lapse Tools

Tools for building time-lapse movies of git projects.

Status: Not ready for prime time. Some names are hardwired (contra the documentation); few errors are handled.

## `git-each-build [--dir DIR] [FILENAMES...] -- COMMAND ARGS...`

Run `COMMAND ARGS...` against each commit in the repo whose working directory is `DIR`.
Results are accumulated into `./git-each-build`.

`DIR` defaults to the current directory.

Each commit is checked out to `DIR`.
On completion, `DIR` is restored to whatever commit it was originally set to.

If `FILENAMES` is supplied, only commits that mention a file in FILENAMES are checked out.

`COMMAND` is run with environment variables GIT_EACH_HASH and GIT_EACH_BUILD_DIR.
Its standard output is placed in `$GIT_EACH_BUILD_DIR/stdout.txt`.
It should place additional output files in $GIT_EACH_BUILD_DIR.

Output files are collected into subdirectories `git-each-build/$isodate`, where `$isodate` is the ISO datetime of the commit.

Examples:

`git-each-build -- tree`

`git-each-build -- cloc .`

`git-each-build page/resources.md -- snapshot-jekyll-page resources`

If each build subdirectory contains only a single file (for example, `stdout.txt`),
you can flatten it in bash via:

    mmv 'git-each-build/*/stdout.txt' git-each-build//#1.txt
    find git-each-build -type d -empty -delete

Requirements: Ruby

## `rm-consecutive-dups`

Example: `rm-consecutive-dups git-each-build`

## `make-html-page-images PATTERN HTML_PAGES`

Convert the pages in HTML_PAGES into image files.

Example: `make-html-page-images git-each-build/*/resources/index.html pages/image-%02d.png`

Requirements: Install imgkit from <https://wkhtmltopdf.org/downloads.html>

## `make-html-timelapse-movie`

Requirements: Python, `pip -f requirements.txt`

Afterwards:

```bash
ffmpeg -r 5 -i frames/frame-%03d.tiff -pix_fmt yuv420p -y out.mp4
```

## Related

`git-each-build` is vaguely like [git-filter-branch](https://git-scm.com/docs/git-filter-branch),
except it accumulates outputs instead of re-writing the tree.

The [git-time-machine](https://atom.io/packages/git-time-machine) Atom plugin displays an interactive commit plot for a single file.

[Git Time-Lapse View](https://github.com/JonathanAquino/git-time-lapse-view) is a Java program that lets you scroll through revisions.

## License

MIT
