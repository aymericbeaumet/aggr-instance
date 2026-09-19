---
title: Coreutils - rejected feature requests
link: https://www.gnu.org/software/coreutils/rejected_requests.html
source: lobste-rs-top-1w
published: 2026-09-15T09:06:36Z
updated: 2026-09-15T09:06:36Z
first_seen: 2026-09-19T21:30:23.395188495Z
authors:
- gnu.org via ryan-duve
labels:
- programming
- unix
summary: Comments
content: extracted
html: 2026-09-15-coreutils-rejected-feature-requests.html
preview:
  file: 2026-09-15-coreutils-rejected-feature-requests.preview-b33a300c09f7.webp
  width: 145
  height: 142
  alt: '[A GNU head]'
  color: '#6a6a6a'
images:
- source: https://www.gnu.org/graphics/heckert_gnu.transp.small.png
  original:
    file: 2026-09-15-coreutils-rejected-feature-requests.image-eafc08109827.png
    width: 145
    height: 142
  color: '#fcfcfc'
---

* * *

Some of the hardest work on coreutils is knowing what to reject and providing appropriate justification to the contributors.

The contributions below while all good ideas, were not included for various reasons detailed on the linked mailing list discussions.

[cat](https://www.gnu.org/software/coreutils/rejected_requests.html#cat)    [chmod](https://www.gnu.org/software/coreutils/rejected_requests.html#chmod)    [cp](https://www.gnu.org/software/coreutils/rejected_requests.html#cp)    [cut](https://www.gnu.org/software/coreutils/rejected_requests.html#cut)    [date](https://www.gnu.org/software/coreutils/rejected_requests.html#date)    [dd](https://www.gnu.org/software/coreutils/rejected_requests.html#dd)    [df](https://www.gnu.org/software/coreutils/rejected_requests.html#df)    [du](https://www.gnu.org/software/coreutils/rejected_requests.html#du)    [join](https://www.gnu.org/software/coreutils/rejected_requests.html#join)    [ls](https://www.gnu.org/software/coreutils/rejected_requests.html#ls)    [mv](https://www.gnu.org/software/coreutils/rejected_requests.html#mv)    [rm](https://www.gnu.org/software/coreutils/rejected_requests.html#rm)    [shred](https://www.gnu.org/software/coreutils/rejected_requests.html#shred)    [sort](https://www.gnu.org/software/coreutils/rejected_requests.html#sort)    [stat](https://www.gnu.org/software/coreutils/rejected_requests.html#stat)    [\*sum](https://www.gnu.org/software/coreutils/rejected_requests.html#checksum)    [touch](https://www.gnu.org/software/coreutils/rejected_requests.html#touch)    [uniq](https://www.gnu.org/software/coreutils/rejected_requests.html#uniq)    [wc](https://www.gnu.org/software/coreutils/rejected_requests.html#wc)    [misc](https://www.gnu.org/software/coreutils/rejected_requests.html#misc)    [New commands](https://www.gnu.org/software/coreutils/rejected_requests.html#new)

### cat

- [cat --timestamp](https://lists.gnu.org/archive/html/coreutils/2011-03/msg00002.html). awk or perl is good enough for this
- [cat -n alternate formats](https://lists.gnu.org/archive/html/coreutils/2012-01/msg00002.html). Manipulation with existing tools supports this better
- [cat --show-ends](https://lists.gnu.org/archive/html/coreutils/2013-02/msg00055.html) to highlight trailing whitespace. grep --color was deemed better/sufficient
- [cat --header](https://lists.gnu.org/archive/html/coreutils/2013-05/msg00015.html) to output filenames for each file. tail -n+1 does this already
- [cat -S](https://lists.gnu.org/archive/html/coreutils/2013-09/msg00005.html) to squeeze lines just containing blank chars. Existing tools like \`sed 's/^ \*$//' | cat -s\` were thought sufficient
- [cat -d,--direct](https://lists.gnu.org/archive/html/bug-coreutils/2013-10/msg00011.html) to use direct I/O. dd has the 'nocache' or 'direct' options and there are general [nocache](https://github.com/Feh/nocache) wrappers

### chmod

- [chmod maintains ctime](https://lists.gnu.org/archive/html/bug-coreutils/2010-01/msg00303.html) when permissions unchanged. The proposed patch was deemed [inefficient](https://lists.gnu.org/archive/html/bug-coreutils/2010-02/msg00208.html)
- [chmod -d](https://lists.gnu.org/archive/html/bug-coreutils/2010-02/msg00201.html) to set perms on just directories. The 'X' mode, or \`find\` with \`chmod\` was deemed sufficient
- [chmod +S](https://lists.gnu.org/archive/html/bug-coreutils/2010-04/msg00008.html) to set setgid on just directories. \`find\` in combination with \`chmod\` was deemed sufficient
- [chmod -D](https://lists.gnu.org/archive/html/bug-coreutils/2011-05/msg00047.html) to set perms on just directories. The 'X' mode, or \`find\` with \`chmod\` was deemed sufficient
- [chmod --parents](https://debbugs.gnu.org/cgi/bugreport.cgi?bug=8736). Doing this with a simple script or with find was deemed sufficient
- [chmod --umask](https://lists.gnu.org/archive/html/bug-coreutils/2012-03/msg00080.html). The existing chmod options were deemed sufficient
- [chmod b10111](https://lists.gnu.org/archive/html/coreutils/2012-11/msg00076.html). Binary conversion can be done easily in bash or ksh
- [disallow chmod to create world writable files](https://lists.gnu.org/archive/html/coreutils/2013-06/msg00075.html). This couldn't be general, and even so could be easily bypassed

### cp

- [cp,mv --to](https://lists.gnu.org/archive/html/bug-coreutils/2009-08/msg00298.html). Though better than --target it wasn't warranted to have two ways to do it
- [unicode symbols for cp,mv --verbose](https://lists.gnu.org/archive/html/bug-coreutils/2009-09/msg00096.html). Rejected for same reasons as [UTF-8 arrows in ls output](https://lists.gnu.org/archive/html/bug-coreutils/2009-08/msg00023.html)
- [distinguished symbols for cp,mv --verbose](https://lists.gnu.org/archive/html/bug-coreutils/2009-12/msg00109.html). It was deemed there was enough info from the context
- [cp,mv --progress](https://lists.gnu.org/archive/html/coreutils/2013-05/msg00020.html). Existing tools cater for this already
- [cp --reflink-range=src\_offset,src\_length,dst\_offset](https://lists.gnu.org/archive/html/coreutils/2011-08/msg00006.html). The was not deemed warranted
- [cp --quiet](https://lists.gnu.org/archive/html/coreutils/2012-02/msg00090.html). Suppressing ENOENT errors can be done by filtering existing files first
- [cp --resume](https://lists.gnu.org/archive/html/coreutils/2012-04/msg00079.html). Use rsync
- [cp --parallel](https://lists.gnu.org/archive/html/coreutils/2012-04/msg00079.html). While this helps copy speed in some situations, the fix is probably best handled at a lower level
- cp --preserve=all should [copy ext2 extended attributes](https://lists.gnu.org/archive/html/coreutils/2013-08/msg00019.html). Would need file system agnostic interface (like copyfile())
- [cp,mv --bwlimit](https://lists.gnu.org/archive/html/coreutils/2014-02/msg00019.html) to throttle data transfer rates. This is better suited to higher level tools, and is available in rsync

### cut

- [cut -d 'string'](https://lists.gnu.org/archive/html/bug-coreutils/2010-01/msg00020.html). sed 's/string/\\x00/g' | cut -d '' was deemed sufficient
- [cut --output-delimiter](https://www.gnu.org/software/coreutils/rejected_requests.html) short option. One can already do cut --ou
- [cut --csv](https://lists.gnu.org/archive/html/bug-coreutils/2010-05/msg00122.html). A [separate](http://freshmeat.sourceforge.net/projects/csvutils) util was deemed best for this complicated task
- [cut -C](https://lists.gnu.org/archive/html/coreutils/2012-04/msg00027.html). There was no need for this alias for --complement (--co)
- [cut -f2,1](https://lists.gnu.org/archive/html/bug-coreutils/2012-12/msg00115.html) to reorder fields. [Using awk or join](https://git.sv.gnu.org/gitweb/?p=coreutils.git;a=commitdiff;h=38cdb01) is deemed sufficient
- [cut --separator](https://bugs.gnu.org/14224) to specify the “line” delimiter. Pre/Post-processing with tr was deemed sufficient

### date

- [date +%f](https://lists.gnu.org/archive/html/coreutils/2011-08/msg00008.html) to flush output. \`stdbuf -oL date ...\` was deemed sufficient
- [date should parse 'DAY MONTH, YEAR'](https://bugs.gnu.org/14613) format. The format was deemed [erroneous](https://www.grammar.com/dates-day-month-year/)
- [date +%J](https://lists.gnu.org/archive/html/coreutils/2013-10/msg00019.html) to support astronomical julian date. This was not thought common enough to support
- [date -v](https://lists.gnu.org/archive/html/coreutils/2013-10/msg00041.html) to provide BSD syntax relative date adjustments. The existing GNU relative date syntax was thought sufficient

### dd

- [I/O throughput limitation for dd](https://lists.gnu.org/archive/html/bug-coreutils/2009-12/msg00199.html). pv and rsync et. al. were deemed better for this
- [dd --limit-speed](https://lists.gnu.org/archive/html/coreutils/2012-09/msg00121.html). It was thought best to leave this to tools like pv or trickle
- [dd conv=noerror](https://lists.gnu.org/archive/html/coreutils/2012-12/msg00146.html) should apply to writes as well as reads. shred is best used for this use case
- [dd iflag=seekable oflag=seekable](https://bugs.gnu.org/13391) to verify lseek(2) support. The feature was not deemed useful/complete enough
- [dd conv=offload](https://lists.gnu.org/archive/html/coreutils/2014-03/msg00055.html) to offload copying to various backends. This was thought too specialized to support explicitly
- [dd conv=truncpost](https://lists.gnu.org/archive/html/coreutils/2014-06/msg00025.html). To support filtering files in place. Due to error handling this was not thought useful enough

### df

- [df,du -g](https://lists.gnu.org/archive/html/bug-coreutils/2009-12/msg00000.html). Specifying “Gigabyte” output format is neither standard or required
- [df autoscale](https://lists.gnu.org/archive/html/bug-coreutils/2010-09/msg00046.html). df -h was thought good enough
- [df -g](https://lists.gnu.org/archive/html/bug-coreutils/2012-07/msg00025.html). Separate options for various output units is best avoided
- [df --without-header](https://lists.gnu.org/archive/html/coreutils/2012-12/msg00067.html). --header options are only really useful for data consumers
- [df --dereference](https://lists.gnu.org/archive/html/coreutils/2013-11/msg00072.html) to process symlink targets. df was changed to reference symlink targets unconditionally

### du

- [du --format](https://lists.gnu.org/archive/html/coreutils/2011-02/msg00072.html) to allow sorting. sort -h handles this
- [accurate du results for OCFS2 reflinked files](https://lists.gnu.org/archive/html/coreutils/2012-10/msg00015.html). It was thought too complicated and specific to add to du
- [du --exclude-dirs](https://lists.gnu.org/archive/html/coreutils/2013-04/msg00043.html) to exclude directories themselves from the usage count. find .. | du was thought sufficient
- [du --sort](https://lists.gnu.org/archive/html/bug-coreutils/2014-05/msg00110.html) to sort by disk usage. This is already supported directly by du -h | sort -h

### join

- [Auto detect output format for join](https://lists.gnu.org/archive/html/bug-coreutils/2009-11/msg00083.html). We need to consider further whether this is useful
- [join more than two files](https://lists.gnu.org/archive/html/coreutils/2012-01/msg00104.html). It would add complexity while not being scalable
- [join more than one field](https://lists.gnu.org/archive/html/coreutils/2012-02/msg00066.html). There wasn't much interest in this
- [comm,join --parallel](https://lists.gnu.org/archive/html/bug-coreutils/2013-11/msg00185.html) to use multiple cores. It was thought best to split the data for multiple processes
- [join -t '\\t'](https://lists.gnu.org/archive/html/coreutils/2013-11/msg00049.html) to use a TAB delimiter. Using the shell to specify the TAB char like join -t $'\\t', was thought ubiquitous enough

### ls

- Change/revert the default quoting style - please see the [ls quotes](https://www.gnu.org/software/coreutils/quotes.html) page for details.
- [UTF-8 arrows in ls](https://lists.gnu.org/archive/html/bug-coreutils/2009-08/msg00023.html). See this [l script](https://www.pixelbeat.org/scripts/l) as an alternative
- [df/ls --blocksize={decimal,binary}](https://lists.gnu.org/archive/html/bug-coreutils/2010-10/msg00022.html). Though more correct, it was deemed overkill
- [ls --sort=class](https://lists.gnu.org/archive/html/coreutils/2012-05/msg00013.html). Sorting by type indicator was deemed of marginal benefit
- [ls --octal](https://lists.gnu.org/archive/html/coreutils/2013-03/msg00049.html) to output octal permissions. Using stat or find is deemed sufficient
- [ls --group-numbers=locale](https://lists.gnu.org/archive/html/coreutils/2013-06/msg00019.html) to output thousands separators. [BLOCK\_SIZE](https://lists.gnu.org/archive/html/coreutils/2013-06/msg00021.html) or [numfmt](https://lists.gnu.org/archive/html/coreutils/2013-06/msg00033.html) were deemed sufficient
- [Suppress trailing slash with ls -F /](https://lists.gnu.org/archive/html/coreutils/2013-02/msg00161.html). The result was deemed [too inconsistent](https://lists.gnu.org/archive/html/coreutils/2013-05/msg00024.html)
- [ls --just=$filetype](https://lists.gnu.org/archive/html/coreutils/2013-11/msg00093.html) to limit file type listed. Filtering classify tags like \`ls --color -lF | sed -n 's#/$##p'\` was thought sufficient
- [ls --sort=inode](https://lists.gnu.org/archive/html/bug-coreutils/2014-11/msg00062.html). It was though find ... | sort was more appropriate for this low level functionality

### mv

- [mv -p](https://debbugs.gnu.org/cgi/bugreport.cgi?bug=5926) (create target dir). It was thought more functional to just \`mkdir -p\` first
- [mv --symbolic-link](https://lists.gnu.org/archive/html/coreutils/2012-11/msg00031.html). It was thought that mv and ln --relative separately give more control
- [mv --safe](https://lists.gnu.org/archive/html/coreutils/2013-04/msg00019.html) to only remove source on completion. \`cp ... && rm\` was deemed sufficient
- [mv --parents](https://lists.gnu.org/archive/html/coreutils/2013-04/msg00076.html) to [recreate a hierarchy](https://lists.gnu.org/archive/html/bug-coreutils/2009-09/msg00003.html). [Using cp -l --parents](https://lists.gnu.org/archive/html/coreutils/2013-05/msg00013.html) is deemed sufficient

### rm

- [rm --parents](https://lists.gnu.org/archive/html/bug-coreutils/2009-07/msg00033.html). Deleting the opposite way up the tree was deemed too dangerous
- [rm -d](https://lists.gnu.org/archive/html/bug-coreutils/2010-06/msg00105.html). rmdir is equivalent and less confusing
- [rm --no-preserve-root](https://lists.gnu.org/archive/html/bug-coreutils/2012-01/msg00096.html). Adding protective prompts would not significantly improve security
- [rm -rf **.**](https://lists.gnu.org/archive/html/bug-coreutils/2012-09/msg00011.html) to delete current directory. It was thought existing support for rm -rf "$PWD" suffices
- [rm -rf **.**](https://bugs.gnu.org/12339#209) to delete all files inside current directory. \`rm -rf \* .\[!.\] .??\*\` and \`find . -delete\` were deemed sufficient
- [rm -s](https://lists.gnu.org/archive/html/coreutils/2013-04/msg00069.html) to behave in a “smarter” fashion. \`rm -I\` or \`find | xargs rm\` were deemed sufficient
- [rm --exclude](https://lists.gnu.org/archive/html/bug-coreutils/2013-09/msg00010.html) to exclude file names. Existing tools like find(1) were thought sufficient
- [rm should use remove()](https://lists.gnu.org/archive/html/bug-coreutils/2013-11/msg00054.html), leaving unlink() to the unlink command. We can't change such standardized functionality

### shred

- [shred --recursive](https://lists.gnu.org/archive/html/bug-coreutils/2009-06/msg00222.html). Deemed better to explicitly select (with find for example)
- [shred -r](https://lists.gnu.org/archive/html/coreutils/2011-12/msg00044.html). shred is of limited use with files anyway

### sort

- [sort --by-length](https://lists.gnu.org/archive/html/bug-coreutils/2009-07/msg00102.html). A “sort by line length” example was added to the info docs
- [min,max commands](https://lists.gnu.org/archive/html/bug-coreutils/2009-07/msg00007.html). (the sort --range={} alternative seems useful though)
- [sort -V auto ignores white-space](https://lists.gnu.org/archive/html/bug-coreutils/2010-03/msg00031.html). One can do that more generally with -b
- [sort -I](https://lists.gnu.org/archive/html/coreutils/2011-06/msg00082.html) to sort IP addresses. It's debatable whether this is warranted
- [SORT\_BUFFER\_SIZE=1234 sort](https://lists.gnu.org/archive/html/coreutils/2012-05/msg00058.html). env vars can be useful when shared by many commands, but are best avoided
- sort to use /var/tmp by default. It was thought best to [keep using /tmp](https://lists.gnu.org/archive/html/coreutils/2012-09/msg00139.html) as its tmp files are stateless
- [sort fixed width fields](https://lists.gnu.org/archive/html/coreutils/2012-12/msg00102.html). This is already supported with: sort -d$'\\n' -k1.5,1.9 ...
- [sort --header](https://lists.gnu.org/archive/html/coreutils/2013-01/msg00027.html) to exclude leading lines from the sort. sed, head, etc. were deemed sufficient

### stat

- [stat --list-fstypes](https://lists.gnu.org/archive/html/coreutils/2013-08/msg00012.html). The internally supported file system IDs were thought best not exposed
- [stat --files0-from=FILE](https://lists.gnu.org/archive/html/coreutils/2014-05/msg00061.html). This is only needed for commands needing to process all arguments in a single invocation
- [stat --digest-type=WORD](https://lists.gnu.org/archive/html/coreutils/2014-05/msg00061.html). It was thought better to use the existing checksum utils and join the file names etc. separately
- [stat --quoting-style=WORD](https://lists.gnu.org/archive/html/coreutils/2014-05/msg00061.html). Adjustments to --format='%N' were thought more appropriate

### \*sum

- [md5sum --threads](https://lists.gnu.org/archive/html/bug-coreutils/2009-10/msg00179.html). The UNIX toolkit already handles processing files in parallel
- [md5sum --base32](https://lists.gnu.org/archive/html/bug-coreutils/2009-11/msg00206.html). There was [little](https://lists.gnu.org/archive/html/bug-coreutils/2010-04/msg00195.html) interest in this Internet Archive specific functionality
- [configurable md5sum buffer size](https://lists.gnu.org/archive/html/bug-coreutils/2010-02/msg00189.html). It was thought better to use NFS parameters to minimize network latency, or the stdbuf utility to control the buffering more generally
- [md5sum --threads](https://lists.gnu.org/archive/html/bug-coreutils/2010-03/msg00262.html). UNIX tools were deemed good enough to [process separate files in parallel](https://lists.gnu.org/archive/html/bug-coreutils/2009-10/msg00179.html)
- [\*sum --ignore-dirs](https://lists.gnu.org/archive/html/bug-coreutils/2011-12/msg00138.html). The use cases were seen as too limited
- [md5sum --pipe](https://lists.gnu.org/archive/html/coreutils/2012-12/msg00130.html) to output checksum to file and data to stdout. tee suffices for this
- [\*sum --color](https://lists.gnu.org/archive/html/coreutils/2013-12/msg00152.html) to colorize the checksum to ease comparisons. This was thought more flexible to perform with separate tools
- [\*sum --no-filename](https://lists.gnu.org/archive/html/coreutils/2014-04/msg00025.html) to only output the checksum. Postprocessing the output was deemed sufficient

### touch

- [touch -R](https://lists.gnu.org/archive/html/bug-coreutils/2010-09/msg00029.html). \`find . -exec touch -am {} +\` is more general
- [touch --mode](https://lists.gnu.org/archive/html/coreutils/2011-01/msg00025.html). Not deemed beneficial enough
- [touch --verbose](https://lists.gnu.org/archive/html/bug-coreutils/2012-04/msg00003.html). This could not be implemented robustly. Also xargs --verbose or (set -x; touch \*) are sufficient
- [touch --create](https://lists.gnu.org/archive/html/coreutils/2013-02/msg00113.html) to only create files. \`test -e file || touch file\` was deemed sufficient

### uniq

- [uniq --unsorted](https://lists.gnu.org/archive/html/coreutils/2011-11/msg00018.html). This would add a lot of complexity that's already contained within sort
- [uniq --ignore-last-fields](https://lists.gnu.org/archive/html/bug-coreutils/2011-12/msg00158.html). \`rev | uniq -f | rev\` was deemed sufficient
- [uniq --acumulate](https://lists.gnu.org/archive/html/coreutils/2012-02/msg00021.html). Adding values was thought too specific for coreutils and is available elsewhere
- [uniq --check-fields=N](https://lists.gnu.org/archive/html/coreutils/2013-02/msg00016.html) to only check N fields. [uniq --key](https://lists.gnu.org/archive/html/bug-coreutils/2006-06/msg00211.html) would be a more general solution
- [uniq -c --total](https://lists.gnu.org/archive/html/bug-coreutils/2010-08/msg00111.html). piping to awk '{t+=$1}END{print t,"total"}1' was deemed sufficient
- [uniq --regex](https://lists.gnu.org/archive/html/coreutils/2013-10/msg00037.html) to use a regular expression to match lines. Existing tools using a DSU pattern is more general

### wc

- [wc --tab-width](https://lists.gnu.org/archive/html/bug-coreutils/2009-06/msg00026.html). Preprocessing with expand is more functional
- [wc -q](https://lists.gnu.org/archive/html/coreutils/2014-02/msg00008.html) to suppress the file name. Redirecting file to stdin is sufficient
- [wc --max-chars=N](https://lists.gnu.org/archive/html/coreutils/2014-04/msg00001.html) to filter out long lines. Existing filters like awk 'length($0) <= 3' were deemed more appropriate

### misc

- [tr -0](https://lists.gnu.org/archive/html/bug-coreutils/2009-06/msg00207.html). Can do the same thing with marginally more tr syntax
- [--at options for commands](https://lists.gnu.org/archive/html/bug-coreutils/2009-11/msg00171.html). This functionality was not deemed required for shell
- [command --examples](https://lists.gnu.org/archive/html/bug-coreutils/2010-11/msg00210.html). This would need to be accepted into the GNU Coding Standards first
- [sleep --random](https://lists.gnu.org/archive/html/bug-coreutils/2009-09/msg00282.html). The existing tools to achieve this were deemed sufficient
- [remove chown,cpio user: shortcut](https://lists.gnu.org/archive/html/bug-coreutils/2009-12/msg00014.html). This was deemed useful and so retained
- [mktemp -tp](https://lists.gnu.org/archive/html/bug-coreutils/2010-06/msg00013.html). It was thought better to create a fifo in a temp dir, rather than a temp fifo directly
- [mktemp --fifo](https://lists.gnu.org/archive/html/bug-coreutils/2011-08/msg00023.html). This was not deemed warranted
- [hostname -b](https://lists.gnu.org/archive/html/bug-coreutils/2010-05/msg00083.html). Setting a default hostname is too platform dependent
- [truncate -s +50%](https://lists.gnu.org/archive/html/bug-coreutils/2010-05/msg00115.html). Percentage calculation was thought best handled outside of truncate
- [fold --indent](https://lists.gnu.org/archive/html/bug-coreutils/2010-05/msg00064.html). \`fmt -t | sed 's/^ / /'\` was deemed sufficient
- [fold --prefix](https://lists.gnu.org/archive/html/coreutils/2013-04/msg00054.html) to add a prefix to each line. fmt and/or sed are deemed sufficient
- [pr --fold](https://bugs.gnu.org/13786) to wrap lines. [fold or fmt](https://lists.gnu.org/archive/html/bug-coreutils/2013-02/msg00108.html) can do this before processing by pr
- [users --all](https://bugs.gnu.org/13738) to show even non logged in users. The system interfaces aren't general enough to support this
- [users -h](https://bugs.gnu.org/13737) to show help. --h{,elp} was deemed sufficient
- [groups -0](https://lists.gnu.org/archive/html/bug-coreutils/2011-11/msg00024.html) to support group names with spaces etc. Instead the more standard [id -Gnz](https://lists.gnu.org/archive/html/bug-coreutils/2013-09/msg00028.html) is provided
- [BLOCK\_SIZE={binary,decimal}](https://bugs.gnu.org/7176) to add 'iB' and 'B' suffixes to various “human” numbers. [numfmt](https://www.pixelbeat.org/docs/numfmt.html) can fill this role
- [test -ed](https://lists.gnu.org/archive/html/bug-coreutils/2010-09/msg00011.html). Using stat in a shell function was deemed sufficient
- [tac -z](https://lists.gnu.org/archive/html/bug-coreutils/2011-02/msg00129.html). tac -s $'\\0' is equivalent
- [mkdir --reference](https://lists.gnu.org/archive/html/bug-coreutils/2011-05/msg00059.html) to copy permissions. Using umasks and ACLs was deemed sufficient
- [mkdir -m ... --parents-mode](https://lists.gnu.org/archive/html/coreutils/2013-11/msg00065.html) to use the mode for all created dirs. This would provide any functional benefit over using chmod
- [chroot --before](https://lists.gnu.org/archive/html/coreutils/2011-07/msg00057.html) to determine UIDs outside the chroot. This is a bit specialized for inclusion
- [uname --distro](https://lists.gnu.org/archive/html/bug-coreutils/2011-08/msg00079.html). lsb\_release --id was deemed sufficient
- [uname -i and -p should infer hardware info](https://bugs.gnu.org/13001). It was [thought better](https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=193170) to just use the provided syscall info
- [split --balanced](https://lists.gnu.org/archive/html/bug-coreutils/2011-09/msg00031.html) to balance lines across the last two buckets. split -nl/$num supports this better
- [rmdir -r](https://lists.gnu.org/archive/html/bug-coreutils/2011-11/msg00000.html). rm -r was deemed sufficient
- [rmdir --one-file-system](https://bugs.gnu.org/12400). \`rm\` or \`find\` are more suitible for such edge-cases
- [realpath -t -b](https://lists.gnu.org/archive/html/coreutils/2012-05/msg00058.html). Short options for existing long options were not seen as appropriate
- [readlink -f output/trailing/slash/](https://lists.gnu.org/archive/html/coreutils/2012-09/msg00062.html). It's easy to add the '/' in shell if needed
- [expand --auto-tabs](https://lists.gnu.org/archive/html/coreutils/2012-09/msg00187.html). It wasn't thought to give much benefit over just specifying --tabs
- [seq --format](https://lists.gnu.org/archive/html/bug-coreutils/2012-09/msg00238.html) support for general printf formats. Prefixing etc. is best done outside of seq
- [head --read-all-input](https://lists.gnu.org/archive/html/coreut`ils/2012-10/msg00043.html). It was thought adding [more control to tee](https://debbugs.gnu.org/cgi/bugreport.cgi?bug=11540) was a more general solution
- [echo -- -e](https://bugs.gnu.org/14115) to terminate options in the common way. This would violate POSIX. Use printf instead
- [expand --auto](https://lists.gnu.org/archive/html/coreutils/2013-01/msg00038.html) to auto determine tab stops. The operation would not be general enough
- [csplit '@1'](https://lists.gnu.org/archive/html/coreutils/2013-02/msg00012.html) to split when field 1 changes. [uniq --group](https://lists.gnu.org/archive/html/coreutils/2013-02/msg00123.html) | [csplit --suppress-matched](https://lists.gnu.org/archive/html/coreutils/2011-03/msg00000.html) was thought to be better
- [cplit --output=N](https://lists.gnu.org/archive/html/coreutils/2014-03/msg00020.html) to output only the Nth file. This was thought too specialized to support
- [ln --absolute](https://lists.gnu.org/archive/html/coreutils/2013-04/msg00011.html) to force absolute symlinks. It's easy to get absolute paths with realpath or $PWD
- [timeout setting a TIMEOUT env var](https://lists.gnu.org/archive/html/coreutils/2014-02/msg00028.html). The use case is unusual and supported with explicitly setting vars with env etc.
- [yes -n](https://lists.gnu.org/archive/html/bug-coreutils/2014-10/msg00079.html) to not output a '\\n'. yes whatever | tr -d '\\n' was thought sufficient
- **New environment variables** are discouraged. Wrapper shell scripts and shell aliases are preferred. \
   See [$LS\_ARGS](https://lists.gnu.org/archive/html/coreutils/2018-02/msg00058.html), [$SORT\_BUFFER\_SIZE](https://lists.gnu.org/archive/html/coreutils/2012-08/msg00172.html), [$HUMAN\_B](https://lists.gnu.org/archive/html/bug-coreutils/2017-08/msg00046.html), [rm](https://lists.gnu.org/archive/html/bug-coreutils/2012-09/msg00150.html); Similarly, [$GREP\_OPTIONS](https://lists.gnu.org/archive/html/bug-grep/2017-03/msg00013.html) and [$GZIP](https://www.gnu.org/software/gzip/manual/html_node/Environment.html).
- [Global configuration file **/etc/gnu.conf**](https://bugs.gnu.org/33787). Aliases, shell functions and shell script wrappers are the recommended ways to modify default behaviour of coreutils programs.

### New commands

- [a path manipulation command](https://lists.gnu.org/archive/html/bug-coreutils/2009-07/msg00134.html). Existing tools deemed rich enough
- [Add a sparse command](https://lists.gnu.org/archive/html/bug-coreutils/2009-12/msg00037.html). cp already supports creating sparse files
- [Add a getlimits command](https://lists.gnu.org/archive/html/bug-coreutils/2009-12/msg00049.html). This would not be standard enough outside of a particular project
- [quoted-printable](https://lists.gnu.org/archive/html/bug-coreutils/2009-11/msg00321.html). recode or perl can easily decode this format
- [An errno utility](https://lists.gnu.org/archive/html/bug-coreutils/2010-01/msg00060.html). A full C wrapper around strerror() was deemed overkill. Maybe we'll add a [script](https://www.pixelbeat.org/scripts/errno) to contrib/
- [where am i](https://lists.gnu.org/archive/html/bug-coreutils/2011-04/msg00064.html). \`hostname; pwd\` is fine
- [tableize](https://lists.gnu.org/archive/html/coreutils/2011-11/msg00103.html). This new command was tought better done as a --border option to column -t
- [physmem](https://lists.gnu.org/archive/html/coreutils/2012-08/msg00204.html). Programs to print mem info like \`hwloc-info\` and \`free\` are already available
- [Provide '0' and '1' utils](https://lists.gnu.org/archive/html/coreutils/2013-04/msg00055.html). These were not seen to benefit shell syntax
- [cksum -a algo1 algo2](https://lists.gnu.org/archive/html/coreutils/2013-11/msg00005.html) (like NetBSD) to do many checksums per read. It was thought more general to use separate processes
- [rename](https://lists.gnu.org/archive/html/coreutils/2014-05/msg00020.html) command (from util-linux). There are existing commands to do this, and adjusting for inclusion in coreutils was thought to create too much flux
- [testline](https://lists.gnu.org/archive/html/coreutils/2014-12/msg00010.html) command to expose bloom filter functionality. It was thought options to existing tools were more appropriate
