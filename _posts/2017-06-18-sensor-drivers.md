---
ID: 375
post_title: Sensor drivers
author: impeccableaslan
post_date: 2017-06-18 14:26:40
post_excerpt: ""
layout: post
permalink: >
  http://rcnode.com/index.php/2017/06/18/sensor-drivers/
published: true
---
[et_pb_section bb_built="1" admin_label="section" _builder_version="3.0.47"][et_pb_row admin_label="row" _builder_version="3.0.47" background_size="initial" background_position="top_left" background_repeat="repeat"][et_pb_column type="4_4"][et_pb_text admin_label="Text" _builder_version="3.0.47" background_size="initial" background_position="top_left" background_repeat="repeat"]
<h1>Frontend / backend split</h1>
An important concept within the sensor driver architecture is the front-end / back-end split.

[caption id="attachment_376" align="alignnone" width="644"]<img class="size-full wp-image-376" src="http://rcnode.com/wp-content/uploads/2017/06/Front-end-back-end.png" alt="" width="644" height="336" /> Image from ardupilot.org[/caption]

[/et_pb_text][et_pb_text admin_label="Text" _builder_version="3.0.51"]

The vehicle code only ever calls into the Library’s (aka sensor driver’s) front-end. On start-up the front-end creates one or more back-ends based either on automatic detection of the sensor (i.e. probing for a response on a known I2C address) or by using the user defined _TYPE params (i.e. RNGFND_TYPE, RNGFND_TYPE2). The front-end maintains pointers to each back-end which are normally held within an array named _drivers[]. User settable parameters are always held within the front-end. So front-end is what the user can interact with and backend is the "private" code.

&nbsp;
<h1 style="background-color: #ffffff;">How and when the driver code is running</h1>
[caption id="attachment_381" align="alignnone" width="575"]<img class="size-full wp-image-381" src="http://rcnode.com/wp-content/uploads/2017/06/driver-code.png" alt="" width="575" height="590" /> Image from ardupilot.org[/caption]

[/et_pb_text][et_pb_text admin_label="Text" _builder_version="3.0.51" background_layout="light" text_orientation="left" border_style="solid"]

Background thread: A background thread is a thread that runs behind the scenes, while the foreground thread continues to run. For instance, a background thread may perform calculations on user input while the user is entering information using a foreground thread.

The image above shows a zoomed in view of the ardupilot architecture. The top-left blue box illustrates how the sensor driver’s back-ends are run in a background thread. Raw data from the sensors is collected, converted into standard units and then held within buffers within the driver.

The vehicle code’s main thread runs regularly (i.e. 400hz for copter) and accesses the latest data available through methods in the driver’s front-end. For example in order to calculate the latest attitude estimate, the AHRS/EKF (Extended Kalman filtering) would pull the latest accelerometer, gyro and compass information from the sensor drivers’ front-ends (So from backend to front-end).

The image is a slight generalization, for drivers using I2C or SPI, they must run in the background thread so that the highrate communication with the sensor does not affect the main loop’s performance but for driver’s using a serial (aka UART) interface, it is safe to run in the main thread because the underlying serial driver itself collects data in the background and includes a buffer. Meaning I2C and SPI does not have a background thread by itself / no buffer.
<h1 style="background-color: #ffffff;"></h1>
<h1 style="background-color: #ffffff;"></h1>
<h1 style="background-color: #ffffff;">Frequency and voltage</h1>
<h1 style="background-color: #ffffff;"></h1>
<h1 style="background-color: #ffffff;">I2C</h1>
[caption id="attachment_387" align="alignnone" width="351"]<img class="size-full wp-image-387" src="http://rcnode.com/wp-content/uploads/2017/06/i2c-1.png" alt="" width="351" height="118" /> Image from ardupilot.org[/caption]

[caption id="attachment_388" align="alignnone" width="1188"]<img class="size-full wp-image-388" src="http://rcnode.com/wp-content/uploads/2017/06/i2c-2.png" alt="" width="1188" height="243" /> Image from ardupilot.org[/caption]
<h1 style="background-color: #ffffff;"></h1>
<h1 style="background-color: #ffffff;">SPI</h1>
[caption id="attachment_389" align="alignnone" width="329"]<img class="size-full wp-image-389" src="http://rcnode.com/wp-content/uploads/2017/06/SPi-1.png" alt="" width="329" height="88" /> Image from ardupilot.org[/caption]

[caption id="attachment_390" align="alignnone" width="482"]<img class="size-full wp-image-390" src="http://rcnode.com/wp-content/uploads/2017/06/SPi-2.png" alt="" width="482" height="264" /> Image from ardupilot.org[/caption]

[/et_pb_text][/et_pb_column][/et_pb_row][/et_pb_section]