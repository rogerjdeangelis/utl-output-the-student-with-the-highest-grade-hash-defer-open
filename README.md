# utl-output-the-student-with-the-highest-grade-hash-defer-open
    Output the student with the highest grade hash defer open                                                        
                                                                                                                     
      Four solutions                                                                                                 
                                                                                                                     
          1. Datastep                                                                                                
          2. Sql outobs=1                                                                                            
          3. HASH DOSUBL open=defer (anoter way to interface with dosubl)                                            
          4. Single datastep HASH (more robust handles ties better than my code) by                                  
              Bartosz Jablonski                                                                                      
              yabwon@gmail.com                                                                                       
                                                                                                                     
                                                                                                                     
                                                                                                                     
    github                                                                                                           
    https://tinyurl.com/y6al4qbr                                                                                     
    https://github.com/rogerjdeangelis/utl-output-the-student-with-the-highest-grade-hash-defer-open                 
                                                                                                                     
    SAS Forum                                                                                                        
    https://tinyurl.com/y3gslccd                                                                                     
    https://communities.sas.com/t5/New-SAS-User/Top-value-in-output/m-p/567494                                       
                                                                                                                     
    *_                   _                                                                                           
    (_)_ __  _ __  _   _| |_                                                                                         
    | | '_ \| '_ \| | | | __|                                                                                        
    | | | | | |_) | |_| | |_                                                                                         
    |_|_| |_| .__/ \__,_|\__|                                                                                        
            |_|                                                                                                      
    ;                                                                                                                
                                                                                                                     
    data have;                                                                                                       
          length name $16 marks 8;                                                                                   
          input name marks;                                                                                          
    cards4;                                                                                                          
    Rao 50                                                                                                           
    Tom 60                                                                                                           
    Matt 90                                                                                                          
    ;;;;                                                                                                             
    run;quit;                                                                                                        
                                                                                                                     
    /*                                                                                                               
    Up to 40 obs WORK.HAVE total obs=3                                                                               
                                                                                                                     
    Obs    NAME    MARKS                                                                                             
                                                                                                                     
     1     Rao       50                                                                                              
     2     Tom       60                                                                                              
     3     Matt      90                                                                                              
    */                                                                                                               
                                                                                                                     
    *            _               _                                                                                   
      ___  _   _| |_ _ __  _   _| |_                                                                                 
     / _ \| | | | __| '_ \| | | | __|                                                                                
    | (_) | |_| | |_| |_) | |_| | |_                                                                                 
     \___/ \__,_|\__| .__/ \__,_|\__|                                                                                
                    |_|                                                                                              
    ;                                                                                                                
                                                                                                                     
    Up to 40 obs WORK.WANT total obs=1                                                                               
                                                                                                                     
    Obs    MARKS    NAME                                                                                             
                                                                                                                     
     1       90     Matt                                                                                             
                                                                                                                     
                                                                                                                     
    Bart (handles ties)                                                                                              
                                                                                                                     
    Input                                                                                                            
                                                                                                                     
    Rao 50                                                                                                           
    Tom 60                                                                                                           
    Matt 90                                                                                                          
    Sarah 50                                                                                                         
    Jenny 90                                                                                                         
                                                                                                                     
    Up to 40 obs from TEST total obs=2                                                                               
                                                                                                                     
    Obs    SCR    CUROBS                                                                                             
                                                                                                                     
     1      90       3   (ob number)                                                                                 
     2      90       5                                                                                               
                                                                                                                     
                                                                                                                     
    *                                                                                                                
     _ __  _ __ ___   ___ ___  ___ ___  ___  ___                                                                     
    | '_ \| '__/ _ \ / __/ _ \/ __/ __|/ _ \/ __|                                                                    
    | |_) | | | (_) | (_|  __/\__ \__ \  __/\__ \                                                                    
    | .__/|_|  \___/ \___\___||___/___/\___||___/                                                                    
    |_|                                                                                                              
    ;                                                                                                                
                                                                                                                     
    *_           _       _            _                                                                              
    / |       __| | __ _| |_ __ _ ___| |_ ___ _ __                                                                   
    | |      / _` |/ _` | __/ _` / __| __/ _ \ '_ \                                                                  
    | |_    | (_| | (_| | || (_| \__ \ ||  __/ |_) |                                                                 
    |_(_)    \__,_|\__,_|\__\__,_|___/\__\___| .__/                                                                  
                                             |_|                                                                     
    ;                                                                                                                
                                                                                                                     
    * make data;                                                                                                     
    data have;                                                                                                       
          length name $16 marks 8;                                                                                   
          input name marks;                                                                                          
    cards4;                                                                                                          
    Rao 50                                                                                                           
    Tom 60                                                                                                           
    Matt 90                                                                                                          
    ;;;;                                                                                                             
    run;quit;                                                                                                        
                                                                                                                     
    data want(rename=(top=marks savNam=name));                                                                       
          retain top 0 savNam;                                                                                       
          set have end=dne;                                                                                          
          if marks > top then do; top=marks; savNam=name; end;                                                       
          if dne then output;                                                                                        
          drop name marks;                                                                                           
    run;quit;                                                                                                        
                                                                                                                     
    *____                 _                                                                                          
    |___ \      ___  __ _| |                                                                                         
      __) |    / __|/ _` | |                                                                                         
     / __/ _   \__ \ (_| | |                                                                                         
    |_____(_)  |___/\__, |_|                                                                                         
                       |_|                                                                                           
    ;                                                                                                                
                                                                                                                     
    proc sql;                                                                                                        
      reset outobs=1;                                                                                                
      create                                                                                                         
         table want as                                                                                               
      select                                                                                                         
         *                                                                                                           
      from                                                                                                           
         have                                                                                                        
      order                                                                                                          
         by descending marks                                                                                         
    ;quit;                                                                                                           
                                                                                                                     
                                                                                                                     
    *_____   _               _           _                 _     _                                                   
    |___ /  | |__   __ _ ___| |__     __| | ___  ___ _   _| |__ | |                                                  
      |_ \  | '_ \ / _` / __| '_ \   / _` |/ _ \/ __| | | | '_ \| |                                                  
     ___) | | | | | (_| \__ \ | | | | (_| | (_) \__ \ |_| | |_) | |                                                  
    |____/  |_| |_|\__,_|___/_| |_|  \__,_|\___/|___/\__,_|_.__/|_|                                                  
                                                                                                                     
    ;                                                                                                                
                                                                                                                     
    data want;                                                                                                       
                                                                                                                     
         set have wanthash(in=res obs=1) open=defer;                                                                 
                                                                                                                     
         if _n_=1 then do;                                                                                           
           rc=dosubl('                                                                                               
             data _null_;                                                                                            
                if 0 then set hav;                                                                                   
                declare hash sortha(dataset: "hav", ordered:"a", multidata:"y");                                     
                sortha.definekey ("name" );                                                                          
                sortha.defineData(all:"yes");                                                                        
                sortha.definedone();                                                                                 
                sortha.output(dataset:"wanthash");                                                                   
                stop;                                                                                                
             run;quit;                                                                                               
           ');                                                                                                       
         end;                                                                                                        
                                                                                                                     
         if res then output;                                                                                         
         drop ec;                                                                                                    
                                                                                                                     
    run;quit;                                                                                                        
                                                                                                                     
    *_  _      ____             _                                                                                    
    | || |    | __ )  __ _ _ __| |_                                                                                  
    | || |_   |  _ \ / _` | '__| __|                                                                                 
    |__   _|  | |_) | (_| | |  | |_                                                                                  
       |_|(_) |____/ \__,_|_|   \__|                                                                                 
                                                                                                                     
    ;                                                                                                                
                                                                                                                     
    Roger,                                                                                                           
                                                                                                                     
    one more code if I may,                                                                                          
                                                                                                                     
    none of below examples handles ties (so small extension), and in case there                                      
    are any(a lot of) other variables in the dataset :-)                                                             
    in fact it is based on the idea of dynamic array (emulated by a hash table)                                      
                                                                                                                     
    Bart                                                                                                             
                                                                                                                     
    /**/                                                                                                             
                                                                                                                     
    data have;                                                                                                       
      length name $16 marks 8;                                                                                       
      input name marks;                                                                                              
      array sentinels[42] (42*17);                                                                                   
    cards4;                                                                                                          
    Rao 50                                                                                                           
    Tom 60                                                                                                           
    Matt 90                                                                                                          
    Sarah 50                                                                                                         
    Jenny 90                                                                                                         
    ;;;;                                                                                                             
    run;                                                                                                             
                                                                                                                     
                                                                                                                     
    data want;                                                                                                       
      length scr curobs 8;                                                                                           
      declare hash SCORE(multidata:"Y");                                                                             
      SCORE.defineKey("scr");                                                                                        
      SCORE.defineData("scr","curobs");                                                                              
      SCORE.defineDone();                                                                                            
      declare hiter iSCORE("SCORE");                                                                                 
                                                                                                                     
      do until(EOF);                                                                                                 
        call missing (of _all_);                                                                                     
        set have(keep = marks) end = EOF curobs=co;                                                                  
                                                                                                                     
        _rc_ = iSCORE.first();                                                                                       
        _rc_ = iSCORE.prev();                                                                                        
                                                                                                                     
        if marks >= scr then                                                                                         
          do;                                                                                                        
            if marks > scr then _rc_ = SCORE.clear();                                                                
            _rc_ = SCORE.add(key:marks, data:marks, data:co);                                                        
          end;                                                                                                       
      end;                                                                                                           
      _rc_ = SCORE.output(dataset:"test");                                                                           
                                                                                                                     
       _rc_ = iSCORE.first();                                                                                        
       do while(SCORE.do_over() = 0);                                                                                
        set have point = curobs;                                                                                     
        output;                                                                                                      
       end;                                                                                                          
                                                                                                                     
      stop;                                                                                                          
      drop scr curobs _rc_;                                                                                          
    run;                                                                                                             
                                                                                                                     
    /**/                                                                                                             
                                                                                                                     
                                                                                                                     
                                                                                                                     
                                                                                                                     
