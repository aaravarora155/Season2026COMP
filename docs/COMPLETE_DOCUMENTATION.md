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


