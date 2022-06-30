### Integrated Gardening Solutions!!!
{% highlight python %}
import argparse
import os
import logging
import pingo
from pyfirmata import Arduino

os.chdir("..")  # change working directory to parent
workingdir = os.getcwd()  # current working directory

#######################################################################################################

# logging file setup
log_format = "%(asctime)s %(processName)s %(filename)s\n\t%(levelname)-5s %(message)s"
log_datefmt = "%m-%d %H:%M:%S"  # 06-20 12:34:26 - month-day hour:minute:second
logging.basicConfig(
    filename="igs.log", format=log_format, datefmt=log_datefmt, level=logging.DEBUG
)

#######################################################################################################

# command line argument parsing setup
argparser = argparse.ArgumentParser()
# first argument
argparser.add_argument(
    "target", type=str, help="Args: [fan, upper, lower, 1, 2, 3, 4, 5]"
)
# second argument
argparser.add_argument("state", type=int, help="Args: [0, 1]")
args = argparser.parse_args()

#######################################################################################################

# setup pin control
rpiboard = pingo.rpi.RaspberryPi
logging.info("Connected to RPI")

inoboard = Arduino("/dev/ttyACM0")
logging.info("Connected to arduino")

switches = [rpiboard.pins[i] for i in [37, 40, 38]]
pumps = [inoboard.digital[i] for i in [6, 7, 8, 9, 10]]

for switch in switches:
    pin.mode = pingo.OUT

#######################################################################################################

target = args.target  # desired target device - the first arg
state = args.State  # the desired state of the device - the second arg

logging.info(f"Target: {target} State: {state}")

#######################################################################################################


class Switches:
    def __init__(self, switch):
        if state == 1:
            switches[switch].lo
        elif state == 0:
            switches[switch].hi
        else:
            logging.error(f"Invalid state: {state}")


class Pumps:
    def __init__(self, pump):
        if state == 1:
            pumps[pump].write(0)
        elif state == 0:
            pumps[pump].write(1)
        else:
            logging.error(f"Invalid state: {state}")

try:
    int(target)
    Pumps(target)
except:
    if target == "fan":
        Switches(0)
    elif target == "upper":
        Switches(1)
    elif target == "lower":
        Switches(2)
    else:
        logging.error(f"Invalid target: {target}")
{% endhighlight %}
