# How to run Multiple tasks in ESP32 Arduino Core
Here is a code snippet explaining how to use ESP32 Adruino Core to run multiple FreeRTOS Tasks.   
Using this, you can avoid the normal use of `millis()` to run multiple parellel routines.

```c++
/* 
    Code snippet below shows how to run 3 different FreeRTOS Tasks with different priority together
    This avoids the burden of manually handling the timing by using millis() function.
    loop() can still run routine which will be run under main Arduino Task
*/


void task1(void* pvParameters) {  // Define the tasks to be executed
    while (1) {  // Keep the task running.  
        // Code to run in this task
        Serial.print("Task1 Uptime (ms): ");
        Serial.println(millis());  // The running time of the serial port
        delay(
            100);  // With a delay of 100ms, it can be seen in the serial
                   // monitor that every 100ms, task 1 will be executed once.
    }
}

void task2(void* pvParameters) {
    while (1) {
        // Code to run in this task
        Serial.print("Task2 Uptime (ms): ");
        Serial.println(millis());
        delay(200);
    }
}

void task3(void* pvParameters) {
    while (1) {
        // Code to run in this task
        Serial.print("Task3 Uptime (ms): ");
        Serial.println(millis());
        delay(1000);
    }
}

void setup() {

    // Any initializing code for your requirements goes here

    // Creat Task1. 
    xTaskCreatePinnedToCore(
        task1,      // Function to implement the task.
        "task1",    // Task reference name
        4096,       // The size of the task stack 
        NULL,       // Pointer that will be used as the parameter for the task 
        1,          // Priority of the task.  
        NULL,       // Task handler.  
        0);         // Core where the task should run.  

    // Task 2
    xTaskCreatePinnedToCore(task2, "task2", 4096, NULL, 2, NULL, 0);

    // Task 3
    xTaskCreatePinnedToCore(task3, "task3", 4096, NULL, 3, NULL, 0);
}

void loop() {

    // You can add code here which will run on the main Arduino Task

}

```
