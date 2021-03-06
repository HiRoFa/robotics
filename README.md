# robotics
robotics project for use in GreenCopper

# example

```javascript
import {Utils as utils} from 'https://raw.githubusercontent.com/HiRoFa/GreenCopperRuntime/main/modules/utils/utils.mes';

async function initRobot() {
    // import robotics module
    let robotics = await import('https://raw.githubusercontent.com/HiRoFa/robotics/main/robot.mes');


    // init steppers and servos
    let [baseStepper, lowerArmStepper, upperArmStepper] = await utils.awaitAll(
        robotics.Stepper.init('/dev/gpiochip0', [23, 24, 25, 4]),
        robotics.Stepper.init('/dev/gpiochip0', [17, 18, 27, 22]),
        robotics.Stepper.init('/dev/gpiochip0', [13, 12, 6, 5])
    );

    let servoDriver = new servoMod.SoftPwmDriver("/dev/gpiochip0", 20, servoMod.MG90SServo);
    let gripperServo = new servoMod.Servo(servoDriver);
    await gripperServo.init();

    // create axis with gearReductions
    let baseAxis = new robotics.StepperAxis(baseStepper, 2);
    let lowerArmAxis = new robotics.StepperAxis(lowerArmStepper, 3);
    let upperArmAxis = new robotics.StepperAxis(upperArmStepper, 3);
    let gripperAxis = new robotics.ServoAxis(gripperServo);

    // create arms with lengths
    let lowerArm = new robotics.Arm(lowerArmAxis, 80);
    let upperArm = new robotics.Arm(upperArmAxis, 80);
    let gripper = new robotics.VGripper(gripperAxis, 40);

    // create robot, with base axis height
    let robot = new robotics.FourAxisRobot(baseAxis, lowerArm, upperArm, gripper, 60);

    return robot;

}

initRobot().then(async (robot) => {
    await robot.zero();
    console.log("robot initialized an zeroed");
    console.log("setting baseAngle to 90");
    await robot.setBaseAngle(90);
    console.log("setting baseAngle to 45");
    await robot.setBaseAngle(45);
    console.log("setting baseAngle to 135");
    await robot.setBaseAngle(135);
    console.log("zeroing");
    await robot.zero();
    console.log("robot zeroed");
});

```
