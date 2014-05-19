.. raw:: html

   <table id="main-table" class="table table-bordered table-condensed">
               <thead>
               <tr>
                   <th>

Name
   </th>
                   <th>
Preferences Reference
   </th>
                   <th>

Attributes — [possible values]


   </th>
                   <th>

Commands


   </th>
               </tr>
               </thead>
               <tbody>         
                   


   <td>
                           

Acceleration Sensor


   </td>
                       <td>
                           

capability.accelerationSensor


   </td>
                       <td><div>                                               
                                       

acceleration — ["inactive", "active"]
                            </div>
                        
                    </td>
                    <td>
                        
                    </td>
                </tr>
            
                <tr class="odd">
                    <td>
                        Actuator
                    </td>
                    <td>
                        capability.actuator
                    </td>
                    <td>
                        
                    </td>
                    <td>
                        
                    </td>
                </tr>
            
                <tr class="even">
                    <td>
                        Alarm
                    </td>
                    <td>
                        capability.alarm
                    </td>
                    <td><div>                                               
                                    alarm — ["on", "off"]
                                
                            </div>
                        
                    </td>
                    <td><div>off()</div>
                        
                            <div>strobe()</div>
                        
                            <div>siren()</div>
                        
                            <div>both()</div>
                        
                    </td>
                </tr>
            
                <tr class="odd">
                    <td>
                        Battery
                    </td>
                    <td>
                        capability.battery
                    </td>
                    <td><div>                                               
                                    battery
                                
                            </div>
                        
                    </td>
                    <td>
                        
                    </td>
                </tr>
            
                <tr class="even">
                    <td>
                        Button
                    </td>
                    <td>
                        capability.button
                    </td>
                    <td><div>                                               
                                    button
                                
                            </div>
                        
                    </td>
                    <td>
                        
                    </td>
                </tr>
            
                <tr class="odd">
                    <td>
                        Carbon Monoxide Detector
                    </td>
                    <td>
                        capability.carbonMonoxideDetector
                    </td>
                    <td><div>                                               
                                    carbonMonoxide — ["clear", "detected", "tested"]
                                
                            </div>
                        
                    </td>
                    <td>
                        
                    </td>
                </tr>
            
                <tr class="even">
                    <td>
                        Color Control
                    </td>
                    <td>
                        capability.colorControl
                    </td>
                    <td><div>                                               
                                    hue
                                
                            </div>
                        
                            <div>
                                
                                    saturation
                                
                            </div>
                        
                    </td>
                    <td><div>setHue(number)</div>
                        
                            <div>setSaturation(number)</div>
                        
                            <div>setColor(color_map)</div>
                        
                    </td>
                </tr>
            
                <tr class="odd">
                    <td>
                        Configuration
                    </td>
                    <td>
                        capability.configuration
                    </td>
                    <td>
                        
                    </td>
                    <td><div>configure()</div>
                        
                    </td>
                </tr>
            
                <tr class="even">
                    <td>
                        Contact Sensor
                    </td>
                    <td>
                        capability.contactSensor
                    </td>
                    <td><div>                                               
                                    contact — ["open", "closed"]
                                
                            </div>
                        
                    </td>
                    <td>
                        
                    </td>
                </tr>
            
                <tr class="odd">
                    <td>
                        Energy Meter
                    </td>
                    <td>
                        capability.energyMeter
                    </td>
                    <td><div>                                               
                                    energy
                                
                            </div>
                        
                    </td>
                    <td>
                        
                    </td>
                </tr>
            
                <tr class="even">
                    <td>
                        Illuminance Measurement
                    </td>
                    <td>
                        capability.illuminanceMeasurement
                    </td>
                    <td><div>                                               
                                    illuminance
                                
                            </div>
                        
                    </td>
                    <td>
                        
                    </td>
                </tr>
            
                <tr class="odd">
                    <td>
                        Image Capture
                    </td>
                    <td>
                        capability.imageCapture
                    </td>
                    <td><div>                                               
                                    image
                                
                            </div>
                        
                    </td>
                    <td><div>take()</div>
                        
                    </td>
                </tr>
            
                <tr class="even">
                    <td>
                        Indicator
                    </td>
                    <td>
                        capability.indicator
                    </td>
                    <td><div>                                               
                                    indicatorStatus — ["when on", "when off", "never"]
                                
                            </div>
                        
                    </td>
                    <td><div>indicatorWhenOn()</div>
                        
                            <div>indicatorWhenOff()</div>
                        
                            <div>indicatorNever()</div>
                        
                    </td>
                </tr>
            
                <tr class="odd">
                    <td>
                        Location Mode
                    </td>
                    <td>
                        capability.locationMode
                    </td>
                    <td><div>                                               
                                    mode
                                
                            </div>
                        
                    </td>
                    <td>
                        
                    </td>
                </tr>
            
                <tr class="even">
                    <td>
                        Lock
                    </td>
                    <td>
                        capability.lock
                    </td>
                    <td><div>                                               
                                    lock
                                
                            </div>
                        
                    </td>
                    <td><div>lock()</div>
                        
                            <div>unlock()</div>
                        
                    </td>
                </tr>
            
                <tr class="odd">
                    <td>
                        Lock Codes
                    </td>
                    <td>
                        capability.lockCodes
                    </td>
                    <td><div>                                               
                                    lock
                                
                            </div>
                        
                    </td>
                    <td><div>lock()</div>
                        
                            <div>unlock()</div>
                        
                            <div>updateCodes(string)</div>
                        
                            <div>setCode(number, string)</div>
                        
                            <div>deleteCode(number)</div>
                        
                            <div>requestCode(number)</div>
                        
                            <div>reloadAllCodes()</div>
                        
                    </td>
                </tr>
            
                <tr class="even">
                    <td>
                        Momentary
                    </td>
                    <td>
                        capability.momentary
                    </td>
                    <td>
                        
                    </td>
                    <td><div>push()</div>
                        
                    </td>
                </tr>
            
                <tr class="odd">
                    <td>
                        Motion Sensor
                    </td>
                    <td>
                        capability.motionSensor
                    </td>
                    <td><div>                                               
                                    motion — ["inactive", "active"]
                                
                            </div>
                        
                    </td>
                    <td>
                        
                    </td>
                </tr>
            
                <tr class="even">
                    <td>
                        Music Player
                    </td>
                    <td>
                        capability.musicPlayer
                    </td>
                    <td><div>                                               
                                    status
                                
                            </div>
                        
                            <div>
                                
                                    level
                                
                            </div>
                        
                            <div>
                                
                                    trackDescription
                                
                            </div>
                        
                            <div>
                                
                                    trackData
                                
                            </div>
                        
                            <div>
                                
                                    mute — ["unmuted", "muted"]
                                
                            </div>
                        
                    </td>
                    <td><div>play()</div>
                        
                            <div>pause()</div>
                        
                            <div>stop()</div>
                        
                            <div>nextTrack()</div>
                        
                            <div>playTrack(string)</div>
                        
                            <div>setLevel(number)</div>
                        
                            <div>playText(string)</div>
                        
                            <div>mute()</div>
                        
                            <div>previousTrack()</div>
                        
                            <div>unmute()</div>
                        
                            <div>setTrack(string)</div>
                        
                            <div>resumeTrack(string)</div>
                        
                            <div>restoreTrack(string)</div>
                        
                    </td>
                </tr>
            
                <tr class="odd">
                    <td>
                        Polling
                    </td>
                    <td>
                        capability.polling
                    </td>
                    <td>
                        
                    </td>
                    <td><div>poll()</div>
                        
                    </td>
                </tr>
            
                <tr class="even">
                    <td>
                        Power Meter
                    </td>
                    <td>
                        capability.powerMeter
                    </td>
                    <td><div>                                               
                                    power
                                
                            </div>
                        
                    </td>
                    <td>
                        
                    </td>
                </tr>
            
                <tr class="odd">
                    <td>
                        Presence Sensor
                    </td>
                    <td>
                        capability.presenceSensor
                    </td>
                    <td><div>                                               
                                    presence — ["present", "not present"]
                                
                            </div>
                        
                    </td>
                    <td>
                        
                    </td>
                </tr>
            
                <tr class="even">
                    <td>
                        Refresh
                    </td>
                    <td>
                        capability.refresh
                    </td>
                    <td>
                        
                    </td>
                    <td><div>refresh()</div>
                        
                    </td>
                </tr>
            
                <tr class="odd">
                    <td>
                        Relative Humidity Measurement
                    </td>
                    <td>
                        capability.relativeHumidityMeasurement
                    </td>
                    <td><div>                                               
                                    humidity
                                
                            </div>
                        
                    </td>
                    <td>
                        
                    </td>
                </tr>
            
                <tr class="even">
                    <td>
                        Sensor
                    </td>
                    <td>
                        capability.sensor
                    </td>
                    <td>
                        
                    </td>
                    <td>
                        
                    </td>
                </tr>
            
                <tr class="odd">
                    <td>
                        Signal Strength
                    </td>
                    <td>
                        capability.signalStrength
                    </td>
                    <td><div>                                               
                                    lqi
                                
                            </div>
                        
                            <div>
                                
                                    rssi
                                
                            </div>
                        
                    </td>
                    <td>
                        
                    </td>
                </tr>
            
                <tr class="even">
                    <td>
                        Smoke Detector
                    </td>
                    <td>
                        capability.smokeDetector
                    </td>
                    <td><div>                                               
                                    smoke — ["tested", "clear", "detected"]
                                
                            </div>
                        
                    </td>
                    <td>
                        
                    </td>
                </tr>
            
                <tr class="odd">
                    <td>
                        Switch
                    </td>
                    <td>
                        capability.switch
                    </td>
                    <td><div>                                               
                                    switch — ["off", "on"]
                                
                            </div>
                        
                    </td>
                    <td><div>on()</div>
                        
                            <div>off()</div>
                        
                    </td>
                </tr>
            
                <tr class="even">
                    <td>
                        Switch Level
                    </td>
                    <td>
                        capability.switchLevel
                    </td>
                    <td><div>                                               
                                    level
                                
                            </div>
                        
                    </td>
                    <td><div>setLevel(number, number)</div>
                        
                    </td>
                </tr>
            
                <tr class="odd">
                    <td>
                        Temperature Measurement
                    </td>
                    <td>
                        capability.temperatureMeasurement
                    </td>
                    <td><div>                                               
                                    temperature
                                
                            </div>
                        
                    </td>
                    <td>
                        
                    </td>
                </tr>
            
                <tr class="even">
                    <td>
                        Thermostat
                    </td>
                    <td>
                        capability.thermostat
                    </td>
                    <td><div>                                               
                                    temperature
                                
                            </div>
                        
                            <div>
                                
                                    heatingSetpoint
                                
                            </div>
                        
                            <div>
                                
                                    coolingSetpoint
                                
                            </div>
                        
                            <div>
                                
                                    thermostatSetpoint
                                
                            </div>
                        
                            <div>
                                
                                    thermostatMode — ["heat", "off", "emergency heat", "cool"]
                                
                            </div>
                        
                            <div>
                                
                                    thermostatFanMode — ["auto", "on", "circulate"]
                                
                            </div>
                        
                            <div>
                                
                                    thermostatOperatingState — ["fan only", "cooling", "idle", "heating", "pending heat", "pending cool", "vent economizer"]
                                
                            </div>
                        
                    </td>
                    <td><div>setHeatingSetpoint(number)</div>
                        
                            <div>setCoolingSetpoint(number)</div>
                        
                            <div>off()</div>
                        
                            <div>heat()</div>
                        
                            <div>emergencyHeat()</div>
                        
                            <div>cool()</div>
                        
                            <div>setThermostatMode(enum)</div>
                        
                            <div>fanOn()</div>
                        
                            <div>fanAuto()</div>
                        
                            <div>fanCirculate()</div>
                        
                            <div>setThermostatFanMode(enum)</div>
                        
                            <div>auto()</div>
                        
                    </td>
                </tr>
            
                <tr class="odd">
                    <td>
                        Three Axis
                    </td>
                    <td>
                        capability.threeAxis
                    </td>
                    <td><div>                                               
                                    threeAxis
                                
                            </div>
                        
                    </td>
                    <td>
                        
                    </td>
                </tr>
            
                <tr class="even">
                    <td>
                        Tone
                    </td>
                    <td>
                        capability.tone
                    </td>
                    <td>
                        
                    </td>
                    <td><div>beep()</div>
                        
                    </td>
                </tr>
            
                <tr class="odd">
                    <td>
                        Valve
                    </td>
                    <td>
                        capability.valve
                    </td>
                    <td><div>                                               
                                    contact — ["open", "closed"]
                                
                            </div>
                        
                    </td>
                    <td><div>open()</div>
                        
                            <div>close()</div>
                        
                    </td>
                </tr>
            
                <tr class="even">
                    <td>
                        Water Sensor
                    </td>
                    <td>
                        capability.waterSensor
                    </td>
                    <td><div>                                               
                                    water — ["wet", "dry"]
                                
                            </div>
                        
                    </td>
                    <td>
                        
                    </td>
                </tr>
            
            </tbody>

        </table>
