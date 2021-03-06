SHELL := bash

DB_HOST 		?= 127.0.0.1
DB_USERNAME ?= upperio_tests
DB_NAME 		?= upperio_tests
DB_PASSWORD ?= upperio_secret
DB_PORT 		?= 27017

BENCHTIME ?= 10s

export DB_HOST
export DB_PORT
export DB_NAME
export DB_PASSWORD
export DB_USERNAME

require-client:
	@if [ -z "$$(which mongo)" ]; then \
		echo 'Missing "mongo" command. Please install the MongoDB client and try again.' && \
		exit 1; \
	fi

generate:
	go get -d -t -v ./...

reset-db: require-client
	mongo $(DB_NAME) --eval 'db.dropDatabase()' --host $(DB_HOST) --port $(DB_PORT) && \
	mongo $(DB_NAME) --eval 'db.dropUser("$(DB_USERNAME)")' --host $(DB_HOST) --port $(DB_PORT) && \
	mongo $(DB_NAME) --eval 'db.createUser({user: "$(DB_USERNAME)", pwd: "$(DB_PASSWORD)", roles: [{role: "readWrite", db: "$(DB_NAME)"}]})' --host $(DB_HOST) --port $(DB_PORT)

bench: reset-db generate
	go test -v -test.bench=. -test.benchtime=$(BENCHTIME) -benchmem
