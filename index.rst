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
          <li><a href="http://manual.ig.inhand.com.cn/zh_CN/latest/">InGateway用户手册</a><span style="margin-left:10px; font-size:14px" >（网关用户手册、快速使用手册等文档）</span></li>
          <li><a href="http://sdk.ig.inhand.com.cn/zh_CN/latest/">Python开发者文档</a><span style="margin-left:10px; font-size:14px" >（Python二次开发指南、API手册等文档）</span></li>
          <li><a href="http://app.ig.inhand.com.cn/zh_CN/latest/">Device Supervisor说明文档</a></li>
          <li><a href="http://docker.ig.inhand.com.cn/zh_CN/latest/">Docker用户手册</a></li>
          </ul>
        </td>
        <td>
          <div style="display:inline-block;">
            <h3 style="width:auto;">English Documentation</h3>
          </div>
          <ul>
          <li><a href="http://manual.ig.inhandnetworks.com/en/latest/">InGateway User Manual</a><span style="margin-left:10px; font-size:14px" >(InGateway user manual, quick user manual and others)</span></li>
          <li><a href="http://sdk.ig.inhandnetworks.com/en/latest/">Python Developer Documentation</a><span style="margin-left:10px; font-size:14px" >(Python secondary development guide, API manual and others)</span></li>
          <li><a href="http://app.ig.inhandnetworks.com/en/latest/">Device Supervisor User Manual</a></li>
          <li><a href="http://docker.ig.inhandnetworks.com/en/latest/">Docker User Manual</a></li>
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
   xx.md
