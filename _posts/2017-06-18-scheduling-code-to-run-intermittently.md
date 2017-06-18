---
ID: 450
post_title: Scheduling Code to Run Intermittently
author: impeccableaslan
post_date: 2017-06-18 16:47:34
post_excerpt: ""
layout: post
permalink: >
  http://rcnode.com/index.php/2017/06/18/scheduling-code-to-run-intermittently/
published: true
---
[et_pb_section fb_built="1" admin_label="section" _builder_version="3.0.47"][et_pb_row admin_label="row" _builder_version="3.0.47" background_size="initial" background_position="top_left" background_repeat="repeat"][et_pb_column type="4_4" _builder_version="3.0.47" parallax="off" parallax_method="on"][et_pb_text admin_label="Text" _builder_version="3.0.47" background_size="initial" background_position="top_left" background_repeat="repeat"]
					
				[/et_pb_text][et_pb_text _builder_version="3.0.51"]<p>A most efficient way to run a code with a given interval is by using a scheduler. This can be achieved by adding functions to a <g class="gr_ gr_24 gr-alert gr_spell gr_inline_cards gr_run_anim ContextualSpelling" id="24" data-gr-id="24">scheduler</g>. The scheduler is a scheduler_tasks array in Arducopter.cpp. There are two lists for this, <g class="gr_ gr_31 gr-alert gr_gramm gr_inline_cards gr_run_anim Grammar only-ins doubleReplace replaceWithoutSep" id="31" data-gr-id="31">upper</g> list is for <g class="gr_ gr_34 gr-alert gr_spell gr_inline_cards gr_run_anim ContextualSpelling multiReplace" id="34" data-gr-id="34">high speed</g> <g class="gr_ gr_25 gr-alert gr_spell gr_inline_cards gr_run_anim ContextualSpelling ins-del multiReplace" id="25" data-gr-id="25">cpus</g> (Like the Pixhawk) and <g class="gr_ gr_32 gr-alert gr_gramm gr_inline_cards gr_run_anim Grammar only-ins doubleReplace replaceWithoutSep" id="32" data-gr-id="32">lower</g> list is for slow <g class="gr_ gr_26 gr-alert gr_spell gr_inline_cards gr_run_anim ContextualSpelling ins-del multiReplace" id="26" data-gr-id="26">cpus</g> (Like the APM2).</p>
<p>Adding to the list is pretty simple, you merely have to create a new row in the list. The higher up the task is in the list, the higher the priority. The first column contains the function name, <g class="gr_ gr_39 gr-alert gr_gramm gr_inline_cards gr_run_anim Grammar only-ins replaceWithoutSep" id="39" data-gr-id="39">2nd</g> column contains the number of 2.5ms units (10ms units if an APM2 is used). Meaning, if you desire a function to be executed at 400hz, this column would contain 1, if you desire a function to be executed at 50hz, the column would contain 8. The last column contains the number of microseconds (millions of a second) that the function is expected to take. This is there to help the scheduler determine whether there is enough time to execute your function before the main loop starts next iteration(Repeating).</p>
<p>&nbsp;</p>
<p>Running your code as part of one of the loops:</p>
<p>You can add functions to <g class="gr_ gr_35 gr-alert gr_gramm gr_hide gr_inline_cards gr_run_anim Grammar multiReplace replaceWithoutSep replaceWithoutSep" id="35" data-gr-id="35">an already existing timed loops</g> instead of adding a new entry to the scheduler task list. The advantages of doing so are very small except in the case of the fast-loop. By adding your function to fast-loop, it will mean that the functions within the loop will run at the highest possible priority. That <g class="gr_ gr_37 gr-alert gr_gramm gr_inline_cards gr_run_anim Punctuation only-ins replaceWithoutSep" id="37" data-gr-id="37">is</g> for example, it is pretty much 100% guaranteed to run at 400hz.</p>
<p></p>
<p>-fast_loop : runs at 100hz on APM2, 400hz on Pixhawk</p>
<p>-fifty_hz_loop : runs at 50hz</p>
<p>-ten_hz_logging_loop: runs at 10hz</p>
<p>-three_hz_loop: runs at 3.3hz</p>
<p>-one_hz_loop : runs at 1hz</p>[/et_pb_text][et_pb_divider show_divider="on" _builder_version="3.0.51" color="#000000" divider_weight="2px"][/et_pb_divider][et_pb_comments _builder_version="3.0.51"][/et_pb_comments][/et_pb_column][/et_pb_row][/et_pb_section]