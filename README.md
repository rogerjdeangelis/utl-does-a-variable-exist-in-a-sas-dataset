# utl-does-a-variable-exist-in-a-sas-dataset

    %let pgm=utl-does-a-variable-exist-in-a-sas-dataset;

    Does a variable exist in a sas dataset

       Three Solutions
           1. https://www.9to5sas.com/variable-exists-sas/  (not shown)
           2. Non macro solution (less Klingon)
           3. macro solution (somewhat less Klingon)
           4. Bart's baseplus sas package
              Bartosz Jablonski
              yabwon@gmail.com
              https://github.com/yabwon
              varExist macro also on the end (has much more functionality)

      1. Lets check in AGE is in sashelp.class
      2. Lets check if AGES is in sashelp.class

    GitHub
    https://tinyurl.com/yjz7dd7k
    https://github.com/rogerjdeangelis/utl-does-a-variable-exist-in-a-sas-dataset

    /*   _                           _
      __| | ___ _ __   ___ _ __   __| | ___ _ __   ___ _   _
     / _` |/ _ \ `_ \ / _ \ `_ \ / _` |/ _ \ `_ \ / __| | | |
    | (_| |  __/ |_) |  __/ | | | (_| |  __/ | | | (__| |_| |
     \__,_|\___| .__/ \___|_| |_|\__,_|\___|_| |_|\___|\__, |
               |_|                                     |___/
    */

    * you need thiis macro;

    %macro dosubl(arg);
      %let rc=%qsysfunc(dosubl(&arg));
    %mend dosubl;

    /*                   _
    (_)_ __  _ __  _   _| |_
    | | `_ \| `_ \| | | | __|
    | | | | | |_) | |_| | |_
    |_|_| |_| .__/ \__,_|\__|
            |_|
    */

    /************************************/
    /*                                  */
    /*  SAHELP.CLASS                    */
    /*                                  */
    /*   Variables in Creation Order    */
    /*                                  */
    /*  #    Variable    Type    Len    */
    /*                                  */
    /*  1    NAME        Char      8    */
    /*  2    SEX         Char      1    */
    /*  3    AGE         Num       8    */
    /*  4    HEIGHT      Num       8    */
    /*  5    WEIGHT      Num       8    */
    /*                                  */
    /************************************/

    /*           _               _
      ___  _   _| |_ _ __  _   _| |_
     / _ \| | | | __| `_ \| | | | __|
    | (_) | |_| | |_| |_) | |_| | |_
     \___/ \__,_|\__| .__/ \__,_|\__|
                    |_|
    */

    /*****************************************************************************/
    /*                                                                           */
    /*  In the log                                                               */
    /*                                                                           */
    /*  * Non macro output                                                       */
    /*                                                                           */
    /*  AGES Variable does not exist                                             */
    /*  AGE  Variable is located in column 3 .                                   */
    /*                                                                           */
    /*  * Macro output                                                           */
    /*                                                                           */
    /*  Variable ages does not exist in dataset sashelp.class                    */
    /*  Variable age exists in dataset sashelp.class and is located in column 3 .*/
    /*                                                                           */
    /*****************************************************************************/

    /*         _       _   _
     ___  ___ | |_   _| |_(_) ___  _ __  ___
    / __|/ _ \| | | | | __| |/ _ \| `_ \/ __|
    \__ \ (_) | | |_| | |_| | (_) | | | \__ \
    |___/\___/|_|\__,_|\__|_|\___/|_| |_|___/
     _ __   ___  _ __    _ __ ___   __ _  ___ _ __ ___
    | `_ \ / _ \| `_ \  | `_ ` _ \ / _` |/ __| `__/ _ \
    | | | | (_) | | | | | | | | | | (_| | (__| | | (_) |
    |_| |_|\___/|_| |_| |_| |_| |_|\__,_|\___|_|  \___/

    */

    * you need to manually edit this code;

    %dosubl('
       data _null_;
        dsid=open("sashelp.class");
        check=varnum(dsid,"ages");
        if check=0 then put "Variable does not exist";
        else put "Variable is located in column " check +(_1) ".";
       run;
    ');

    /*
     _ __ ___   __ _  ___ _ __ ___
    | `_ ` _ \ / _` |/ __| `__/ _ \
    | | | | | | (_| | (__| | | (_) |
    |_| |_| |_|\__,_|\___|_|  \___/

    */

    * just in case they exist in open code;
    %symdel dsn var / nowarn;

    %macro varexist(dsn,var);

    %dosubl('
       data _null_;
        dsid=open("&dsn");
        check=varnum(dsid,"&var");
        if check=0 then put "Variable &var does not exist in dataset &dsn";
        else put "Variable &var exists in dataset &dsn and is located in column " check +(_1) ".";
       run;
    ');

    %mend varexist;

    %varexist(sashelp.class,age);
    %varexist(sashelp.class,ages);

    /*___             _
    | __ )  __ _ _ __| |_
    |  _ \ / _` | `__| __|
    | |_) | (_| | |  | |_
    |____/ \__,_|_|   \__|

    */

    * Create folder d:/SAS_PACKAGES;

    data _null_;
      rc=dcreate("sas_packages","d:/");
    run;quit;

    * Copy SPFinit.sas to d:/SAS_PACKAGES;

    filename SPFinit url "https://raw.githubusercontent.com/yabwon/SAS_PACKAGES/main/SPF/SPFinit.sas";
    data _null_;
      infile SPFinit;
      input;;
      file "d:/SAS_PACKAGES/SPFinit.sas";
      put _infile_;
    run;quit;

    * execute the package;
    %inc "d:/SAS_PACKAGES/SPFinit.sas";

    %macro varExist(var, ds)/minoperator;
    /*
    https://github.com/yabwon/SAS_PACKAGES/blob/main/packages/baseplus.md#getvars-macro
    */
    %if %upcase(&var.) in %upcase((%getVars(&ds.)))
      %then %put Exist!;
      %else %put Does NOT exist!;


    %varExist(AGE, sashelp.class)
    %varExist(AGES, sashelp.class)

    /*                                           _
     _ __ ___   __ _  ___ _ __ ___     __ _  ___| |___   ____ _ _ __ ___
    | `_ ` _ \ / _` |/ __| `__/ _ \   / _` |/ _ \ __\ \ / / _` | `__/ __|
    | | | | | | (_| | (__| | | (_) | | (_| |  __/ |_ \ V / (_| | |  \__ \
    |_| |_| |_|\__,_|\___|_|  \___/   \__, |\___|\__| \_/ \__,_|_|  |___/
                                      |___/
    */

    %macro getVars(
      ds               /* Name of the dataset, required. */
    , sep = %str( )    /* Variables separator, space is the default one. */
    , pattern = .*     /* Variable name regexp pattern, .*(i.e. any text) is the default, case INSENSITIVE! */
    , varRange = _all_ /* Named range list of variables, _all_ is the default. */
    , quote =          /* Quotation symbol to be used around values, blank is the default */
    , mcArray =        /* Name of macroArray to be generated from list of variables */
    )
    /des = 'The %getVars() and %QgetVars() macro functions allows to extract variables list from dataset.'
    ;
    /*%local t;*/
    /*%let t = %sysfunc(time());*/

      %local VarList di dx i VarName VarCnt;
      %let VarCnt = 0;

      %let di = %sysfunc(open(&ds.(keep=&varRange.), I)); /* open dataset with subset of variables */
      %let dx = %sysfunc(open(&ds.                 , I)); /* open dataset with ALL variables */
      %if &di. > 0 %then
        %do;
          %do i = 1 %to %sysfunc(attrn(&dx., NVARS)); /* iterate over ALL variables names */
            %let VarName = %sysfunc(varname(&dx., &i.));

            %if %sysfunc(varnum(&di., &VarName.)) > 0 /* test if the variable is in the subset */
                AND
                %sysfunc(prxmatch(/%bquote(&pattern.)/i, &VarName.)) > 0 /* check the pattern */
            %then
              %do;
                %let VarCnt = %eval(&VarCnt. + 1);
                %if %superq(mcArray) = %then
                  %do;
                    %local VarList&VarCnt.;
                      %let VarList&VarCnt. = %nrbquote(&quote.)&VarName.%nrbquote(&quote.);
                  %end;
                %else
                  %do;
                    %global &mcArray.&VarCnt.;
                       %let &mcArray.&VarCnt. = %unquote(%nrbquote(&quote.)&VarName.%nrbquote(&quote.));
                  %end;
                /*
                %if %bquote(&VarList.) = %then
                  %let VarList = %nrbquote(&quote.)&VarName.%nrbquote(&quote.);
                %else
                  %let VarList = &VarList.%nrbquote(&sep.)%nrbquote(&quote.)&VarName.%nrbquote(&quote.);
                */
              %end;
          %end;
        %end;
      %let di = %sysfunc(close(&di.));
      %let dx = %sysfunc(close(&dx.));
    /*%put (%sysevalf(%sysfunc(time()) - &t.));*/
    %if %superq(mcArray) = %then
      %do;
        %do i = 1 %to &VarCnt.;%unquote(&&VarList&i.)%if &i. NE &VarCnt. %then %do;%unquote(&sep.)%end;%end;
      %end;
    %else
      %do;
      /*-----------------------------------------------------------------------------------------------------------*/
        %put NOTE-;
        %put NOTE: When mcArray= parameter is active the getVars macro cannot be called within the %nrstr(%%put) statement.;
        %put NOTE: Execution like: %nrstr(%%put %%getVars(..., mcArray=XXX)) will result with an e.r.r.o.r.;
                   /*e.r.r.o.r. - explicit & radical refuse of run */
        %put NOTE-;
        %local mtext rc;
        %let mtext = _%sysfunc(int(%sysevalf(1000000000 * %sysfunc(rand(uniform)))))_;
        %global &mcArray.LBOUND &mcArray.HBOUND &mcArray.N;
        %let &mcArray.LBOUND = 1;
        %let &mcArray.HBOUND = &VarCnt.;
        %let &mcArray.N      = &VarCnt.;
        %let rc = %sysfunc(doSubL( /*%nrstr(%%put ;)*/
        /*===============================================================================================*/
          options nonotes nosource %str(;)
          DATA _NULL_ %str(;)
           IF %unquote(&VarCnt.) > 0
           THEN
             CALL SYMPUTX("&mtext.",
              ' %MACRO ' !! "&mcArray." !! '(J,M);' !!
              '%local J M; %if %qupcase(&M.)= %then %do;' !! /* empty value is output, I is input */
              '%if %sysevalf( (&&&sysmacroname.LBOUND LE &J.) * (&J. LE &&&sysmacroname.HBOUND) ) %then %do;&&&sysmacroname.&J.%end;' !!
              '%else %do; ' !!
                '%put WARNING:[Macroarray &sysmacroname.] Index &J. out of range.;' !!
                '%put WARNING-[Macroarray &sysmacroname.] Should be between &&&sysmacroname.LBOUND and &&&sysmacroname.HBOUND;' !!
                '%put WARNING-[Macroarray &sysmacroname.] Missing value is used.;' !!
              '%end;' !!
              '%end;' !!
              '%else %do; %if %qupcase(&M.)=I %then %do;' !!
              '%if %sysevalf( (&&&sysmacroname.LBOUND LE &J.) * (&J. LE &&&sysmacroname.HBOUND) ) %then %do;&sysmacroname.&J.%end;' !!
              '%else %do;' !!
                '%put ERROR:[Macroarray &sysmacroname.] Index &J. out of range.;' !!
                '%put ERROR-[Macroarray &sysmacroname.] Should be between &&&sysmacroname.LBOUND and &&&sysmacroname.HBOUND;' !!
              '%end;' !!
              '%end; %end;' !!
              '%MEND;', 'G') %str(;)
           ELSE
             CALL SYMPUTX("&mtext.", ' ', 'G') %str(;)
           STOP %str(;)
          RUN %str(;)
        /*===============================================================================================*/
        )); &&&mtext. %symdel &mtext. / NOWARN ;
        %put NOTE:[&sysmacroname.] &VarCnt. macrovariables created;
      /*-----------------------------------------------------------------------------------------------------------*/
      %end;
    %mend getVars;



    /*              _
      ___ _ __   __| |
     / _ \ `_ \ / _` |
    |  __/ | | | (_| |
     \___|_| |_|\__,_|

    */
