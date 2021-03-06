蓝牙
====

:link_to_translation:`en:[English]`

.. raw:: html

   <style>
   body {counter-reset: h2}
     h2 {counter-reset: h3}
     h2:before {counter-increment: h2; content: counter(h2) ". "}
     h3:before {counter-increment: h3; content: counter(h2) "." counter(h3) ". "}
     h2.nocount:before, h3.nocount:before, { content: ""; counter-increment: none }
   </style>

--------------

移植例程 gatt_server 出现头文件不存在的错误？
-----------------------------------------------

  移植例程 gatt_server 出现错误提示 ``fatal error: esp_gap_ble_api.h: No such file or directory``，但头文件已经包含此文件。

  - 检查 sdkconfig，是否未从例程中移植 sdkconfig.defaults。通常 SDK 中蓝牙默认关闭不编译，需要配置开启。
  - 如果使用 cmake 需要将例程中 CMakeLists.txt 文件内的链接配置一同复制。

--------------

ESP32 可以支持 Bluetooth® LE 5.0 吗？
---------------------------------------------

  ESP32 硬件不支持 Bluetooth LE 5.0，支持 Bluetooth LE 4.2。

  ESP32 目前通过了 Bluetooth LE 5.0 的认证，但 Bluetooth LE 5.0 的新功能 ESP32 不支持（未来会有其它芯片支持 Bluetooth LE 5.0 全部新功能）。

--------------

为什么 Bluetooth® LE 开始广播后，有些手机扫描不到？
------------------------------------------------------------

  - 需确认手机是否支持 Bluetooth LE 功能：有的手机在“设置” -> “蓝牙”中只显示默认的经典蓝牙，Bluetooth LE 广播会被手机过滤掉。
  - 建议使用专用的 Bluetooth LE App 来调试 Bluetooth LE 功能。例如，苹果手机可以使用 LightBlue App。
  - 需确认广播包的格式符合规范，手机一般会对不符合格式的广播包进行过滤，只有格式正确的才能被显示出来。

--------------

ESP32 能否使用蓝牙进行 OTA？
----------------------------------

  可以使用蓝牙进行 OTA。如果是用 Bluetooth®，可以基于 `bt_spp_acceptor <https://github.com/espressif/esp-idf/tree/master/examples/bluetooth/bluedroid/classic_bt/bt_spp_acceptor>`_ 
  和 `bt_spp_initiator <https://github.com/espressif/esp-idf/tree/master/examples/bluetooth/bluedroid/classic_bt/bt_spp_initiator>`_ 修改；如果是用 Bluetooth LE，
  可以基于 `ble_spp_server <https://github.com/espressif/esp-idf/tree/master/examples/bluetooth/bluedroid/ble/ble_spp_server>`_ 
  和 `ble_spp_client <https://github.com/espressif/esp-idf/tree/master/examples/bluetooth/bluedroid/ble/ble_spp_client>`_ 修改。

--------------

ESP32 的蓝牙双模如何共存及使用？
------------------------------------

  ESP32 支持的双模蓝牙并没有特殊的地方，不需要做复杂的配置或调用即可使用。从开发者的⻆度来看，Bluetooth® LE 调用 Bluetooth LE 的 API，经典蓝牙调用经典蓝牙的 API。

  经典蓝牙与 Bluetooth LE 共存说明可参考文档 `ESP32 Bluetooth & Bluetooth LE 双模蓝牙共存说明 <https://www.espressif.com/sites/default/files/documentation/btble_coexistence_demo_cn.pdf>`_。

--------------

ESP32 的 Bluetooth® LE 吞吐量是多少？
---------------------------------------------

  - ESP32 的 Bluetooth LE 吞吐率取决于各种因素，例如环境干扰、连接间隔、MTU 大小以及对端设备性能等等。
  - ESP32 板子之间的 Bluetooth LE 通信最大吞吐量可达 700 Kbps，约 90 KB/s，具体可以参考 ESP-IDF 中的 ble_throughput example。

--------------

