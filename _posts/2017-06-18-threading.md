---
ID: 397
post_title: Threading
author: impeccableaslan
post_date: 2017-06-18 15:25:54
post_excerpt: ""
layout: post
permalink: >
  http://rcnode.com/index.php/2017/06/18/threading/
published: true
---
[et_pb_section fb_built="1" admin_label="section" _builder_version="3.0.47"][et_pb_row admin_label="row" _builder_version="3.0.47" background_size="initial" background_position="top_left" background_repeat="repeat"][et_pb_column type="4_4" _builder_version="3.0.47" parallax="off" parallax_method="on"][et_pb_text admin_label="Text" _builder_version="3.0.47" background_size="initial" background_position="top_left" background_repeat="repeat"][/et_pb_text][et_pb_text _builder_version="3.0.51"]<p><span>Imagine </span>a main<span> application (main app) is running on the computer (left side box code).Imagine </span>a main<span> application (main app) is running on the computer (left side box code).[caption width="474" id="attachment_401" align="alignnone"]<img src="http://rcnode.com/wp-content/uploads/2017/06/Threading.png" width="474" height="336" alt="" class="wp-image-401 size-full" /> Image from ardupilot.org[/caption]</span></p>
<p>Say this main app wants to do a complex time consuming or dedicated task. Then it can start (not call as in subroutine) a function which will run in addition to the main app. This newly started function is called thread ( right side box code). Now both thread and main app code is running in time sharing mode on the processor.</p>
<p>In the case of call to a normal function, if the main app calls a function, then only the function will run. Only after return from the called function will the main app resume. This is not so in case of thread.</p>
<h1></h1>
<h1>Single threaded and multi-threaded</h1>
<p>Single thread is where there is only one thread that executes all task. Only one thread will be executed at a time (Depends on the CPU). A multi-thread on the other hand has multiple threads that can execute tasks. So if single thread as tasks: 1,2,3,4,5,6, multi-thread has: Thread1-1,2; Thread2-3,4; and so on. Therefore, multi-thread is better if you have multiple CPUS, since it can divide the threads between the CPUS.</p>
<h1></h1>
<h1>Hardware abstraction library (HAL)</h1>
<p>Ardupilot has their own HAL library, as seen by the AP. A hardware abstraction library is there for the users. It serves as a wrapper around the API (Application programming interface) to enhance the code's readibility and the user's experience. It should not contain any "private" codes or background thread. It should not contain the "how".</p>
<h1></h1>
<h1>The Timer Callbacks</h1>
<p>The code has a default timer function which is called at 1KHz (A very short amount of time as T=1/f). All registered timer functions are called sequentially. So each will be called after the previous one is completed. This mechanism is used as it is very portable, and very useful. A timer callback can be registered by calling the scheduler, so hal.scheduler-&gt;register_timer_process() as so: hal.scheduler-&gt;register_timer_process(AP_HAL_MEMBERPROC(&amp;AP_Baro_MS5611::_update));</p>
<p>The AP_HAL_MEMBERPROC() macro provides a way to encapsulate a C++ member function as a callback argument (bundling up the object context with the function pointer). So uses a function as an argument to a callback, much like a parameter.</p>
<p>When a piece of code wants something to happen at less than 1kHz then it should maintain it&rsquo;s own &ldquo;last_called&rdquo; variable and return immediately if not enough time has passed. You can use the hal.scheduler-&gt;millis() and hal.scheduler-&gt;micros() functions to get the time since boot in milliseconds and microseconds to support this.</p>
<p>Should the piece of code wants something to happen at less than 1KHz, it should keep its own time and return immediately once its designated time has passed (Even if it's below 1KHz).</p>
<h1></h1>
<h1>HAL specific thread</h1>
<p>On platforms that support multiple threads, the AP-HAL will create threads to support basic operation. This depends entirely on the platform used.</p>
<p>For example, on PX4 the following HAL specific threads are created:</p>
<p>-The UART thread, for reading and writing UARTs (and USB)</p>
<p>-The timer thread, which supports the 1kHz timer functionality described above</p>
<p>-The IO thread, which supports writing to the microSD card, EEPROM and FRAM</p>
<p>A common use of threads is to provide drivers a way to schedule slow tasks without interrupting the drone's flight.</p>
<h1></h1>
<h1>Driver specific threads</h1>
<p>It is also possible to create driver specific threads, to support asynchronous processing in a manner specific to one driver. Currently you can only create driver specific threads in a manner that is platform dependent, so this is only appropriate if your driver is intended to run only on one type of autopilot board (As these depend on the previous operation's drivers). If you want it to run on multiple AP_HAL targets then you have two choices:</p>
<p>-Use a register</p>
<p>-Add a new HAL interface</p>
<h1></h1>
<h1>Ardupilot drivers versus platform drivers</h1>
<p>For Ardupilot, there are a MPU6000 driver in libraries/AP_InertalSensor/AP_InertialSensor_MPU6000.cpp, and another MPU6000 driver in PX4Firmware/src/drivers/mpu6000. The reason for this duplication is that the PX4 project already provides a set of well tested drivers for hardware that comes with PX4 boards, and we enjoy a good collaborative relationship with the PX4 project on developing and enhancing these drivers. So when we build ArduPilot for PX4 we take advantage of the PX4 drivers by writing small &ldquo;shim&rdquo; drivers which present the PX4 drivers with the standard ArduPilot library interface. If you look at libraries/AP_InertialSensor/AP_InertialSensor_PX4.cpp you will see a small shim driver that asks the PX4 what IMU drivers are available on this board and automatically makes all of them available as part of the ArduPilot AP_InertialSensor library. So if we have an MPU6000 on the board we use the AP_InertialSensor_MPU6000.cpp driver on non-PX4 platforms, and the AP_InertialSensor_PX4.cpp driver on PX4 based platforms. The same type of split can also happen for other AP_HAL ports. For example, we could use Linux kernel drivers for some sensors on Linux boards. For other sensors we use the generic AP_HAL I2C and SPI interfaces to use the ArduPilot &ldquo;in-tree&rdquo; drivers which work across a wide range of boards.</p>
<h1></h1>
<h1>The AP_Scheduler system</h1>
<p>The AP_Scheduler library is used to divide up time within the main vehicle thread, while providing some simple mechanisms to control how much time is used for each operation (called a &lsquo;task&rsquo; in AP_Scheduler). The way it works is that the loop() function for each vehicle implementation contains some code that does this:</p>
<p>-Wait for a new IMU (Inertial measurement unit, so basically all the flight data stuff) sample to arrive</p>
<p>-Call a set of tasks between each IMU sample</p>
<p>Table driven design: In many programs, there is a need to deal with entities whose handling requires a variety of distinct behaviors. A straight-forward approach to algorithm design deals with the different kinds of entities using case statements or extended if-then-else statements. However, if the number of types of the entities is large then this approach lead to large amounts of code, often with poor run time characteristics. A table driven approach uses tables to classify the different kinds of entities, aiming at a reduction of the number of cases requiring distinct code.</p>
<p>It is a table driven scheduler, and each vehicle type has a AP_Scheduler::Task table.</p>
<p>Example:</p>
<p>static const AP_Scheduler::Task scheduler_tasks[]</p>
<p>PROGMEM <b>=</b> {<br /> &nbsp;{ ins_update, 1, 1000 },<br /> &nbsp;{ one_hz_print, 50, 1000 },<br /> &nbsp;{ five_second_call, 250, 1800 },<br /> };</p>
<p>The first number after each function name is the call frequency, in units controlled by the ins.init() call. For this example sketch the ins.init() uses RATE_50HZ, so each scheduling step is 20ms. That means the ins_update() call is made every 20ms, the one_hz_print() function is called every 50 times (ie. once a second) and the five_second_call() is called every 250 times (ie. once every 5 seconds).</p>
<p>The third number is the maximum time that the function is expected to take. This is used to avoid making the call unless there is enough time left in this scheduling run to run the function. When scheduler.run() is called it is passed the amount of time (in microseconds) available for running tasks, and if the worst case time for this task would mean it wouldn&rsquo;t fit before that time runs out then it won&rsquo;t be called.</p>
<p>Metronome: Ticks that marks time at a selected rate by giving a regular tick</p>
<p>ins.wait_for_sample() is a function call for Ardupilot that serves as a metronome. It blocks execution of the main vehicle thread until a new IMU sample is available. The time between IMU samples is controlled by the arguments to the ins.init() call. So the drone would not fly when it cannot receive IMU samples. This is metronome as there is a "maximum timer" that limits the time available for running task.</p>
<p>Note that tasks in AP_Scheduler tables must have the following attributes:</p>
<p>-they should never block (except for the ins.update() call until new IMU samples are acquired)</p>
<p>-they should never call sleep functions when flying (a drone, like a real pilot, should never sleep while flying)</p>
<p>-they should have predictable worst case timing (Predictable=reliable)</p>
<h1></h1>
<h1>Semaphores</h1>[/et_pb_text][/et_pb_column][/et_pb_row][/et_pb_section]