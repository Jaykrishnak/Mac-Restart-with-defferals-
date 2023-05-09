#!/bin/bash

# Set the number of deferrals allowed
max_deferrals=3
deferral_count=0

# Set the time between deferrals (in seconds)
deferral_time=3600 # 1 hour

# Set the restart message
restart_message="Your Mac will restart in 10 minutes. Please save any unsaved work."

# Set the restart command
restart_command="sudo shutdown -r now"

# Display the restart message and give the user the option to defer
while true; do
    echo "$restart_message"
    read -p "Do you want to restart now? (y/n) " choice
    case $choice in
        [Yy]* ) $restart_command; exit;;
        [Nn]* ) break;;
        * ) echo "Please answer yes or no.";;
    esac

    # Increment the deferral count
    deferral_count=$((deferral_count+1))

    # Check if the user has exceeded the maximum number of deferrals
    if [ $deferral_count -ge $max_deferrals ]; then
        echo "You have exceeded the maximum number of deferrals. Your Mac will restart in 10 minutes."
        sleep 600 # Wait 10 minutes before restarting
        $restart_command
        exit
    fi

    # Display the deferral message and wait for the deferral time
    echo "You have chosen to defer the restart. Your Mac will restart in $((deferral_time/60)) minutes."
    sleep $deferral_time
done

exit
