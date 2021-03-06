Wi-Fi Mesh 应用框架
===================

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

Wi-Fi Mesh 占用多大内存？是否需要外部 PSRAM ？
----------------------------------------------

  Wi-Fi Mesh 内存占用约 60KB，是否需要外部 PSRAM 取决于应用场景的复杂情况，一般性应用无需外部 PSRAM。

--------------

Wi-Fi Mesh 能否批量 OTA ？
--------------------------

  Wi-Fi Mesh 设备支持批量 OTA。OTA ⽅式为：根节点下载固件，然后再发至其他节点。具体示例请参考: `mupgrade <https://github.com/espressif/esp-mdf/tree/master/examples/function_demo/mupgrade>`__。

--------------

ESP32 支持多少设备进行 Wi-Fi Mesh 组网？
----------------------------------------

  ESP32 支持 1000 个设备进行 Wi-Fi Mesh 组网。若要稳定连接大量设备，建议在每个 Wi-Fi Mesh 网络下，组网设备不超过 512 台设备。

--------------

ESP32 的 Wi-Fi Mesh Router 模式与 No Router 模式有什么区别？
------------------------------------------------------------

  - WiFi Mesh 网络的 Router 模式是根据路由器进行组网，根节点连路由器。
  - No Router 模式是无路由器的场景下进行自组网，此模式下不可与外部数据交互。

--------------

ESP32 的 Wi-Fi Mesh 能否在子设备搜索不到路由器信号时完成组网？
--------------------------------------------------------------

  ESP32 的 Wi-Fi Mesh 能实现：在拥有配置相同 Wi-Fi 的 SSID ，子设备没有搜索到 Wi-Fi 时，子设备也可以连接到根节点。

--------------

ESP32 Wi-Fi Mesh 是否可自动修复网络？
-------------------------------------

  ESP32 Wi-Fi Mesh 可自动修复网络，Wi-Fi Mesh 有检测网络断线的机制。

--------------

使用 ESP32 Wi-Fi Mesh，如何设置可以在没连接到 Wi-Fi 的情况下形成自组网？
------------------------------------------------------------------------

  需要指定一个设备作为 Root 节点，可参考 `说明 <https://github.com/espressif/esp-mdf/blob/master/examples/function_demo/mwifi/README_cn.md>`_ 和 `示例 <https://github.com/espressif/esp-mdf/tree/master/examples/function_demo/mwifi>`_。

--------------

使用 ESP32 进行 Wi-Fi MESH 应用，在组网自动选举根节点时，是否可以指定局部模块进行选举？
---------------------------------------------------------------------------------------

  ESP32 Wi-Fi MESH 可以指定设备为子节点，它将不参与根节点的选举，可实现局部模块进行选举。

--------------

使用 ESP32 进行 Wi-Fi MESH 应用，无路由场景下，多个根节点之间能互发消息吗？
---------------------------------------------------------------------------

  无路由的场景下，多个根节点之间不能互发消息。

--------------

Wi-Fi Mesh 可以批量 OTA 吗？
-------------------------------

  - Wi-Fi mesh 设备可以批量 OTA 的。
  - OTA 的方式是根节点下载固件，然后再发至其他节点。
  - 具体示例请参考 https://github.com/espressif/esp-mdf/tree/master/examples/function_demo/mupgrade

--------------

ESP-MESH APP 端源码如何查询？
-------------------------------

  - iOS 端源码链接: https://github.com/EspressifApp/EspMeshForiOS
  - 安卓端源码链接: https://github.com/EspressifApp/EspMeshForAndroid

--------------

ESP-MESH 是否有无路由方案完成自组网？
--------------------------------------

  Demo 中有 `no-router <https://github.com/espressif/esp-mdf/tree/master/examples/function_demo/mwifi/no_router>`__, 以及 `get-started <https://github.com/espressif/esp-mdf/tree/master/examples/get-started>`__ 两个是无路由方案，可以参考。

--------------

利用 Mwifi 自动组网后，如何获得某个节点的所有潜在父节点的信号强度 (rssi) ？
---------------------------------------------------------------------------

  - 可以通过 ``mwifi_get_parent_rssi()`` 获取其父节点的信号强度
  - 可以参与 https://github.com/espressif/esp-mdf/blob/master/examples/wireless_debug/main/debug_cmd.c#L345 获取其他结点的信号强度

--------------

在 esp-mdf 的 MESH 网络内部，节点之间的通信是什么协议的？
-----------------------------------------------------------

  Mesh 网络内部，是基于数据链路层的自定义协议，即我们核心之一。有 ack 机制，但是没有 超时/重传 机制，如有需求自行可以在应用层添加。

--------------

ESP-MESH 可以将所有的节点都连接至路由上吗？
-------------------------------------------

  - 数据的延时与设备所处层级、网络环境有关系，我们实验室测试一层的延时大约在 10～30 ms，和普通 Wi-Fi 设备的延时差别并不是很大。
  - 如果需要连接路由，建议使用有路由版本的组网方案。固定根节点的方案，如果根节点瘫痪，网络是会出现问题，因此建议可以采用多个根节点进行备份。