ESP32 是否支持 BT4.2 DLE (Data Length Extension)？
---------------------------------------------------------

  支持。ESP-IDF 所有版本都支持 Bluetooth® 4.2 DLE，暂无对应的 sample code，可直接调相关接口实现，参见：`esp_ble_gap_set_pkt_data_len <https://docs.espressif.com/projects/esp-idf/en/latest/esp32/api-reference/bluetooth/esp_gap_ble.html?highlight=esp_ble_gap_set_pkt_data_len#_CPPv428esp_ble_gap_set_pkt_data_len13esp_bd_addr_t8uint16_t>`_。

--------------

ESP32 的蓝⽛和 Wi-Fi 如何共存？
----------------------------------

  在 menuconfig 中，有个特殊选项 “Software controls WiFi/Bluetooth coexistence”，⽤于通过软件来控制 ESP32 的蓝⽛和 Wi-Fi 共存，可以平衡 Wi-Fi、蓝⽛控制 RF 的共存需求。

  - 如果使能 ``Software controls WiFi/Bluetooth coexistence`` 选项，Bluetooth® LE scan 间隔不应超过 ``0x100 slots`` （约 160 ms）。若只是 Bluetooth LE 与 Wi-Fi 共存，则开启这个选项和不开启均可正常使⽤。但不开启的时候需要注意 Bluetooth LE scan window 应大于 150 ms，并且 Bluetooth LE scan interval 尽量⼩于 500 ms。 
  - 若经典蓝⽛与 Wi-Fi 共存，则建议开启这个选项。

--------------

ESP32 蓝牙的兼容性测试报告如何获取？
------------------------------------

  请联系 sales@espressif.com 获得兼容性测试报告。

--------------

ESP32 蓝牙的发射功率是多少？
--------------------------------

  ESP32 蓝牙的发射功率有 9 档，对应功率 -12 ~ 12 dBm，间隔 3 dBm 一档。控制器软件对发射功率进行限制，根据产品声明的对应功率等级选取档位。

--------------

ESP32 可以实现 Wi-Fi 和 Bluetooth® LE 桥接的功能吗？
--------------------------------------------------------------------

  可以实现，这个属于应⽤层开发：可以通过 Bluetooth LE 获取数据，由 Wi-Fi 转出去。可参考 `Wi-Fi 和蓝⽛共存的 demo <https://github.com/espressif/esp-idf/tree/release/v4.0/examples/bluetooth/esp_ble_mesh/ble_mesh_wifi_coexist>`_，修改为⾃⼰的应⽤即可。

--------------

ESP32 的 Bluetooth® LE 工作电流是多少？
------------------------------------------------

  +--------------------------------------------------------------+---------------+---------------+----------+
  | 电流                                                         | 最大值 (mA)   | 最小值 (mA)   | 平均值   |
  +==============================================================+===============+===============+==========+
  | Advertising: Adv Interval = 40ms                             | 142.1         | 32            | 42.67    |
  +--------------------------------------------------------------+---------------+---------------+----------+
  | Scanning: Scan Interval = 160ms,Window = 20ms                | 142.1         | 32            | 44.4     |
  +--------------------------------------------------------------+---------------+---------------+----------+
  | Connection(Slave): Connection Interval = 20ms, Iatency = 0   | 142.1         | 32            | 42.75    |
  +--------------------------------------------------------------+---------------+---------------+----------+
  | Connection(Slave): Connection Interval = 80ms, Iatency = 0   | 142.1         | 32            | 35.33    |
  +--------------------------------------------------------------+---------------+---------------+----------+

--------------

ESP32 支持哪些 Bluetooth® LE Profile？
--------------------------------------------

  目前支持完整的 GATT/SMP 等基础模块，支持自定义配置；已经实现的配置有 Bluetooth LE HID（设备端）、电池、DIS、Blu-Fi（蓝牙配网）等。

--------------

