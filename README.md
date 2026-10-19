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

### Notes

Although the model is stable in the `scene.xml`, you might see unstable behavior
when combined with other models (i.e. end-effectors). Switching the integrator
from `Euler` to `RK4` helps in these cases.

## Changelog

See [CHANGELOG.md](./CHANGELOG.md) for a full history of changes.

## License

This model is released under a [BSD-3-Clause License](LICENSE).
