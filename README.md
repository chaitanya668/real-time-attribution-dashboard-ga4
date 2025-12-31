![IMG_20260101_012041](https://github.com/user-attachments/assets/c430415d-6536-449f-a4ea-d4d48001aef1)
# real-time-attribution-dashboard-ga4
Near-real-time attribution pipeline using GA4 public data, BigQuery, dbt, and a simulated streaming ingestion with a live dashboard.

real-time-attribution-dashboard-ga4/
├── architecture/
│   └── architecture.png
├── dbt/
│   ├── models/
│   │   ├── staging/
│   │   │   └── stg_ga4_events.sql
│   │   ├── intermediate/
│   │   │   └── int_user_sessions.sql
│   │   └── marts/
│   │       └── mart_attribution_first_last.sql
│   └── schema.yml
├── streaming/
│   └── stream_events.py
├── dashboards/
│   └── looker_layout.md
├── worklog.md
├── README.md
└── demo.mp4

# architectural diagram content 
[ GA4 Public Dataset ]
          |
          |  Batch (BigQuery)
          v
[ BigQuery Raw Events ]
          |
          |  dbt
          v
[ stg_ga4_events ]
          |
          v
[ int_user_sessions ]
          |
          v
[ mart_attribution_first_last ]
          |
          +--------------------+
          |                    |
[ Looker Studio ]     [ Streaming Script ]
   Dashboard                  |
                               v
                     [ BigQuery realtime_events ]
                     
  # architectural diagram content
                    
  ![IMG_20260101_000506](https://github.com/user-attachments/assets/151d750a-7820-4292-b3ba-01a40e56e0eb)

                     

# Real-time Attribution Dashboard – GA4
![IMG_20260101_012041](https://github.com/user-attachments/assets/4226f27e-1707-4b79-ada7-4c854cc25f64)


# Mock Live SQL Interview Questions

1️⃣ First vs Last Click Attribution (Core)

A) select
  user_id,
  first_value(source) over (
    partition by user_id
    order by event_time
  ) as first_click,
  last_value(source) over (
    partition by user_id
    order by event_time
    rows between unbounded preceding and unbounded following
  ) as last_click
from events;

2️⃣ Deduplication Logic

A) select *
from (
  select *,
         row_number() over (partition by event_id order by event_time desc) as rn
  from realtime_events
)
where rn = 1;

3️⃣ Attribution Lookback Window (14 days)

A) where click_time between
      timestamp_sub(conversion_time, interval 14 day)
  and conversion_time

4️⃣ Last Non-Direct Click

A) last_value(
  case when source != 'direct' then source end
  ignore nulls
) over (...)

5️⃣ Incremental dbt Logic

A) {% if is_incremental() %}
where event_date > (select max(event_date) from {{ this }})
{% endif %}
