#
# Makefile
# fbreitwieser, 2016-10-20 11:54
#

PORT=5000

build:
	docker build -t jband/pavian .

run:
	docker run --rm -p $(PORT):80 jband/pavian

push:
	docker push jband/pavian

#inspect:
#	docker run --rm -i -t --entrypoint /bin/bash -p $(PORT):80 jband/pavian

stop:
	docker stop jband/pavian; docker rm jband/pavian

# Added
exec:
	docker exec jband/pavian cp import/ srv/shiny-server/example-data/

# vim:ft=make
#
