---
ID: 360
post_title: Introduction to core
author: impeccableaslan
post_date: 2017-06-18 13:58:13
post_excerpt: ""
layout: post
permalink: >
  http://rcnode.com/index.php/2017/06/18/introduction-to-core/
published: true
---
[et_pb_section fb_built="1" admin_label="section" _builder_version="3.0.47"][et_pb_row admin_label="row" _builder_version="3.0.47" background_size="initial" background_position="top_left" background_repeat="repeat"][et_pb_column type="4_4" _builder_version="3.0.47" parallax="off" parallax_method="on"][et_pb_text admin_label="Text" _builder_version="3.0.47" background_size="initial" background_position="top_left" background_repeat="repeat"]<h1>Basic structure</h1>
<p>[caption id="attachment_361" align="alignnone" width="897"]<img class="size-full wp-image-361" src="http://rcnode.com/wp-content/uploads/2017/06/Intro-to-core.png" alt="" width="897" height="761" /> Image from ardupilot.org[/caption]</p>
[/et_pb_text][et_pb_text _builder_version="3.0.51"]<p>The basic structure of ArduPilot is broken up into 5 main parts:</p>
<p>-Vehicle code</p>
<p>-Shared libraries -hardware abstraction layer (AP_HAL) -tools directories -external support code (i.e.</p>
<p>-Hardware abstraction layer (AP_HAL) -tools directories -external support code (i.e.</p>
<p>-Tools directories -external support code (i.e.</p>
<p>-External support code (i.e. <g class="gr_ gr_3 gr-alert gr_spell gr_inline_cards gr_run_anim ContextualSpelling ins-del multiReplace" id="3" data-gr-id="3">mavlink</g>, <g class="gr_ gr_4 gr-alert gr_spell gr_inline_cards gr_run_anim ContextualSpelling ins-del multiReplace" id="4" data-gr-id="4">dronekit</g>)</p>
<p><br/></p>
<p class="">Vehicle <g class="gr_ gr_268 gr-alert gr_spell gr_inline_cards gr_run_anim ContextualSpelling ins-del multiReplace" id="268" data-gr-id="268">code</g>: The vehicle directories are the top level directories that define the firmware for each vehicle type.</p>
<p class="">Libraries: Libraries include sensor drivers, attitude and position estimation (aka EKF) and control code (i.e. PID controllers)</p>
<p class="">AP_HAL: The AP_HAL layer (Hardware Abstraction Layer) is how we make ArduPilot portable to lots of different platforms. There is a top level AP_HAL in libraries/AP_HAL that defines the interface that the rest of the code has to specific board features, then there is <g class="gr_ gr_290 gr-alert gr_gramm gr_inline_cards gr_run_anim Grammar multiReplace" id="290" data-gr-id="290">a AP_HAL_XXX</g> subdirectory for each board type, for <g class="gr_ gr_291 gr-alert gr_gramm gr_inline_cards gr_run_anim Punctuation only-ins replaceWithoutSep" id="291" data-gr-id="291">example</g> AP_HAL_AVR for AVR based boards, AP_HAL_PX4 for Pixhawk boards and AP_HAL_Linux for Linux based boards.</p>
<p class="">Tools directories: The tools directories are miscellaneous support directories.</p>[/et_pb_text][et_pb_divider show_divider="on" _builder_version="3.0.51" color="#000000" divider_weight="2px"][/et_pb_divider][/et_pb_column][/et_pb_row][et_pb_row _builder_version="3.0.51"][et_pb_column type="4_4" _builder_version="3.0.51" parallax="off" parallax_method="on"][et_pb_comments _builder_version="3.0.51"][/et_pb_comments][/et_pb_column][/et_pb_row][/et_pb_section]