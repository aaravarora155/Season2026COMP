# Season 2026 Documentation
This is the documentation for the Griffins1884 complete Season 2026 Library. It consists of System Architecture and Key Checkpoints for debugging.

## Constants Architecture

**Declaring Constants:** `private/public static final type name`

There are 4 Levels of Constants:

1. ***System-Wide:*** *(ex. GlobalConstants.java, BuildConstants.java, Config.java, CanIDConstants.java)*
2. ***Multi-Subsystem-Wide:*** *(ex. SuperstructureConstants.java)*
3. ***Subsystem-Wide:*** *(ex. IntakePivotConstants.java, ShooterPivotConstants.java, SwerveConstants.java, TurretConstants.java, LEDConstants.java, IndexerConstants.java, SpindexerConstants.java, etc.)*
4. ***Class-Wide:*** *These are constants declared only for use within a class. They are usually private and declared at the top of the class according to **Java** coding standards*

## Subsystems Architecture
***NOTE:*** CAN IDs are declared in `CanIDConstants.java`

### Subsytem List:
- Shooter
    - Consists of shooter & shooter pivot related classes
- Turret
    - Consists of turret related classes
- Indexer
    - Consists of indexer & spindexer related classes
- Intake
    - Consists of intake & intake pivot related classes
- LEDs
    - Consists of LED related classes
- Swerve
    - Consists of gyro, module, & swerve related classes
- Vision
    - Consists of april tag, limelight, megatag, game piece, fiducial observation and vision related classes

### General Architecture
Each subsystem ***(excluding LEDs and vision)*** contains at least a generic IO Class, a Kraken IO Class, a Sim IO Class, a constants Class, & a subsystem class.

### System-Specific Architecture

#### Swerve:
- GyroIO
    - NavX & Pigeon 2 gyros are supported by the current version of the library
    - Responsible for gyro functions and definition
- ModuleIO
    - Responsible for defining each swerve module
    - Uses `Module.java` as its 'subsystem' class
- SwerveSubsystem
    - Creates For Module objects that are used together to control the drivetrain of the robot
- SwerveConstants
    - Defines all of the important constants and retrieves CAN IDs from its constants file.
    - This is also where swerve offsets are declared *(check a later section for how to find/calculate for these values)*
- SwerveCalibration
    - Contains all of the Helper Methods used to calibrate swerves
- PhoenixOdometryThread
    - Takes all odometry input and adds them to a linear queue for processing
- SwerveMusicPlayer
    - Non essential class occasionally used for debugging but mostly for having fun!

#### Turret:
- TurretIO
    - Only Kraken  Motors are supported in the current version of the codebase
    - Responsible for creating a turret instance

- TurretConstants
    - Defines all required constants for the turret such as the gear ratio between the motor and the turret as well as CAN ID Constants

- TurretSubsystem
    - Initiates a constructor that creates an instance of the turret with functions that can be used for control over the turret

#### Shooter:
- ShooterIO
    - Only Kraken Motors are supported in the current version of the codebase
    - Responsible for creating a shooter instance
- ShooterPivotIO
    - Kraken, Flex, and Max motors are supported in the current version of the codebase
    - Responsible for creating a shooter pivot instance
- ShooterSubsystem
    - Responsible for controlling shooter mechanism
- ShooterPivotSubsystem
    - Initiates a constructor that creates an instance of the shooter with functions that can be used to control the shooter
- ShooterPivot & Shooter Constants
    - Declares the necessary constants for Shooter & ShooterPivot Subsystems

#### Indexer:
***(Spindexer is likely deprecated)***
- IndexerIO
    - Only Krakens are supported in the current version of the codebase
    - Responsible for creating a indexer instance
- SpindexerIO
    - No Motors are supported. Only Sim
    - Responsible for creating a spindexer instance
- IndexerConstants & SpindexerConstants
    - Contains required constants to control indexer.
- IndexerSubsystem & SpindexerSubsystem
    - Contains code to control indexer and spindexer mechanisms

#### Intake:
- IntakeIO
    - Only Krakens are supported in the current version of the codebase
    - Responsible for creating an intake instance
- IntakePivotIO
    - Krakens, Flex & Max motors are supported in the current version of the codebase
    - Responsible for creating an IntakePivot Instance
- Intake & IntakePivot Subsystem
    - Contains constructuors & methods required to control the Intake and Intake Pivot of the robot
- Intake & IntakePivot Constants
    - Contains necessary constants required for Intake & Intake Pivot to work correctly
___
***(Likely Deprecated)***
- ToothRolloutIO
    - Contains IO Decleration to aid the Intake in intaking game pieces
- ToothRolloutConstants
    - Contains necessary Constants for ToothRollout to work correctly
- ToothRolloutSubsystem
    - Contains necessary constructors and methods required to control ToothRollout

### LEDs
- LEDConstants
    - Contains necessary Constants for LEDs to work correctly
- LEDIO
    - Both PWM (Real LEDs) and SIM LEDs are supported in the current version of the codebase
    - Responsible for creating an LED Instance
- LED Subsystem
    - Contains necessary methods and constructors required to control the LEDs

### Vision
**5 Main Groups**
- AprilTag
    - Helpers
        - Contains a mathematical function that returns a matrix of the standard deviation 
    - Constants
        - Contains Decleration of constants for all limelights that look for AprilTags
    - IO
        - Supports Limelight & Northstar implementations
        - Acts as a subsystem class that uses various functions to validate and calculate PoseEstimates alongside remaining classes in this subsystem
- GamePiece
    - Constants
        - Contains the constants necessary to use a custom trained Limelight 3 to detect the 2026 Rebuilt Game Piece
    - IO
        - Contains code & constructors to help make it possible to detect game pieces
- Limelight
    - Helpers
        - Contains code that can be used to get poses of the robot from different angles and based on different relations to the field
    - ProfileResolver
        - Defines the Limelight Profiles based on if it is an LL3 or LL4
- PoseEstimate
    - Megatag
        - Allows us to get MegatagPoseEstimates from LimelightPoseEstimates
        - There are 2 types of MegaTag: 1 & 2. Megatag 1 uses multiple AprilTags to get a pose estimate whereas Megatag2 uses multiple AprilTags + the gyro (IMU) to get a pose estimate. The other way to get PoseEstimates are LimelightPoseEstimates which use a single AprilTag to estimate Pose
    - VisionField
        - Creates a record which holds PoseEstimate, Timestamp, Confidence Score (StdDev) & the number of tags that were used as an input
- Vision
    - VisionIO
        - Declares core vision reject reasons and the camera types and their appropriate constants
    - VisionTargetProvider
        - Declares Base Framework for the Vision Class
    - Vision
        - Contains Core functionality which allows us to get useful information from the limelights
- FiducialObservation
    - Creates a record of where an april tag appears in the camera's frame, how much space it takes up in the frame, how far away the camera is from the april tag, how trustworthy the actual detection is, and the april tag ID.