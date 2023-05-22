# Various Python tools to inspect and modify floppy disk images

## dump

Performs a deep inspection and optional re-write of a Teledisk image.

There are several levels of detail that can be presented. By default, just the image's header data is provided.

![Dump Example](.images/dump.png?raw=true "Example")


However, this may also be tuned to present a mix of header, detail, and summary information.

Detail provides a sector-by-sector dump of sector header and data for the entire image. Summary provides details of how each sector on each track is stored.

The command looks like this:

```
$ ./dump --help
Usage: dump [OPTIONS] FILENAME

Options:
  --writefile TEXT
  --writemode [verbatim|cylinder|side]
  --resequence / --no-resequence
  --header / --no-header
  --detail / --no-detail
  --summary / --no-summary
  --help              Show this message and exit.
```


If `writefile` is specified, then a IMA image is written. By default,
the `writemode` is `verbatim`, meaning that sectors are written in the
same order that they were read. if writemode is set to cylinder, then
then sectors are written in cylinder priority: so cylinder 0 side 0
followed by cylinder 0 side 1, followed by cylinder 1 side 0, and so
on. Conversely, setting `writemode` to `side` will cause all of the
cylinders on side 0 to be written, followed by all of the cylinders
on side 1.

For performance reasons, disks may have a sector 'skip', so that adjacent sectors are placed so many physical secros away from one another.  Teledisk images keep this skip. For more information, see

https://web.archive.org/web/20160527014011/http://www.coco3.com/community/2010/06/disk-skip-factor-explained-please/

Setting the `resequence` flag will reorder the sectors in logical order, eliminating the skip.

## examine

Inspects an input file block-by-block, and renders a block-by-block color coded representation to the screen. This is useful for making a high level comparison of how a file is laid out.

$ ./examine --help                            
Usage: examine [OPTIONS] FILENAME

```
Options:
  --blocksize INTEGER
  --columnwidth INTEGER
  --columns INTEGER
  --help              Show this message and exit.
$ 
```

The optional argument `blocksize` determines how many bytes are used to calculate a particular cell. The optional arguments `columnwidth` and `columns` determine the number of cells per column, and how many columns are presented on each line, respectively.

The default values for these options provide for a block size of 256 bytes, and two columns of 16 cells.

Like this:

![Examine Example](.images/examine.png?raw=true "Example")




