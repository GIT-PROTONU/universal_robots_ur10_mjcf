# Universal Robots UR10 Description (MJCF)

A high-fidelity, physically accurate MuJoCo XML (MJCF) model of the
[Universal Robots UR10](https://www.universal-robots.com/products/ur10-robot/),
designed for robotics research, reinforcement learning, and control simulation.

> [!IMPORTANT]
> Requires MuJoCo 2.3.3 or later.

<p float="left">
  <img src="ur10.png" width="600">
</p>

## Overview

This package contains a simplified robot description (MJCF) of the UR10. It is
derived from the [publicly available URDF
description](https://github.com/ros-industrial/universal_robot/tree/kinetic-devel/ur_description).

## Repository contents

| File | Description |
| --- | --- |
| `ur10.xml` | The robot model: bodies, joints, actuators, materials and a `home` keyframe. |
| `scene.xml` | Includes `ur10.xml` and adds a textured ground plane, skybox and haze. |
| `assets/` | OBJ meshes referenced by the model. |
| `ur10.png` | Render preview of the model. |
| `CHANGELOG.md` | Full history of changes. |
| `LICENSE` | BSD-3-Clause license. |

## Usage

Load the bare robot model:

```python
import mujoco

model = mujoco.MjModel.from_xml_path("ur10.xml")
data = mujoco.MjData(model)
```

Load the robot with the ground plane, skybox and lights:

```python
import mujoco

model = mujoco.MjModel.from_xml_path("scene.xml")
data = mujoco.MjData(model)
```

Or launch the interactive viewer:

```bash
python -m mujoco.viewer --mjcf=scene.xml
```

### URDF → MJCF derivation steps

1. Converted the DAE [mesh
   files](https://github.com/ros-industrial/universal_robot/tree/kinetic-devel/ur_description/meshes)
   to OBJ format using [Blender](https://www.blender.org/).
   - In this process, the Z axis of the joint that a link is connected to should point up.
2. Processed `.obj` files with [`obj2mjcf`](https://github.com/kevinzakka/obj2mjcf).
3. Added `<mujoco> <compiler discardvisual="false"/> </mujoco>` to the URDF's
   `<robot>` clause in order to preserve visual geometries.
4. Loaded the URDF into MuJoCo and saved a corresponding MJCF.
5. Added a tracking light to the base.
6. Manually edited the MJCF to extract common properties into the `<default>` section.
7. Manually edited colors to match UR10 colors.
8. Added position-controlled actuators and joint damping and armature. Note
   that these values have not been carefully tuned -- contributions are more
   than welcome to improve them.
9. Added home joint configuration as a `keyframe`.
10. Manually designed collision geometries to be as realistic as the UR10's shape.
11. Added `scene.xml`, which includes the robot, with a textured ground plane, skybox, and haze.

### Notes

Although the model is stable in the `scene.xml`, you might see unstable behavior
when combined with other models (i.e. end-effectors). Switching the integrator
from `Euler` to `RK4` helps in these cases.

## Changelog

See [CHANGELOG.md](./CHANGELOG.md) for a full history of changes.

## License

This model is released under a [BSD-3-Clause License](LICENSE).
