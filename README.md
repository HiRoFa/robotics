# robotics
robotics project for use in ESses

# example

```javascript

async function init_robot() {
    // import robotics module
    let robotics = await import('https://raw.githubusercontent.com/HiRoFa/robotics/main/robot.mes');
    
    // init steppers and servos
    let baseStepper = await robotics.Stepper.init('/dev/gpiochip0', [1, 2, 3, 4]);
    let lowerArmStepper = await robotics.Stepper.init('/dev/gpiochip0', [5, 6, 7, 8]);
    let upperArmStepper = await robotics.Stepper.init('/dev/gpiochip0', [9, 10, 11, 12]);
    let gripperServo = await robotics.Servo.init('/dev/gpiochip0', [13, 14, 15, 16]);
    
    // create axis with gearReductions
    let baseAxis = new robotics.ServoAxis(baseStepper, 2);
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

init_robot().then((robot) => {
    robot.zero()
    .then(() => {
        console.log("robot initialized an zeroed");
    });
    
});



```