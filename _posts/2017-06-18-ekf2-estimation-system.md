---
ID: 432
post_title: EKF2 Estimation System
author: impeccableaslan
post_date: 2017-06-18 16:33:10
post_excerpt: ""
layout: post
permalink: >
  http://rcnode.com/index.php/2017/06/18/ekf2-estimation-system/
published: true
---
[et_pb_section fb_built="1" admin_label="section" _builder_version="3.0.47"][et_pb_row admin_label="row" _builder_version="3.0.47" background_size="initial" background_position="top_left" background_repeat="repeat"][et_pb_column type="4_4" _builder_version="3.0.47" parallax="off" parallax_method="on"][et_pb_text admin_label="Text" _builder_version="3.0.47" background_size="initial" background_position="top_left" background_repeat="repeat"]
					
				[/et_pb_text][et_pb_text _builder_version="3.0.51"]<h1>What is it?</h1>
<p>This is <g class="gr_ gr_80 gr-alert gr_gramm gr_inline_cards gr_run_anim Grammar multiReplace" id="80" data-gr-id="80">a 24 states</g> Extended Kalman Filter in the AP_NavEKF2 library of which estimates these states:</p>
<p>-Velocity (North, East, Down)</p>
<p>-Attitude - the orientation of an aircraft or spacecraft, relative to the direction of travel (Quaternions)</p>
<p>-Gyro bias <g class="gr_ gr_82 gr-alert gr_gramm gr_inline_cards gr_run_anim Style replaceWithoutSep" id="82" data-gr-id="82"><g class="gr_ gr_83 gr-alert gr_gramm gr_inline_cards gr_run_anim Style replaceWithoutSep" id="83" data-gr-id="83">offsets(X,Y,Z)</g></g></p>
<p>-Position (North, East, Down)</p>
<p>-Gyro scale factors <g class="gr_ gr_84 gr-alert gr_gramm gr_inline_cards gr_run_anim Style replaceWithoutSep" id="84" data-gr-id="84"><g class="gr_ gr_85 gr-alert gr_gramm gr_inline_cards gr_run_anim Style replaceWithoutSep" id="85" data-gr-id="85">(X,Y,Z)</g></g></p>
<p>-Earth magnetic field (North, East, Down)</p>
<p>-Z accel bias</p>
<p>-Wind velocity (North, East)</p>
<p>-Body magnetic field <g class="gr_ gr_86 gr-alert gr_gramm gr_inline_cards gr_run_anim Style replaceWithoutSep" id="86" data-gr-id="86"><g class="gr_ gr_87 gr-alert gr_gramm gr_inline_cards gr_run_anim Style replaceWithoutSep" id="87" data-gr-id="87">(X,Y,Z)</g></g></p>
<p></p>
<p>Of which is based off: <a href="https://github.com/priseborough/InertialNav/blob/master/derivations/RotationVectorAttitudeParameterisation/GenerateNavFilterEquations.m%60__">https://github.com/priseborough/InertialNav/blob/master/derivations/RotationVectorAttitudeParameterisation/GenerateNavFilterEquations.m`__</a>.</p>
<p></p>
<p>Advantages:</p>
<ol>
<li>Each IMU can operate a separate EKF2, thus recovery from an IMU fault is all the more likely</li>
<li>Should there be any faults in a magnetometer, it can switch between magnetometers</li>
<li>It's able to estimate the scale factor of the gyroscope to improve accuracy during high rate <g class="gr_ gr_104 gr-alert gr_spell gr_inline_cards gr_run_anim ContextualSpelling multiReplace" id="104" data-gr-id="104">manoeuvres</g></li>
<li>It's able to simultaneously estimate both orientation and gyro offsets on the drone's startup whilst moving (The thing carrying the drone, not the drone itself). It also does not rely on the Direction Cosine Matrix (DCM) for its initial orientation. Thus, it is efficient in flying from platforms that are in motion when the gyroscope is not calibrated (Adjusted for this motion)</li>
<li>It's capable of handling large gyroscope bias changes during flight</li>
<li>It's more capable in recovering from bad sensor data</li>
<li>It has a smoother output</li>
<li>It's more accurate than <g class="gr_ gr_107 gr-alert gr_gramm gr_inline_cards gr_run_anim Grammar only-del replaceWithoutSep" id="107" data-gr-id="107">the <g class="gr_ gr_108 gr-alert gr_spell gr_inline_cards gr_run_anim ContextualSpelling" id="108" data-gr-id="108">it's</g></g> predecessor</li>
<li>It uses less computing power</li>
<li>It starts using GPS when checks pass rather than waiting for the vehicle motors to arm&nbsp;</li>
</ol>
<h1></h1>
<h1>How does it do this?:</h1>
<ol>
<li>Instead of estimating the quaternion orientation directly, the algorithm estimates the error of the rotation vector and uses it to apply a correction to the quaternion from inertial navigation equations. This is more efficient when the errors are largely angle errors as this avoid errors with linearizing quaternions through large changes in angle. In other words, quaternion involves a multitude of data, so it is more prone to errors. By using a single variable for one type of error, the estimations will be more specific, hence accurate.</li>
</ol>
<p>See &ldquo;Rotation Vector in Attitude Estimation&rdquo;, Mark E. Pittelkau, Journal of Guidance, Control, and Dynamics, 2003&rdquo; for details on this approach.</p>
<p>2. This new Extended Kalman Filter operates on a delayed time horizon so when measurements are combined, the measurements, filter <g class="gr_ gr_89 gr-alert gr_gramm gr_inline_cards gr_run_anim Punctuation only-ins replaceWithoutSep" id="89" data-gr-id="89">states</g> and covariance matrix are taken from the same point in time. The old EKF uses a similar method but instead of using the covariance matrix from the time same as that of the others, it uses the covariance matrix from the current time. So this difference in time decreases accuracy and thus the new EKF of which all elements are taken from the same point in time is more accurate.</p>
<p>The delayed filter states are then used to predict in the current time horizon due to a complementary filter that removes the delay in time. It also filters out sudden change in values of states that arise when the measurements are being combined.</p>
<p>This approach was inspired by the output predictor work done by Ali Khosravian from ANU. &ldquo;Recursive Attitude Estimation in the Presence of Multi-rate and Multi-delay Vector Measurements,&rdquo; A Khosravian, J <g class="gr_ gr_71 gr-alert gr_spell gr_inline_cards gr_run_anim ContextualSpelling ins-del multiReplace" id="71" data-gr-id="71">Trumpf</g>, R Mahony, T Hamel - American Control Conference, vol.-, 2015.</p>
<p>This method of computation is cheaper than the original EKF as the original EKF has to wind the states forward by using buffered IMU data at each update, at the cost of slight accuracy as it would be more accurate if we have current data to <g class="gr_ gr_96 gr-alert gr_spell gr_inline_cards gr_run_anim ContextualSpelling" id="96" data-gr-id="96">reference</g> to. However, the original EKF smooths out the state corrections by implementing them through incrementing them over time until the next measurement is received. This reduces the stability of the filter as the corrections can change a lot.</p>
<p>3. Better use of mathematics and code to improve speed and such are implemented.</p>
<p>4. The compass yaw angle fusion method with fixed declination is implemented as well of which is used on the ground or in cases where magnetic interference prevents the use of a more accurate 3 axis-6-state magnetometer fusion technique.</p>
<h1></h1>
<h1>Deterministic errors</h1>
<p>There are two types of deterministic error, bias and scale factor. Bias errors are due to the electronic registering a non-zero output when the input is zero. It could be due to earth's nature or the sort. <g class="gr_ gr_79 gr-alert gr_gramm gr_inline_cards gr_run_anim Grammar multiReplace" id="79" data-gr-id="79">Scale</g> factor is the offset from the true value, causes for this range from aging to manufacturing tolerances.</p>[/et_pb_text][et_pb_divider color="#000000" show_divider="on" divider_weight="2px" _builder_version="3.0.51"][/et_pb_divider][et_pb_comments _builder_version="3.0.51"][/et_pb_comments][/et_pb_column][/et_pb_row][/et_pb_section]