
<div align="justify">

#### ❯ Top Panel
![stats](https://user-images.githubusercontent.com/11185794/220017124-f0f26b72-f824-4236-8350-2f6f60187526.png)

**`Lookup Avg`**  
Current average time it took to answer recursive queries. It does not include cached replies. Average is calculated over all the recursive replies since Unbound start.

**`Req Max`**  
Maximum size attained by the internal recursive processing request list.

**`Req Avg`**  
Average number of requests in the internal recursive processing request list on new incoming recursive processing query.

<div align="center">
  <img src="https://user-images.githubusercontent.com/11185794/205388020-99c057ad-ee9d-440b-8df9-587f5c133f2e.png?raw=true" alt="divider"/>
</div>

#### ❯ Recursion Time
Graph shows recursion times for last 6 hrs with 10 minutes interval.
  
![recursion-graph](https://user-images.githubusercontent.com/11185794/220017145-ecfd028a-7e77-4503-9113-010fdbc66819.png)

**`Avg`**  
Average time it took to answer recursive queries. It does not include cached replies. It is calculated over all the recursive replies since Unbound start.
  
**`Median`**  
Median of the time it took to answer recursive queries. It indicates 50% of queries were answered in less than this time. Average can be much higher than the median, usually due to few queries to non responsive servers.  It is calculated over all the recursive replies since Unbound start.

<div align="center">
  <img src="https://user-images.githubusercontent.com/11185794/205388020-99c057ad-ee9d-440b-8df9-587f5c133f2e.png?raw=true" alt="divider"/>
</div>

#### ❯ Client Requests / Queries / Live Clients
Bar graphs show stats for last 24 hrs with 30 minutes interval. Each bar represents accumulated data for last 30 minutes. Graph can be filtered by clicking item in the legend. Tooltip shows detailed information for that interval. **IPs** in `Client Requests` and `Live Clients` panels can be mapped to **hostnames** with `Transform` ➟ `Organize fields`.

🔹 `Client Requests` show each client's requests

![client-requests](https://user-images.githubusercontent.com/11185794/220108487-9bc0ac1b-2900-476d-a5c3-855601c7b57c.png)

🔹 `Client Requests` filtered on client

![client-requests-filtered](https://user-images.githubusercontent.com/11185794/220108557-e52a10ad-87ce-4354-bd12-aa3b8ac1ebe3.png)

🔹 `Client Requests` IPs mapped to hostnames

![client-requests-mapped](https://user-images.githubusercontent.com/11185794/220220888-0950aebf-bc65-44e4-baf6-e54024ce1f53.png)

🔹 `Live Clients` show active clients
  
![live-clients](https://user-images.githubusercontent.com/11185794/220217145-79888d69-b51b-419b-aa02-6168c860e500.png)

<div align="center">
  <img src="https://user-images.githubusercontent.com/11185794/205388020-99c057ad-ee9d-440b-8df9-587f5c133f2e.png?raw=true" alt="divider"/>
</div>

#### ❯ Top Client Queries
Table shows stats for last 24 hrs. Each row depicts domain along with count requested by client individually. Column filters can be used to filter on client and/or domain. 

🔹 `Top Client Queries` filtered on client
  
![top-client-queries-filtered-client](https://user-images.githubusercontent.com/11185794/220221744-6d756162-57be-43ea-bbbc-7fd64e392fd9.png)

🔹 `Top Client Queries` filtered on domain

![top-client-queries-filtered-domain](https://user-images.githubusercontent.com/11185794/220221770-5c885f77-bf5b-4156-9cd9-ddf5a3f90c76.png)


<div align="center">
  <img src="https://user-images.githubusercontent.com/11185794/205388020-99c057ad-ee9d-440b-8df9-587f5c133f2e.png?raw=true" alt="divider"/>
</div>

#### ❯ Live Blocked
Table shows live blocked domains for last 24 hrs. Dashboard auto refresh interval is set at 5 minutes, `Refresh dashboard` button can fetch to the minute data. It can be helpful for troubleshooting a blocked domain.

![live-blocked](https://user-images.githubusercontent.com/11185794/220223333-0e1b4a2c-2351-41a1-930f-c858852c726c.png)

<div align="center">
  <img src="https://user-images.githubusercontent.com/11185794/205388020-99c057ad-ee9d-440b-8df9-587f5c133f2e.png?raw=true" alt="divider"/>
</div>

#### ❯ Top Client Queries / Live Queries / Live Blocked
**IPs** under _Client_ column can be mapped to **hostnames**. Add `Overrides` ➟ `Value mappings` for _Client_ column.

🔹 `Top Client Queries` IPs mapped to hostnames

![top-client-queries-mapped](https://user-images.githubusercontent.com/11185794/220246706-66c3fd0a-ce95-40e3-b74a-500aec5a0b42.png)
</div>
