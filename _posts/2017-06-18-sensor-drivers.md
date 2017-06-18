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
[et_pb_section fb_built="1" admin_label="section" _builder_version="3.0.47"][et_pb_row admin_label="row" _builder_version="3.0.47" background_size="initial" background_position="top_left" background_repeat="repeat"][et_pb_column type="4_4" _builder_version="3.0.47" parallax="off" parallax_method="on"][et_pb_text admin_label="Text" _builder_version="3.0.47" background_size="initial" background_position="top_left" background_repeat="repeat"]<h1>Frontend / backend split</h1>
<p>An important concept within the sensor driver architecture is the front-end / back-end split.</p>
<p>[caption id="attachment_376" align="alignnone" width="644"]<img class="size-full wp-image-376" src="http://rcnode.com/wp-content/uploads/2017/06/Front-end-back-end.png" alt="" width="644" height="336" /> Image from ardupilot.org[/caption]</p>
[/et_pb_text][et_pb_text admin_label="Text" _builder_version="3.0.51"]<p>The vehicle code only ever calls into the Library’s (aka sensor driver’s) front-end. On start-up the front-end creates one or more back-ends based either on automatic detection of the sensor (i.e. probing for a response on a known I2C address) or by using the user defined _TYPE params (i.e. RNGFND_TYPE, RNGFND_TYPE2). The front-end maintains pointers to each back-end which are normally held within an array named _drivers[]. User settable parameters are always held within the front-end. So front-end is what the user can interact with and backend is the "private" code.</p>
<p>&nbsp;</p>
<h1 style="background-color: #ffffff;">How and when the driver code is running</h1>
<p>[caption id="attachment_381" align="alignnone" width="575"]<img class="size-full wp-image-381" src="http://rcnode.com/wp-content/uploads/2017/06/driver-code.png" alt="" width="575" height="590" /> Image from ardupilot.org[/caption]</p>
[/et_pb_text][et_pb_text admin_label="Text" _builder_version="3.0.51"]<p class=""><span style="font-size: 14px;">Background thread: A background thread is a thread that runs behind the scenes, while the foreground thread continues to run. For instance, a background thread may perform calculations on user input while the user is entering information using a foreground thread.</span></p>
<p><br/></p>
<p>The image above shows a zoomed in view of the <g class="gr_ gr_197 gr-alert gr_spell gr_inline_cards gr_disable_anim_appear ContextualSpelling ins-del multiReplace" id="197" data-gr-id="197">ardupilot</g> architecture. The top-left blue box illustrates how the sensor driver&rsquo;s <g class="gr_ gr_198 gr-alert gr_spell gr_inline_cards gr_run_anim ContextualSpelling" id="198" data-gr-id="198">back-ends</g> are run in a background thread. Raw data from the sensors is collected, converted into standard units and then held within buffers within the driver.</p>
<p><br/></p>
<p>The vehicle code&rsquo;s main thread runs regularly (i.e. 400hz for <g class="gr_ gr_194 gr-alert gr_spell gr_inline_cards gr_run_anim ContextualSpelling" id="194" data-gr-id="194">copter</g>) and accesses the latest data available through methods in the driver&rsquo;s front-end. For <g class="gr_ gr_202 gr-alert gr_gramm gr_inline_cards gr_disable_anim_appear Punctuation only-ins replaceWithoutSep" id="202" data-gr-id="202">example</g> in order to calculate the latest attitude estimate, the AHRS/EKF (Extended Kalman filtering) would pull the latest accelerometer, gyro and compass information from the sensor drivers&rsquo; front-ends (So from <g class="gr_ gr_195 gr-alert gr_spell gr_inline_cards gr_run_anim ContextualSpelling" id="195" data-gr-id="195">backend</g> to <g class="gr_ gr_196 gr-alert gr_spell gr_inline_cards gr_run_anim ContextualSpelling" id="196" data-gr-id="196">front-end</g>).</p>
<p><br/></p>
<p>The image is a slight generalization, for drivers using I2C or SPI, they must run in the background thread so that the <g class="gr_ gr_204 gr-alert gr_spell gr_inline_cards gr_disable_anim_appear ContextualSpelling ins-del multiReplace" id="204" data-gr-id="204">highrate</g> communication with the sensor does not affect the main loop&rsquo;s performance but for driver&rsquo;s using a serial (aka UART) interface, it is safe to run in the main thread because the underlying serial driver itself collects data in the background and includes a buffer. Meaning I2C and SPI does not have a background thread by itself / no buffer.</p>
<p><br/></p>
<h1 style="background-color: #ffffff;">Frequency and voltage</h1>
<p><br/></p>
<p class="">Voltage can be a steady value or a repetitive waveform (square wave, <g class="gr_ gr_207 gr-alert gr_spell gr_inline_cards gr_run_anim ContextualSpelling" id="207" data-gr-id="207">sinewave</g>, triangular etc). Frequency is the number of cycles that a voltage waveform repeats itself per seconds. A voltage with 0 frequency in effect is steady at a certain value which is also known as DC voltage. Any other frequency means a voltage will change from 0 volts to a maximum positive value then back to 0 and over to the same maximum but negatively then back to 0, this process is ONE cycle and is a simplistic way of defining AC voltage. So 240V AC 50 Hz, means that the voltage waveform does the above cycle fifty time per second and swings between positive 240Volts and negative 240Volts.</p>
<h1 style="background-color: #ffffff;"></h1>
<h1 style="background-color: #ffffff;">I2C</h1>
[caption id="attachment_387" align="alignnone" width="351"]<img class="size-full wp-image-387" src="http://rcnode.com/wp-content/uploads/2017/06/i2c-1.png" alt="" width="351" height="118" /> Image from ardupilot.org[/caption][caption id="attachment_388" align="alignnone" width="1188"]<img class="size-full wp-image-388" src="http://rcnode.com/wp-content/uploads/2017/06/i2c-2.png" alt="" width="1188" height="243" /> Image from ardupilot.org[/caption]
<h1 style="background-color: #ffffff;"></h1>
<p class="">-One master, many slaves is possible</p>
<p class="">-A relatively simple protocol which is good for communicating over short-distances (i.e. less than 1m) -Bus runs at 100kHz or 400kHz but the data rate is relatively low compared to other protocols -only 4 pins are required (VCC, GND, SDA, SCL)</p>
<p class="">-Bus runs at 100kHz or 400kHz but the data rate is relatively low compared to other protocols</p>
<p class="">-Only 4 pins are required (VCC, GND, SDA, SCL)</p>
<h1 style="background-color: #ffffff;"></h1>
<p><br/></p>
<h1 style="background-color: #ffffff;">SPI</h1>
[caption id="attachment_389" align="alignnone" width="329"]<img class="size-full wp-image-389" src="http://rcnode.com/wp-content/uploads/2017/06/SPi-1.png" alt="" width="329" height="88" /> Image from ardupilot.org[/caption][caption id="attachment_390" align="alignnone" width="482"]<img class="size-full wp-image-390" src="http://rcnode.com/wp-content/uploads/2017/06/SPi-2.png" alt="" width="482" height="264" /> Image from ardupilot.org[/caption][/et_pb_text][et_pb_text _builder_version="3.0.51"]<p>-One master one slave</p>
<p>-20Mhz+ speed meaning it is very fast especially compared to I2C</p>
<p>-Only works over short distances (10cm)</p>
<p>-Requires at least 5 pins (VCC, GND, SCLK, Master-Out-Slave-In, Master-In-Slave-Out) + 1 slave select pin per slave</p>
<h1 style="background-color: #ffffff;"></h1>
<h1 style="background-color: #ffffff;">Serial / UART</h1>
<p class="">-One master one slave</p>
<p class="">-Character based protocol good for communicating over longer distances compared to I2C and SPI (i.e. 1m)</p>
<p class="">-Relatively fast at 57Kbps ~ 1.5Mbps</p>
<p class="">-At least 4 pins required (VCC, GND, TX, RX), plus 2 optional pins (Clear-To-Send, Clear-To-Receive)</p>[/et_pb_text][et_pb_divider show_divider="on" _builder_version="3.0.51" color="#000000" divider_weight="2px"][/et_pb_divider][et_pb_comments _builder_version="3.0.51"][/et_pb_comments][/et_pb_column][/et_pb_row][/et_pb_section]