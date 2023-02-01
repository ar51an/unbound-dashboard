## unbound dashboard
<div align="center">

![unbound](https://img.shields.io/badge/-unbound‑dashboard-D8BFD8?logo=unrealengine&logoColor=3a3a3d)
&nbsp;&nbsp;[![release](https://img.shields.io/github/v/release/ar51an/unbound-dashboard?display_name=release&logo=rstudio&color=90EE90&logoColor=8FBC8F)](https://github.com/ar51an/unbound-dashboard/releases/latest/)
&nbsp;&nbsp;![downloads](https://img.shields.io/github/downloads/ar51an/unbound-dashboard/total?color=orange&label=downloads&logo=github)
&nbsp;&nbsp;![visitors](https://shields-io-visitor-counter.herokuapp.com/badge?page=ar51an.unbound-dashboard&label=visitors&logo=github&color=4883c2)
&nbsp;&nbsp;![license](https://img.shields.io/github/license/ar51an/unbound-dashboard?color=CED8E1)

<br/>

![Dark](https://user-images.githubusercontent.com/11185794/213861987-28fe6e43-4021-454d-95e8-58e878ddbe89.png)

<br/>

![Light](https://user-images.githubusercontent.com/11185794/213861993-465a2ae2-5ee5-40cb-b60e-e845f8891ce2.png)
</div>
<div align="justify">

### Summary
🔸 Unbound dashboard in `Grafana`  
🔸 `Prometheus` time series data source  
🔸 Unbound metrics exporter in `Go`  
🔸 Unbound `setup` is available at [unbound-redis](https://github.com/ar51an/unbound-redis)  
🔸 [What's next](https://github.com/ar51an/unbound-dashboard#whats-next)

#### Specs:
> |Grafana|Prometheus|Go      |OS                           |HW                      |
> |:------|:---------|:-------|:----------------------------|:-----------------------|
> |`9.3.1`|`2.24.1`  |`1.19.4`|`raspios-bullseye-arm64-lite`|`Raspberry Pi 4 Model B`|

#
### Steps
&nbsp;&nbsp;🔸 Grafana ➜ Prometheus ➜ Unbound Exporter ➜ Import Dashboard
#### ❯ Grafana
* **Download:**  
  There are 2 versions **OSS** and **Enterprise**. OSS version is more than enough. Enterprise version installs too many extra packages (like unattended-upgrades and more). Below cmd downloads _Grafana OSS_ for arm64.
  > `wget https://dl.grafana.com/oss/release/grafana_9.3.1_arm64.deb`
  
* **Install:**
  > `sudo dpkg -i grafana_9.3.1_arm64.deb`

* **Start:**
  > `sudo systemctl daemon-reload`  
  > `sudo systemctl enable grafana-server`  
  > `sudo systemctl start grafana-server`

* **UI:**
  > `http://<RP-IP>:3000/`  
  > Default user/pass ➟ admin/admin

#
#### ❯ Prometheus
* **Install:**
  > `sudo apt install prometheus`

* **Config:**  
  Enable _unbound-exporter_ scraping in prometheus. A trimmed down prometheus config, `prometheus.yml` is available in the release. Take a backup of existing _prometheus.yml_, if you are interested in the default config. Copy `prometheus.yml` from the release to `/etc/prometheus/` dir.
  
  > `ℹ️` **Note:**  
  > Provided `prometheus.yml` has only _unbound-exporter_ metric collection enabled. Default metric collection for _node_ and _prometheus_ exporters are removed. Scraping interval is set to _5m_.
  
* **Remove Node Exporter:**  
  Node exporter exports machine metrics. It is installed as part of prometheus pkg and runs as systemd service. It is not needed for _unbound-dashboard_. Unless you are already using it, remove node exporter. Below cmd will remove 8 node-exporter related pkgs. 
  > **Remove:**  
  > `sudo apt --purge autoremove prometheus-node-exporter`  
  > **Disable Scrape Config:**  
  > Provided `prometheus.yml` has _node_ exporter scrapping config removed. 

* **UI:**
  > `http://<RP-IP>:9090/`

#
#### ❯ Unbound Exporter
* I wrote my own exporter in `Go`. It is more efficient and tailored for this dashboard. A prebuilt binary (for arm64) is available in the release. Source code is available at [unbound-exporter](https://github.com/ar51an/unbound-exporter).

* **Config:**  
  Edit `/etc/unbound/unbound.conf`
  * Enable extended stats. Add option under **server:** tag  
    > `extended-statistics: yes`

  * Enable Unix domain socket for collecting stats. It is faster than default TCP. Add options under **remote-control:** tag  
    > `control-interface: "/var/run/unbound.sock"`  
    > `control-use-cert: no`

* **Install:**  
  Copy `unbound-exporter` binary from the release to `/usr/local/bin/` dir. Make sure it is under ownership of root and executable.
  > **Change ownership [If needed]:**  
  > `sudo chown root:root /usr/local/bin/unbound-exporter`  
  > **Make it executable [If needed]:**  
  > `sudo chmod +x /usr/local/bin/unbound-exporter`

* **Service:**  
  Create service to automatically run _unbound-exporter_ at startup. Copy `prometheus-unbound-exporter.service` from the release to `/etc/systemd/system/` dir. Make sure it is under the ownership of root. Enable and start the service.
  > **Change ownership [If needed]:**  
  > `sudo chown root:root /etc/systemd/system/prometheus-unbound-exporter.service`  
  > **Enable:**  
  > `sudo systemctl enable prometheus-unbound-exporter`  
  > **Start:**  
  > `sudo systemctl start prometheus-unbound-exporter`  
  > **Status:**  
  > `sudo systemctl status prometheus-unbound-exporter`

  > `ℹ️` **Note:**  
  > Provided `prometheus-unbound-exporter.service` passes 2 paramaters. Blocklist file path and Unbound unix domain socket URI. Change them accordingly if you are using different path and name.

* **Usage:**
  > `unbound-exporter -h`  
  >
  > ![Usage](https://user-images.githubusercontent.com/11185794/213899374-457728c8-c92a-4280-b638-5116d165f934.png)

#
#### ❯ Import Dashboard
* Open Grafana UI ➟ `http://<RP-IP>:3000/`

  * Click `Data Sources` under `Configuration`. Click `Add data source` select `Prometheus`. Add below options:  
    > _Default_ ➟ `Switch On`  
    > Add _URL_ ➟ `http://localhost:9090`  
    > Add _Scrape interval_ ➟ `5m`  
    > Hit ➟ `Save & test`

  * Dashboard, `unbound-dashboard.json` is available in the release. Click `Import` under `Dashboards`. Click `Upload JSON file`. Select `unbound-dashboard.json`. Add below options:  
    > _Folder_ ➟ `General`  
    > Select _Prometheus_ ➟ `Data Source`  
    > Hit ➟ `Import`

#
#### ❯ `ℹ️` Tips & Notes
* **Grafana:**  
  How to ➟ Change grafana `landing page` to unbound dashboard **&** Switch between `Dark` (default) and `Light` theme.

  > Open Grafana UI ➟ `http://<RP-IP>:3000/`  
  > Click `Preferences` under `Configuration`  
  > Select `General/Unbound` in `Home Dashboard` drop down  
  > Change `UI Theme`

  There is an additional panel in the dashboard at the top right, not visible in the preview. It shows _unbound-exporter_ status and may be beneficial. If you are not interested in that simply remove it. Screenshot below:  
  > ![Metrics](https://user-images.githubusercontent.com/11185794/213899342-9065ecbc-11c8-4abf-9090-a2d1a27be0ea.png)

* **Prometheus:**  
  How to ➟ `Remove` time series (metrics) collected by prometheus instantly for fresh start **&** Reduce prometheus journal `logging`.
  > Enable admin API:  
  > `sudo nano /etc/default/prometheus`  
  > Add at the top:
  > `ARGS="--web.enable-admin-api --log.level=warn"`  
  > Save & Exit

  > To delete all metrics of specific exporter add `job_name` as last argument in delete cmd:  
  > Delete:  
  > `curl -X POST -g 'http://localhost:9090/api/v1/admin/tsdb/delete_series?match[]={job="node"}'`  
  > Purge:  
  > `curl -X POST -g 'http://localhost:9090/api/v1/admin/tsdb/clean_tombstones'`  
  > Restart:  
  > `sudo systemctl restart prometheus`
  
  Check exporter status in prometheus.
  > Open Prometheus UI ➟ `http://<RP-IP>:9090/`  
  > Select `Status > Targets`  
  >
  > ![Status](https://user-images.githubusercontent.com/11185794/213899326-fcc86252-53a1-42b1-819d-b82361069d22.png)

#
#### ❯ Cmds
&nbsp;&nbsp;🔸 Few handy cmds
* **Grafana:**  
  > Status:  
  > `sudo systemctl status grafana-server`  
  > Tail Log:  
  > `sudo journalctl -u grafana-server -n 200 -f`  
  > Remove:  
  > `sudo apt --purge autoremove grafana`

* **Prometheus:**  
  > Status:  
  > `sudo systemctl status prometheus`  
  > Tail Log:  
  > `sudo journalctl -u prometheus -n 200 -f`

#
### What's Next
Add _Loki_ to the stack and create live monitoring dashboard.
<details>
  <summary>Spoiler Alert</summary>
  
  <sub>**_* Not included in the current release_**</sub>
  
  ![Future-Dark](https://user-images.githubusercontent.com/11185794/215806047-ae9c7d3b-f2b3-4822-9eb8-a439506b4a0b.png)
</details>
</div>
