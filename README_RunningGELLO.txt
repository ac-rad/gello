---RUNNING TELEOP---

Instructions to run the robots:

0. Make sure the Docker container is launched. You can launch it with 

sudo python3 gello_software/gello_software/scripts/launch.py

1. Put the UR robots in known configurations. The one I currently have is 90, -90, 90, 90, 90, -180, 0 (in radians: 1.57 -1.57 1.57 1.57 1.57 -3.14 0) for both arms. Try to match GELLO to
this positon as much as you can.

2. Run

python scripts/gello_get_offset.py --start-joints 1.57 -1.57 1.57 1.57 1.57 -3.14 --joint-signs 1 1 -1 1 1 1 --port /dev/serial/by-id/usb-FTDI_USB__-__Serial_Converter_FTA7NN89-if00-port0 

for the joint configurations of the left UR arm. If needed, modify the offsets in gello/agents/gello_agent.py

3. Run

python scripts/gello_get_offset.py --start-joints 1.57 -1.57 1.57 1.57 1.57 -3.14 --joint-signs 1 1 -1 1 1 1 --port /dev/serial/by-id/usb-FTDI_USB__-__Serial_Converter_FTA7NNQU-if00-port0 

for the joint configurations of the right UR arm. If needed, modify the offsets in gello/agents/gello_agent.py

4. Run 

python experiments/launch_nodes.py --robot=bimanual_ur 

to run both. Remove bimanual_ur flag to just run the right arm.

5. In another terminal, while running the command above, run (still should be inside the docker container)

python experiments/run_env.py --agent=gello --bimanual

Remove --bimanual flag for only the right arm.

6. To run the same docker container in another terminal, run

docker ps 

docker exec -it <container id from above> bash

7. Everything should work after these steps. If not, the IP's could be an issue, they should match the host and robot in the run_env.py and launch_nodes.py


---SAVING TRAJECTORY DATA---

1. The authors provided using the keyboard to save some trajectories. Sadly they didn't provide any of the internal dependencies, so I wrote my own super-simplified version to process joint angles for each joint. Run:

python experiments/run_env.py --agent=gello --bimanual --use_save_interface

A pop-up window should appear. Once the grey screen pops up, you can save interfaces with 's', use 'c' to continue, and 'q' to quit. The screen turns green while you are saving an trajectory.

2. Run these if there are some missing dependencies:

export XDG_RUNTIME_DIR=/tmp/runtime-$(whoami)
mkdir -p $XDG_RUNTIME_DIR
chmod 700 $XDG_RUNTIME_DIR
export DISPLAY=:1

3. If external screen doesn't pop up, make sure you enabled all external permissions. Run this on host machine:

xhost +

4. Run

cd ~
python /gello/gello/data_utils/demo_to_gdict.py  

This should save plots of your saved trajectories in gello_software/gello_software/processed_data :
Action Values corresponds to joint angle in radians. 
Timestep values are the index of each action in the trajectory.

