## Functional Requirements

#### 1.
In case app_1 sends SubscribeVehicleData (param_1) to SDL   

and SDL does not have this param_1 in list of stored successfully subscribed params  

SDL must:  

- transfer SubscribeVehicleData(param_1) to HMI
- in case SDL receives SUCCESS from HMI for param_1  
SDL must store this param in list 
- respond with corresponding result SUCCESS received from HMI to app_1

_please see relative [SDL state chart](https://github.com/KhrystynaDubovyk/Requirements/blob/subscribe_vd/docs/State/SubscribeVehicleData_state_change.md#state_1)_
#### 2.  
In case app_1 sends SubscribeVehicleData (param_1) to SDL  

and SDL does not have this param_1 in list of stored successfully subscribing params  

SDL must:  
- transfer SubscribeVehicleData(param_1) to HMI  
- in case SDL receives erroneous result from HMI for param_1  
SDL must NOT store the parameter in SDL list   
- respond SubscribeVehicleData (Result Code, success:false) to app_1

#### 3.  
In case app_2 sends SubscribeVehicleData (param_1, param_2) to SDL  

and  SDL has already successfully subscribed app_1 to param_1 and param_2  

SDL must:  

- NOT send SubscribeVehicleData(param_1, param_2) to HMI  
- respond via SubscribeVehicleData (SUCCESS) to app_2  

_please see relative [SDL state chart](https://github.com/KhrystynaDubovyk/Requirements/blob/subscribe_vd/docs/State/SubscribeVehicleData_state_change.md#state_2)_

#### 4.
In case mobile application sends SubscribeVehicleData_request  

that is NOT included (omitted) in the PolicyTable group(s) assigned to the application  

SDL must:  
return DISALLOWED, success:false to this mobile app  

#### 5.  
In case mobile application already subscribed on a single or multiple `<vehicleData>`  

and sends SubscribeVehicleData_request with all `<vehicleData>` subscribed by the application before  

SDL must:  

- not transfer a resuest to HMI  
- respond with (IGNORED, success:false {`<vehicleData>`: DATA_ALREADY_SUBSCRIBED} ) to mobile app 

#### 6.  
In case mobile application sends valid SubscribeVehicleData_request 

trying to subscribe the parameter the application has already subscribed for (even if the request contains at least one non-subscribed parameter)

SDL must:  

- ignore alredy subscribed items and transfer not subscribed params of SubscribeVehicleData to HMI
- get the response with general_result_Code_from_HMI and newly-subscribed parameters with their correponding individual-result-codes from HMI 
- respond to mobile application with "ResultCode: IGNORED, success: `<applicable flag>`" + "info" parameter+ all parameters and their correponding individual result codes got from HMI and all ignored parameter(s) with the individual result code(s): DATA_ALREADY_SUBSCRIBED for items ignored by SDL  

_Important Note: success:true and resultCode: IGNORED is applicabe for general_result_Code_from_HMI SUCCESS/WARNINGS, otherwise success:false and resultCode: general_result_Code_from_HMI must be returned to mobile application._  

#### 7.  
	
In case SubscribeVehicleData RPC is allowed by Policies with less than supported by protocol parameters  

and the app assigned with such policies requests SubscribeVehicleData with one and-or more NOT-allowed params only  

SDL must:  

- send nothing to HMI  
- return the individual results of DISALLOWED to response to mobile application + "ResultCode:DISALLOWED, success: false"  

#### 8.
In case mobile app sends SubscribeVehicleData_request to SDL  

and this request is allowed by Policies for this mobile application  

and "parameters" field is **empty** at PolicyTable for SubscribeVehicleData RPC  

SDL must:

- respond with "DISALLOWED, success:false", "info: Requested parameters are disallowed by Policies" 
- NOT transfer this request to HMI  

#### 9.
In case mobile application sends SubscribeVehicleData_request to SDL  

and this request is allowed by Policies for this mobile application  

and "parameters" field is **omitted** at PolicyTable for this request  

SDL must:  
- transfer received request with all requested parameters as is to HMI  
- respond with `<received_resultCode_from_HMI>` to mobile app


## Non-Functional Requirements
### Mobile API
```
<function name="SubscribeVehicleData" functionID="SubscribeVehicleDataID" messagetype="request">
    <description>
    	Subscribes for specific published data items.
    	The data will be only sent if it has changed.
    	The application will be notified by the onVehicleData notification whenever new data is available.
    	To unsubscribe the notifications, use unsubscribe with the same subscriptionType.
    </description>

    <param name="gps" type="Boolean" mandatory="false">
      <description>See GPSData</description>
    </param>
    <param name="speed" type="Boolean" mandatory="false">
      <description>The vehicle speed in kilometers per hour</description>
    </param>
    <param name="rpm" type="Boolean" mandatory="false">
      <description>The number of revolutions per minute of the engine</description>
    </param>
    <param name="fuelLevel" type="Boolean" mandatory="false">
      <description>The fuel level in the tank (percentage)</description>
    </param>
    <param name="fuelLevel_State" type="Boolean" mandatory="false">
      <description>The fuel level state</description>
    </param>
    <param name="instantFuelConsumption" type="Boolean" mandatory="false">
      <description>The instantaneous fuel consumption in microlitres</description>
    </param>
<param name="fuelRange" type="Boolean" mandatory="false">
      <description>Range in KM based on fuel consumption/level</description>
    </param>		 
	 <param name="abs_State" type="Boolean" mandatory="false">
      <description>See ABS_STATE</description>
    </param>
    <param name="externalTemperature" type="Boolean" mandatory="false">
      <description>The external temperature in degrees celsius</description>
    </param>
    <param name="prndl" type="Boolean" mandatory="false">
      <description>See PRNDL</description>
    </param>
    <param name="tirePressure" type="Boolean" mandatory="false">
      <description>See TireStatus</description>
    </param>
<param name="tirePressureValue" type="Boolean" mandatory="false">
      <description>See TirePressureValue</description>
    </param>	 
    <param name="tpms" type="Boolean" mandatory="false">
      <description>See TPMS</description>
    </param>	 
    <param name="turnSignal" type="Boolean" mandatory="false">
      <description>See TurnSignal</description>
    </param>
    <param name="odometer" type="Boolean" mandatory="false">
      <description>Odometer in km</description>
    </param>
    <param name="beltStatus" type="Boolean" mandatory="false">
      <description>The status of the seat belts</description>
    </param>
    <param name="bodyInformation" type="Boolean" mandatory="false">
      <description>The body information including power modes</description>
    </param>
    <param name="deviceStatus" type="Boolean" mandatory="false">
      <description>The device status including signal and battery strength</description>
    </param>
    <param name="driverBraking" type="Boolean" mandatory="false">
      <description>The status of the brake pedal</description>
    </param>
    <param name="wiperStatus" type="Boolean" mandatory="false">
      <description>The status of the wipers</description>
    </param>
    <param name="headLampStatus" type="Boolean" mandatory="false">
      <description>Status of the head lamps</description>
    </param>
    <param name="engineTorque" type="Boolean" mandatory="false">
      <description>Torque value for engine (in Nm) on non-diesel variants</description>
    </param>
    <param name="accPedalPosition" type="Boolean" mandatory="false">
      <description>Accelerator pedal position (percentage depressed)</description>
    </param>
    <param name="steeringWheelAngle" type="Boolean" mandatory="false">
      <description>Current angle of the steering wheel (in deg)</description>
    </param>
  </function>
  ```
  ```
  <function name="SubscribeVehicleData" functionID="SubscribeVehicleDataID" messagetype="response">
   <param name="success" type="Boolean" platform="documentation">
		<description> true, if successful; false, if failed </description>
	  </param>

    <param name="resultCode" type="Result" platform="documentation">
      <description>See Result</description>
      <element name="SUCCESS"/>
      <element name="WARNINGS"/>
      <element name="INVALID_DATA"/>
      <element name="OUT_OF_MEMORY"/>
      <element name="TOO_MANY_PENDING_REQUESTS"/>
      <element name="APPLICATION_NOT_REGISTERED"/>
      <element name="GENERIC_ERROR"/>
      <element name="IGNORED" />
      <element name="DISALLOWED"/>
      </param>

    <param name="info" type="String" maxlength="1000" mandatory="false" platform="documentation">
      <description>Provides additional human readable info regarding the result.</description>
    </param>

    <param name="gps" type="VehicleDataResult" mandatory="false">
      <description>See GPSData</description>
    </param>
    <param name="speed" type="VehicleDataResult" mandatory="false">
      <description>The vehicle speed in kilometers per hour</description>
    </param>
    <param name="rpm" type="VehicleDataResult" mandatory="false">
      <description>The number of revolutions per minute of the engine</description>
    </param>
    <param name="fuelLevel" type="VehicleDataResult" mandatory="false">
      <description>The fuel level in the tank (percentage)</description>
    </param>
    <param name="fuelLevel_State" type="VehicleDataResult" mandatory="false">
      <description>The fuel level state</description>
    </param>
    <param name="instantFuelConsumption" type="VehicleDataResult" mandatory="false">
      <description>The instantaneous fuel consumption in microlitres</description>
    </param>
<param name="fuelRange" type="VehicleDataResult" mandatory="false">
      <description>Range in KM based on fuel consumption/level</description>
    </param>		 
	 <param name="abs_State" type="VehicleDataResult" mandatory="false">
      <description>See ABS_STATE</description>
    </param>
    <param name="externalTemperature" type="VehicleDataResult" mandatory="false">
      <description>The external temperature in degrees celsius.</description>
    </param>
    <param name="prndl" type="VehicleDataResult" mandatory="false">
      <description>See PRNDL</description>
    </param>
    <param name="tirePressure" type="VehicleDataResult" mandatory="false">
      <description>See TireStatus</description>
    </param>
<param name="tirePressureValue" type="VehicleDataResult" mandatory="false">
      <description>See TirePressureValue</description>
    </param>	
    <param name="tpms" type="VehicleDataResult" mandatory="false">
      <description>See TPMS</description>
    </param>	
	 <param name="turnSignal" type="VehicleDataResult" mandatory="false">
      <description>See TurnSignal</description>
    </param>
    <param name="odometer" type="VehicleDataResult" mandatory="false">
      <description>Odometer in km</description>
    </param>
    <param name="beltStatus" type="VehicleDataResult" mandatory="false">
      <description>The status of the seat belts</description>
    </param>
    <param name="bodyInformation" type="VehicleDataResult" mandatory="false">
      <description>The body information including power modes</description>
    </param>
    <param name="deviceStatus" type="VehicleDataResult" mandatory="false">
      <description>The device status including signal and battery strength</description>
    </param>
    <param name="driverBraking" type="VehicleDataResult" mandatory="false">
      <description>The status of the brake pedal</description>
    </param>
    <param name="wiperStatus" type="VehicleDataResult" mandatory="false">
      <description>The status of the wipers</description>
    </param>
    <param name="headLampStatus" type="VehicleDataResult" mandatory="false">
      <description>Status of the head lamps</description>
    </param>
    <param name="engineTorque" type="VehicleDataResult" mandatory="false">
      <description>Torque value for engine (in Nm) on non-diesel variants</description>
    </param>
    <param name="accPedalPosition" type="VehicleDataResult" mandatory="false">
      <description>Accelerator pedal position (percentage depressed)</description>
    </param>
    <param name="steeringWheelAngle" type="VehicleDataResult" mandatory="false">
      <description>Current angle of the steering wheel (in deg)</description>
    </param>
  </function>
  ```
#### Structures used:
**VehicleDataResult**
```
  <struct name="VehicleDataResult">
    <description>Individual published data request result</description>
    <param name="dataType" type="VehicleDataType">
      <description>Defined published data element type.</description>
    </param>
    <param name="resultCode" type="VehicleDataResultCode">
      <description>Published data result code.</description>
    </param>
  </struct>
```
**'ABS_STATE'**
```
  <enum name="ABS_STATE">
    <description>State of Anti lock Brake System</description>
    <element name="INACTIVE">
    	<description>ABS is inactive</description>
    </element>
    <element name="ACTIVE">
    	<description>ABS is active</description>
    </element>	 
  </enum> 
  ```
  **'TurnSignal'**
  ```
    <enum name="TurnSignal">
    <description>The turn light indicator.</description>
    <element name="OFF">
    	<description>Turn signal is OFF</description>
    </element>
    <element name="LEFT">
    	<description>Left turn signal is on</description>
    </element>	 
    <element name="RIGHT">
    	<description>Right turn signal is on</description>
    </element>	 	 
    <element name="UNUSED">
    	<description>Unused</description>
    </element>	 	 	 
  </enum>
  ```
  **'TirePressureValue'**
  ```
  <struct name="TirePressureValue">
   <description>The value of tire pressure.</description>
   <param name="leftFront" type="Integer" minvalue="0" maxvalue="65535" mandatory="false">
      <description>The pressure value of left front tire in kilo pascal.</description>
	</param>
    <param name="rightFront" type="Integer" minvalue="0" maxvalue="65535" mandatory="false">
      <description>The pressure value of right front tire in kilo pascal.</description>
	</param>
    <param name="leftRear" type="Integer" minvalue="0" maxvalue="65535" mandatory="false">
      <description>The pressure value of left rear tire/outer left rear tire on dual tire vehicles in kilo pascal.</description>
	</param>
    <param name="rightRear" type="Integer" minvalue="0" maxvalue="65535" mandatory="false">
      <description>The pressure value of right rear tire/outer right rear tire on dual tire vehicles in kilo pascal.</description>
	</param>
    <param name="innerLeftRear" type="Integer" minvalue="0" maxvalue="65535" mandatory="false">
      <description>The pressure value of the inner left rear on dual tire vehicles in kilo pascal.</description>
	</param>
    <param name="innerRightRear" type="Integer" minvalue="0" maxvalue="65535" mandatory="false">
      <description>The pressure value of the inner right rear  on dual tire vehicles in kilo pascal.</description>
	</param>
  </struct>
  ```
  **'TPMS'**
  ```<enum name="TPMS">
    <description>The state of TPMS.</description>
    <element name="UNKNOWN">
    	<description>State is unknown</description>
    </element>
    <element name="SYSTEM_FAULT">
    	<description>TPMS system is in a state of Fault</description>
    </element>	 
    <element name="SENSOR_FAULT">
    	<description>Sensor is faulty </description>
    </element>	 	 
    <element name="LOW">
    	<description>Low Composite stat </description>
    </element>	 	 	 
    <element name="SYSTEM_ACTIVE">
    	<description>System Active Composite stat </description>
    </element>	 	 	 
	 <element name="TRAIN_LF_TIRE">
    	<description>Train left front tire </description>
    </element>	 	 	 
    <element name="TRAIN_RF_TIRE">
    	<description>Train right front tire </description>
    </element>	 	 	 
    <element name="TRAIN_RR_TIRE">
    	<description>Train right rear tire </description>
    </element>	 	 	 	 
    <element name="TRAIN_ORR_TIRE">
    	<description>Train outer right rear tire </description>
    </element>	 	 	 	 	 
    <element name="TRAIN_IRR_TIRE">
    	<description>Train inner right rear tire </description>
    </element>	 	 	 	 	
    <element name="TRAIN_LR_TIRE">
    	<description>Train left rear tire </description>
    </element>	 	 	 	 	
    <element name="TRAIN_OLR_TIRE">
    	<description>Train outer left rear tire </description>
    </element>	 	 	 	 	
    <element name="TRAIN_ILR_TIRE">
    	<description>Train inner left rear tire </description>
    </element>	 	 	 	 	 
    <element name="TRAINING_COMPLETE">
    	<description>Training Complete </description>
    </element>	 	 	 	 		 
    <element name="TIRES_NOT_TRAINED">
    	<description>Tires are not trained</description>
    </element>	 	 	 	 		 
  </enum>  
 ```
 
**VehicleDataResultCode**
```
  <enum name="VehicleDataResultCode">
    <description>Enumeration that describes possible result codes of a vehicle data entry request.</description>
    <element name="SUCCESS" internal_name="VDRC_SUCCESS"/>
    <element name="TRUNCATED_DATA" internal_name="VDRC_TRUNCATED_DATA"/>
    <element name="DISALLOWED" internal_name="VDRC_DISALLOWED"/>
    <element name="USER_DISALLOWED" internal_name="VDRC_USER_DISALLOWED"/>
    <element name="INVALID_ID" internal_name="VDRC_INVALID_ID"/>
    <element name="VEHICLE_DATA_NOT_AVAILABLE" internal_name="VDRC_DATA_NOT_AVAILABLE"/>
    <element name="DATA_ALREADY_SUBSCRIBED" internal_name="VDRC_DATA_ALREADY_SUBSCRIBED"/>
    <element name="DATA_NOT_SUBSCRIBED" internal_name="VDRC_DATA_NOT_SUBSCRIBED"/>
    <element name="IGNORED" internal_name="VDRC_IGNORED"/>
  </enum>
```
### HMI API
```
 <function name="SubscribeVehicleData" messagetype="request">
    <description>
        Subscribes for specific published data items.
        The data will be only sent if it has changed.
        The application will be notified by the onVehicleData notification whenever new data is available.
        To unsubscribe the notifications, use unsubscribe with the same subscriptionType.
    </description>
    <param name="gps" type="Boolean" mandatory="false">
      <description>See GPSData</description>
    </param>
    <param name="speed" type="Boolean" mandatory="false">
      <description>The vehicle speed in kilometers per hour</description>
    </param>
    <param name="rpm" type="Boolean" mandatory="false">
      <description>The number of revolutions per minute of the engine</description>
    </param>
    <param name="fuelLevel" type="Boolean" mandatory="false">
      <description>The fuel level in the tank (percentage)</description>
    </param>
    <param name="fuelLevel_State" type="Boolean" mandatory="false">
      <description>The fuel level state</description>
    </param>
    <param name="instantFuelConsumption" type="Boolean" mandatory="false">
      <description>The instantaneous fuel consumption in microlitres</description>
    </param>
<param name="fuelRange" type="Boolean" mandatory="false">
      <description>Range in KM based on fuel consumption/level</description>
    </param>		 
	 <param name="abs_State" type="Boolean" mandatory="false">
      <description>See ABS_STATE</description>
    </param>	
    <param name="externalTemperature" type="Boolean" mandatory="false">
      <description>The external temperature in degrees celsius</description>
    </param>
    <param name="prndl" type="Boolean" mandatory="false">
      <description>See PRNDL</description>
    </param>
    <param name="tirePressure" type="Boolean" mandatory="false">
      <description>See TireStatus</description>
    </param>
<param name="tirePressureValue" type="Boolean" mandatory="false">
      <description>See TirePressureValue</description>
    </param>	 
    <param name="tpms" type="Boolean" mandatory="false">
      <description>See TPMS</description>
    </param>	 
    <param name="turnSignal" type="Boolean" mandatory="false">
      <description>See TurnSignal</description>
    </param>
    <param name="odometer" type="Boolean" mandatory="false">
      <description>Odometer in km</description>
    </param>
    <param name="beltStatus" type="Boolean" mandatory="false">
      <description>The status of the seat belts</description>
    </param>
    <param name="bodyInformation" type="Boolean" mandatory="false">
      <description>The body information including power modes</description>
    </param>
    <param name="deviceStatus" type="Boolean" mandatory="false">
      <description>The device status including signal and battery strength</description>
    </param>
    <param name="driverBraking" type="Boolean" mandatory="false">
      <description>The status of the brake pedal</description>
    </param>
    <param name="wiperStatus" type="Boolean" mandatory="false">
      <description>The status of the wipers</description>
    </param>
    <param name="headLampStatus" type="Boolean" mandatory="false">
      <description>Status of the head lamps</description>
    </param>
    <param name="engineTorque" type="Boolean" mandatory="false">
      <description>Torque value for engine (in Nm) on non-diesel variants</description>
    </param>
    <param name="accPedalPosition" type="Boolean" mandatory="false">
      <description>Accelerator pedal position (percentage depressed)</description>
    </param>
    <param name="steeringWheelAngle" type="Boolean" mandatory="false">
      <description>Current angle of the steering wheel (in deg)</description>
    </param>
     </function>
 ```
 ```
 <function name="SubscribeVehicleData" messagetype="response">
    <param name="gps" type="Common.VehicleDataResult" mandatory="false">
      <description>See GPSData</description>
    </param>
    <param name="speed" type="Common.VehicleDataResult" mandatory="false">
      <description>The vehicle speed in kilometers per hour</description>
    </param>
    <param name="rpm" type="Common.VehicleDataResult" mandatory="false">
      <description>The number of revolutions per minute of the engine</description>
    </param>
    <param name="fuelLevel" type="Common.VehicleDataResult" mandatory="false">
      <description>The fuel level in the tank (percentage)</description>
    </param>
    <param name="fuelLevel_State" type="Common.VehicleDataResult" mandatory="false">
      <description>The fuel level state</description>
    </param>
    <param name="instantFuelConsumption" type="Common.VehicleDataResult" mandatory="false">
      <description>The instantaneous fuel consumption in microlitres</description>
    </param>
<param name="fuelRange" type="Common.VehicleDataResult" mandatory="false">
      <description>Range in KM based on fuel consumption/level</description>
    </param>		 
	 <param name="abs_State" type="Common.VehicleDataResult" mandatory="false">
      <description>See ABS_STATE</description>
    </param>
    <param name="externalTemperature" type="Common.VehicleDataResult" mandatory="false">
      <description>The external temperature in degrees celsius.</description>
    </param>
    <param name="prndl" type="Common.VehicleDataResult" mandatory="false">
      <description>See PRNDL</description>
    </param>
    <param name="tirePressure" type="Common.VehicleDataResult" mandatory="false">
      <description>See TireStatus</description>
    </param>
<param name="tirePressureValue" type="Common.VehicleDataResult" mandatory="false">
      <description>See TirePressureValue</description>
    </param>	
    <param name="tpms" type="Common.VehicleDataResult" mandatory="false">
      <description>See TPMS</description>
    </param>	
	 <param name="turnSignal" type="Common.VehicleDataResult" mandatory="false">
      <description>See TurnSignal</description>
    </param>
    <param name="odometer" type="Common.VehicleDataResult" mandatory="false">
      <description>Odometer in km</description>
    </param>
    <param name="beltStatus" type="Common.VehicleDataResult" mandatory="false">
      <description>The status of the seat belts</description>
    </param>
    <param name="bodyInformation" type="Common.VehicleDataResult" mandatory="false">
      <description>The body information including power modes</description>
    </param>
    <param name="deviceStatus" type="Common.VehicleDataResult" mandatory="false">
      <description>The device status including signal and battery strength</description>
    </param>
    <param name="driverBraking" type="Common.VehicleDataResult" mandatory="false">
      <description>The status of the brake pedal</description>
    </param>
    <param name="wiperStatus" type="Common.VehicleDataResult" mandatory="false">
      <description>The status of the wipers</description>
    </param>
    <param name="headLampStatus" type="Common.VehicleDataResult" mandatory="false">
      <description>Status of the head lamps</description>
    </param>
    <param name="engineTorque" type="Common.VehicleDataResult" mandatory="false">
      <description>Torque value for engine (in Nm) on non-diesel variants</description>
    </param>
    <param name="accPedalPosition" type="Common.VehicleDataResult" mandatory="false">
      <description>Accelerator pedal position (percentage depressed)</description>
    </param>
    <param name="steeringWheelAngle" type="Common.VehicleDataResult" mandatory="false">
      <description>Current angle of the steering wheel (in deg)</description>
    </param>
  </function>
  ```
