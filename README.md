# Docker Compose Centos

本repo提供一些基本的Dockerfile, 方便快速佈署環境使用


# 使用步驟

* 確認有無安裝docker, 推薦kitematic無腦安裝 https://kitematic.com/

    windows作業系統執行 Docker 前請注意BIOS Virtual Technology有開啟

    安裝後執行 Docker Quickstart Terminal 及 Kitematic

* 下載本repo

	```php	
	git clone https://github.com/burgess1109/docker_compose_centos.git
	```

* 請依需求各自修改 docker-compose.yml 開放的ports 跟 掛載volumes路徑(本地路徑必須在c槽 user家目錄底下)

    不需要的container或其他設定可用 # 號註解掉

* 在 Docker Quickstart Terminal 中切換到本repo目錄

	執行 

	```php
	docker-compose -f docker-compose.yml up -d
	```

	相關的容器將會被依序安裝

# 版本資訊

* 版本訊息列於各資料夾中


