.. InGateway documentation master file, created by
   sphinx-quickstart on Tue Dec 17 17:24:52 2019.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

=====================================
Device Supervisor App说明文档
=====================================

----------------------
InGateway文档网站导航
----------------------

.. raw:: html

    <table style="width:100%;">
      <tr>
        <td>
          <div style="display:inline-block;">
            <h3 style="width:auto;">中文文档</h3>
          </div>
          <ul>
          <li><a href="http://fw.ig.inhand.com.cn/zh_CN/latest/">固件说明文档</a></li>
          <li><a href="http://sdk.ig.inhand.com.cn/zh_CN/latest/">Python开发文档</a></li>
          <li><a href="http://app.ig.inhand.com.cn/zh_CN/latest/">Device Supervisor说明文档</a></li>
          <li><a href="http://docker.ig.inhand.com.cn/zh_CN/latest/">Docker说明文档</a></li>
          </ul>
        </td>
        <td>
          <div style="display:inline-block;">
            <h3 style="width:auto;">英文文档</h3>
          </div>
          <ul>
          <li><a href="http://fw.ig.inhandnetworks.com/en/latest/">firmware doc</a></li>
          <li><a href="http://sdk.ig.inhandnetworks.com/en/latest/">Python doc</a></li>
          <li><a href="http://app.ig.inhandnetworks.com/en/latest/">Device Supervisor doc</a></li>
          <li><a href="http://docker.ig.inhandnetworks.com/en/latest/">Docker doc</a></li>
          </ul>
        </td>
      </tr>
    </table>

Device Supervisor App为用户提供了便捷且可靠的数据采集、数据二次处理和数据上云等功能，支持ISO on TCP、ModbusRTU等多种工业协议解析。如果您想快速实现多种工业协议的数据采集并在本地预处理设备数据，处理过滤后的数据能够经过简单配置即可上传至自建MQTT云平台，那么Device Supervisor将是您理想的选择。

.. toctree::
   :maxdepth: 1
   :caption: 快速入门

   PLC Supervisor用户手册.md
   MobiusPi Python QuickStart-CN.md
   MobiusPi Python QuickStart-EN.md
   API手册.md
   modbus-CN.md
