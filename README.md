# lineuse
Cache line use and branch mispredict reporting tool

This script works with the Linux perf tool to capture and
analyze LBR (Last Branch Record) data to determine how
well I-cache lines are being utilized and how frequently
branches are mispredicted. This information is then presented
in spreadsheet (.xls and .csv) form.

Note: the branch mispredict information is a conservative
estimate. That is, there are _at least_ as many mispredictions
as shown in the spreadsheet. Branches with no know mispredictions
will not appear in the spreadsheet.

The reason that mispredictions can't be precise is that LBRs
can't provide any information on branches that were predicted to
be taken but ended up falling through.

The exact misprediction ratio would be
(mispredicted_ultimately_taken + mispredicted_ultimately_not_taken) /
(branches_taken + branches_fall_through). Everything in that formula
can be derived from LBR data except mispredicted_ultimately_not_taken.

##Dependencies:

Lineuse uses the Spreadsheet::WriteExcel Perl Module.
You can install it with:

    sudo apt-get install libspreadsheet-writeexcel-perl
or

    sudo yum install perl-Spreadsheet-WriteExcel

##Use

There are two ways to run the lineuse script, installed mode
and stand-alone mode. Installed mode is the preferred method.

###Installed use:

Initial setup (requires root):

    mkdir -p /usr/libexec/perf-core/scripts/perl/bin
    cp lineuse-re* /usr/libexec/perf-core/scripts/perl/bin/
    cp lineuse.pl /usr/libexec/perf-core/scripts/perl/

To capture and report on 5 seconds worth of data:

    export LINEUSE_EXEC_NAME=<your_executable_path>
    perf script record lineuse -p <your_pid> sleep 5
    perf script report lineuse

###Stand-alone use:

To capture data for 5 seconds:

    perf record -e instructions -c 2000003 -j u -p <your_pid> sleep 5

To process the data:

    export LINEUSE_EXEC_NAME=<your_executable_path>
    perf script -s lineuse.pl
