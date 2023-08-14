# 教程清单

星火 1 号 SDK 目录结构如下

```
|-- README.md
|-- docs
|-- libraries
|   |-- Board_Drivers
|   |-- HAL_Drivers
|   `-- STM32F4xx_HAL
|-- projects
|-- rt-thread
`-- sdk-bsp-rt-spark.yaml
```

- docs：星火 1 号原理图、用户手册等
- libraries：STM32F4 固件库、通用外接驱动程序
- projects：示例项目文件夹，包括各种示例代码
- rt-thread：rt-thread 源代码
- sdk-bsp-rt-spark.yaml：描述 星火 1 号 的硬件信息

### 教程

项目文件夹按照不同的学习阶段添加了数字编号，如 01_kernel，02_basic_led_blink，分别表示两个不同的学习阶段，编号小则内容相对简单，建议初学者按照数字编号大小顺序进行学习，循序渐进掌握 RT-Thread。

示例项目文件夹和相应学习阶段如下所示：


| 阶段                     | 序号 | 项目实现功能                      | 项目文件夹名称           |
| ------------------------ | ---- | :-------------------------------- | ------------------------ |
| 01，入门 RT-Thread 内核  | 01   | RT-Thread 内核学习                | 01_kernel                |
| 02，简单外设的使用       | 02   | LED 闪烁例程                      | 02_basic_led_blink       |
|                          | 03   | RGB LED 例程                      | 02_basic_rgb_led         |
|                          | 04   | 按键输入例程                      | 02_basic_key             |
|                          | 05   | 蜂鸣器和 LED 控制例程             | 02_basic_irq_beep        |
|                          | 06   | 红外遥控例程                      | 02_basic_ir              |
|                          | 07   | RTC 和 RTC 闹钟的使用例程         | 02_basic_rtc             |
| 03，板载外设模块的使用   | 08   | LCD 显示例程                      | 03_driver_lcd            |
|                          | 09   | AHT10 温湿度传感器例程            | 03_driver_temp_humi      |
|                          | 10   | AP3216C 接近与光强传感器例程      | 03_driver_als_ps         |
|                          | 11   | ICM20608 六轴传感器例程           | 03_driver_axis           |
|                          | 12   | CAN 通信例程                      | 03_driver_can            |
|                          | 13   | LED MATRIX 闪烁例程               | 03_driver_led_matrix     |
| 04，RT-Thread 组件的使用 | 14   | USB 鼠标例程                      | 04_component_usb_mouse   |
|                          | 15   | TF 卡文件系统例程                 | 04_component_fs_tf_card  |
|                          | 16   | 低功耗例程                        | 04_component_pm          |
|                          | 17   | Flash 分区管理例程                | 04_component_fal         |
|                          | 18   | KV 参数存储例程                   | 04_component_kv          |
|                          | 19   | SPI Flash 文件系统例程            | 04_component_fs_flash    |
| 05，IoT 相关组件的使用   | 20   | WiFi 管理例程                     | 05_iot_wifi_manager      |
|                          | 21   | MQTT 协议通信例程                 | 05_iot_mqtt              |
|                          | 22   | HTTP Client 功能实现例程          | 05_iot_http_client       |
|                          | 23   | MEMBEDTLS 例程                    | 05_iot_mbedtls           |
|                          | 24   | Ymodem 协议固件升级例程           | 05_iot_ota_ymodem        |
|                          | 25   | HTTP 协议固件升级例程             | 05_iot_ota_http          |
|                          | 26   | 网络小工具集使用例程              | 05_iot_netutils          |
|                          | 27   | 中国移动 OneNET 云平台接入例程    | 05_iot_cloud_onenet      |
|                          | 28   | 阿里云物联网平台接入例程          | 05_iot_cloud_ali_iotkit  |
|                          | 29   | 使用 Web 服务器组件：WebNet       | 05_iot_web_server        |
| 06，综合 Demo 学习       | 30   | LVGL 例程                         | 06_demo_lvgl             |
|                          | 31   | MicroPython 例程                  | 06_demo_micropython      |
|                          | 32   | 板载 LED matrix 和 RS485 驱动例程 | 06_demo_rs485_led_matrix |
|                          | 33   | nes 模拟器实验                    | 06_demo_nes_simulator    |
|                          | 34   | 开发板综合 Demo（出厂 Demo）      | 06_demo_factory          |



