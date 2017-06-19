---
ID: 495
post_title: Power modules
author: impeccableaslan
post_date: 2017-06-19 05:14:03
post_excerpt: ""
layout: post
permalink: >
  http://rcnode.com/index.php/2017/06/19/power-modules/
published: true
---
[et_pb_section fb_built="1" admin_label="section" _builder_version="3.0.47"][et_pb_row admin_label="row" _builder_version="3.0.47" background_size="initial" background_position="top_left" background_repeat="repeat"][et_pb_column type="4_4" _builder_version="3.0.47" parallax="off" parallax_method="on"][et_pb_text admin_label="Text" _builder_version="3.0.47" background_size="initial" background_position="top_left" background_repeat="repeat"]
					
				[/et_pb_text][et_pb_text _builder_version="3.0.51"]<p>The example this time will be using an AirBotPower power module, of which is one of the most reliable and simple ways to power a Pixhawk and other UAV components.[caption width="342" id="attachment_499" align="<g class="gr_ gr_17 gr-alert gr_spell gr_inline_cards gr_run_anim ContextualSpelling ins-del multiReplace" id="17" data-gr-id="17">alignnone</g>"]<img src="http://rcnode.com/wp-content/uploads/2017/06/power-module-1.png" width="342" height="338" alt="" class="wp-image-499 size-full" /> Image from ardupilot.org[/caption][caption width="340" id="attachment_500" align="<g class="gr_ gr_25 gr-alert gr_spell gr_inline_cards gr_run_anim ContextualSpelling ins-del multiReplace" id="25" data-gr-id="25">alignnone</g>"]<img src="http://rcnode.com/wp-content/uploads/2017/06/power-module-2.png" width="340" height="335" alt="" class="wp-image-500 size-full" /> Image from ardupilot.org[/caption]</p>
