# FTC Freight Frenzy

Robot control software for the [FIRST Tech Challenge](https://www.firstinspires.org/robotics/ftc) **Freight Frenzy** season (2021–2022). This repository contains the autonomous and teleop code for a mecanum-drive robot with vision-based element detection, carousel control, and a multi-stage scoring mechanism.

## Overview

This project is built on the FTC SDK and extends it with [Road Runner](https://github.com/acmerobotics/road-runner) for odometry and trajectory following, [EasyOpenCV](https://github.com/OpenFTC/EasyOpenCV) for camera-based detection, and [FTC Dashboard](https://github.com/acmerobotics/ftc-dashboard) for live telemetry and parameter tuning.

The codebase supports:

- **Autonomous routines** for both alliance colors and starting positions (near and far from the alliance wall)
- **Mecanum drive** with IMU-assisted localization and spline-based path following
- **Computer vision** pipelines for identifying game elements on the field
- **Mechanism control** for intake, carousel, cascade lift, and rack-and-pinion scoring systems
- **Drive tuning tools** for calibrating feedforward, PID, tracking wheels, and velocity limits

## Tech Stack

| Component | Version |
|-----------|---------|
| FTC SDK | 6.2.x |
| Road Runner | 0.5.3 |
| EasyOpenCV | 1.5.0 |
| FTC Dashboard | 0.4.1 |
| Android Gradle Plugin | 4.0.1 |
| Compile SDK | 29 |

## Project Structure

```
ftc_freightfrenzy/
├── TeamCode/                          # Team-specific robot code
│   └── src/main/java/.../teamcode/
│       ├── auto_blueint.java          # Autonomous — blue alliance, near start
│       ├── auto_bluext.java           # Autonomous — blue alliance, far start
│       ├── auto_redint.java           # Autonomous — red alliance, near start
│       ├── auto_redext.java           # Autonomous — red alliance, far start
│       ├── newauto.java               # Experimental autonomous routine
│       ├── telemetry.java             # Telemetry debug teleop
│       ├── drive/
│       │   ├── SampleMecanumDrive.java    # Road Runner mecanum drive base
│       │   ├── DriveConstants.java       # Motor and kinematic constants
│       │   ├── advanced/                  # Advanced teleop and auto features
│       │   └── opmode/                    # Drive calibration and test op modes
│       └── util/                          # IMU, logging, and helper utilities
├── FtcRobotController/                # FTC SDK robot controller module
├── libs/                              # Local AAR dependencies
└── doc/                               # FTC SDK documentation and media
```

## Prerequisites

- [Android Studio](https://developer.android.com/studio) (Arctic Fox or compatible with Gradle 4.0.x)
- A **Robot Controller** Android device (Control Hub or phone)
- A **Driver Station** Android device
- USB connection or wireless ADB for deployment

## Getting Started

### 1. Clone the repository

```bash
git clone https://github.com/catalin21java/ftc_freightfrenzy.git
cd ftc_freightfrenzy
```

### 2. Open in Android Studio

1. Select **Open an Existing Project** and choose the repository root.
2. Allow Gradle to sync and download dependencies.
3. Select the **TeamCode** run configuration.

### 3. Deploy to the Robot Controller

1. Connect the Robot Controller device via USB (or configure wireless debugging).
2. Click **Run** to build and install the app.
3. On the Driver Station, select the desired OpMode from the list and press **Init**, then **Start**.

## OpModes

### Autonomous

| OpMode | Description |
|--------|-------------|
| `AUTONOMOUS_blueint` | Blue alliance, near-side starting position |
| `AUTONOMOUS_bluext` | Blue alliance, far-side starting position |
| `AUTONOMOUS_redint` | Red alliance, near-side starting position |
| `AUTONOMOUS_redext` | Red alliance, far-side starting position |
| `NewAuto` | Experimental autonomous routine |
| `AutonomusGood` | Alternative autonomous implementation |

### Teleop & Debug

| OpMode | Description |
|--------|-------------|
| `TELEMETRY` | Live telemetry readout for hardware debugging |
| `Test_drive` | Basic drive testing |
| Advanced teleop modes | Field-centric driving, localization, and augmented trajectory control (in `drive/advanced/`) |

### Drive Tuning

Road Runner tuning op modes are located in `TeamCode/.../drive/opmode/` and include:

- Feedforward tuning (manual and automatic)
- Velocity and follower PID tuning
- Tracking wheel offset and lateral distance tuning
- Track width, max velocity, and max angular velocity tuning
- Straight, strafe, turn, and spline trajectory tests

## Hardware Configuration

Configure the following device names in the Robot Controller configuration to match the code:

| Device Name | Type | Purpose |
|-------------|------|---------|
| `Webcam 1` | Webcam | Vision processing |
| `cremaliera` | DcMotorEx | Rack-and-pinion mechanism |
| `cascade` | DcMotorEx | Cascade lift |
| `intake` | DcMotorEx | Intake roller |
| `carusel` | DcMotor | Carousel spinner |
| `ruleta`, `ruleta_x`, `ruleta_z` | CRServo | Scoring arm servos |
| Mecanum motors | DcMotorEx | Drive base (configured in `SampleMecanumDrive`) |
| IMU | BNO055 | Heading and localization |

Drive constants (wheel radius, track width, gear ratio, PID coefficients) are defined in `DriveConstants.java` and can be adjusted at runtime via FTC Dashboard at `https://192.168.49.1:8080/dash` when connected to the robot's WiFi network.

## Development

### Tuning drive parameters

1. Connect to the robot's WiFi and open FTC Dashboard.
2. Run the appropriate tuning OpMode from the `drive` group.
3. Save tuned values back to `DriveConstants.java` — dashboard changes do not persist across app restarts.

### Vision pipelines

- `DetectionPipeline` — grid-based luminosity detection for identifying game elements
- `SamplePipeline` — base OpenCV pipeline used in autonomous routines

### Building from the command line

```bash
./gradlew :TeamCode:assembleDebug
```

The output APK will be located in `TeamCode/build/outputs/apk/`.

## Acknowledgments

- [FIRST Tech Challenge](https://www.firstinspires.org/robotics/ftc) for the competition platform and SDK
- [Road Runner](https://github.com/acmerobotics/road-runner) by Acme Robotics for trajectory following
- [EasyOpenCV](https://github.com/OpenFTC/EasyOpenCV) by OpenFTC for camera integration
- Upstream repository: [MihutMatei/FreightFrenzy](https://github.com/MihutMatei/FreightFrenzy)

## License

This project includes components from the FTC SDK, which is subject to its own license terms. See the `doc/legal/` directory for third-party license information.
