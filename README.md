# zephyr_fusb302b
WIP Zephyr driver for the FUSB302B

## Using
This is a west module. To use it, add the following to your west manifest:
```yaml
manifest:
  remotes:
    - name: ottojo
      url-base: https://github.com/ottojo

  projects:
    - name: zephyr_fusb302b
      remote: ottojo
      revision: main
```

Instantiate the FUSB302B in your devicetree(-overlay), and assign it to a USB-C port:
```c
&i2c2 {
    status = "okay";

    fusb: fusb302b@22 {
        compatible = "fcs,fusb302b";
        status = "okay";
        reg = <0x22>;
    };
};

/ {
 fvbus: fusb302b-vbus {
        compatible = "fcs,fusb302b-vbus";
        fusb302b = <&fusb>;
    };

    ports {
        #address-cells = <1>;
        #size-cells = <0>;

        port1: usbc-port@1 {
            compatible = "usb-c-connector";
            reg = <1>;
            tcpc = <&fusb>;
            vbus = <&fvbus>;
            power-role = "sink";
            sink-pdos = <PDO_FIXED(9000, 100, 0)>;
        };
    };
};
```
