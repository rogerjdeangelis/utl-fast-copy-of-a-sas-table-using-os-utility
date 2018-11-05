# utl-fast-copy-of-a-sas-table-using-os-utility
Fast copy of a sas table using os utility.
    Fast copy of a sas table using os utility

    For tiny data like this(34mm rows 50 cols), I copied the table in 12 seconds.

    Classic SAS on a 2012 Dell laptop with workstation and SAS 9.4M2 WIn 7 64bit

    Are you on an EG server?

    HAVE
    ====

    34 million rows and 50 columns

    Middle Observation(17000000 ) of d_wrk.million34 - Total Obs 34,000,000

     -- NUMERIC --

    Var            type       Value

    FIFTY1           N8       1234
    FIFTY2           N8       1234
    FIFTY3           N8       1234
    FIFTY4           N8       1234
    FIFTY5           N8       1234
    FIFTY6           N8       1234
    ..

    FIFTY48          N8       1234
    FIFTY49          N8       1234
    FIFTY50          N8       1234

    PROCESS
    =======

    * copy to c/wrk;

    options xsync xwait;
    %let beg=%sysfunc(time());

    data _null_;
    call system('robocopy d:\wrk c:\wrk\ million34.sas7bdat');
    run;quit;

    %let elap = %sysevalf(%sysfunc(time()) - &beg);
    %put &=elap;

    LOG
    ===
    NOTE: DATA statement used (Total process time):
          real time           11.57 seconds
          cpu time            0.03 seconds


    1047  %let elap = %sysevalf(%sysfunc(time()) - &beg);
    1048  %put &=elap;
    ELAP=11.5799999236987


    MAKE DATA
    =========

    * copying a tiny 34 million table with 50 columns;

    libname d_wrk "d:/wrk";
    data d_wrk.million34(compress=binary);
     array fifty[50] (50*1234);
     do i=1 to 34000000;
       output;
     end;
     drop i;
    run;quit;



