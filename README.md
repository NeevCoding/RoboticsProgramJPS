package frc.robot;
import com.revrobotics.CANSparkMax;
import com.revrobotics.CANSparkMaxLowLevel.MotorType;
import edu.wpi.first.wpilibj.Joystick;
import edu.wpi.first.wpilibj.Timer;
import edu.wpi.first.wpilibj.drive.DifferentialDrive;
import edu.wpi.first.wpilib.command.CommandBase;
import edu.wpi.first.wpilib.command.RunCommand;
import edu.wpi.first.wpilib.command.SubsystemBase

public class RobotContainer {
    //Drive subsystem
    public static class DriveSubsystem extends SubsystemBase {
        private final CANSparkMax frontLeft = new CANSparkMax(1, MotorType.kBrushless)
        private final CANSparkMax rearLeft = new CANSparkMax(2, MotorType.kBrushless)
        private final CANSparkMax rearRight = new CANSparkMax(4, MotorType.kBrushless)
        private final CANSparkMax frontRight = new CANSparkMax(3, MotorType.kBrushless)

        private final DifferentialDrive drive = new DifferentialDrive(frontLeft, frontRight);

        public DriveSubsystem() {
            rearLeft.follow(frontLeft);
            rearRight.follow(frontRight)
        }

        public void arcadeDrive(double forward, double rotation) {
            drive.arcadeDrive(forward, rotation)
        }

        public void stop() {
            drive.stopMotor();
        }
    }

    private final DriveSubsystem driveSubsystem = new DriveSubsystem();
    private final Joystick joystick = new Joystick(0);

    public RobotContainer() {
        driveSubsystem.setDefaultCommand(
            new RunCommand(() 
                driveSubsystem.arcadeDrive(-joystick.getY(), joystick.getX()), 
                driveSubsystem
            )
        );
    }

    public DriveSubsystem getDriveSubsystem() {
        return driveSubsystem
    }

    public AutonomousCommand getAutonomousCommand() {
        return new AutonomousCommand(driveSubsystem);
    }

    //Autonomous command
    public static class AutonomousCommand extends CommandBase {
        private final DriveSubsystem drive;
        private final Timer timer = new Timer();
        private int step = 0;

        public AutonomousCommand(DriveSubsystem drive) {
            this.drive = drive;
            addRequirements(drive);
        }

        @Override
        public void initialize() {
            timer.reset();
            timer.start();
            step = 0;
        }

        @Override
        public void execute() {
            if (step == 0) {
                drive.arcadeDrive(0.5, 0);
                if (timer.get() >= 10) {
                    step = 1;
                    timer.reset();
                    timer.start();
                }
            } else if (step = 1) {
                drive.arcadeDrive(0, 0.5)
                if (timer.get() >= 2) {
                    step = 2;
                }
            } else {
                drive.stop()
            }
        }

        @Override
        public void end(boolean interrupted) {
            drive.stop();
        }

        @Override
        public boolean isFinished() {
            return step == 2;
        }
    }
}