如何使用 ESP32 蓝牙连接手机播放音乐？
-------------------------------------

  用手机通过蓝牙播放音乐，ESP32 用作 A2DP Sink。A2DP Sink Demo 只是通过手机获取 SBC 编码的数据流，若要播放出声音，需要做编解码转换，通常需要编解码器、数/模转换器、扬声器等模块。

--------------

ESP32 的 SPP 性能如何？
--------------------------

  使用两块 ESP32 开发板对跑 SPP，单向吞吐量量可达 1900 Kbps，约 235 KB/s，已接近规范里的理论值。

--------------

ESP32 的 Bluetooth® LE 传输速率最大为多少？
-----------------------------------------------------

  屏蔽箱测试 Bluetooth LE 传输速率可以达到 800 Kbps。

--------------

ESP32 Bluetooth® LE 如何进入 Light-sleep 模式呢？
---------------------------------------------------------

  硬件上需要外加 32 kHz 的外部晶振，否则 Light-sleep 模式不会生效。

  软件上（SDK4.0 以及以上版本才会支持）在 menuconfig 中需要使能以下配置：

  - Power Management:| menuconfig ---> Component config ---> Power management --->[*] Support for power management

  - Tickless Idle:| menuconfig ---> Component config ---> FreeRTOS --->[*] Tickless idle support (3) Minimum number of ticks to enter sleep mode for (NEW)

  .. note:: 需使能 "Tickless idle" 功能使 ESP32 自动进入 Light-sleep 模式。如果在 3 个节拍（默认）内无任务运行，则 FreeRTOS 将进入 Light-sleep 模式，即 100 Hz 节拍率下为 30 ms。若您希望缩短 Light-sleep 模式的持续时间，可通过将 FreeRTOS 节拍率调高来实现，如：menuconfig ---> Component config ---> FreeRTOS ->(1000) Tick rate (Hz)。

  - | Configure external 32.768Hz crystal as RTC clock source :| menuconfig ---> Component config ---> ESP32-specific --->RTC clock source (External 32kHz crystal)[*] Additional current for external 32kHz crystal

  .. note:: "additional current" 选项为解决 ESP32 晶振失败的替代方案。请在您使用外部 32 kHz 晶体时使能该选项。该硬件问题将在下一个 ECO 芯片中解决。

  - | Enable Bluetooth modem sleep with external 32.768kHz crystal as low power clock :| menuconfig ---> Component config ---> Bluetooth ---> Bluetooth controller ---> MODEM SLEEP Options --->[*] Bluetooth modem sleep

--------------

选择 ESP32 芯片实现蓝牙配网的方式，是否有文档可以提供参考？
-----------------------------------------------------------

  蓝牙配网说明可参考 `ESP32 Blufi <https://docs.espressif.com/projects/esp-idf/zh_CN/latest/esp32/api-guides/blufi.html?highlight=blufi>`_。蓝牙配网示例可以参考 `Blufi <https://github.com/espressif/esp-idf/tree/master/examples/bluetooth/bluedroid/ble/blufi>`_。

--------------

ESP32 经典蓝牙 SPP 的传输速率能达到多少？
-----------------------------------------

  在开放环境下，双向同时收发，实测可达到 1400+ Kbps 到 1590 Kbps（此数据仅作为参考，实际情况建议客户根据应用环境实测）。

--------------

ESP32 的蓝牙是否兼容 Bluetooth® ver2.1 + EDR 协议？
---------------------------------------------------------------------

  兼容。ESP32 的蓝牙是向下兼容的，您可以使用官方的 `蓝牙示例 <https://github.com/espressif/esp-idf/tree/master/examples/bluetooth>`_ 进行测试。

--------------

ESP32 支持多少蓝牙客户端连接？
------------------------------

  Bluetooth® LE Server 最大支持 9 个客户端连接，应用中需查看配置参数 ble_max_conn。测试稳定连接为 3 个客户端。

--------------