--------------

mesh 的 root 节点能否通过 4G 拨号实现联网？
--------------------------------------------

  功能可以实现,目前没有专门针对该场景的应用，可参考 ESP-MDF 中 ``no-router demo``，该 demo 根节点直接通过串口和电脑通讯，可修改成将数据通过 4G 模块进行传输。

--------------

esp_mesh_set_parent 函数成功连接后，断开 AP ，该函数会不断发起重新连接 ，如何设置重新连接次数？
-----------------------------------------------------------------------------------------------

  - 如果你使用自组网方案, ESP-MESH 默认不会重连, 当断开时你需要调用 ``esp_wifi_scan_start`` ,获取可以连接的设备重新设置父节点.参见: `Mesh Manual Networking Example <https://github.com/espressif/esp-idf/tree/4a9f339447cd5b3143f90c2422d8e1e1da9da0a4/examples/mesh/manual_networking>`__
  - 我们推荐你使用自组网的方案进行开发。

--------------

设置按钮后报错： ``phy_init: failed to load RF calibration data``:
---------------------------------------------------------------------

  乐鑫芯片初次上电会有 RF 自校准，并将数据存在 NVS 里，若擦除了该部分，就会出现这行打印，做全校准。

--------------

如何暂停/恢复 Mwifi ？
------------------------

  使用 ``mwifi_stop/mwifi_start`` 暂停/开始 mesh. 

--------------

ESP32-S 无路由 MESH 组网， APP 怎么连接 root 接口的 softAP ？
-------------------------------------------------------------

  MESH 的 AP 不支持 非mesh 设备接入, 你可以使用一个 ESP32 作用 softap。

--------------

MESH 能连到 AP ,但不能 connect 到 AP 上的 TCP SERVER？
---------------------------------------------------------

  请参考 GitHub issue: `mesh -> "with-router" example doesn't work with espressif IDF softAP #71 <https://github.com/espressif/esp-mdf/issues/71>`__

--------------

Mwifi 例程怎么修改网络的 AP connect ，和最大层数？ 通信时的最大带宽和延时是多少？
----------------------------------------------------------------------------------

  .. code-block:: c

    mwifi_init_config_t cfg   = MWIFI_INIT_CONFIG_DEFAULT();
    mwifi_config_t config     = {
        .router_ssid     = CONFIG_ROUTER_SSID,
        .router_password = CONFIG_ROUTER_PASSWORD,
        .mesh_id         = CONFIG_MESH_ID,
        .mesh_password   = CONFIG_MESH_PASSWORD,
    };

  - 连接的 AP 和最大层数在这两个配置变量中可以修改,详细的可以看这个 `文档 <https://docs.espressif.com/projects/esp-idf/en/latest/esp32/api-guides/mesh.html>`_。
  - 通信性能参考： `performance <https://docs.espressif.com/projects/esp-idf/en/latest/esp32/api-guides/mesh.html#performance>`__

--------------

如何获得实时的传感器返回值？
-----------------------------

  我们设备端是一个 HTTP server 所以只能由 APP 发起请求, 你可以采用如下两种方式获取实时数据:

  - 当传感器数据变化时,通过 UDP 通知手机, 来主动请求数据。如果使用我们本地通信的协议,发送如下命令 APP 将主动请求设备数据:

  .. code-block:: c  

    data_type.protocol = MLINK_PROTO_NOTICE;
    ret = mwifi_write(NULL, &data_type, "status", strlen("status"), true);
    MDF_ERROR_CONTINUE(ret != MDF_OK, "<%s> mlink_handle", mdf_err_to_name(ret));

  - 搭建一个服务器 (TCP/MQTT/HTTP server), 与服务器建立 TCP 长连接, 传感器数据变化主动上报

--------------

新节点可能已经安装在设备中，且该设备已经安装在距离 ROOT 节点较远的位置，请问该节点如何加入 mesh 网络？
----------------------------------------------------------------------------------------------------------

  - 您使用的应该是 get-started demo 。为了方便用户测试，该 Demo 是无路由的一种方案，即指定了根节点，所以会出现当 root crash 后，其余设备无法恢复。
  - 可参考 development_kit 中 light demo，该 Demo 可配合 ESP-Mesh App 进行使用（Android 版可在 `官网 <https://www.espressif.com/zh-hans/support/download/apps>`_ 下载，iOS 版可在 App Store 搜索 ESP-Mesh 下载测试）。
  - 该 Demo 示例不指定根节点，由设备自行选举产生，需要配合路由器使用，此种方案下如果 root 出现故障，剩余设备会自动重新完成组网并连上路由，不需要用户干预。

--------------

ESP-Mesh App 源码是否开放？
------------------------------

  - 我们已经将 ESP-Mesh App 源码开放到了 GitHub 上，如下链接：https://github.com/EspressifApp/EspMeshForAndroid
  - 如果在使用中有任何疑问或 Bug，都可以在 GitHub 或者这里进行留言提问，我们都会第一时间处理。
