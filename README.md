## unbound dashboard
<div align="center">

![unbound](https://img.shields.io/badge/-unbound‑dashboard-D8BFD8?logo=unrealengine&logoColor=3a3a3d)
&nbsp;&nbsp;[![release](https://img.shields.io/github/v/release/ar51an/unbound-dashboard?display_name=release&logo=rstudio&color=90EE90&logoColor=8FBC8F)](https://github.com/ar51an/unbound-dashboard/releases/latest/)
&nbsp;&nbsp;![downloads](https://img.shields.io/github/downloads/ar51an/unbound-dashboard/total?color=orange&label=downloads&logo=github)
&nbsp;&nbsp;![visitors](https://img.shields.io/endpoint?color=4883c2&label=visitors&logo=github&url=https%3A%2F%2Fhits.dwyl.com%2Far51an%2Funbound-dashboard.json)
&nbsp;&nbsp;![license](https://img.shields.io/github/license/ar51an/unbound-dashboard?color=CED8E1)
</div>

<br/>

![Dark](https://user-images.githubusercontent.com/11185794/220276925-6a1f4e0c-e6ec-443d-9ccf-cc3eb5da3405.png)

<details>
  <summary>Light Theme</summary>

  ![Light](https://user-images.githubusercontent.com/11185794/220276981-82c05a6d-0c26-4473-a8aa-902b58237ad3.png)

</details>

<div align="center">
  <img src="https://user-images.githubusercontent.com/11185794/205388020-99c057ad-ee9d-440b-8df9-587f5c133f2e.png?raw=true" alt="divider"/>
</div>

<div align="justify">

### Summary
🔸 Unbound dashboard in `Grafana`  
🔸 `Prometheus` time series database  
🔸 Unbound metrics exporter in `Go`  
🔸 Log aggregation with `Loki`  
🔸 Unbound `setup` is available at [unbound-redis](https://github.com/ar51an/unbound-redis)  
🔸 Refer to `info.md` for dashboard details

#### Specs:
> |Grafana|Prometheus|Loki   |Go      |OS                           |HW                      |
> |:------|:---------|:------|:-------|:----------------------------|:-----------------------|
> |`10.1.0`|`2.33.5`  |`2.8.4`|`1.19.4`|`raspios-bullseye-arm64-lite`|`Raspberry Pi 4 Model B`|

#
### Steps
&nbsp;&nbsp;🔸 Grafana ➜ Prometheus ➜ Unbound Exporter ➜ Loki ➜ Import Dashboard
#### ❯ Grafana
* **Download:**  
  There are 2 versions **OSS** and **Enterprise**. OSS version is more than enough. Enterprise version installs too many extra packages (like unattended-upgrades and more). Below cmd downloads _Grafana OSS_ for arm64.
  > `wget https://dl.grafana.com/oss/release/grafana_10.1.0_arm64.deb`

* **Install:**
  > `sudo dpkg -i grafana_10.1.0_arm64.deb`

  > `ℹ️` **Note:**  
  > A tweaked `grafana.ini` is available in the release. It reduces memory footprint, removes usage collection, stops calls to grafana server/repo and has few more optimizations. You can use _grafana.ini_ **either** from the release **or** the default one. Default config is located at `/etc/grafana/grafana.ini`

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
  > `sudo apt install prometheus/bullseye-backports`

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
  Modify Unbound config. Edit `/etc/unbound/unbound.conf`
  * Enable extended stats. Add option under **`server:`** tag  
    > `extended-statistics: yes`

  * Enable Unix domain socket for collecting stats. It is faster than default TCP. Add below options under **`remote-control:`** tag  
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
#### ❯ Loki
* **Download:**  
  Download `Loki` and `Promtail`
  > `curl -O -L "https://github.com/grafana/loki/releases/download/v2.7.3/loki_2.7.3_arm64.deb"`  
  > `curl -O -L "https://github.com/grafana/loki/releases/download/v2.7.3/promtail_2.7.3_arm64.deb"`

* **Install:**
  > `sudo dpkg -i loki_2.7.3_arm64.deb`  
  > `sudo dpkg -i promtail_2.7.3_arm64.deb`

* **Logging:**  
  Enable Unbound logging.
  * Edit `/etc/unbound/unbound.conf`. Add/Modify below options under **`server:`** tag  
    > `log-replies: yes`  
    > `log-tag-queryreply: yes`  
    > `log-local-actions: yes`  
    > `logfile: /var/log/unbound/unbound.log`

    > `ℹ️` **Note:**  
    > Make sure `verbosity:` is set to 0.

  * Create log dir  
    > `sudo mkdir /var/log/unbound`  
    > `sudo chown unbound:unbound /var/log/unbound`

  * Enable log rotation
    > Copy file `unbound` from the release under _logrotate_ dir to `/etc/logrotate.d/` dir. Make sure it is under the ownership of root.

* **Config:**  
  Replace `/etc/loki/config.yml` and `/etc/promtail/config.yml` with the `config.yml` files in the release under _loki_ and _promtail_ dirs respectively. Both should be under the ownership of root.

  > `ℹ️` **Note:**  
  > Provided _loki_ `config.yml` is optimized to process large data set and metrics calculation for Unbound logs. It is tweaked after some thorough testing in Unbound logs specific dashboard with 6 panels. Default _loki_ config can hardly handle 2 panels with small data set, it throws errors and timeouts with large data set and multiple requests.  
  > Provided _promtail_ `config.yml` enables Unbound logs scrapping.

* **Restart:**
  > `sudo systemctl restart loki`  
  > `sudo systemctl restart promtail`

#
#### ❯ Import Dashboard
* Open Grafana UI ➟ `http://<RP-IP>:3000/`

  * Click `Data Sources` under `Configuration`. Click `Add data source` select **`Prometheus`**. Add below options:  
    > _Name_ ➟ `Prometheus`  
    > _Default_ ➟ `On`  
    > Add _URL_ ➟ `http://localhost:9090`  
    > Add _Scrape interval_ ➟ `5m`  
    > Hit ➟ `Save & test`

  * Click `Data Sources` under `Configuration`. Click `Add data source` select **`Loki`**. Add below options:  
    > _Name_ ➟ `Loki`  
    > Add _URL_ ➟ `http://localhost:3100`  
    > Add _Maximum lines_ ➟ `100000`  
    > Hit ➟ `Save & test`

  * Dashboard, `unbound-dashboard.json` is available in the release. Click `Import` under `Dashboards`. Click `Upload JSON file`. Select `unbound-dashboard.json`. Add below options:  
    > _Folder_ ➟ `General`  
    > Select _Prometheus_ ➟ `Data Source`  
    > Select _Loki_ ➟ `Data Source`  
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
  > ![Metrics](https://user-images.githubusercontent.com/11185794/217952236-ef8ec0cb-a754-49d1-a1b4-d3a7cf5f49ef.png)

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
</div>
