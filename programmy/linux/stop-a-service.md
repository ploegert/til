# Stop a Service



To Stop a service, there are multiple ways of checking to see the service state if you need to execute:



Reference: [https://stackoverflow.com/questions/48431372/removing-broken-packages-in-ubuntu](https://stackoverflow.com/questions/48431372/removing-broken-packages-in-ubuntu)

```
function stop_service() {
    FUNC="stop_service"

    log_banner "Stop & clean [$1]..." $FUNC

    if [ $# -gt 2 ] || [ $# -lt 1 ]; then
        echo "Incorrect parameters passed" # If parameters no equal 2
    fi

    # Get Service State
    log_info "Executing: ==> systemctl show -p SubState --value $1"  $FUNC
    #ps -ef | grep $1
    #systemctl is-active --quiet $1 && echo Service is running
    #systemctl show -p SubState --value $1     # returns dead/running/notrunning
    #systemctl show -p ActiveState --value $1  # returns active/inactive
    
    SERVICE_STATE=$(systemctl show -p SubState --value $1)
    log_info "Service [$1] is $SERVICE_STATE"  $FUNC
    
    #log_info "Searching for [$1] Service..."  $FUNC
    #if (( $(ps -ef | grep -v grep | grep $1 | wc -l) > 0 ))
    if [ $SERVICE_STATE = "running" ];
    then
      
      log_info "[$1] is running - we'll need to stop that..." $FUNC
      sudo systemctl stop $1
      log_success "[$1] is stopped!!!" $FUNC

      log_info "Executing: ==> sudo systemctl $2 $1"  $FUNC
      sudo systemctl $2 $1
    else
      log_info "no action required as service [$1] is not running or not found." $FUNC
    fi 
}




```
