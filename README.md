# Memory-Usage-Check
# It will check whether the memory usage exceeds your set critical/warning threshold; and notify through email if critical level is reach

#!/bin/bash

  while getopts c:w:e flag
  do
    case ${flag} in
      c) 
      critical=${OPTARG};;
      w) 
      warning=${OPTARG}
      if (( $(( warning )) >= $(( critical)) ))
      then
      echo "Error: Critical threshold should be greater than warning threshold."
      fi
      exit 1;;
      e) 
      email=${OPTARG};;  
      :)
      echo "Error: -$OPTARG requires an argument"
      exit 1;;
      esac
   done
   
   ramusage=$(free | awk '/Mem/{printf("Ram Usage: %.0f\n"), $3/$2*100}'|awk '{print $3}')
   
   if (( $ramusage >= $(( critical )) ))
   then
    subj="$(date + "%Y%m%d %HH:%MM") Critical"
    mail -s "$subj" "$email" < /dev/null
    echo "2"
    exit 2
    
    elif (( $ramusage >= $(( warning )) ))
    then
    echo "1"
    exit 1
    
    else
    exit 0
    
    fi
   
   



