### Project Architecture
---
- Data Layer : postgresql, sqlite 지원
- API server : actix web
- Front : node nextjs
- Metrics Collector Manager 
- App (controller)
  - Metric Updater
  - Scaling component manager
  - Scaling planner & Manager      

<br>

### Workflow
---
- Initialize Work flow
	- init data layer -> parse yaml and insert to db
	- init metric collector manager
	- init api server and web app
	- init app
	- looping data change work flow
	
 - Data Changed Work flow
	- notice changes by `tokio::watch_receiver` in data-layer 
	- `metric_collector_manager`.run
	- stop metric updater
	- apply `scaling component def`
	- stop `scaling plans`
	- run metric updater
	- apply `scaling plans def`
	- run loop `scaling plans` -> this is core loop
		1. check cronjob
		2. check js-expression -> 이거 어떻게 동작하는건지 잘 모르겠다. js expression 을 실행하는데 metric collector ( telegraf or vector) 에서 직접적으로 정보를 가져오나?
		3. run plan
		4. logging

- Metric Collectors
	- Vector 
	- Telegraf
	- wa_generator?
