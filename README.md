# utl-does-a-variable-exist-in-a-sas-dataset

    %let pgm=utl-does-a-variable-exist-in-a-sas-dataset;

    Does a variable exist in a sas dataset

       Three Solutions
           1. https://www.9to5sas.com/variable-exists-sas/  (not shown
           2. Non macro solution (less Klingon)
           3. macro solution (somewhat less Klingon)

      1. Lets check in AGE is in sashelp.class
      2. Lets check if AGES is in sashelp.class

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

    /*              _
      ___ _ __   __| |
     / _ \ `_ \ / _` |
    |  __/ | | | (_| |
     \___|_| |_|\__,_|

    */
