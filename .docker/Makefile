up:
	docker-compose up -d

down:
	docker-compose down

build:
	docker-compose build

import-db:
	@echo "Running import-db task..."
	@if [ -f ./db_import/new_dump.sql ]; then \
		echo "File found, starting import..."; \
		docker cp ./db_import/new_dump.sql example_m2_db:/root/new_dump.sql; \
		echo "File copied to container, running import command..."; \
		docker exec -i example_m2_db sh -c "mysql -uroot -proot magento < /root/new_dump.sql"; \
		echo "Import finished."; \
	else \
		echo "No new_dump.sql found in ./db_import"; \
	fi

magento-setup:
	@echo "Running setup-magento task..."
	docker exec -i example_m2_php sh -c "bin/magento setup:upgrade"
	docker exec -i example_m2_php sh -c "bin/magento setup:di:compile"
	docker exec -i example_m2_php sh -c "bin/magento setup:static-content:deploy en_US uk_UA ru_RU -f"
	docker exec -i example_m2_php sh -c "bin/magento cache:clean"
	docker exec -i example_m2_php sh -c "bin/magento cache:flush"
	echo "Setup magento finished."

magento-perm:
	@echo "Running permission magento task..."
	docker exec -i example_m2_php sh -c "find . -type d -exec chmod 770 {} \;"
	docker exec -i example_m2_php sh -c "find . -type f -exec chmod 660 {} \;"
	docker exec -i example_m2_php sh -c "chgrp -R www-data ."
	docker exec -i example_m2_php sh -c "find . -type d -exec chmod g+s {} \;"
	docker exec -i example_m2_php sh -c "chown -R www-data: var pub/static pub/media generated"
	docker exec -i example_m2_php sh -c "chmod -R g+rw var pub/static pub/media generated"
	echo "finished."
