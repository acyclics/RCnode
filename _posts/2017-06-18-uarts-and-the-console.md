---
ID: 407
post_title: UARTs and the Console
author: impeccableaslan
post_date: 2017-06-18 15:58:22
post_excerpt: ""
layout: post
permalink: >
  http://rcnode.com/index.php/2017/06/18/uarts-and-the-console/
published: true
---
[et_pb_section fb_built="1" admin_label="section" _builder_version="3.0.47"][et_pb_row admin_label="row" _builder_version="3.0.47" background_size="initial" background_position="top_left" background_repeat="repeat"][et_pb_column type="4_4" _builder_version="3.0.47" parallax="off" parallax_method="on"][et_pb_text admin_label="Text" _builder_version="3.0.47" background_size="initial" background_position="top_left" background_repeat="repeat"]
					
				[/et_pb_text][et_pb_text _builder_version="3.0.51"]<p>The majority if the Quadcopter's component <g class="gr_ gr_15 gr-alert gr_gramm gr_inline_cards gr_run_anim Grammar multiReplace" id="15" data-gr-id="15">rely</g> on UART. For stuff such as GPS modules, debug output and telemetry. Comprehending how HAL communicates with UARTs will help a lot in understanding the drone's code.</p>
<p>Telemetry: This is the process in which data are collected automatically and transmitted to a receiver for monitoring.</p>
<p>There are 5 UARTs:</p>
<p>The drone itself has 5 UARTs, the hardware abstraction library does not set any specific roles for each UARTs but each of these UARTs <g class="gr_ gr_16 gr-alert gr_gramm gr_inline_cards gr_run_anim Grammar multiReplace" id="16" data-gr-id="16">are</g> assumed to perform particular functions.</p>
<p><g class="gr_ gr_14 gr-alert gr_spell gr_inline_cards gr_run_anim ContextualSpelling ins-del multiReplace" id="14" data-gr-id="14">UARTA</g>: This is part of the console (USB usually, runs MAVLink telemetry)</p>
<p>UARTB: This runs the first GPS</p>
<p>UARTC: Primary telemetry (telem1 on Pixhawk, 2nd radio on APM2)</p>
<p>UARTD: Secondary telemetry (telem2 on Pixhawk)</p>
<p><g class="gr_ gr_29 gr-alert gr_spell gr_inline_cards gr_run_anim ContextualSpelling ins-del multiReplace" id="29" data-gr-id="29">UARTE</g>: The second GPS</p>
<p>As you are coding your own drone library and thus, the HAL, you can assign these UART to anything you like. But do try to follow the above guideline as it is both efficient and allows you to follow the provided code.</p>
<p>There are some UART with dual roles. "For <g class="gr_ gr_43 gr-alert gr_gramm gr_inline_cards gr_run_anim Punctuation only-ins replaceWithoutSep" id="43" data-gr-id="43">example</g> there is a parameter SERIAL2_PROTOCOL changes uartD from being used for MAVLink versus being used for Frsky telemetry."</p>
<h1></h1>
<h1>Debug console</h1>
<p>Aside from the 5 UARTs, some platforms might readily have <g class="gr_ gr_52 gr-alert gr_gramm gr_inline_cards gr_run_anim Grammar multiReplace" id="52" data-gr-id="52">debug</g> consoles. This can be checked through the "HAL_OS_POSIX_IO macro"</p>
<p>Please check all the codes <g class="gr_ gr_83 gr-alert gr_gramm gr_inline_cards gr_run_anim Punctuation only-del replaceWithoutSep" id="83" data-gr-id="83">at:</g>&nbsp;http://ardupilot.org/dev/docs/learning-ardupilot-uarts-and-the-console.html</p>[/et_pb_text][et_pb_divider show_divider="on" _builder_version="3.0.51" color="#000000" divider_weight="2px"][/et_pb_divider][et_pb_comments _builder_version="3.0.51"][/et_pb_comments][/et_pb_column][/et_pb_row][/et_pb_section]