ESP32 如何获取蓝牙设备的 MAC 地址？
------------------------------------

  可调用 `esp_bt_dev_get_address(void); <https://github.com/espressif/esp-idf/blob/f1b8723996d299f40d28a34c458cf55a374384e1/components/bt/host/bluedroid/api/include/api/esp_bt_device.h#L33>`_ API 来获取蓝牙配置的 MAC 地址。也可以调用 `esp_err_t esp_read_mac(uint8_t* mac,esp_mac_type_ttype); <https://github.com/espressif/esp-idf/blob/6c17e3a64c02eff3a4f726ce4b7248ce11810833/components/esp_system/include/esp_system.h#L233>`_ API 获取系统预设的分类 MAC 地址。

--------------

ESP32 SDK 中默认的蓝牙的发射功率是多少？
-------------------------------------------------

  - ESP32 SDK 中默认情况下使用功率级别 4，相应的发射功率为 0 dBm。
  - ESP32 蓝牙的发射功率从 0 到 7，共有 8 个功率级别，发射功率范围从 –12 dBm 到 9 dBm。功率电平每增加 1 时，发射功率增加 3 dBm。

--------------

ESP32 Wi-Fi Smartconfig 配网和 Bluetooth® LE Mesh 可以同时使用吗？
-------------------------------------------------------------------

  不推荐同时打开。
  
  - Smartconfig 需要一直收配网数据，所以会一直占用天线，如果和 Bluetooth LE Mesh 共同使用，会导致失败率非常高。

  - Bluetooth LE Mesh 可以和 Blufi 同时使用，所以推荐配网方式选择 Blufi 配网。

--------------

ESP32 的经典蓝牙工作电流是多少？
---------------------------------------

  A2DP (Single core CPU 160 Mhz，DFS = false，commit a7a90f)

  +--------------------------------------------------------------+---------------+---------------+----------+
  | 电流                                                         | 最大值 (mA)   | 最小值 (mA)   | 平均值   |
  +==============================================================+===============+===============+==========+
  | Scanning                                                     | 106.4         | 30.8          | 37.8     |
  +--------------------------------------------------------------+---------------+---------------+----------+
  | Sniff                                                        | 107.6         | 31.1          | 32.2     |
  +--------------------------------------------------------------+---------------+---------------+----------+
  | Play Music                                                   | 123           | 90.1          | 100.4    |
  +--------------------------------------------------------------+---------------+---------------+----------+

------------

ESP32 如何修改蓝牙的发射功率？
---------------------------------------------------

  蓝牙发射功率可通过 esp_ble_tx_power_set(); 函数进行设置，可参见 `esp_bt.h <https://github.com/espressif/esp-idf/blob/c77c4ccf6c43ab09fd89e7c907bf5cf2a3499e3b/components/bt/include/esp_bt.h>`_。

--------------

ESP32 的 Bluetooth® LE 蓝牙配网兼容性如何？是否开源？
-----------------------------------------------------------------

  - ESP32 的蓝牙配网，简称 Blu-Fi 配网，兼容性与 Bluetooth LE 兼容性一致，测试过苹果、华为、小米、OPPO、魅族、 一加、中兴等主流品手机，兼容性良好。
  - 目前 Blu-Fi 协议及手机应用部分的代码不开源。

--------------

ESP32 运行 bt_spp_acceptor 例程时， IOS 设备无法扫描到 ESP32 设备是什么原因？
-----------------------------------------------------------------------------

  - 苹果开放的蓝牙有：A2DP、HID 的 keyboard、avrcp 以及 SPP（需要 MFI）和高端的 Bluetooth® LE 外加给予 Bluetooth LE 的 ANCS。
  - 如果 IOS 设备想要和对端设备通过 SPP 通信，那么对端设备的 SPP 需要通过 MFI 认证。目前 ESP32 SPP 没有通过 MFI 认证，因此 IOS 设备无法扫描到 ESP32。

--------------

ESP32 Bluetooth® LE/Bluetooth® Secure Simple Pairing (SSP) 与 legacy pairing 安全性对比？
----------------------------------------------------------------------------------------------------------

  - Secure Simple Pairing (SSP) 比 legacy pairing 更加安全。
  - legacy pairing 使用对称加密算法，Secure Simple Pairing (SSP) 使用的是非对称加密算法。

