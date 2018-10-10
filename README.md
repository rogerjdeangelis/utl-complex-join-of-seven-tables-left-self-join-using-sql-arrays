# utl-complex-join-of-seven-tables-left-self-join-using-sql-arrays
Complex join of seven tables using sql arrays .

    Complex join of seven tables using sql arrays                                                                                      
                                                                                                                                       
    github                                                                                                                             
    https://tinyurl.com/ybt3m8tl                                                                                                       
    https://github.com/rogerjdeangelis/utl-complex-join-of-seven-tables-left-self-join-using-sql-arrays                                
                                                                                                                                       
    SAS  Forum                                                                                                                         
    https://tinyurl.com/y7khbd2l                                                                                                       
    https://communities.sas.com/t5/SAS-Programming/How-do-I-Change-the-previous-values-based-upon-the-next-values/m-p/503003           
                                                                                                                                       
    Macros                                                                                                                             
    https://github.com/rogerjdeangelis/utl-macros-used-in-many-of-rogerjdeangelis-repositories                                         
                                                                                                                                       
    This is a flexible extensible solution, key static data is external.                                                               
    Also these kinds of solutions can be very fast for                                                                                 
    Teradata, Oracle Exadata or SAS SPDS with Big Data.                                                                                
                                                                                                                                       
    INPUT                                                                                                                              
    =====                                                                                                                              
                                                                                                                                       
    WORK.HAVE total obs=14                                                                                                             
                                                                                                                                       
        MONTH     ID    BONUS                                                                                                          
                                                                                                                                       
       201701    101      0                                                                                                            
       201701    102      0                                                                                                            
       201702    101      0                                                                                                            
       201702    102      1                                                                                                            
       201703    101      0                                                                                                            
       201703    102      0                                                                                                            
       201704    101      0                                                                                                            
       201704    102      0                                                                                                            
       201705    101      0                                                                                                            
       201705    102      0                                                                                                            
       201706    101      0                                                                                                            
       201706    102      0                                                                                                            
       201707    101      1                                                                                                            
       201707    102      0                                                                                                            
                                                                                                                                       
                                                                                                                                       
    RULES                                                                                                                              
    =====                                                                                                                              
                                                                                                                                       
    SIMPLFY USING SQL ARRAYS                                                                                                           
                                                                                                                                       
    proc sql noprint;                                                                                                                  
        CREATE TABLE want AS                                                                                                           
        SELECT                                                                                                                         
            a.*                                                                                                                        
           ,b.bonus AS bonus1                                                                                                          
           ,c.bonus as bonus2                                                                                                          
           ,d.bonus as bonus3                                                                                                          
           ,e.bonus as bonus4                                                                                                          
           ,f.bonus as bonus5                                                                                                          
           ,g.bonus as bonus6                                                                                                          
        FROM                                                                                                                           
            have a                                                                                                                     
        LEFT JOIN                                                                                                                      
             have b                                                                                                                    
        ON                                                                                                                             
            b.ID=a.ID AND b.Month=a.Month+1+88*(mod(a.MONTH,100)>11)                                                                   
        LEFT JOIN                                                                                                                      
            have c                                                                                                                     
        ON                                                                                                                             
            c.ID=a.ID AND c.Month=a.Month+2+88*(mod(a.MONTH,100)>10)                                                                   
        LEFT JOIN                                                                                                                      
            have d                                                                                                                     
        ON                                                                                                                             
           d.ID=a.ID AND d.Month=a.Month+3+88*(mod(a.MONTH,100)>9)                                                                     
         LEFT JOIN                                                                                                                     
            have e                                                                                                                     
        ON                                                                                                                             
            e.ID=a.ID AND e.Month=a.Month+4+88*(mod(a.MONTH,100)>8)                                                                    
        LEFT JOIN                                                                                                                      
             have f                                                                                                                    
        ON                                                                                                                             
             f.ID=a.ID AND f.Month=a.Month+5+88*(mod(a.MONTH,100)>7)                                                                   
        LEFT JOIN                                                                                                                      
             have g                                                                                                                    
        ON                                                                                                                             
             g.ID=a.ID AND g.Month=a.Month+6+88*(mod(a.MONTH,100)>6)                                                                   
        ORDER                                                                                                                          
             BY Month, ID                                                                                                              
        ;                                                                                                                              
    quit;                                                                                                                              
                                                                                                                                       
                                                                                                                                       
    EXAMPLE OUTPUT                                                                                                                     
    ---------------                                                                                                                    
     WORK.WANT total obs=14                                                                                                            
                                                                                                                                       
      MONTH     ID    BONUS    BONUS1    BONUS2    BONUS3    BONUS4    BONUS5    BONUS6                                                
                                                                                                                                       
     201701    101      0         0         0         0         0         0         1                                                  
     201701    102      0         1         0         0         0         0         0                                                  
     201702    101      0         0         0         0         0         1         .                                                  
     201702    102      1         0         0         0         0         0         .                                                  
     201703    101      0         0         0         0         1         .         .                                                  
     201703    102      0         0         0         0         0         .         .                                                  
     201704    101      0         0         0         1         .         .         .                                                  
     201704    102      0         0         0         0         .         .         .                                                  
     201705    101      0         0         1         .         .         .         .                                                  
     201705    102      0         0         0         .         .         .         .                                                  
     201706    101      0         1         .         .         .         .         .                                                  
     201706    102      0         0         .         .         .         .         .                                                  
     201707    101      1         .         .         .         .         .         .                                                  
     201707    102      0         .         .         .         .         .         .                                                  
                                                                                                                                       
                                                                                                                                       
    PROCESS                                                                                                                            
    ========                                                                                                                           
                                                                                                                                       
    %array(ts,values=1-6);                                                                                                             
    %array(vs,values=11 10 9 8 7 6);                                                                                                   
                                                                                                                                       
    proc sql noprint;                                                                                                                  
        CREATE TABLE want AS                                                                                                           
        SELECT a.*                                                                                                                     
          ,%do_over(ts,phrase=a?.bonus as bonus?,between=comma)                                                                        
        FROM have a                                                                                                                    
           %do_over(ts vs, phrase=%str(                                                                                                
             LEFT JOIN have a?ts                                                                                                       
               ON a?ts.ID=a.ID AND a?ts.Month=a.Month+ ?ts +88*(mod(a.MONTH,100) > ?vs))                                               
          )                                                                                                                            
        ORDER BY Month, ID                                                                                                             
        ;                                                                                                                              
    quit;                                                                                                                              
                                                                                                                                       
                                                                                                                                       
    OUTPUT                                                                                                                             
    ======                                                                                                                             
                                                                                                                                       
    see above                                                                                                                          
                                                                                                                                       
                                                                                                                                       
    *                _               _       _                                                                                         
     _ __ ___   __ _| | _____     __| | __ _| |_ __ _                                                                                  
    | '_ ` _ \ / _` | |/ / _ \   / _` |/ _` | __/ _` |                                                                                 
    | | | | | | (_| |   <  __/  | (_| | (_| | || (_| |                                                                                 
    |_| |_| |_|\__,_|_|\_\___|   \__,_|\__,_|\__\__,_|                                                                                 
                                                                                                                                       
    ;                                                                                                                                  
                                                                                                                                       
                                                                                                                                       
    data have;                                                                                                                         
        input month id bonus;                                                                                                          
        cards4;                                                                                                                        
    201701 101 0                                                                                                                       
    201701 102 0                                                                                                                       
    201702 101 0                                                                                                                       
    201702 102 1                                                                                                                       
    201703 101 0                                                                                                                       
    201703 102 0                                                                                                                       
    201704 101 0                                                                                                                       
    201704 102 0                                                                                                                       
    201705 101 0                                                                                                                       
    201705 102 0                                                                                                                       
    201706 101 0                                                                                                                       
    201706 102 0                                                                                                                       
    201707 101 1                                                                                                                       
    201707 102 0                                                                                                                       
    ;;;;                                                                                                                               
    run;quit;                                                                                                                          
                                                                                                                                       
                                                                                                                                       
