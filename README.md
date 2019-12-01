# mv_perl

A collection of perl scripts (replacement in files, syncing dirs etc)

Author: Martin Väth (martin at mvath.de)

This project is under the BSD license 2.0 (“3-clause BSD license”).

This is a loose collection of some useful and less useful perl scripts.

For installation just put the content of bin somewhere into your `$PATH`.
Also put the files of the subdirectory zsh into your `$fpath` to obtain
zsh completion support. (If you do not have root access, you can add the
corresponding directory with `fpath+=("...")` before you
call `compdef` from your zsh initialization files).

For installation under Gentoo, you can use the ebuild from the mv overlay
(which is available by layman).

Here is a brief overview of the scripts:

### plrep

(Perl regular expression).
Note that there is a successor project of this script in python:
https://github.com/vaeth/replacer

This script provides among a more user-friendly variant of the well-known
grep (with perl regular expressions) a method to replace regular expression
within the files. Special features include interactive replacement, recursive
directory handling, colorized display (adaptable for various terminal colors
since v2.1), readable output also for mixed text/binary files (conversion to
printable characters, very luxury truncation of lines), and also interactive
replacement/grep of expressions across multiple lines
(experimental since v2.1).

For a complete implementation of multiple line replacement use the
successor project "replacer" mentioned above.

With `plrep --man` you get a detailed help as a manpage (new since v2.1).

This script requires Perl 5.6 or newer.
If you must use Perl 4, see the scripts __prep__ and __rrep__ below which
together provide similar functionality with less convenience.

### patchdirs

This script serves two purposes:

1. It allows to keep directory trees in sync for computers which have no
   direct connection. On the one hand, it allows to (recursively) list files
   together with their checksums (and dates, owners, modes).
   On the other hand, it can use that information to reconstruct all file
   renamings/deletions/copies (and owner, date, mode-changes) based on the
   checksum information. Even circular renamings (like swappings of
   filenames) can be reconstructed.
   Of course, this script requires a checksum utility which provides a
   sufficiently strong checksum (by default, the perl `Digest::MD5` module
   is used, but not mandatory: other programs are also supported).
   The new release finally supports also copies of files,
   changes of directory names, and stores/restores symbolic links.

2. This script is also able to look for double files (where duplicates are
   recognized by equal checksums). Since checksum information can be stored,
   this is particularly useful if frequently new files must be compared
   against a given directory tree. (If you want more reliable information
   than only a checksum comparison, use the __find_double__ perl script of this
   collection).

This script requires Perl 5.8 or newer.
The `String::Escape` and `String::ShellQuote` modules are highly recommended
(although there are fallbacks which work in most cases)

### patchdirs-ls

This is somewhat similar to __patchdirs__ but can only reconstruct from an
`ls --recursive` output exclusively on the base of names.
This is very fast and requires no checksum module/utility.
However, circular renamings can of course not be recognized.
Although this script is rather old and hardly tested, it has been recently
rewritten to a more modern perl style. This script requires Perl 5.6 or newer.
For improved output, the `String::ShellQuote` module is recommended.

### rendirs

The "very standard" ISO9660 CD-Rom format only allows names
of 8+3 characters. If there are some reasons to use this format with
with non-packaged files, it is often convenient to create
additionally a list containing the original filenames (and perhaps
with a small description of the files). This script renames the
files corresponding to such a list.
This script requires perl 5.6 or newer.
For improved output, the `String::ShellQuote` module is recommended.

### commlist

Print lines common to two list of file resp. contained in only one of
these lists. This is similar to the standard unix comm utility, but deals
with unsorted input/output. This script requires perl 5.6 or newer.

### sshconfig

If you frequently ssh/scp/sftp/rsync to computers with a
dynamic IP address, you will find this script very useful.
It modifies/creates a ~/.ssh/config file for you such that you can
conveniently use the "real" hostname in your commands while the correct
IP address is used (enter the IP address only once for running the script).
From a security aspect, this script is what you should use in order to
associate the correct fingerprint to the host, even if its IP address changes.
This helps you to detect "man in the middle" attacks.
This script requires perl 5.6 or newer.

### prep

(Perl regular expression).
Similar to the well-known grep but with perl regular expressions and
possibility to recurse into subdirectories.
This is an old script which should work with perl 4 without any modules.
It is hardly maintained.
If you have a current perl version, it is better to use `plrep`, instead.

### rrep

(Replace regular expression).
As prep above, but in addition, it is possible to replace the
regular expressions in the files (interactively or non-interactively).
Very flexible.
This is an old script which should work with perl 4 without any modules.
It is hardly maintained.
If you have a current perl version, it is better to use `plrep`, instead.

### find_double

Looks for double files (the user may decide whether equal names and/or
equal content count as double). Needs only O(n log(n)) comparisons in the mean.
See also the `patchdirs` script of this collection.
This is an old script which should work with perl 4 without any modules.
It is hardly maintained.

### renreg

Rename and/or number a set of files using regular expressions.
This is an old script which should work with perl 4 without any modules.
It is hardly maintained.

### changecase

A very simple script which allows to change filenames
to upper or lower case (including directory names, recursively).
Very useful in connection with DOS partitions or archives.
This is an old script which should work with perl 4 without any modules.
It is hardly maintained.

### comp-old

Similar to `diff -r -q`, but compares also metadata like attributes, time,
owner, ...
Also calls like `comp-old *.tex DIR` are admissible.
This script requires perl-5.12.
This script is renamed: It was previously called __comp__.
A ground-up rewrite of comp-old is splitted from this project and
maintained as a separate project under https://github.com/vaeth/comp/

### touch.difference

Correct times of files (e.g. by a daylight saving offset).
The script can also be used to "round" the timestamps to a full second in
order to simplify comparison of timestamps on filesystems of different
resolution. However, for the latter you can also use `touchdirs`.
This script requires perl 5.6 or newer.

### touchdirs

Touch directories according to their newest file (recursively).
This script can also be used to "round" file/directory timestamps
to a full second. This script requires perl 5.8 or newer.

For improved output, the `String::ShellQuote module` is recommended.
For improved handling of symlinks, the `File::lchown` module (0.02 or newer)
is strongly recommended.
Note that the `Time::HiRes` module (used by the script if available)
is buggy in some perl implementations; the script takes care of this
by some heuristics. Type `touchdirs --man` for more details.

### sort_shadow

Tool to match the order and line-ends of /etc/{g,}shadow to that of /etc/group
and /etc/passwd, respectively.
