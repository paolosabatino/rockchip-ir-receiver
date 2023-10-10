Rockchip IR Driver

This driver mixes the base gpio-ir-receiver driver and the features from the Trust OS for virtual poweroff to allow the remote controller wake up the board from suspend or shutdown when the power key is pressed.

To compile the driver you need the linux kernel headers, then run:

```
make
make install
```

Also you need to add this node in the device tree:
```
	rockchip_ir_receiver: rockchip-ir-receiver {
		compatible = "rockchip-ir-receiver";
		reg = <0x110b0030 0x10>;
		gpios = <&gpio1 RK_PB3 GPIO_ACTIVE_LOW>;
		clocks = <&cru PCLK_PWM>;
		interrupts = <GIC_SPI 50 IRQ_TYPE_LEVEL_HIGH>;
		pinctrl-names = "default", "suspend";
		pinctrl-0 = <&ir_int>;
		pinctrl-1 = <&pwm3_pin>;
		pwm-id = <3>;
		//suspend-is-virtual-poweroff;
		shutdown-is-virtual-poweroff;
		wakeup-source;
		status = "disabled";
	};
```

and please also disable the regular gpio-ir-receiver and pwm3 nodes, otherwise kernel won't allow the drivers to work together because resource clashing.
