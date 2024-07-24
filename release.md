#### Release Notes:
* **2.3 Release:**  
  - Updated dashboard to support Grafana 11.1.0. It includes layout and functional changes.  
  - Supports Loki 3.1.0. Config changes to support new schema and removed deprecated options.  
  - Check the [screenshots](https://github.com/ar51an/unbound-dashboard/tree/main/screenshots) dir to view the new layout.  
  - Make sure you install the `musl` package on RaspberryPi. It is required since Grafana 10.2.3.  
    > `sudo apt install musl`
  - Old Loki config will not work. Make sure you use the latest Loki config provided in the release.
  

* **2.2 Release:**  
  Updated dashboard to support Grafana 10.1.0  
  unbound-exporter compiled with Golang 1.21.5  
  Updated documentation to reflect RaspberryPi OS Bookworm changes

* **2.1 Release:**  
  Improved dashboard with new panels.  
  To update from 2.0 to 2.1, simply delete the current dashboard and import the new dashboard json in Grafana.

* **2.0 Release:**  
  Unbound statistics dashboard.  
  Query monitoring with Loki.

* **1.0 Release:**  
  Unbound statistics dashboard.  
  No query logs.  
  Refer to `1.0` branch _README.md_
