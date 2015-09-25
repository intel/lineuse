# lineuse
Cache line use and branch mispredict reporting tool

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

    cp lineuse-re* /usr/libexec/perf-core/scripts/perl/bin/
    cp lineuse.pl /usr/libexec/perf-core/scripts/perl/

To capture and report on 5 seconds worth of data:

    export LINEUSE_EXEC_NAME=<your_executable_path>
    perf script record lineuse -p <your_pid> sleep 5
    perf script report lineuse
or

    export LINEUSE_EXEC_NAME=<your_executable_path>
    perf script lineuse -p <your_pid> sleep 5

###Stand-alone use:

To capture data for 5 seconds:

    perf record -e instructions -c 2000003 -j u -p <your_pid> sleep 5

To process the data:

    export LINEUSE_EXEC_NAME=<your_executable_path>
    perf script -s lineuse.pl
