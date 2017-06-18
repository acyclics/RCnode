---
ID: 426
post_title: Extended Kalman Filtering
author: impeccableaslan
post_date: 2017-06-18 16:24:49
post_excerpt: ""
layout: post
permalink: >
  http://rcnode.com/index.php/2017/06/18/extended-kalman-filtering/
published: true
---
[et_pb_section fb_built="1" admin_label="section" _builder_version="3.0.47"][et_pb_row admin_label="row" _builder_version="3.0.47" background_size="initial" background_position="top_left" background_repeat="repeat"][et_pb_column type="4_4" _builder_version="3.0.47" parallax="off" parallax_method="on"][et_pb_text admin_label="Text" _builder_version="3.0.47" background_size="initial" background_position="top_left" background_repeat="repeat"]
					
				[/et_pb_text][et_pb_text _builder_version="3.0.51"]<p></p>
<p>[caption width="200" id="attachment_430" align="<g class="gr_ gr_7 gr-alert gr_spell gr_inline_cards gr_disable_anim_appear ContextualSpelling ins-del multiReplace" id="7" data-gr-id="7">alignnone</g>"]<img src="http://rcnode.com/wp-content/uploads/2017/06/Kalman.png" width="200" height="150" alt="" class="wp-image-430 size-full" /> Image from https://en.wikipedia.org/wiki/Aircraft_principal_axes[/caption]This page shall describe the Extended Kalman Filtering algorithm used by drones to estimate velocity, vehicle position, angular orientation based on rate gyroscopes, compass (magnetometer), accelerometer, GPS, barometric pressure measurements and airspeed. It comprises of both an overview of the algorithm and available tuning parameters.</p>
<h1>Overview</h1>
<p>Availability of better and faster processors such as that of Pixhawk and PX4 has allowed for more advanced mathematical algorithms to be implemented for estimating orientation, <g class="gr_ gr_170 gr-alert gr_gramm gr_inline_cards gr_run_anim Punctuation only-ins replaceWithoutSep" id="170" data-gr-id="170">velocity</g> and position of the drone. An algorithm named Extended Kalman Filtering has been developed that uses rate gyroscopes, compass, accelerometers, GPS, airspeed and barometric pressure measurements to estimate the velocity, angular <g class="gr_ gr_172 gr-alert gr_gramm gr_inline_cards gr_run_anim Punctuation only-ins replaceWithoutSep" id="172" data-gr-id="172">orientation</g> and position of the drone. This algorithm is implemented in the AP_NavEKF library and is based off: <a href="https://github.com/priseborough/InertialNav">https://github.com/priseborough/InertialNav</a>.</p>
<p>Note that InertialNav is not <g class="gr_ gr_132 gr-alert gr_spell gr_inline_cards gr_run_anim ContextualSpelling ins-del multiReplace" id="132" data-gr-id="132">kalman</g> filtering.&nbsp;</p>
<p>PX4 and Pixhawk users can use this algorithm instead of the legacy complementary filters by setting AHRS_EKF_USE = 1. However, do not do so unless you've&nbsp; calibrated an accelerometer and compass. Failure to do so might result in poor flight path due to bad sensor data.</p>
<p>&nbsp;The purpose of EKF is that it <g class="gr_ gr_217 gr-alert gr_spell gr_inline_cards gr_run_anim ContextualSpelling" id="217" data-gr-id="217">fuses</g> all available measurements and is thus better able to reject measurements with significant errors. The drone will be less susceptible to errors produced by a single sensor. Another feature of EKF is that it's able to estimate offsets from compass readings (So errors involved in compass reading) and also to estimate the earth's magnetic field for the drone. It is thus able to account for compass calibration errors more so than other algorithms (DCM, INAV). In addition, it can make use of optional sensors such as optical flow and range finders to assist in navigation.&nbsp;</p>
<p>Theory:</p>
<p>&nbsp;States-Think the current state of the drone, state of motor etc,</p>
<p>Extended Kalman Filtering estimates a total of 22 states</p>
<p>&nbsp;Angular: Denoting physical properties or quantities measured with reference to or by means of an angle, especially those associated with rotation. So it has direction.</p>
<p>Angular rates: Rates means something over time so angular rates would be the data acquired over time.</p>
<p>Angular position: The position conveyed with angles</p>
<p></p>
<ol>
<li>Inertial measurement unit, of which is composed of accelerometers, gyroscopes and maybe even magnetometers, integrates angular rates to calculate the angular position</li>
<li>Inertial measurement unit acquired from an accelerometer are converted using angular position (from <g class="gr_ gr_167 gr-alert gr_gramm gr_inline_cards gr_disable_anim_appear Grammar only-ins replaceWithoutSep" id="167" data-gr-id="167">accelerometer</g>) so body <g class="gr_ gr_168 gr-alert gr_gramm gr_inline_cards gr_disable_anim_appear Style replaceWithoutSep" id="168" data-gr-id="168"><g class="gr_ gr_169 gr-alert gr_gramm gr_inline_cards gr_disable_anim_appear Style replaceWithoutSep" id="169" data-gr-id="169">X,Y,Z</g></g> to earth/compass bearings so North, east, down axes with adjustment for gravity.</li>
<li>The acceleration is integrated to calculate the velocity</li>
<li>The velocity is integrated to calculate the position. Yes, it would be the displacement but the algorithm would've used the previous readings as a reference point and thus it is <g class="gr_ gr_161 gr-alert gr_gramm gr_inline_cards gr_disable_anim_appear Grammar only-ins doubleReplace replaceWithoutSep" id="161" data-gr-id="161">position</g>. Note that displacement is a vector quantity, thus it has direction. And thus the (current) position.</li>
</ol>
<p>Explanation from 1 to 4 are what is known as a "state prediction". <g class="gr_ gr_189 gr-alert gr_gramm gr_inline_cards gr_disable_anim_appear Grammar multiReplace" id="189" data-gr-id="189">State</g> is variables the algorithm are trying to estimate, height, yaw, pitch and roll (Easier to understand by searching "yaw pitch and roll" on Google images), wind speed etc. Each of these is a state. The filter <g class="gr_ gr_191 gr-alert gr_gramm gr_inline_cards gr_disable_anim_appear Grammar multiReplace" id="191" data-gr-id="191">contain</g> other <g class="gr_ gr_193 gr-alert gr_spell gr_inline_cards gr_disable_anim_appear ContextualSpelling" id="193" data-gr-id="193">states beside</g> velocity, <g class="gr_ gr_192 gr-alert gr_gramm gr_inline_cards gr_disable_anim_appear Punctuation only-ins replaceWithoutSep" id="192" data-gr-id="192">angles</g> and position that are assumed to change slowly over time. For example Z accelerometer bias, gyro biases, wind velocities, earth's magnetic field and compass biases. <g class="gr_ gr_195 gr-alert gr_gramm gr_inline_cards gr_disable_anim_appear Grammar multiReplace" id="195" data-gr-id="195">These kind</g> of states aren't modified directly by "State prediction, description 1 to 4" but can be modified through <g class="gr_ gr_194 gr-alert gr_gramm gr_inline_cards gr_disable_anim_appear Grammar multiReplace" id="194" data-gr-id="194">other sort</g> of measurements, due to the fact that "State prediction" cannot by itself account for the above elements and would need external gadgets. It is merely the core estimation of which can be built on.</p>
<p>5. Noise determines the minimum resolution of the sensor. It is an electrical noise in a sensor's output that <g class="gr_ gr_181 gr-alert gr_gramm gr_inline_cards gr_disable_anim_appear Grammar multiReplace" id="181" data-gr-id="181">limit</g> its smallest possible measurement. So there will always be some deviation from the actual value. All electrical components produce small random changes in voltage potentials that combine throughout the circuitry and appear as a band of noise when viewed with an oscilloscope. The EKF will estimate gyro and accelerometer's noise (EKF_GYRO_NOISE and EKF_ACC_NOISE)from <g class="gr_ gr_183 gr-alert gr_gramm gr_inline_cards gr_disable_anim_appear Style multiReplace" id="183" data-gr-id="183">Ardupilot&nbsp; and</g> uses it to estimate the errors of velocities, <g class="gr_ gr_182 gr-alert gr_gramm gr_inline_cards gr_disable_anim_appear Punctuation only-ins replaceWithoutSep" id="182" data-gr-id="182">angles</g> and position calculated using IMU data. Should these parameters grow larger, perhaps the velocity increases etc, it will cause the filters error estimate to grow faster. The estimated errors are stored in a large matrix called "State Covariance Matrix".</p>
<p>Description 1 to 5 <g class="gr_ gr_149 gr-alert gr_gramm gr_inline_cards gr_disable_anim_appear Grammar multiReplace" id="149" data-gr-id="149">are</g> repeated when new IMU data is acquired or until new data from other measurements of another sensor are available.</p>
<p>Should the initial error estimate be perfect, IMU measurements <g class="gr_ gr_174 gr-alert gr_gramm gr_inline_cards gr_disable_anim_appear Grammar multiReplace" id="174" data-gr-id="174">is</g> perfect as well, and perfect calculations, that is there is no change whatsoever, then it would just keep repeating 1-4 throughout the flight without need of additional calculation. However, since there might be errors in the initial values, errors in IMU measurements and rounding errors in calculation, the drone's <g class="gr_ gr_175 gr-alert gr_gramm gr_inline_cards gr_disable_anim_appear Punctuation only-ins replaceWithoutSep" id="175" data-gr-id="175">velocity</g> and position error will eventually become too large.</p>
<p>EKF also provide us a way to combine data from the IMU, compass, GPS, barometer, airspeed and various other sensors to calculate a more accurate estimation of the drone's velocity, <g class="gr_ gr_164 gr-alert gr_gramm gr_inline_cards gr_disable_anim_appear Punctuation only-ins replaceWithoutSep" id="164" data-gr-id="164">position</g> and angular orientation.&nbsp;</p>
<p>This shall proceed to describe how the usage of GPS horizontal position measurements, however, the basic principle applies to all other measurement types (GPS, barometric altitude etc)</p>
<p>6. Once the GPS measurement arrives, the filter will calculate the difference between the predicted position from 4 and that of the position from GPS. The difference is called "Innovation"</p>
<p>7. The combination of "Innovation", "State covariance matrix" and the GPS' measurement error determined by Ardupilot's EKF_POSNE_NOISE calculates a correction for each of the filter states. This is called "State correction".</p>
<p>This is what makes EKF so special - it's able to make use of correlations between different errors and different states to adjust states other than the one being measured. That is, the GPS position measurements can correct errors in velocity, position, gyro bias and angles.</p>
<p>The amount of correction works on a ratio <g class="gr_ gr_154 gr-alert gr_spell gr_inline_cards gr_disable_anim_appear ContextualSpelling" id="154" data-gr-id="154">bases</g>, it is controlled by the estimated ratio of error in the states to <g class="gr_ gr_177 gr-alert gr_gramm gr_inline_cards gr_disable_anim_appear Grammar only-ins doubleReplace replaceWithoutSep" id="177" data-gr-id="177">error</g> in the measurements. Should the filter thinks that the position calculated by itself is more accurate than the position calculated by GPS, then the correction due to the GPS measurements will be smaller.&nbsp; Should the filter thinks that the position calculated by itself is less accurate than the position calculated by GPS, then the correction due to the GPS measurements will be larger. The assumed accuracy of the GPS measurement is controlled by the EKF_POSNE_NOISE parameter. Making the EKF_POSNE_NOISE larger would cause the filter to dictate that the GPS position is less accurate.</p>
<p>8. As the values are adjusted and measurements <g class="gr_ gr_155 gr-alert gr_gramm gr_inline_cards gr_disable_anim_appear Grammar multiReplace" id="155" data-gr-id="155">taken</g>, the uncertainty in each of the states of which has been updated is reduced. The amount reduced is calculated by the filter with the aid of the "State correction". It will then update the "State covariance matrix" and goes back to procedure 1.</p>[/et_pb_text][et_pb_divider show_divider="on" _builder_version="3.0.51" color="#000000" divider_weight="2px"][/et_pb_divider][et_pb_comments _builder_version="3.0.51"][/et_pb_comments][/et_pb_column][/et_pb_row][/et_pb_section]