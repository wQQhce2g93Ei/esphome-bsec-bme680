This component facilitates use of the Bosch BME680 sensor with [ESPHome](https://esphome.io) via the closed source [Bosch BSEC library](https://github.com/BoschSensortec/BSEC-Arduino-library) providing additional calculated indoor air quality measurements not available with the [core component](https://esphome.io/components/sensor/bme680.html):

![Home Assistant Entities](ha-screenshot.png)

## Installation
Copy the `bme680_bsec` directory into your ESPHome `custom_components` directory (creating it if it does not exist).

⚠️ **Note**: The Bosch BSEC library is only available for use after accepting its software license agreement. By enabling this component, you are explicitly agreeing to the terms of the [BSEC license agreement](https://www.bosch-sensortec.com/media/boschsensortec/downloads/bsec/2017-07-17_clickthrough_license_terms_environmentalib_sw_clean.pdf).

## Dependencies
The [I2C Bus](https://esphome.io/components/i2c.html#i2c) must be set up in order for this component to work.

## Example configuration
The following configuration shows the sensors and the available optional settings for the component. For each sensor all other options from [Sensor](https://esphome.io/components/sensor/index.html#config-sensor) and [TextSensor](https://esphome.io/components/text_sensor/index.html#base-text-sensor-configuration) are also available for filtering, automation and so on.

```yaml
bme680_bsec:
    # i2c address override (default is 0x76)
    address: 0x77

    # Temperature offset if device is in enclosure and reads too high (default is 0)
    temperature_offset: 0.9

    # Mode for IAQ sensors if device is mobile (default is static)
    iaq_mode: mobile

    # Interval at which to save BSEC state (default is 6 hours)
    state_save_interval: 4h

sensor:
  - platform: bme680_bsec
    temperature:
      name: "BME680 Temperature"
    pressure:
      name: "BME680 Pressure"
    humidity:
      name: "BME680 Humidity"
    gas_resistance:
      name: "BME680 Gas Resistance"
    iaq:
      name: "BME680 IAQ"
    co2_equivalent:
      name: "BME680 CO2 Equivalent"
    breath_voc_equivalent:
      name: "BME680 Breath VOC Equivalent"

text_sensor:
  - platform: bme680_bsec
    iaq_accuracy:
      name: "BME680 IAQ Accuracy"
```

## Multiple sensors
It should be possible to read from multiple sensors as follows:
```yaml
bme680_bsec:
  - id: bme680_one
    address: 0x76
  - id: bme680_two
    address: 0x77

sensor:
  - platform: bme680_bsec
    bme680_bsec_id: bme680_one
    temperature:
      name: "BME680 One Temperature"
  - platform: bme680_bsec
    bme680_bsec_id: bme680_two
    temperature:
      name: "BME680 Two Temperature"
```
