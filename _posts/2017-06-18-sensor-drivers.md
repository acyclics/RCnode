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
[caption id="attachment_376" align="alignnone" width="644"]<img class="size-full wp-image-376" src="http://rcnode.com/wp-content/uploads/2017/06/Front-end-back-end.png" alt="" width="644" height="336" /> Image from ardupilot.org[/caption][/et_pb_text][et_pb_text _builder_version="3.0.51"]<p>The vehicle code only ever calls into the Library&rsquo;s (aka sensor driver&rsquo;s) front-end. On <g class="gr_ gr_9 gr-alert gr_gramm gr_inline_cards gr_run_anim Punctuation only-ins replaceWithoutSep" id="9" data-gr-id="9">start-up</g> the front-end creates one or more back-ends based either on automatic detection of the sensor (i.e. probing for a response on a known I2C address) or by using the user defined _TYPE params (i.e. RNGFND_TYPE, RNGFND_TYPE2). The front-end maintains pointers to each back-end which are normally held within an array named _drivers[]. <g class="gr_ gr_8 gr-alert gr_spell gr_inline_cards gr_run_anim ContextualSpelling ins-del multiReplace" id="8" data-gr-id="8">User settable</g> parameters are always held within the front-end. So front-end is what the user can interact with and backend is the "private" code.</p>
<p><br/></p>
<h1 style="background-color: #ffffff;">How and when the driver code is running</h1>
<p><br/></p>[/et_pb_text][/et_pb_column][/et_pb_row][/et_pb_section]