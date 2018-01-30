# utl_creating_a_new_column_of_consecutive_tokens_like_n_gram
Creating a new column of consecutive tokens like n gram. Keywords: sas sql join merge big data analytics macros oracle teradata mysql sas communities stackoverflow statistics artificial inteligence AI Python R Java Javascript WPS Matlab SPSS Scala Perl C C# Excel MS Access JSON graphics maps NLP natural language processing machine learning igraph DOSUBL DOW loop stackoverflow SAS community.
    Creating a new column of consecutive tokens like n gram

    github
    https://goo.gl/ZfTNrT
    https://github.com/rogerjdeangelis/utl_creating_a_new_column_of_consecutive_tokens_like_n_gram

    Certain sequential processing is best done with WPS/SAS?

    see
    https://goo.gl/15fRjV
    https://stackoverflow.com/questions/48483105/creating-a-new-column-of-consecutive-token-like-n-gram-in-r

    Linus profile
    https://stackoverflow.com/users/3540072/linus

    INPUT
    =====

     Algorithm

        1. Cut A into consecutive 3 character pieces

           Example URBAN is split into three records

             URB
             RBA
             BAN


     WORK.HAVE total obs=3

       A           B

       URBAN       1
       PLAN        2
       ROGERDEA    3


    PROCESS
    =======

     WPS/SAS

       data want(keep=cut a b);
         set have;
         n=lengthn(a);
         if n < 4 then output;
         else do;
           do  idx= 1 to n-2;
             cut=substr(a,idx,3);
             output;
           end;
         end;
       run;quit;

     WPS/R (base R solution)

       %utl_submit_wps64('
       libname sd1 sas7bdat "d:/sd1";
       options set=R_HOME "C:/Program Files/R/R-3.3.2";
       libname wrk sas7bdat "%sysfunc(pathname(work))";
       proc r;
       submit;
       library(haven);
       df<-read_sas("d:/sd1/have.sas7bdat");
       result_list <-list();
       for(i in 1:nrow(df)){
         your_word <- df[i,1];
         for(j in 1:(nchar(your_word)-2)){
           result_list[[length(result_list)+1]] <- c(your_word, substr(your_word, j, j+2), df[i,2]);
         };
       };
       want <- as.data.frame(do.call(rbind, result_list));
       list2env(want,envir=.GlobalEnv);
       want<-cbind(t(as.data.frame(A)),t(as.data.frame(B)),t(as.data.frame(V2)));
       endsubmit;
       import r=want data=wrk.want_r;
       run;quit;
       ');

    OUTPUT
    ======

      SAS/WPS

       WORK.WANT total obs=11

        Obs    A           B    CUT

          1    URBAN       1    URB
          2    URBAN       1    RBA
          3    URBAN       1    BAN
          4    PLAN        2    PLA
          5    PLAN        2    LAN
          6    ROGERDEA    3    ROG
          7    ROGERDEA    3    OGE
          8    ROGERDEA    3    GER
          9    ROGERDEA    3    ERD
         10    ROGERDEA    3    RDE
         11    ROGERDEA    3    DEA

     WPS/R

       WORK.WANT_R total obs=11

        Obs    V1          V2   V3

          1    URBAN       1    URB
          2    URBAN       1    RBA
          3    URBAN       1    BAN
          4    PLAN        2    PLA
          5    PLAN        2    LAN
          6    ROGERDEA    3    ROG
          7    ROGERDEA    3    OGE
          8    ROGERDEA    3    GER
          9    ROGERDEA    3    ERD
         10    ROGERDEA    3    RDE
         11    ROGERDEA    3    DEA

    *          _       _   _
     ___  ___ | |_   _| |_(_) ___  _ __  ___
    / __|/ _ \| | | | | __| |/ _ \| '_ \/ __|
    \__ \ (_) | | |_| | |_| | (_) | | | \__ \
    |___/\___/|_|\__,_|\__|_|\___/|_| |_|___/

    ;

    WPS/SAS
    ========

    %utl_submit_wps64('
    libname sd1 "d:/sd1";
    libname wrk sas7bdat "%sysfunc(pathname(work))";
       data wrk.want_wps(keep=cut a b);
         set sd1.have;
         n=lengthn(a);
         if n < 4 then output;
         else do;
           do  idx= 1 to n-2;
             cut=substr(a,idx,3);
             output;
           end;
         end;
       run;quit;
    run;quit;
    ');


    WPS/R
    =====

    %utl_submit_wps64('
    libname sd1 sas7bdat "d:/sd1";
    options set=R_HOME "C:/Program Files/R/R-3.3.2";
    libname wrk sas7bdat "%sysfunc(pathname(work))";
    proc r;
    submit;
    library(haven);
    df<-read_sas("d:/sd1/have.sas7bdat");
    result_list <-list();
    for(i in 1:nrow(df)){
      your_word <- df[i,1];
      for(j in 1:(nchar(your_word)-2)){
        result_list[[length(result_list)+1]] <- c(your_word, substr(your_word, j, j+2), df[i,2]);
      };
    };
    want <- as.data.frame(do.call(rbind, result_list));
    list2env(want,envir=.GlobalEnv);
    want<-cbind(t(as.data.frame(A)),t(as.data.frame(B)),t(as.data.frame(V2)));
    endsubmit;
    import r=want data=wrk.want_r;
    run;quit;
    ');

