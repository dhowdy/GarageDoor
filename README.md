
Web interface for garage door opener on a Pi.

You'll need to create some files:

/var/www/door.sh:

    #!/bin/bash 
    echo "1" > /sys/class/gpio/gpio17/value && sleep .5 && echo "0" > /sys/class/gpio/gpio17/value



/var/www/status.sh:

    #!/bin/bash 
    if [[ $(cat /sys/class/gpio/gpio24/value) -eq 0 && $(cat /sys/class/gpio/gpio23/value) -eq 1 ]] 
    then 
	    echo opened 
	elif [[ $(cat /sys/class/gpio/gpio24/value) -eq 1 && $(cat sys/class/gpio/gpio23/value) -eq 0 ]] 
	then 
	    echo closed 
	else 
	    echo error 
	fi



/etc/rc.local (add these lines):

    echo "17" > /sys/class/gpio/export 
    echo "23" > /sys/class/gpio/export 
    echo "24" > /sys/class/gpio/export 
    echo "out" > /sys/class/gpio/gpio17/direction 
    echo "in" > /sys/class/gpio/gpio24/direction 
    echo "in" > /sys/class/gpio/gpio23/direction
