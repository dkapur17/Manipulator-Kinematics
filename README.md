# Manipulator-Kinematics
Notebook Simulations for Forward and Inverse Kinematics for an N-R Manipulator

This project demonstrates the Forward and Inverse Kinematics of Planar Manipulators with `n` Revolute Joints. The simulation uses `matplotlib` and `ipynb_widgets`
to make interactive plots of the arms.

### `fk.ipynb`

This notebook simulates the Forward Kinematics of the manipulator. The simulation cell has one slider each for each joint, to control its joint angle.
The plot below the sliders updates in real time to reflect the pose of the manipulator.

![FK Image](https://cdn.discordapp.com/attachments/746692909515931681/897433959368105984/FK.gif)

The text on the top right of the plot indicates the end-effector position.

The number of links (and by extension the number of joints) can be changed by updating the code in Cell 3:

```python
# Number of links
N_LINKS = 2 # <==== Change this

# {length, color}
links_config = [{'l': 3, 'c': 'r'}, {'l': 2, 'c':'g'}, {'l':2, 'c':'b'}] # <==== Add a configuration for each link here

assert(len(links_config) >= N_LINKS)

robot = [Segment(0, 0, links_config[0]['l'], 0, links_config[0]['c'])]
for i in range(1, N_LINKS):
    robot.append(Segment.with_parent(robot[i-1], links_config[i]['l'], 0, links_config[i]['c']))

theta_sliders = [widgets.IntSlider(value=0,min=-180,max=180,step=1,description=f'Theta_{i}') for i in range(len(robot))]
theta = {f'theta_{i}':slider for i, slider in enumerate(theta_sliders)}
```

*Note that `links_config` should have at least as many entries as the value of `N_LINKS` as indicated by the assertion.*

### `ik.ipynb`

This notebook simulates the Inverse Kinematics of the manipulator. The simulation cell has two sliders, one for the `x` and one for the `y` coordinates of the 
desired end-effector position.

The plot below the sliders updates in real time to reflect the pose of the manipulator as well as the desired end-effector position indicated by the dot.

![IK Image](https://cdn.discordapp.com/attachments/746692909515931681/897437194921340938/IK.gif)

The text on the top right of the plot are the joint angles.

Again, the number of links (and by extension the number of joints) can be changed by updating the code in Cell 3:

```python
# Number of Links
N_LINKS = 2 # <==== Change this

# {length, color}
links_config = [{'l': 3, 'c': 'r'}, {'l': 2, 'c':'g'}, {'l':2, 'c':'b'}] # <==== Add a configuration for each link here

assert(len(links_config) >= N_LINKS)

robot = [Segment(0, 0, links_config[0]['l'], 0, links_config[0]['c'])]

for i in range(1, N_LINKS):
    robot.append(Segment.with_parent(robot[i-1], links_config[i]['l'], 0, links_config[i]['c']))
    
base = vector.obj(x=0, y=0)
```
*Note that `links_config` should have at least as many entries as the value of `N_LINKS` as indicated by the assertion.*

**This code is completely scalable and can work with several links, but the simulation drastically slows down when the number of links is increase. This is because of
how much `matplotlib` has to draw for each iteration. To improve performance, some of the aesthetic components can be removed from the plots such as gridlines and
legends.**


