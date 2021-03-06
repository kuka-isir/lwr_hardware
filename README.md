Kuka LWR FRI interface
=============================

This package implements two orocos components for communicating with the Kuka LWR through the Fast Research Interface (FRI). One component is used to read diagnostics messages from the robot, when the onther one serves status/command data.

You can compile it for gnulinux or xenomai+rtnet targets if you want 1Khz hard-realtime communication.

### Usage (ops script)
##### Start LWR FRI component
```bash
import("rtt_ros")
ros.import("lwr_fri")
loadComponent("lwr","lwr_fri::FRIComponent")
setActivity("lwr",0,99,ORO_SCHED_RT)
# Period is zero because the components does a blocking wait on the network = it goes as fast as the FRISTART() 
lwr.configure()
lwr.start()
```
##### Start diagnostics component
```bash
import("rtt_ros")
ros.import("lwr_fri")
loadComponent("diagnostics","FRIDiagnostics")
setActivity("diagnostics",0.02,99,ORO_SCHED_OTHER)
// We suppose lwr is launched already
connect("lwr","diagnostics")
diagnostics.configure()
diagnostics.start()
```
### Ports available

#### Commands
* **CartesianImpedanceCommand** (InputPort, lwr_fri::CartesianImpedance)
* **CartesianWrenchCommand** (InputPort, geometry_msgs::Wrench)
* **CartesianPositionCommand** (InputPort, geometry_msgs::Pose)
* **JointImpedanceCommand** (InputPort, lwr_fri::FriJointImpedance)
* **JointPositionCommand** (InputPort, Eigen::VectorXd)
* **JointTorqueCommand** (InputPort, Eigen::VectorXd)

#### Status
* **CartesianWrench** (OutputPort, geometry_msgs::Wrench)
* **JointVelocity** (OutputPort, Eigen::VectorXd)
* **CartesianVelocity** (OutputPort, geometry_msgs::Twist)
* **CartesianPosition** (OutputPort, geometry_msgs::Pose)
* **MassMatrix** (OutputPort, Eigen::MatrixXd)
* **Jacobian** (OutputPort, KDL::Jacobian)
* **JointTorque** (OutputPort, Eigen::VectorXd)
* **GravityTorque** (OutputPort, Eigen::VectorXd)
* **JointPosition** (OutputPort, Eigen::VectorXd)
* **JointTorqueRaw** (OutputPort, Eigen::VectorXd)
* **JointPositionFRIOffset** (OutputPort, Eigen::VectorXd)

#### Others
* **toKRL** (InputPort, tFriKrlData)
* **fromKRL** (OutputPort, tFriKrlData)
* **RobotState** (OutputPort, tFriRobotState)
* **FRIState** (OutputPort, tFriIntfState)


### Use it in other packages

Add in you package.xml : 

```xml
<build_depend>kuka_lwr_fri</build_depend>
<build_depend>lwr_fri</build_depend>
<run_depend>lwr_fri</run_depend>
```

