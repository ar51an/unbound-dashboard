#### Release Notes:
* **2.3 Release:**  
  - Updated dashboard to support Grafana 11.1.0. Includes layout and functional changes.  
  - Added support for Loki 3.1.0. Config changes to support new schema and removed deprecated options.  
  - Use the v2.3 release Loki config with Loki 3.1.0.
  - `musl` package is required. Check readme for instructions.  
  
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