<p>AirbotPower provides three power feeds: 2x 5.3V &amp; 1x 12V (up to 3.5 amps each) and can supply main power to Pixhawk&rsquo;s power port, backup power to the servo rail and power for an FPV system. It provides current and voltage measurements through the main power port, up to 150 amps and 8S.</p>
<h1></h1>
<h1>Key features</h1>
<ul>
<li>2 x 5.3 Volts power feeds up to 3.5 amps</li>
<li>1 x 12 Volts power feed up to 3.5 amps (requires &gt;=4S batteries)</li>
<li>Equivalent of three L/C filters on each power feed</li>
<li>Equivalent of three ferrites on each power feed</li>
<li>Voltage spikes suppression with 5.6V Zener diode (5Watts) + Capacitor on Pixhawk servo rail</li>
<li>HALL EFFECT current measurement (ACS758) for currents to 150 amps</li>
<li>A <g class="gr_ gr_60 gr-alert gr_spell gr_inline_cards gr_run_anim ContextualSpelling" id="60" data-gr-id="60">dip-switch</g> configurable 3S to 8S Voltage measurement</li>
<li>A primary DF13 6 position connector for direct plugging into Pixhawk power port</li>
<li>A redundant 5.3V servo connector for standard servo cabling into Pixhawk servo rail</li>
<li>A standard 12V servo connector output for powering FPV devices</li>
<li>Oversized double redundant parallel battery inputs (solder through pads, for 10 or 12AWG wires)</li>
<li>Modular design separating the Power functions and the distribution function in two <g class="gr_ gr_63 gr-alert gr_gramm gr_inline_cards gr_run_anim Style multiReplace" id="63" data-gr-id="63">boards :</g> allows <g class="gr_ gr_61 gr-alert gr_gramm gr_hide gr_inline_cards gr_run_anim Grammar multiReplace replaceWithoutSep replaceWithoutSep" id="61" data-gr-id="61">to retrofit any drone already assembled with an existing Power Distribution Board (PDB)</g>. Stackable optional AirbotPDB distributionBoard, via XT150 connectors (less than 3cm height with stacked optional PDB)</li>
<li>Large XT150 heavy duty connectors solder pads to either stack AirbotPDB or to link to an existing PDB. Flexibility to connect to PDB via connectors or soldered cables.</li>
<li>Overcurrent, ESD and shorts protections on all three BECs</li>
<li>No messy wiring &amp; no cable fiddling thanks to a 6-pin DF13 output connector to connect the board to Pixhawk&rsquo;s 6-pin power port.</li>
<li>Board <g class="gr_ gr_58 gr-alert gr_spell gr_inline_cards gr_run_anim ContextualSpelling ins-del multiReplace" id="58" data-gr-id="58">dimensions</g> (W x D) : 50 x 50 mm. A very compact format using standard 45x45 mm screw holes spacing (M3)</li>
<li>Lightweight and clean surface mount design: 21 grams</li>
</ul>
<h1></h1>
<h1>Connecting to a Pixhawk</h1>
<p>Connecting the main power supply, backup power supply and servo rail &ldquo;safety&rdquo; connector, are shown below.[caption width="844" id="attachment_501" align="<g class="gr_ gr_124 gr-alert gr_spell gr_inline_cards gr_run_anim ContextualSpelling ins-del multiReplace" id="124" data-gr-id="124">alignnone</g>"]<img src="http://rcnode.com/wp-content/uploads/2017/06/airbotpower.png" width="844" height="515" alt="" class="wp-image-501 size-full" /> Image from ardupilot.org[/caption]</p>
<p>The board also comes with a Zener Diode 5.6V (5W) + capacitor and easy instructions to assemble them into an extra safety module that plugs on Pixhawk&rsquo;s servo rail. This is required on Pixhawk&rsquo;s servo rail to trim any short voltage spikes (above 5.6V) that would be fed back to Pixhawk&rsquo;s servo rail from external devices &amp; servos (possibly causing Pixhawk to shut itself down).</p>
<h1></h1>
<h1>A few practical guidelines</h1>
<ol>
<li>Don&rsquo;t use servos on the backup AUX 5.3V supply as they can supply just a few amps and too many servos might exceed this amperage.</li>
<li>At least one of the DIP switch switches MUST be switched ON towards the ON mark (or towards the battery number marks); In the first batch of boards, DIP switches labels have been printed in reverse order: 3S, 4S,...,7S, 8S should instead be read reversely 8S, 7S? ...4S, 3S. Later batches will have their labels printed in the correct order.</li>
<li>On the bottom of the Power Board, there are some gold plated traces between the sensor and the 7mm holes and between BAT- and the <g class="gr_ gr_168 gr-alert gr_gramm gr_inline_cards gr_run_anim Style multiReplace" id="168" data-gr-id="168">PDB- .</g> It is advised to add some solder on them for extreme currents. To do this you&rsquo;ll need at least <g class="gr_ gr_169 gr-alert gr_gramm gr_inline_cards gr_run_anim Grammar multiReplace" id="169" data-gr-id="169">a 80W</g> soldering iron and must use good solder with 2-2.5% non-corrosive flux. Higher power soldering iron leads to shorter heating time which is better for both the PCB itself and the components.</li>
</ol>
<h1></h1>
<h1>AirbotPDB Power Distribution Board</h1>
<p>The AirbotPDB board (&ldquo;10 solder pads&rdquo; power distribution board) is stackable companion board (via XT150 connectors) that can optionally be used with AirbotPower.[caption width="295" id="attachment_502" align="alignnone"]<img src="http://rcnode.com/wp-content/uploads/2017/06/airbot-pdb.png" width="295" height="292" alt="" class="wp-image-502 size-full" /> Image from ardupilot.org[/caption][caption width="559" id="attachment_503" align="alignnone"]<img src="http://rcnode.com/wp-content/uploads/2017/06/airbot-pdb-2.png" width="559" height="375" alt="" class="wp-image-503 size-full" /> Image from ardupilot.org[/caption]</p>[/et_pb_text][et_pb_divider show_divider="on" _builder_version="3.0.51" color="#000000" divider_weight="2px"][/et_pb_divider][et_pb_comments _builder_version="3.0.51"][/et_pb_comments][/et_pb_column][/et_pb_row][/et_pb_section]