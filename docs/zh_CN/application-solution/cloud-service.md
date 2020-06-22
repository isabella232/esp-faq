# 云服务

<style>
body {counter-reset: h2}
  h2 {counter-reset: h3}
  h2:before {counter-increment: h2; content: counter(h2) ". "}
  h3:before {counter-increment: h3; content: counter(h2) "." counter(h3) ". "}
  h2.nocount:before, h3.nocount:before, { content: ""; counter-increment: none }
</style>

---

## OTA 升级有没有相关 demo 参考？

- ESP8266 OTA 请参考 https://github.com/espressif/ESP8266_RTOS_SDK/tree/master/examples/system/ota
- ESP32 及 ESP32-S2 OTA 请参考 https://github.com/espressif/esp-idf/tree/master/examples/system/ota

---

## ESP8266 NONOS SDK OTA 为何云端需要 "user1.bin" 和 "user2.bin" 两个 bin 文件？

- 这是由于 ESP8266 Cache 偏移仅支持 1MB 为单位偏移，当分区设置为 512+512 模式时，user1.bin 与 user2.bin 指令地址并不相同，不可以相互替换。
- 所以同一个版本需要云端放置两个不同版本的固件用于设备升级。1024+1024 模组由于分区大小满足 Cache 偏移，所以不受该限制。