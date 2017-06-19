---
ID: 481
post_title: Smart batteries
author: impeccableaslan
post_date: 2017-06-19 05:01:22
post_excerpt: ""
layout: post
permalink: >
  http://rcnode.com/index.php/2017/06/19/smart-batteries/
published: true
---
[et_pb_section fb_built="1" admin_label="section" _builder_version="3.0.47"][et_pb_row admin_label="row" _builder_version="3.0.47" background_size="initial" background_position="top_left" background_repeat="repeat"][et_pb_column type="4_4" _builder_version="3.0.47" parallax="off" parallax_method="on"][et_pb_text admin_label="Text" _builder_version="3.0.47" background_size="initial" background_position="top_left" background_repeat="repeat"]
					
				[/et_pb_text][et_pb_text _builder_version="3.0.51"]<p>Varying voltage: Voltage that changes to represent the electric signal (So it can be interpreted)</p>
<p>Single-ended signaling: It is the simplest and most common method for transmitting electrical signals over wires. One wire carries a varying voltage that represents the signal while the other wire is connected to a reference voltage, usually ground (As potential difference is meaningless without a reference - 5V compared to 3V is different from 5V compared to 1V)</p>
<p>Bandwidth: The bit-rate of available or consumed information capacity expressed typically in metric multiples of bits per second. So the amount of data in bits it can transfer per second.</p>
<h1></h1>
<h1>SMBus</h1>
<p>It is a single-ended simple two-wire bus for the purpose of lightweight communication. Most commonly it is found in computer motherboards for communication with the power source for ON/OFF instructions. It is derived from I&sup2;C for communication with low-bandwidth devices on a motherboard, especially power related chips such as a laptop's rechargeable battery subsystem.</p>
<h1></h1>
<h1>Smart batteries[caption width="274" id="attachment_485" align="<g class="gr_ gr_43 gr-alert gr_spell gr_inline_cards gr_run_anim ContextualSpelling ins-del multiReplace" id="43" data-gr-id="43">alignnone</g>"]<img src="http://rcnode.com/wp-content/uploads/2017/06/smart-bat-1.png" width="274" height="350" alt="" class="wp-image-485 size-full" /> Image from ardupilot.org[/caption][caption width="370" id="attachment_486" align="<g class="gr_ gr_55 gr-alert gr_spell gr_inline_cards gr_run_anim ContextualSpelling ins-del multiReplace" id="55" data-gr-id="55">alignnone</g>"]<img src="http://rcnode.com/wp-content/uploads/2017/06/Smart-bat-2.png" width="370" height="250" alt="" class="wp-image-486 size-full" /> Image from ardupilot.org[/caption]</h1>
<p>While not yet very common, smart batteries are easier to attach and detach from the vehicle and are capable of providing more information on the state of the battery including capacity and individual cell voltages (not yet supported)</p>
<h1>Connecting to the Pixhawk[caption width="932" id="attachment_487" align="alignnone"]<img src="http://rcnode.com/wp-content/uploads/2017/06/puxhawk-smart-bat.png" width="932" height="368" alt="" class="wp-image-487 size-full" /> Image from ardupilot.org[/caption]</h1>
<p>The diagram above shows how to connect the Maxell battery to a Pixhawk.</p>
<p>SMBus is close enough to I2C that the GND, SDA and SCLK lines from the battery can be connected to the Pixhawk&rsquo;s I2C connector</p>[/et_pb_text][et_pb_divider show_divider="on" _builder_version="3.0.51" color="#000000" divider_weight="2px"][/et_pb_divider][et_pb_comments _builder_version="3.0.51"][/et_pb_comments][/et_pb_column][/et_pb_row][/et_pb_section]