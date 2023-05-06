# utl-use-the-value-in-slot-column-to-fill-the-associated-slot-in-the-array
Use the value in slot column to fill the associated slot in the array  
    %let pgm=utl-use-the-value-in-slot-column-to-fill-the-associated-slot-in-the-array;

    Use the value in slot column to fill the associated slot in the array

    The posted solution is incorrect? it does match the ops 'want' dataset. see section problem.


    github
    https://tinyurl.com/5cd5n65k
    https://github.com/rogerjdeangelis/utl-use-the-value-in-slot-column-to-fill-the-associated-slot-in-the-array

    sas communities
    https://tinyurl.com/ymh2bau9
    https://communities.sas.com/t5/SAS-Programming/How-to-increment-a-column-based-on-the-character-value-of-a-row/td-p/871617

    I'd like to increment a specific column that corresponds to a character value of another column.

    Ops 'want' table

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /* OPs want table                                                                                                         */
    /*                                                                                                                        */
    /* data want;                                                                                                             */
    /* input customer_ID product $1. count_A count_B count_C;                                                                 */
    /* datalines;                                                                                                             */
    /* 1 A 1 0 0                                                                                                              */
    /* 1 A 2 0 0                                                                                                              */
    /* 1 B 0 1 0                                                                                                              */
    /* 1 C 0 0 1                                                                                                              */
    /* 2 B 0 1 0                                                                                                              */
    /* 2 C 0 0 1                                                                                                              */
    /* 2 C 0 0 2                                                                                                              */
    /* ;                                                                                                                      */
    /* run;                                                                                                                   */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

      Two Solutions

          1. SAS datastep
          2. WPS datastep

    /*                   _
    (_)_ __  _ __  _   _| |_
    | | `_ \| `_ \| | | | __|
    | | | | | |_) | |_| | |_
    |_|_| |_| .__/ \__,_|\__|
            |_|
    */
    data have ;
      input id slot $1. slota slotb slotc ;
    datalines;
    0 A 0 0 0
    0 B 0 0 0
    0 C 0 0 0
    1 A 0 0 0
    1 A 0 0 0
    1 B 0 0 0
    1 C 0 0 0
    2 B 0 1 1
    2 C 0 1 1
    2 C 0 1 1
    ;;;;
    run;quit;

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /*                                                                                                                        */
    /*                            | RULES (If slot=A then start a seq by puting 1 one in var A                                */
    /*                            |        If slot=A in the next row put A +1 or 2 in var A                                   */
    /*                            |                                                                                           */
    /* ID  SLOT SLOTA SLOTB SLOTC |   A    B    C                                                                             */
    /*                            |                                                                                           */
    /*  0   A      0     0     0  |   1    0    0   1st occurance of A put 1 slot A                                           */
    /*  0   B      0     0     0  |   0    1    0   1st occurance of B put 1 slot B                                           */
    /*  0   C      0     0     0  |   0    0    1   1st occurance of C put 1 slot C                                           */
    /*                                                                                                                        */
    /*                            | This is the original data which matches the ops want table                                */
    /*                            |                                                                                           */
    /*  1   A      0     0     0  |   1    0    0   1st occurance of A put 1 slot A                                           */
    /*  1   A      0     0     0  |   2    0    0   2nd occurance of A put 2 slot A                                           */
    /*  1   B      0     0     0  |   0    1    0                                                                             */
    /*  1   C      0     0     0  |   0    0    1                                                                             */
    /*                                                                                                                        */
    /*  2   B      0     1     1  |   0    1    0                                                                             */
    /*  2   C      0     1     1  |   0    0    1                                                                             */
    /*  2   C      0     1     1  |   0    0    2                                                                             */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*           _               _
      ___  _   _| |_ _ __  _   _| |_
     / _ \| | | | __| `_ \| | | | __|
    | (_) | |_| | |_| |_) | |_| | |_
     \___/ \__,_|\__| .__/ \__,_|\__|
                    |_|
    */

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /*  Up to 40 obs from WANT total obs=10 05MAY2023:17:35:09                                                                */
    /*                                                                                                                        */
    /*                                         SOLUTION                                                                       */
    /*                                         ============                                                                   */
    /*  Obs    ID    SLOTA    SLOTB    SLOTC    A    B    C                                                                   */
    /*                                                                                                                        */
    /*    1     0      0        0        0      1    0    0                                                                   */
    /*    2     0      0        0        0      0    1    0                                                                   */
    /*    3     0      0        0        0      0    0    1                                                                   */
    /*    4     1      0        0        0      1    0    0                                                                   */
    /*    5     1      0        0        0      2    0    0                                                                   */
    /*    6     1      0        0        0      0    1    0                                                                   */
    /*    7     1      0        0        0      0    0    1                                                                   */
    /*    8     2      0        1        1      0    1    0                                                                   */
    /*    9     2      0        1        1      0    0    1                                                                   */
    /*   10     2      0        1        1      0    0    2                                                                   */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*                   _       _            _
     ___  __ _ ___    __| | __ _| |_ __ _ ___| |_ ___ _ __
    / __|/ _` / __|  / _` |/ _` | __/ _` / __| __/ _ \ `_ \
    \__ \ (_| \__ \ | (_| | (_| | || (_| \__ \ ||  __/ |_) |
    |___/\__,_|___/  \__,_|\__,_|\__\__,_|___/\__\___| .__/
                                                     |_|
    */

    data want ;
      set have;
      by id ;
      retain a b c;
      array seq a b c;
      if first.id then do a=0;b=0;c=0; end;
      col=whichc(slot,'A','B','C');
      seq[col]=seq[col]+1;
      /*---- on a change of slot do note retain the other slots ----*/
      select(col);
         when (1) do; b=0;c=0; end;
         when (2) do; a=0;c=0; end;
         when (3) do; a=0;b=0; end;
      end;
      keep id slota slotb slotc a b c;
    run;quit;

    /*                        _       _            _
    __      ___ __  ___    __| | __ _| |_ __ _ ___| |_ ___ _ __
    \ \ /\ / / `_ \/ __|  / _` |/ _` | __/ _` / __| __/ _ \ `_ \
     \ V  V /| |_) \__ \ | (_| | (_| | || (_| \__ \ ||  __/ |_) |
      \_/\_/ | .__/|___/  \__,_|\__,_|\__\__,_|___/\__\___| .__/
             |_|                                          |_|
    */

    %let _pth=%sysfunc(pathname(work));

    %utl_submit_wps64("

    libname wrk '&_pth';

    data want ;
      set wrk.have;
      by id ;
      retain a b c;
      array seq a b c;
      if first.id then do a=0;b=0;c=0; end;
      col=whichc(slot,'A','B','C');
      seq[col]=seq[col]+1;
      select(col);
         when (1) do; b=0;c=0; end;
         when (2) do; a=0;c=0; end;
         when (3) do; a=0;b=0; end;
      end;
      keep id slota slotb slotc a b c;
    run;quit;

    proc print;
    run;quit;

    ");

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /* The WPS System                                                                                                         */
    /*                                                                                                                        */
    /* Obs    ID    SLOTA    SLOTB    SLOTC    A    B    C                                                                    */
    /*                                                                                                                        */
    /*   1     0      0        0        0      1    0    0                                                                    */
    /*   2     0      0        0        0      0    1    0                                                                    */
    /*   3     0      0        0        0      0    0    1                                                                    */
    /*   4     1      0        0        0      1    0    0                                                                    */
    /*   5     1      0        0        0      2    0    0                                                                    */
    /*   6     1      0        0        0      0    1    0                                                                    */
    /*   7     1      0        0        0      0    0    1                                                                    */
    /*   8     2      0        1        1      0    1    0                                                                    */
    /*   9     2      0        1        1      0    0    1                                                                    */
    /*  10     2      0        1        1      0    0    2                                                                    */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*               _     _
     _ __  _ __ ___ | |__ | | ___ _ __ ___
    | `_ \| `__/ _ \| `_ \| |/ _ \ `_ ` _ \
    | |_) | | | (_) | |_) | |  __/ | | | | |
    | .__/|_|  \___/|_.__/|_|\___|_| |_| |_|
    |_|
    */

    data have;
      input customer_ID product $1. count_a count_b count_c ;
    datalines;
    1 A 0 0 0
    1 A 0 0 0
    1 B 0 0 0
    1 C 0 0 0
    2 B 0 1 1
    2 C 0 1 1
    2 C 0 1 1
    ;

    data want;
      set have;
      by customer_id product ;
      array counts count_a count_b count_c ;
      row+1;
      if first.product then row=1;
      index=whichc(product,'A','B','C');
      counts[index]+row;
      drop index row;
    run;

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /*  My solution matches op but posred does not                                                                            */
    /*                                                                                                                        */
    /*   CUSTOMER_                                                My  OPs                                                     */
    /*       ID       PRODUCT    COUNT_A    COUNT_B    COUNT_C     C                                                          */
    /*                                                                                                                        */
    /*       1           A          1          0          0        0   0                                                      */
    /*       1           A          2          0          0        0   0                                                      */
    /*       1           B          0          1          0        0   0                                                      */
    /*       1           C          0          0          1        1   1                                                      */
    /*       2           B          0          2          1*       0   0                                                      */
    /*       2           C          0          1          2*       1   1                                                      */
    /*       2           C          0          1          3*       2   2                                                      */
    /*  * incorrect records                                                                                                   */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*              _
      ___ _ __   __| |
     / _ \ `_ \ / _` |
    |  __/ | | | (_| |
     \___|_| |_|\__,_|

    */
