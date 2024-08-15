# Robotic-Arm
Using Multi Axis Joystick
#include <stdio.h>
#include <stdlib.h>

// Define limits for each joint
#define BASE_MIN_ANGLE 0
#define BASE_MAX_ANGLE 180
#define SHOULDER_MIN_ANGLE 0
#define SHOULDER_MAX_ANGLE 180
#define ELBOW_MIN_ANGLE 0
#define ELBOW_MAX_ANGLE 180

// Structure to store the current position of the robotic arm
typedef struct {
    int baseAngle;
    int shoulderAngle;
    int elbowAngle;
} RoboticArm;

// Function prototypes
void initializeArm(RoboticArm *arm);
void updateArmPosition(RoboticArm *arm, int xAxis, int yAxis, int zAxis);
void displayArmPosition(const RoboticArm *arm);
int clamp(int value, int min, int max);

// Main function
int main() {
    RoboticArm arm;
    int xAxis, yAxis, zAxis;
    
    // Initialize the robotic arm to default position
    initializeArm(&arm);
    
    while (1) {
        // Simulate joystick input (Replace with actual joystick input in a real application)
        printf("Enter joystick values (X, Y, Z): ");
        scanf("%d %d %d", &xAxis, &yAxis, &zAxis);
        
        // Update the robotic arm position based on joystick input
        updateArmPosition(&arm, xAxis, yAxis, zAxis);
        
        // Display the updated position of the robotic arm
        displayArmPosition(&arm);
    }
    
    return 0;
}

// Function to initialize the robotic arm to a default position
void initializeArm(RoboticArm *arm) {
    arm->baseAngle = 90;      // Middle position
    arm->shoulderAngle = 90;  // Middle position
    arm->elbowAngle = 90;     // Middle position
}

// Function to update the robotic arm position based on joystick input
void updateArmPosition(RoboticArm *arm, int xAxis, int yAxis, int zAxis) {
    // Update base angle using X-axis
    arm->baseAngle += xAxis;
    arm->baseAngle = clamp(arm->baseAngle, BASE_MIN_ANGLE, BASE_MAX_ANGLE);
    
    // Update shoulder angle using Y-axis
    arm->shoulderAngle += yAxis;
    arm->shoulderAngle = clamp(arm->shoulderAngle, SHOULDER_MIN_ANGLE, SHOULDER_MAX_ANGLE);
    
    // Update elbow angle using Z-axis
    arm->elbowAngle += zAxis;
    arm->elbowAngle = clamp(arm->elbowAngle, ELBOW_MIN_ANGLE, ELBOW_MAX_ANGLE);
}

// Function to display the current position of the robotic arm
void displayArmPosition(const RoboticArm *arm) {
    printf("Robotic Arm Position:\n");
    printf("Base Angle: %d degrees\n", arm->baseAngle);
    printf("Shoulder Angle: %d degrees\n", arm->shoulderAngle);
    printf("Elbow Angle: %d degrees\n", arm->elbowAngle);
    printf("-------------------------------\n");
}

// Function to clamp a value between a minimum and maximum range
int clamp(int value, int min, int max) {
    if (value < min) {
        return min;
    } else if (value > max) {
        return max;
    } else {
        return value;
    }
}
