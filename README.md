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
