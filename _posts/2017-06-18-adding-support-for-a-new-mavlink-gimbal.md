---
ID: 468
post_title: Adding Support for a new MAVLink Gimbal
author: impeccableaslan
post_date: 2017-06-18 17:00:15
post_excerpt: ""
layout: post
permalink: >
  http://rcnode.com/index.php/2017/06/18/adding-support-for-a-new-mavlink-gimbal/
published: true
---
[et_pb_section fb_built="1" admin_label="section" _builder_version="3.0.47"][et_pb_row admin_label="row" _builder_version="3.0.47" background_size="initial" background_position="top_left" background_repeat="repeat"][et_pb_column type="4_4" _builder_version="3.0.47" parallax="off" parallax_method="on"][et_pb_text admin_label="Text" _builder_version="3.0.47" background_size="initial" background_position="top_left" background_repeat="repeat"]
					
				[/et_pb_text][et_pb_text _builder_version="3.0.51"]<p>Gimbal is a pivoted support that rotates an object about a single axis (So on one plane). <g class="gr_ gr_18 gr-alert gr_gramm gr_inline_cards gr_run_anim Grammar multiReplace" id="18" data-gr-id="18">A MAVLink</g> enabled gimbal allows for MAVLink to control its rotation.</p>
<p>The SToRM32 gimbal is a good reference as it supports MAVLink.</p>
<p></p>
<p>Messages the gimbal should support:</p>
<ol>
<li>The gimbal in question should listen for the HEARTBEAT message from the serial port. A HEARTBEAT message is much like a heartbeat in the sense that the serial port's MAVLink constantly emits a message that shows that it's present and responding. So the system with the serial port will treat any message sent from the gimbal as appropriate (that it's functional). It can be generally assumed that the very first HEARTBEAT detected by the gimbal will be from the drone. But to be very certain we can check the "type" field, just to be sure that it's something sensible (i.e. MAV_TYPE = 1 for fixed wing, 2 for quadcopters but 6 for GCSs should be ignored).</li>
<li><g class="gr_ gr_15 gr-alert gr_spell gr_inline_cards gr_run_anim ContextualSpelling ins-del multiReplace" id="15" data-gr-id="15">AaBb</g></li>
</ol>[/et_pb_text][et_pb_divider show_divider="on" _builder_version="3.0.51" color="#000000" divider_weight="2px"][/et_pb_divider][et_pb_comments _builder_version="3.0.51"][/et_pb_comments][/et_pb_column][/et_pb_row][/et_pb_section]