--------------

ESP32 Bluetooth® LE MTU 大小如何确定？
----------------------------------------

  - ESP32 端蓝牙 Bluetooth LE 默认的 MTU 为 23 字节，最大可以设置为 517 字节。
  - 手机端的 MTU 由手机端自行定义，最终通信的 MTU 选择两端 MTU 较小的那一个。

--------------

ESP32 Bluetooth® LE 模式下广播数据时遇到 "W (17370) BT_BTM: data exceed max adv packet length" 如何解决？
--------------------------------------------------------------------------------------------------------------------------

  - 出现该警告的原因是广播的数据长度超出最大广播数据包长度限制。
  - 广播有效载荷数据长度最大为 31 字节。如果超过 31 字节，那么蓝牙协议栈会丢弃一些数据，并且给出警告。
  - 如果需要广播的数据长度超出最大限制，超出的数据可以放在扫描响应数据包 (scan response data) 中。

--------------

ESP32 Bluetooth® LE 能否同时支持主从模式，作 gatt server 的同时，也可作为 gatt client 接收其他设备的广播数据？
-----------------------------------------------------------------------------------------------------------------------------------

  - 支持，可参考例程 `gattc_gatts_coex <https://github.com/espressif/esp-idf/tree/master/examples/bluetooth/bluedroid/coex/gattc_gatts_coex>`_。

--------------

ESP32 的 Bluetooth® LE 连接数 6 个以上会有哪些风险？
---------------------------------------------------------------

  - 通常要根据具体的应用决定，在常规场景下，ESP32 Bluetooth LE 连接 3 个设备可以稳定通信。
  - Bluetooth LE 的最大连接数未有一个准确的值，在多个 Bluetooth LE 设备同时连接的的时候，RF 是分时复用的，需要设计者保证每一个设备不会长时间占用导致其他设备超时断开。
  - 连接参数里面有 connection interval、connection window、latency、timeout, 可以在 ``latency`` 以内的不应答，但是若超过 ``timeout`` 的时间，将会导致连接断开。
  - 假设配置参数中 ``interval`` 是 100，window 是 5 , Wi-Fi 关闭时，将会连接较多设备。如果用了 Wi-Fi，或者 ``interval`` 设置的太小，将只能连接较少设备。
  - 当 Bluetooth LE 支持多的设备并发连接时，RF 的 solt 管理出错概率会增加，所以 Bluetooth LE 设备连接较多时，需要针对具体场景调试。

----------------

使用 ESP32 设备作为 Bluetooth® LE 主机，最大支持多少台从机设备进行连接？
--------------------------------------------------------------------------------------

  - ESP32 的 Bluetooth LE 最大支持 9 台从机设备进行连接，建议连接数量 3 个设备以内。
  - 可通过 menuconfig -> Component config -> Bluetooth -> Bluetooth controller -> BLE MAX Connections 进行配置。

----------------

ESP32 如何通过 Bluetooth® BR/EDR 传文件？
------------------------------------------------------------

  - 可参考链接 `classic bt <https://github.com/espressif/esp-idf/tree/master/examples/bluetooth/bluedroid/classic_bt>`_ 下的 ``bt_spp_acceptor`` 或者 ``bt_spp_initiator`` 例程。

---------------

ESP32 下载 ESP_SPP_SERVER 例程，如何修改蓝牙设备名称？
-----------------------------------------------------------------

  - 蓝牙设备名称可以通过修改 ``adv`` 参数实现：

  .. code-block:: text

    static const uint8_t spp_adv_data[23] = {
      0x02,0x01,0x06,
      0x03,0x03,0xF0,0xAB,
      0x0F,0x09,0x45,0x53,0x50,0x5f,0x53,0x50,0x50,0x5f,0x53,0x45,0x52,0x56,0x45,0x52};

  - 第三行 0x0F 表示后续数据长度为 15，0x09 表示数据类型（固定不变），0x45 开始后续数据代表设备名称对应的 ASCII 码（默认为：BLE_SPP_SERVER)。
  